---
title: Materi 3
sidebar_position: 3
---

# Django REST (CBV, URL, CRUD, Response)

## What is REST?

**Representational State Transfer (REST) adalah suatu architectural style di mana client dan server terpisah**. Keterpisahan ini memungkinkan code di sisi client untuk dimodifikasi tanpa mengganggu sisi server dan kedua sisi juga tidak perlu mengetahui state masing-masing untuk bisa beroperasi. Selama kedua sisi mengetahui format yang digunakan untuk berkomunikasi dengan satu sama lain, yaitu HTTP request dari client ke server dan JSON response dari server ke client, kedua sisi dapat berkomunikasi dan bertukar informasi dengan satu sama lain 

##  Django REST Framework

### Class-Based View

View adalah suatu proses yang menerima sebuah request dan memberi response yang sesuai. Pada Django, terdapat dua cara untuk membuat view; sebagai function dan sebagai class. Kita akan fokus terhadap view sebagai class, yaitu class-based view.

Dengan class-based view, kita bisa membuat **struktur view yang lebih flexible dan complex**. Di mana kita mengatur response yang sesuai untuk method-method HTTP tertentu dengan conditional branching pada function-based view, kita bisa membuat method-method khusus untuk method-method HTTP tersebut pada class-based-view.

#### Contoh Versi Class-Based View
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from django.shortcuts import get_object_or_404
from .models import Student, Course
from .serializers import StudentSerializer, CourseSerializer

class AllStudentsAPIView(APIView):
    # Melihat seluruh Student
    def get(self, request):
        students = Student.objects.all()
        serializer = StudentSerializer(students, many=True)
        return Response(serializer.data, status=status.HTTP_200_OK)

    # Membuat instance Student baru
    def post(self, request):
        serializer = StudentSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

