---
title: Materi 2
sidebar_position: 2
---

# Django Advanced (Objects, Model, Serializers)

## Tujuan Pembelajaran
Setelah sesi ini, peserta diharapkan dapat:
- Memahami implementasi OOP di Django.
- Memahami relasi antar model di Django.
- Memahami fungsi serializer dan cara membuatnya.

## Object Oriented Programming
### Definisi OOP

OOP (Object-Oriented Programming) adalah paradigma pemrograman yang berfokus pada objek sebagai representasi dari sesuatu di dunia nyata maupun konsep abstrak.

Objek = gabungan antara data (disebut atribut / properties) dan perilaku (disebut method / fungsi).

Tujuan utamanya: membuat kode lebih terstruktur, mudah dipelihara, reusable, dan mudah dikembangkan.

### Konsep Dasar OOP
Terdapat 4 pilar utama yang sebaiknya ada dalam tiap implementasi OOP yang baik. Keempat pilar tersebut adalah sebagai berikut:
    1. **Encapsulation (Enkapsulasi)**: Menyembunyikan detail internal dari suatu objek dan hanya menyediakan interface tertentu untuk berinteraksi.<br />
    Contoh: atribut saldo di bank nggak bisa diubah langsung, tapi lewat method deposit() atau withdraw()  
    2. **Inheritance (Pewarisan)**: Membuat kelas baru (child) dengan mewarisi properti & method dari kelas lain (parent).<br/>
    Contoh: Mobil bisa diwarisi ke MobilSport atau MobilTruk.
    3. **Polymorphism (Polimorfisme)**: Satu method bisa punya implementasi berbeda tergantung objeknya.<br/>
    Contoh: method suara() pada Hewan → kalau dipanggil dari Kucing hasilnya "meong", dari Anjing hasilnya "guk".
    4. **Abstraction (Abstraksi)**: Menyembunyikan detail implementasi, hanya memperlihatkan hal penting.<br/>
    Contoh: kamu tinggal pakai print() tanpa harus tahu detail gimana teks dikirim ke console.

```python
# Abstraction
class Kendaraan(ABC):
    def __init__(self, merk):
        self.merk = merk
        self._kecepatan = 0   # Encapsulation (atribut dilindungi dengan _) menjadi protected atribute

    @abstractmethod
    def suara_mesin(self):
        pass

    def tambah_kecepatan(self, nilai):
        if nilai > 0:
            self._kecepatan += nilai
        return self._kecepatan

    def get_kecepatan(self):
        return self._kecepatan

# Inheritance (Mobil subclass dari Kendaraan)
class Mobil(Kendaraan):
    def suara_mesin(self):   # Polymorphism (implementasi berbeda)
        return "Brumm brumm"

# Inheritance (Motor subclass dari Kendaraan)
class Motor(Kendaraan):
    def suara_mesin(self):   # Polymorphism
        return "Ngeeng ngeeng"


# Pemakaian
mobil = Mobil("Toyota")
motor = Motor("Honda")
mobil.tambah_kecepatan(40)
motor.tambah_kecepatan(60)
print(f"{mobil.merk} bersuara '{mobil.suara_mesin()}' dengan kecepatan {mobil.get_kecepatan()} km/jam")
print(f"{motor.merk} bersuara '{motor.suara_mesin()}' dengan kecepatan {motor.get_kecepatan()} km/jam")
```
#### Penjelasan Pilar dalam Contoh
Encapsulation → Atribut _kecepatan tidak bisa diakses langsung, hanya lewat tambah_kecepatan() dan get_kecepatan(). <br/>
Inheritance → Mobil dan Motor mewarisi class Kendaraan. <br/>
Polymorphism → suara_mesin() berbeda implementasinya di Mobil dan Motor. <br/>
Abstraction → Class Kendaraan adalah abstract base class (pakai ABC) dan punya method abstrak suara_mesin() yang wajib diimplementasikan oleh subclass.

