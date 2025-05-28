# Online Kütüphane Sistemi - OpenAPI 3.0 Tanımı

Bu repo bir çevrim içi üniversite kütüphane sistemine ait RESTful API tanımını gösteren ödev için gerekli dosyaları içerir. API, kitap, öğrenci ve ödünç alma işlemlerini kapsar.

## Kullanım

Test etmek için:

1. https://editor.swagger.io adresine gidin.
2. openapi.yaml dosyasını aktarın.
3. Endpoint'leri görüntüleyebilir ve test edebilirsiniz.


## Entities

- Book: Kitap bilgilerini tutar.
- Student: Öğrenci bilgilerini tutar.
- Loan: Ödünç alınan kitap kayıtlarını tutar.

Bu entityler, `components/schemas` altında taslak olarak tanımlanmıştır.

## CRUD İşlemleri

### Book

- `GET /books`: Tüm kitapları listeler
- `GET /books/{id}`: Belirli kitabı getirir
- `POST /books`: Yeni kitap ekler
- `PUT /books/{id}`: Kitabı günceller
- `DELETE /books/{id}`: Kitabı siler

### Student

- `GET /students`: Tüm öğrencileri listeler
- `GET /students/{id}`: Belirli öğrenciyi getirir
- `POST /students`: Yeni öğrenci ekler
- `PUT /students/{id}`: Öğrenciyi günceller
- `DELETE /students/{id}`: Öğrenciyi siler

### Loan

- `GET /loans`: Tüm ödünç kayıtlarını listeler
- `GET /loans/{id}`: Belirli ödünç kaydını getirir
- `POST /loans`: Yeni ödünç kaydı oluşturur
- `PATCH /loans/{id}/return`: Kitabı iade eder

## components Kullanımı

### schemas

Her varlık için veri yapıları burada tanımlanmıştır:

- Book: id, title, author, isbn, publisher, pageCount, stock
- Student: id, name, email, department, studentNumber
- Loan: id, studentId, bookId, loanDate, returnDate, returned

### parameters

Tekrarlayan parametreler burada tanımlanmıştır:

- page, size (sayfalama)
- id (path parametresi)

## Sayfalama

- `GET /books`, `GET /students`, `GET /loans` endpoint’lerinde `page` ve `size` query parametreleriyle sayfalama uygulanır.
- Varsayılan değerler tanımlıdır.

## Hata Yönetimi

Her endpoint, `components/responses` altında tanımlı standart yanıtları kullanır:

- 400: BadRequest
- 404: NotFound
- 500: InternalError

Yanıt örnekleri de tanım dosyasında verilmiştir.

## Dosya Yapısı

- `openapi.yaml`: Ana API tanım dosyası
- `README.md`: Bu açıklama dosyası
- `Delivery.md`: Ödev için gerekli detayları içeren dosya