class StudentDetailAPIView(APIView):
    # Mengambil salah satu Student
    def get_object(self, id):
        return get_object_or_404(Student, id=id)

    # Melihat salah satu Student
    def get(self, request, id):
        student = self.get_object(id)
        serializer = StudentSerializer(student)
        return Response(serializer.data)

    # Meng-update salah satu Student
    def put(self, request, id):
        student = self.get_object(id)
        serializer = StudentSerializer(student, data=request.data, partial=True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    # Menghapus salah satu Student
    def delete(self, request, id):
        student = self.get_object(id)
        student.delete()
        return Response({'message': 'Student deleted successfully'}, status=status.HTTP_200_OK)
```

#### Contoh Versi Function-Based View
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from django.shortcuts import get_object_or_404
from .models import Student, Course
from .serializers import StudentSerializer, CourseSerializer

@api_view(['GET', 'POST'])
def allStudents(request):
    # Melihat seluruh Student
    if request.method == 'GET':
        students = Student.objects.all()
        serializer = StudentSerializer(students, many=True)
        return Response(serializer.data, status=status.HTTP_200_OK)

    # Membuat instance Student baru
    elif request.method == 'POST':
        serializer = StudentSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


@api_view(['GET', 'PUT', 'DELETE'])
def studentDetails(request, id):
    student = get_object_or_404(Student, id=id)

    # Melihat salah satu Student
    if request.method == 'GET':
        serializer = StudentSerializer(student)
        return Response(serializer.data)

    # Meng-update salah satu Student
    elif request.method == 'PUT':
        serializer = StudentSerializer(student, data=request.data, partial=True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    # Menghapus salah satu Student
    elif request.method == 'DELETE':
        student.delete()
        return Response({'message': 'Student deleted successfully'}, status=status.HTTP_200_OK)
```

#### Filtering & Pagination Dengan Class-Based View
```python
class StudentListCreateAPIView(APIView):
    """
    Handles:
    - GET: List all students, with filtering, searching, and pagination
    - POST: Create a new student
    """

    def get(self, request):
        students = Student.objects.all()

        name_query = request.query_params.get("name")
        npm_query = request.query_params.get("npm")
        search_query = request.query_params.get("search")

        # Filtering
        if name_query:
            students = students.filter(name__icontains=name_query)
        if npm_query:
            students = students.filter(npm__icontains=npm_query)
        if search_query:
            students = students.filter(name__icontains=search_query) | students.filter(npm__icontains=search_query)

        # Pagination
        page = request.query_params.get("page", 1)
        page_size = int(request.query_params.get("page_size", 5))
        paginator = Paginator(students, page_size)
        page_obj = paginator.get_page(page)

        serializer = StudentSerializer(page_obj, many=True)
        return Response({
            "count": paginator.count,
            "num_pages": paginator.num_pages,
            "results": serializer.data
        }, status=status.HTTP_200_OK)
```

#### Filtering & Pagination Dengan Function-Based View
```python
@api_view(['GET', 'POST'])
def allStudents(request):
    if request.method == 'GET':
        students = Student.objects.all()
        name = request.query_params.get("name")
        npm = request.query_params.get("npm")
        search = request.query_params.get("search")

        # Filtering
        if name:
            students = students.filter(name__icontains=name)
        if npm:
            students = students.filter(npm__icontains=npm)
        if search:
            students = students.filter(name__icontains=search) | students.filter(npm__icontains=search)

        # Pagination
        page = request.query_params.get("page", 1)
        page_size = int(request.query_params.get("page_size", 5))
        paginator = Paginator(students, page_size)
        page_obj = paginator.get_page(page)

        serializer = StudentSerializer(page_obj, many=True)
        return Response({
            "count": paginator.count,
            "num_pages": paginator.num_pages,
            "results": serializer.data
        })
```

---
### URL

**Uniform Resource Locator (URL) me-map suatu request dengan suatu function/class yang memberi suatu response**. Pada Django, mapping URL dengan function/classnya dilakukan di urls.py. Saat suatu request dikirim oleh browser, URL pattern yang sesuai akan dicari, lalu, jika URL ditemukan, function/class yang bersangkutan akan di-call untuk mendapat response. Jika URL pattern tidak ditemukan, Django akan me-return status code 404 (Not Found).

#### Contoh URL
```python
from django.contrib import admin
from django.urls import path, include
from .views import AllStudentsAPIView, StudentDetailAPIView allStudents, studentDetails

app_name = 'studentApp'

urlpatterns = [
    path('admin/', admin.site.urls),           # Django admin
    path('', include('main.urls')),            # Menambahkan URL dari main.urls
    path('accounts/', include('users.urls')),  # Menambahkan URL dari users.urls

    # URL CBV
    path('all_students/', AllStudentsAPIView.as_view(), name='allStudents_Class'),
    path('student/<int:id>/', StudentDetailAPIView.as_view(), name='studentDetails_Class'),

    # URL FBV
    path('all_students_2/', allStudents, name='allStudents_Function'),
    path('student_2/<int:id>/', studentDetails, name='studentDetails_Function'),
]
```

Pada contoh URL di atas, dapat dilihat adanya **name**. Name dari suatu URL mempermudahnya untuk di-reference.

```python
# Reference di HTML page
{% url 'studentApp:studentDetails_Class' id=5 %}

# Reference di Python
reverse_lazy('studentApp:studentDetails_Class', args=[5]) # Lebih disarankan untuk CBV agar tidak terkena import-time error
reverse('studentApp:studentDetails_Class', args=[5])
reverse('student-section-detail', kwargs={'id': 5, 'section': 'A'}) # Misal pathnya students/<int:id>/<str:section>/
redirect('studentApp:studentDetails_Class', id=5) # Functional di CBV dan FBV
render(request, 'studentDetails.html', {'student': Student.object.all().filter(pk=5)}) # Misal nama HTML pagenya studentDetails.html

```

---
### CRUD

Operasi Create, Retrieve, Update, and Delete (CRUD) adalah empat operasi fundamental dalam pemeliharaan data dalam sebuah database.

**Create** : Operasi membuat data entry baru ke dalam database.  
**Retrieve** : Operasi mengambil data dari database.  
**Update** : Operasi memodifikasi data ada di dalam database.  
**Delete** : Operasi menghapus data dalam database.

\
Method-method HTTP yang ekuivalen dengan operasi-operasi di atas adalah:

**Create** : **POST**  
**Retrieve** : **GET**  
**Update** : **PUT** & **PATCH**  
**Delete** : **DELETE**  

\
**PUT vs PATCH**

Walaupun sama-sama meng-update data object, terdapat perbedaan antara PUT dan PATCH; **PUT akan memodifikasi seluruh bagian dari object sedangkan PATCH hanya memodifikasi sebagian dari object tersebut**. Oleh karena itu, request body dari PUT harus mengandung seluruh data yang dimiliki suatu object dan request body dari PATCH hanya mengandung sebagian dari data tersebut. Jika terdapat field yang tidak dicantumkan, PUT akan menghapus/men-default-kan field itu dan PATCH tidak akan mengubah datanya.

#### Contoh CRUD
```python
# Create
def post(self, request):
        serializer = StudentSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# Read
def get(self, request, pk):
        student = self.get_object(pk)
        if not student:
            return Response({'error': 'Student not found'}, status=status.HTTP_404_NOT_FOUND)
        serializer = StudentSerializer(student)
        return Response(serializer.data, status=status.HTTP_200_OK)

# Update
def put(self, request, pk):
        student = self.get_object(pk)
        if not student:
            return Response({'error': 'Student not found'}, status=status.HTTP_404_NOT_FOUND)
        serializer = StudentSerializer(student, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_200_OK)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# Update (lagi)
def patch(self, request, pk):
    student = self.get_object(pk)
    if not student:
        return Response({'error': 'Student not found'}, status=status.HTTP_404_NOT_FOUND)
    serializer = StudentSerializer(student, data=request.data, partial=True)
    if serializer.is_valid():
        serializer.save()
        return Response(serializer.data, status=status.HTTP_200_OK)
    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# Delete
def delete(self, request, pk):
    student = self.get_object(pk)
    if not student:
        return Response({'error': 'Student not found'}, status=status.HTTP_404_NOT_FOUND)
    student.delete()
    return Response({'message': 'Student deleted successfully'}, status=status.HTTP_204_NO_CONTENT)
```

---
### Response

Response, sesuai dengan namanya, adalah **jawaban dari API terhadap suatu HTTP request**. Jawaban tersebut berupa content yang sesuai dengan request yang diterima. Signature dan penjelasan dari arguments class Response bawaan Django REST Framework adalah sebagai berikut:


**Response(data, status=None, template_name=None, headers=None, content_type=None)**

\
**data** : Data yang sudah diserialize.  
**status** : Status code dari response.  
**template_name** : Nama template jika menggunakan HTMLRenderer.  
**headers** : Dictionary header HTTP yang bisa digunakan.  
**content_type** : Tipe content dari response. Umumnya sudah diset secara otomatis oleh renderer.

\
Selain Response, Django (Django ori bukan DRF) juga memiliki beberapa tipe response lain:

**HttpResponse**              : Basic HTML.\
**JsonResponse**              : Memberi data dalam bentuk JSON untuk API.\
**HttpResponseRedirect**      : Redirect ke URL lain.\
**FileResponse**              : Memberi file yang bisa didownload.\
**StreamingHttpResponse**     : Stream file yang berukuran besar.\
**HttpResponseBadRequest**    : Jika client memberi request yang invalid.\
**HttpResponseForbidden**     : Jika client tidak memiliki izin untuk mengakses suatu fitur.\
**HttpResponseNotFound**      : Jika resource yang di-request tidak ada.\
**HttpResponseServerError**   : Server-side error.

\
Untuk memberi 'kabar' dari hasil suatu request, kita dapat menggunakan **status code**. Beberapa status code yang umum digunakan adalah sebagai berikut:

**HTTP_200_OK**                     : Success\
**HTTP_201_CREATED**                : Resource created\
**HTTP_204_NO_CONTENT**             : No data\
**HTTP_400_BAD_REQUEST**            : Request invalid\
**HTTP_401_UNAUTHORIZED**           : Not authenticated\
**HTTP_403_FORBIDDEN**              : Permission denied\
**HTTP_404_NOT_FOUND**              : Resource not found\
**HTTP_500_INTERNAL_SERVER_ERROR**  : Server error

---

## References

API Guide (n.d.). Django REST framework. https://www.django-rest-framework.org/#

What is REST? (n.d.). Codeacademy. https://www.codecademy.com/article/what-is-rest

CRUD Full Form (2024). GeeksforGeeks. https://www.geeksforgeeks.org/websites-apps/crud-full-form/

Documentation (n.d.). Django. https://docs.djangoproject.com/

REST API Introduction (2025). GeeksforGeeks. https://www.geeksforgeeks.org/node-js/rest-api-introduction/

Tutorial (n.d.). Django REST framework. https://www.django-rest-framework.org/#