## Implementasi OOP di Django
Kita dapat melihat implementasi OOP di berbagai bagian dari sebuah kode django.
Contoh implementasi OOP saat membuat sebuah Model:
```python
from django.db import models
from django.contrib.auth.models import User
import uuid

class News(models.Model): # Inheritance! News merupakan subclass (child) dari models.Model
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    title = models.CharField(max_length=255)
    news_views = models.PositiveBigIntegerField(default=0)
    created_at = models.DateTimeField(auto_now_add=True)

    # Tidak ada method __init__() --> menggunakan method __init__() dari parent (models.Model)!

    def __str__(self): # Polymorphism
        return self.title
    
    @property
    def is_news_hot(self):
        return self.news_views > 20
    
    def increment_views(self): # Method tersendiri untuk class News!
        self.news_views += 1
        self.save()
```

## Relasi Antar Model
### Review: Apa Itu Model?
Model bertugas untuk mengatur dan mengelola data pada sebuah aplikasi. Pada Django sendiri, sudah disediakan Object Relational Mapping (ORM) sehingga kamu bisa bekerja dengan command python, tanpa SQL langsung seperti pada database umumnya.

Contoh model:
```python
class News(models.Model):
    title = models.CharField(max_length=255)
    content = models.TextField()
    date_added = models.DateField(auto_now_add=True)
```
### Parameter Umum di models.Field

Hampir semua field di Django punya parameter berikut:

`primary_key=True`
Menjadikan field ini sebagai primary key.

Kalau sudah pakai ini, tidak perlu lagi `unique=True` (karena PK otomatis unik).

Contoh:
```python
id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
```

`unique=True`
Nilai field harus unik di seluruh tabel (nggak boleh ada duplikat).

Contoh:
```python
email = models.CharField(max_length=100, unique=True)
```

`null=True`
Field boleh kosong di database (menyimpan NULL).

Biasanya dipakai di field non-string.

Contoh:

```python
tanggal_lahir = models.DateField(null=True)
```

blank=True
Field boleh kosong di form Django.

Beda dengan `null=True` → ini soal validasi di level aplikasi, bukan database.

Contoh:
```python
alamat = models.CharField(max_length=255, blank=True)
```

`default=value`
Memberi nilai default kalau user nggak mengisi field.

Contoh:
```python
jumlah_view = models.IntegerField(default=0)
```

`choices=[(...)]`
Membatasi isi field ke pilihan tertentu.

Contoh:
```python
JENIS_KELAMIN = [
    ("L", "Laki-laki"),
    ("P", "Perempuan"),
]
gender = models.CharField(max_length=1, choices=JENIS_KELAMIN)
```

`editable=False`
Field tidak bisa diubah lewat form admin.

Contoh:
```python
dibuat = models.DateTimeField(auto_now_add=True, editable=False)
```

`help_text="..."`
Memberi keterangan di form admin.

Contoh:
```python
email = models.EmailField(help_text="Gunakan email aktif")
```

`verbose_name="..."`
Nama field lebih ramah untuk admin panel.

Contoh:
```python
created_at = models.DateTimeField(auto_now_add=True, verbose_name="Tanggal dibuat")
```

`db_index=True`
Membuat index di database untuk mempercepat query.

Contoh:
```python
npm = models.CharField(max_length=10, unique=True, db_index=True)
```

### Apa Itu Relasi?
Relasi dalam konteks basis data (termasuk Django ORM) adalah **hubungan antar tabel atau model**.  
Tujuannya supaya data tidak terduplikasi, lebih terstruktur, dan mudah digunakan kembali.

Contoh di dunia nyata:
- Seorang `mahasiswa` bisa meminjam banyak `buku`.
- Sebuah `buku` hanya bisa dipinjam oleh seorang `mahasiswa`.  
- Sebuah `perpustakaan` bisa menyimpan banyak `buku`.
- Sebuah `buku` bisa disimpan oleh banyak `perpustakaan`.
- Setiap `universitas` hanya dan pasti memiliki satu  `perpustakaan`.
- Setiap `mahasiswa` hanya boleh berkuliah di satu `universitas`.
- Setiap `universitas` bisa menguliahi banyak `mahasiswa`.

