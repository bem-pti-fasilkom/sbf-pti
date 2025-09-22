---
title: Authentication, Validation, and Best Practices
sidebar_position: 4
---

# Authentication, Validation, and Best Practices

## Tujuan Pembelajaran

Setelah menyelesaikan materi ini, peserta diharapkan bisa:

1. **Authentication**

   * Memahami perbedaan antara authentication dan authorization.
   * Mengimplementasikan berbagai metode autentikasi (Session, Token, JWT).
   * Membuat fungsi authentication sederhana menggunakan DRF.
   * Menyusun strategi keamanan dasar seperti token management, penggunaan HTTPS, serta konfigurasi CSRF dan CORS.

2. **Validation**

   * Memahami pentingnya validasi dalam API development.
   * Membuat custom validator untuk kebutuhan tertentu (misalnya password strength).
   * Menyusun error handling yang konsisten dan mudah dipahami.

3. **Best Practices**

---

## Authentication

### Definisi

Authentication adalah proses untuk memverifikasi identitas seorang pengguna, sistem, atau entitas digital. Tujuannya adalah memastikan bahwa pihak yang mencoba mengakses aplikasi benar-benar adalah siapa yang mereka klaim. Proses authentication umumnya terjadi saat kita melakukan login pada suatu aplikasi/web.

Singkatnya, authentication menjawab pertanyaan: “Siapa kamu?”

:::caution

**Authentication** dan **Authorization** adalah hal yang berbeda.

**Authentication** adalah proses verifikasi identitas pengguna, misalnya login dengan username dan password untuk memastikan bahwa ia benar-benar pemilik akun.
Sementara itu, **Authorization** adalah proses pemberian hak akses setelah identitas terverifikasi. Misalnya, admin boleh menghapus data sedangkan pengguna biasa hanya bisa melihat data.

:::

### Jenis Authentication

#### 1. **Basic Authentication**

   * **Cara kerja**: username dan password dikirim setiap request di header HTTP, biasanya dalam format Base64
   * **Kelebihan**: sederhana dan mudah dipakai.
   * **Kekurangan**: sangat tidak aman tanpa HTTPS karena kredensial bisa dicuri.
   * **Contoh header request**:
        ```http
        Authorization: Basic noldsafOINoSNfslsfw
        ```

#### 2. **Session Authentication**

   * **Cara kerja**: setelah login, server menyimpan session ID di database lalu mengirimkan cookie ke client. Setiap request berikutnya membawa cookie ini → server mengenali pengguna berdasarkan session. Umumnya digunakan di Django web app biasa (fullstack).
   * **Kelebihan**: aman karena session disimpan di server.
   * **Kekurangan**: lebih cocok untuk aplikasi web tradisional, kurang efisien untuk API modern.
   

#### 3. **Token Authentication**

   * **Cara kerja**: setelah login, server memberikan sebuah token unik. Client harus mengirim token tersebut di setiap request (biasanya via header Authorization).
   * **Kelebihan**: sederhana, cocok untuk API atau back-end application.
   * **Kekurangan**: token biasanya tidak ada masa kadaluarsa, sehingga kurang fleksibel.
   * **Contoh header request**:
        ```http
        Authorization: Token 1234567890abcdef
        ```

#### 4. **JWT (JSON Web Token)**

   * **Cara kerja**: setelah login, server memberikan JWT (encoded string) yang berisi informasi user (payload) + tanda tangan digital. Client menyertakan JWT pada setiap request. JWT bisa memiliki access token (masa berlaku singkat) dan refresh token (untuk memperbarui access token).
   * **Kelebihan**: stateless (server tidak perlu menyimpan session), umum digunakan di SPA (React, Vue, Angular) dan mobile apps.
   * **Kekurangan**: lebih kompleks, harus hati-hati dalam menyimpan token.
   * **Contoh header request**:
        ```http
        Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR...
        ```

#### 5. **OAuth2 dan Social Login**

   * **Cara kerja**: mekanisme login menggunakan akun pihak ketiga (Google, Facebook, GitHub, dll).
   * **Kelebihan**: user tidak perlu membuat akun baru, keamanan lebih baik dengan kontrol akses (scopes).
   * **Kekurangan**: implementasi lebih rumit, butuh integrasi dengan provider eksternal.

