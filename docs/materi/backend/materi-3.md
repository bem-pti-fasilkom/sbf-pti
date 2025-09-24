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

### URL


### CRUD

Operasi Create, Retrieve, Update, and Delete (CRUD) adalah empat operasi fundamental dalam pemeliharaan data dalam sebuah database.

**Create** : Operasi membuat data entry baru ke dalam database.  
**Retrieve** : Operasi mengambil data dari database.  
**Update** : Operasi memodifikasi data ada di dalam database.  
**Delete** : Operasi menghapus data dalam database.

Method-method HTTP yang ekuivalen dengan operasi-operasi di atas adalah:

**Create** : **POST**  
**Retrieve** : **GET**  
**Update** : **PUT** & **PATCH**  
**Delete** : **DELETE**  

### Response

Response, sesuai dengan namanya, adalah **jawaban dari API terhadap suatu client request**. Jawaban tersebut berupa content yang sesuai dengan request yang diterima. Signature dan penjelasan dari arguments class Response bawaan Django adalah sebagai berikut:

**Signature: Response(data, status=None, template_name=None, headers=None, content_type=None)**

**data** : Data yang sudah diserialize.  
**status** : Status code dari response.  
**template_name** : Nama template jika menggunakan HTMLRenderer.  
**headers** : Dictionary header HTTP yang bisa digunakan.  
**content_type** : Tipe content dari response. Umumnya sudah diset secara otomatis oleh renderer.


## References

API Guide (n.d.). Django REST framework. https://www.django-rest-framework.org/#

 What is REST? (n.d.). Codeacademy. https://www.codecademy.com/article/what-is-rest

CRUD Full Form (2024). GeeksforGeeks. https://www.geeksforgeeks.org/websites-apps/crud-full-form/

Documentation (n.d.). Django. https://docs.djangoproject.com/

REST API Introduction (2025). GeeksforGeeks. https://www.geeksforgeeks.org/node-js/rest-api-introduction/

Tutorial (n.d.). Django REST framework. https://www.django-rest-framework.org/#