Bayangkan bahwa `mahasiswa`, `buku`, `perpustakaan`, dan `universitas` merupakan model tersendiri.
Relasi membantu kita **menghubungkan model-model** ini dengan cara yang efisien.

### Relasi Dalam Django
Django menyediakan tiga jenis relasi utama yang langsung bisa digunakan lewat field bawaan:

1. **One-to-One (`OneToOneField`)**  
   Setiap record di model A hanya bisa berhubungan dengan satu record di model B.  
   > Contoh: User ↔ Profile <br/>
   > **Setiap `User`** dalam sebuah aplikasi hanya boleh memiliki **satu `Profile`**

2. **One-to-Many (`ForeignKey`)**  
   Satu record di model A bisa punya banyak record terkait di model B.  
   > Contoh: Penulis → Buku <br/>
   > **Setiap `Penulis`** bisa menjadi penulis dari **banyak / beberapa `Buku`**

3. **Many-to-Many (`ManyToManyField`)**  
   Banyak record di model A bisa berhubungan dengan banyak record di model B.  
   > Contoh: Mahasiswa ↔ Mata Kuliah <br/>
   > **Banyak / beberapa `Mahasiswa`** bisa mengambil **banyak / beberapa `Mata Kuliah`**

### Mengapa Penting?
- Menghindari data duplikat.  
- Membuat query lebih efisien.  
- Mencerminkan hubungan nyata antar entitas di dunia nyata.  
- Mempermudah navigasi data lewat ORM Django.

### Contoh Implementasi
```python
from django.db import models
import uuid

class Mahasiswa(models.Model):
    nama = models.CharField(max_length=100)
    npm = models.CharField(max_length=10, primary_key=True)
    nama_universitas = models.ForeignKey(to=Universitas, on_delete=models.CASCADE, related_name='mahasiswa')

class Buku(models.Model):
    id = models.UUIDField(default=uuid.uuid4, primary_key=True)
    ditambahkan = models.DateTimeField(auto_now_add=True)
    judul = models.CharField(max_length=100)
    pengarang = models.CharField(max_length=100)
    npm_peminjam = models.ForeignKey(to=Mahasiswa, on_delete=models.CASCADE, related_name='buku')

class Perpustakaan(models.Model):
    nama = models.CharField(max_length=255, primary_key=True)
    ditambahkan = models.DateTimeField(auto_now_add=True)
    alamat_perpustakaan = models.CharField(max_length=255)
    daftar_buku = models.ManyToManyField(to=Buku, related_name='perpustakaan')

class Universitas(models.Model):
    nama = models.CharField(primary_key=True)
    alamat = models.CharField()
    nama_perpustakaan = models.OneToOneField(to=Perpustakaan, on_delete=models.CASCADE, related_name='universitas')
```

## Serializers

### Apa Itu Serializers?
**Serializer** adalah komponen di Django REST Framework (DRF) yang digunakan untuk:
- **Memvalidasi Data**<br/>Memastikan data yang dikirim user sesuai aturan field (required, unique, max_length, null, dsb).
- **Mengubah data kompleks (Model, QuerySet, Object Python)**<br/>Menjadi format yang lebih sederhana seperti **JSON atau XML** sebagai response dari API.
- **Mengubah data JSON**<br/>Mengubah data yang dikirim user (data request) menjadi object Python untuk disimpan ke database.<br/>

Dengan serializer, data dari database bisa **di-expose ke API** dengan mudah.

### Contoh Kasus
Misalkan kita punya model `Student` dan `Course`:

```python
# models.py
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    npm = models.CharField(max_length=15, unique=True)

    def __str__(self):
        return self.name

class Course(models.Model):
    name = models.CharField(max_length=100)
    code = models.CharField(max_length=10, unique=True)
    students = models.ManyToManyField(Student, related_name="courses")

    def __str__(self):
        return self.name
```

### serializers.Serializer
Dengan `serializers.Serializer`, kita harus definisikan field satu per satu.