#### 6. **Multi-Factor Authentication (MFA)**

   * **Cara kerja**: selain password, pengguna juga harus memberikan faktor tambahan (OTP via SMS/email, kode dari aplikasi authenticator, biometrik).
   * **Kelebihan**: meningkatkan keamanan, sulit ditembus walaupun password bocor.
   * **Kekurangan**: bisa merepotkan pengguna jika tidak didesain dengan baik + biaya tambahan saat implementasi :(.

#### 7. **Biometric Authentication**

   * **Cara kerja**: menggunakan faktor biologis seperti sidik jari, pengenalan wajah, atau retina.
   * **Kelebihan**: sangat user-friendly, cepat, dan aman.
   * **Kekurangan**: butuh hardware khusus, rawan isu privasi.


### Contoh Implementasi Authentication

#### 1. **Session Authentication**
    
    **Konfigurasi**

        ```python
        # settings.py
        INSTALLED_APPS = [
            ...,
            'rest_framework',
            'django.contrib.sessions',
        ]

        MIDDLEWARE = [
            ...,
            'django.contrib.sessions.middleware.SessionMiddleware',
            'django.middleware.csrf.CsrfViewMiddleware',  
            ...
        ]

        REST_FRAMEWORK = {
            'DEFAULT_AUTHENTICATION_CLASSES': [
                'rest_framework.authentication.SessionAuthentication',
            ],
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.IsAuthenticated', # opsional
            ],
        }
        ```

    **Method Register**

        ```python
        # serializers.py
        from django.contrib.auth.models import User
        from rest_framework import serializers

        class RegisterSerializer(serializers.ModelSerializer):
            password = serializers.CharField(write_only=True)
            confirm_password = serializers.CharField(write_only=True)

            class Meta:
                model = User
                fields = ["username", "email", "password", "confirm_password"]

            def validate(self, data):
                if data["password"] != data["confirm_password"]:
                    raise serializers.ValidationError({"confirm_password": "Password tidak sama."})
                return data

            def create(self, validated_data):
                validated_data.pop("confirm_password")
                user = User.objects.create_user(**validated_data)  # password di-hash otomatis
                return user
        ```

        ```python
        # views.py
        from django.contrib.auth import login, authenticate
        from rest_framework.views import APIView
        from rest_framework.response import Response
        from rest_framework import status
        from .serializers import RegisterSerializer

        class RegisterSessionView(APIView):
            authentication_classes = []    # publik
            permission_classes = []        # publik

            def post(self, request):
                s = RegisterSerializer(data=request.data)
                s.is_valid(raise_exception=True)
                user = s.save()

                # Opsi: auto-login -> buat session & cookie
                auth_user = authenticate(username=user.username, password=request.data["password"])
                if auth_user:
                    login(request, auth_user)  # akan membuat cookie sessionid
                return Response({"message": "Registrasi berhasil"}, status=status.HTTP_201_CREATED)
        ```

    **Endpoint**

        ```python
        # urls.py
        from django.urls import path
        from django.contrib.auth import views as auth_views

        urlpatterns = [
            path('/login/', auth_views.LoginView.as_view(), name='login'),
            path('/logout/', auth_views.LogoutView.as_view(), name='logout'),
        ]
        ```

    **View Protection**

        ```python
        # views.py
        from rest_framework.views import APIView
        from rest_framework.response import Response
        from rest_framework.permissions import IsAuthenticated

        class MeView(APIView):
            permission_classes = [IsAuthenticated]  # if user.isAuthenticated == true

            def get(self, request):
                return Response({"user": request.user.username})

        ```

        :::note

        Browser otomatis mengirim cookie sessionid pada tiap request.
        Untuk POST/PUT/PATCH/DELETE, sertakan CSRF token:
            Ambil csrftoken dari cookie, kirim header X-CSRFToken: [csrftoken].
        :::

#### 2. **Token Authentication**

    **Konfigurasi**

        ```python
        # settings.py
        INSTALLED_APPS = [
            ...,
            'rest_framework',
            'rest_framework.authtoken',   # penting
        ]

        REST_FRAMEWORK = {
            'DEFAULT_AUTHENTICATION_CLASSES': [
                'rest_framework.authentication.TokenAuthentication',
            ],
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.IsAuthenticated',  # opsional
            ],
        }

        ```

    **Method Register**

        ```python
        # views.py
        from rest_framework.authtoken.models import Token
        from rest_framework.views import APIView
        from rest_framework.response import Response
        from rest_framework import status
        from .serializers import RegisterSerializer

        class RegisterTokenView(APIView):
            authentication_classes = []
            permission_classes = []

            def post(self, request):
                user_entry = RegisterSerializer(data=request.data)
                user_entry.is_valid(raise_exception=True)
                user = user_entry.save()

                return Response({
                    "message": "Registrasi berhasil",
                }, status=status.HTTP_201_CREATED)
        ```

    **Endpoint**

        ```python
        # urls.py
        from django.urls import path
        from rest_framework.authtoken.views import obtain_auth_token

        urlpatterns = [
            path('/login/', obtain_auth_token, name='api_token_login'),
        ]
        ```

    **Contoh Request**

        ```http
        POST /api/login/
        Content-Type: application/json

        {"username":"Arung","password":"Secret"}
        ```

    **Contoh Response**

        ```http
        {"token": "9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b"}
        ```

    **View Protection**

        ```python
        # views.py
        from rest_framework.views import APIView
        from rest_framework.response import Response
        from rest_framework.permissions import IsAuthenticated

        class ProfileView(APIView):
            permission_classes = [IsAuthenticated]   

            def get(self, request):
                return Response({"username": request.user.username})

        ```

    **Cara Pakai Pada Header Request**

        ```http
        GET /api/profile/
        Authorization: Token <token>
        ```

        :::note

        Saat melakukan request pada endpoint /login/
        Server akan mengirimkan sebuah token yang bisa digunakan sebagai headers pada Authorization untuk request lainnya.
        Jika client tidak menambahkan header Authorization berupa token maka view yang diproteksi tidak akan bisa diakses (diblock oleh IsAuthenticated).
        TokenAuthentication bawaan biasanya stateful (ada tabel token di DB); cocok untuk API sederhana/internal.

        :::

#### 3. **JWT (JSON Web Token)**
    **Instalasi & Konfigurasi**

        ```bash
        pip install djangorestframework-simplejwt
        ```

        ```python
        # settings.py
        INSTALLED_APPS = [
            ...,
            'rest_framework',
        ]

        REST_FRAMEWORK = {
            'DEFAULT_AUTHENTICATION_CLASSES': [
                'rest_framework_simplejwt.authentication.JWTAuthentication',
            ],
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.IsAuthenticated',  # opsional
            ],
        }
        ```

    **Register Method**

        ```python
        # views.py
        from rest_framework.views import APIView
        from rest_framework.response import Response
        from rest_framework import status
        from rest_framework_simplejwt.tokens import RefreshToken
        from .serializers import RegisterSerializer

        class RegisterJWTView(APIView):
            authentication_classes = []
            permission_classes = []

            def post(self, request):
                user_entry = RegisterSerializer(data=request.data)
                user_entry.is_valid(raise_exception=True)
                user = user_entry.save()

                return Response({
                    "message": "Registrasi berhasil",
                }, status=status.HTTP_201_CREATED)
        ```

    **Endpoint**

        ```python
        # urls.py
        from django.urls import path
        from rest_framework_simplejwt.views import (
            TokenObtainPairView,  # login → dapat access & refresh
            TokenRefreshView,     # refresh access token
        )

        urlpatterns = [
            path('login/', TokenObtainPairView.as_view(), name='jwt_token_obtain_pair'),
            path('refresh/', TokenRefreshView.as_view(), name='jwt_token_refresh'),
        ]
        ```

    **Contoh Request Login**

        ```http
        POST /api/login/
        Content-Type: application/json

        {"username":"Arung","password":"Secret"}
        ```

    **Contoh Response**

        ```http
        {
            "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
            "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6..."
        }
        ```

    *Contoh Request Refresh**

        ```http
        POST /api/refresh/
        Content-Type: application/json

        {"refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6..."} <- Masukkan refresh_token yang didapat dari response login
        ```
    

    **View Protection**

        ```python
        # views.py
        from rest_framework.views import APIView
        from rest_framework.permissions import IsAuthenticated
        from rest_framework.response import Response

        class SecureDataView(APIView):
            permission_classes = [IsAuthenticated]

            def get(self, request):
                return Response({"msg": "OK, JWT valid"})

        ```

    **Cara Pakai Pada Header Request**

        ```http
        GET /api/secure-data/
        Authorization: Bearer <access_token>
        ```
    
        :::note

        Saat melakukan request pada endpoint /login/
        Server akan mengirimkan sebuah 2 token yaitu access yang bisa digunakan sebagai headers pada Authorization untuk request lainnya dan refresh untuk digunakan pada request refresh token.
        Jika client tidak menambahkan header Authorization berupa token maka view yang diproteksi tidak akan bisa diakses (diblock oleh IsAuthenticated).
        TokenAuthentication bawaan biasanya stateful (ada tabel token di DB); cocok untuk API sederhana/internal.

        :::

## Validation

### Definisi

Validation adalah proses untuk memeriksa dan memastikan bahwa data yang diterima oleh sistem benar, sesuai aturan, dan aman sebelum data tersebut diproses atau disimpan ke dalam database.

Dalam API development, terutama menggunakan Django REST Framework, validation dilakukan untuk menjaga integritas data dan mencegah kesalahan maupun serangan akibat input yang tidak sesuai.

### Tujuan Umum

1. **Menjaga konsistensi data** -> contoh: umur harus berupa angka positif.
2. **Mencegah error saat menyimpan ke database** -> contoh: field email harus berbentuk format email valid.
3. **Melindungi sistem dari input berbahaya** -> contoh: sanitasi input untuk menghindari SQL injection atau script injection.
4. **Memberikan feedback yang jelas ke pengguna** -> jika data tidak valid, sistem mengembalikan pesan error yang menjelaskan kesalahan.

### Jenis-jenis Validation

#### 1. **Field-level Validation**

   * **Cara kerja**: Validasi dilakukan untuk satu field tertentu.
   * **Cara implementasi**: Membuat method validate_[fieldname] dalam serializer.
   * **Kekurangan**: sangat tidak aman tanpa HTTPS karena kredensial bisa dicuri.
   * **Contoh kasus**: memastikan email valid atau umur tidak negatif.
   * **Contoh kode**:
        ```python
        from rest_framework import serializers

        class UserSerializer(serializers.Serializer):
            username = serializers.CharField(max_length=50)
            age = serializers.IntegerField()

            def validate_age(self, value):
                if value < 0:
                    raise serializers.ValidationError("Umur tidak boleh negatif.")
                return value

        ```

#### 2. **Object-level Validation**

   * **Cara**: Validasi yang berlaku untuk **beberapa field sekaligus**.
   * **Cara implementasi**: Override method `validate()` pada serializer.
   * **Contoh kasus**: `password` harus sama dengan `confirm_password`, atau `start_date` harus lebih kecil dari `end_date`.
   * **Contoh kode**:
        ```python
        class RegisterSerializer(serializers.Serializer):
            password = serializers.CharField(write_only=True)
            confirm_password = serializers.CharField(write_only=True)

            def validate(self, data):
                if data['password'] != data['confirm_password']:
                    raise serializers.ValidationError({
                        "confirm_password": "Password tidak sama."
                    })
                return data
        ```

#### 3. **Custom Validators**

   * **Definisi**: Fungsi atau class terpisah yang dapat dipakai ulang untuk memvalidasi data tertentu.
   * **Kelebihan**: Reusable di banyak serializer/field.
   * **Contoh kasus**: validasi kekuatan password, validasi format NIK/KTP, validasi domain email tertentu.
   * **Contoh kode**:
        ```python
        def validate_even(value):
            if value % 2 != 0:
                raise serializers.ValidationError("Angka harus genap.")

        class NumberSerializer(serializers.Serializer):
            number = serializers.IntegerField(validators=[validate_even])
        ```

#### 4. **Model Validation**

   * **Definisi**: Validasi yang didefinisikan langsung di model Django (`clean()` atau field constraints).
   * **Kelebihan**: Lebih dekat dengan database → data tetap tervalidasi meskipun tidak lewat serializer.
   * **Contoh kode**:
        ```python
        from django.db import models
        from django.core.exceptions import ValidationError

        class Product(models.Model):
            name = models.CharField(max_length=100)
            price = models.DecimalField(max_digits=10, decimal_places=2)

            def clean(self):
                if self.price < 0:
                    raise ValidationError("Harga tidak boleh negatif.")
        ```

#### 5. **Unique & Constraint Validation**

   * **Definisi**: Validasi otomatis berdasarkan constraint di model/serializer.
   * **Contoh kasus**: field `email` harus unik.
   * **Contoh kode**:
        ```python
        class RegisterSerializer(serializers.ModelSerializer):
                class Meta:
                    model = User
                    fields = ['username', 'email', 'password']
                    extra_kwargs = {
                        'email': {'validators': [UniqueValidator(queryset=User.objects.all())]}
                    }
        ```

## Best Practice

### 1. Struktur Project yang Rapi

Pisahkan kode sesuai tanggung jawabnya:

* `models.py` → definisi struktur data.
* `serializers.py` → validasi & konversi data.
* `views.py` → logika API endpoint.
* `urls.py` → routing.

Jika aplikasi besar, gunakan pendekatan **modular dengan banyak app Django** agar lebih mudah dikelola.

### 2. Gunakan ViewSet & Router

* Untuk operasi standar CRUD, gunakan **ViewSet** dan **Router** agar kode lebih singkat dan konsisten.
* Contoh:

  ```python
  from rest_framework.viewsets import ModelViewSet
  from .models import Book
  from .serializers import BookSerializer

  class BookViewSet(ModelViewSet):
      queryset = Book.objects.all()
      serializer_class = BookSerializer
  ```

  ```python
  from rest_framework.routers import DefaultRouter
  from .views import BookViewSet

  router = DefaultRouter()
  router.register(r'books', BookViewSet)
  urlpatterns = router.urls
  ```

### 3. Konsistensi Response & Error Handling

* Gunakan format response yang konsisten.
* Saat error, selalu sertakan **pesan jelas** agar mudah dipahami klien.
* Contoh format error:

  ```json
  {
    "errors": {
      "email": ["Email sudah digunakan."],
      "password": ["Password terlalu lemah."]
    }
  }
  ```

### 4. Gunakan Pagination, Filtering, dan Searching

* API dengan banyak data perlu **pagination** agar ringan diakses.
* Tambahkan **filtering & search** untuk memudahkan klien.
* Contoh:

  ```python
  REST_FRAMEWORK = {
      'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
      'PAGE_SIZE': 10
  }
  ```

### 5. Perhatikan Keamanan

* Selalu gunakan **HTTPS**.
* Terapkan **authentication** & **permission** dengan benar (`IsAuthenticated`, `IsAdminUser`, dll.).
* Gunakan **rate limiting** untuk mencegah abuse (misalnya dengan `django-ratelimit`).
* Jangan pernah menyimpan password dalam bentuk plain text → selalu hash.

### 6. Testing API

* Buat **unit test** untuk setiap endpoint agar bug cepat terdeteksi.
* Gunakan `APITestCase` bawaan DRF.
* Contoh:

  ```python
  from rest_framework.test import APITestCase
  from django.urls import reverse

  class BookAPITest(APITestCase):
      def test_create_book(self):
          url = reverse('book-list')
          data = {"title": "New Book", "author": "Helven"}
          response = self.client.post(url, data, format='json')
          self.assertEqual(response.status_code, 201)
  ```

### 7. Dokumentasi API

* Dokumentasi mempermudah developer lain (atau tim frontend) memahami API.
* Gunakan library seperti **drf-yasg** atau **drf-spectacular** untuk membuat dokumentasi otomatis berbasis OpenAPI/Swagger.
* Contoh: akses `/swagger/` untuk dokumentasi interaktif.

### 8. Versioning API

* Untuk API publik atau skala besar, gunakan **versioning** (`/api/v1/`, `/api/v2/`) agar tidak merusak klien lama saat ada update besar.

### 9. Optimasi Query

* Gunakan `select_related` atau `prefetch_related` untuk menghindari **N+1 query problem**.
* Contoh:

  ```python
  books = Book.objects.select_related('author').all()
  ```

### 10. Logging & Monitoring

* Tambahkan **logging** untuk error & aktivitas penting.
* Gunakan tools monitoring (misalnya Sentry) untuk melacak error di production.