```python
# serializers.py
from rest_framework import serializers
from .models import Student, Course

class StudentSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    name = serializers.CharField(max_length=100)
    npm = serializers.CharField(max_length=15)

    # Method yang dipanggil ketika user melakukan request dengan metode http POST (create)
    # Untuk membuat student baru
    def create(self, validated_data):  
        return Student.objects.create(**validated_data)

    # Method yang dipanggil ketika user melakukan request dengan metode http PATCH (update)
    # Untuk mengupdate data sebuah student
    def update(self, instance, validated_data): 
        instance.name = validated_data.get("name", instance.name)
        instance.npm = validated_data.get("npm", instance.npm)
        instance.save()
        return instance

class CourseSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    name = serializers.CharField(max_length=100)
    code = serializers.CharField(max_length=10)
    students = StudentSerializer(many=True, read_only=True)

    # Method yang dipanggil ketika user melakukan request dengan metode http POST (create)
    # Untuk membuat course baru
    def create(self, validated_data):
        return Course.objects.create(**validated_data)

    # Method yang dipanggil ketika user melakukan request dengan metode http PATCH (update)
    # Untuk mengupdate data sebuah course
    def update(self, instance, validated_data):
        instance.name = validated_data.get("name", instance.name)
        instance.code = validated_data.get("code", instance.code)
        instance.save()
        return instance
```

### ModelSerializer
Dengan DRF, kita bisa bikin serializer menggunakan ModelSerializer.

```python
from rest_framework import serializers
from .models import Student, Course

class StudentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Student
        fields = ["id", "name", "npm"]

class CourseSerializer(serializers.ModelSerializer):
    students = StudentSerializer(many=True, read_only=True)  # nested serializer

    class Meta:
        model = Course
        fields = ["id", "name", "code", "students"]
```

Kalau kita pakai ModelSerializer, maka:
- `create()` dan `update()` sudah disediakan otomatis oleh DRF berdasarkan model yang kamu daftarkan di Meta.
- Tidak perlu lagi nulis ulang method itu, kecuali diperlukan bikin logika custom di method tersebut.

### Contoh Output JSON

Misalkan ada data di database: <br/>
Student: Owi (npm=17081945) <br/>
Course: Pemrograman Lanjut (code=CS102)<br/>
Hasil serialisasi:<br/>
```json
{
  "id": 1,
  "name": "Pemrograman Lanjut",
  "code": "CS102",
  "students": [
    {
      "id": 1,
      "name": "Sahroni",
      "npm": ""
    }
  ]
}

```

### Deserialisasi (JSON → Object)
Serializer juga bisa dipakai untuk validasi input JSON dari user.
```python
data = {"name": "Algoritma", "code": "CS103"} # dictionary di-treat layaknya JSON
serializer = CourseSerializer(data=data)

if serializer.is_valid():
    course = serializer.save()  # simpan ke database
```

## Kesimpulan

### OOP di Django
Django sangat erat dengan konsep OOP. Model, view, hingga serializer dibangun dengan prinsip inheritance, encapsulation, polymorphism, dan abstraction sehingga kode lebih modular, reusable, dan mudah dikembangkan.

### Model & Relasi
Model adalah representasi tabel di database. Dengan memanfaatkan relasi One-to-One, One-to-Many, dan Many-to-Many, kita bisa menghubungkan entitas (misalnya mahasiswa, buku, universitas, perpustakaan) secara efisien tanpa duplikasi data.

### Serializer
Serializer adalah jembatan antara data kompleks (object, model, queryset) dan format sederhana (JSON/XML) yang dipakai di API.

Bisa dipakai untuk serialisasi (object → JSON).

Bisa dipakai untuk deserialisasi (JSON → object/database).

Tersedia dua cara: serializers.Serializer (lebih manual) dan ModelSerializer (otomatis berdasarkan model).

👉 Dengan memahami OOP, relasi model, dan serializer di Django REST Framework, kita bisa membangun API yang terstruktur, aman, dan efisien.