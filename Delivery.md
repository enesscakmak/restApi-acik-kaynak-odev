# OpenAPI Tasarımı Ödevi Teslim Raporu

## 👤 Öğrenci Bilgileri
- **Ad Soyad**: Enes Çakmak
- **Öğrenci Numarası**: 171422012

---

## 📂 OpenAPI YAML Dosyası

```
openapi: 3.0.3
info:
  title: University Online Library API
  description: RESTful API for managing a university's online library system.
  version: 1.0.0
servers:
  - url: https://api.university-library.edu/v1
    description: Production server
  - url: http://localhost:3000
    description: Local development server

tags:
  - name: books
    description: Operations related to books
  - name: students
    description: Operations related to students
  - name: loans
    description: Operations related to book loans

paths:
  /books:
    get:
      tags:
        - books
      summary: Get all books
      description: Retrieves a paginated list of all books in the library.
      parameters:
        - in: query
          name: page
          schema:
            type: integer
            minimum: 1
          description: Page number
        - in: query
          name: size
          schema:
            type: integer
            minimum: 1
            maximum: 100
          description: Page size
      responses:
        '200':
          description: A list of books
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Book'
    post:
      tags:
        - books
      summary: Add a new book
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Book'
      responses:
        '201':
          description: Book created
        '400':
          description: Invalid input
  /books/{id}:
    get:
      tags:
        - books
      summary: Get a book by ID
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Book found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Book'
        '404':
          description: Book not found
    put:
      tags:
        - books
      summary: Update a book
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Book'
      responses:
        '200':
          description: Book updated
        '400':
          description: Invalid input
        '404':
          description: Book not found
    delete:
      tags:
        - books
      summary: Delete a book
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '204':
          description: Book deleted
        '404':
          description: Book not found

  /students:
    get:
      tags:
        - students
      summary: Get all students
      responses:
        '200':
          description: List of students
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Student'
    post:
      tags:
        - students
      summary: Add a new student
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Student'
      responses:
        '201':
          description: Student created
        '400':
          description: Invalid input
  /students/{id}:
    get:
      tags:
        - students
      summary: Get student by ID
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Student found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Student'
        '404':
          description: Student not found
    put:
      tags:
        - students
      summary: Update student
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Student'
      responses:
        '200':
          description: Student updated
        '400':
          description: Invalid input
        '404':
          description: Student not found
    delete:
      tags:
        - students
      summary: Delete student
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '204':
          description: Student deleted
        '404':
          description: Student not found

  /loans:
    get:
      tags:
        - loans
      summary: Get all loans
      responses:
        '200':
          description: List of loans
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Loan'
    post:
      tags:
        - loans
      summary: Create a new loan
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Loan'
      responses:
        '201':
          description: Loan created
        '400':
          description: Invalid input

  /loans/{id}:
    get:
      tags:
        - loans
      summary: Get loan by ID
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Loan found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Loan'
        '404':
          description: Loan not found

  /loans/{id}/return:
    patch:
      tags:
        - loans
      summary: Return a book
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Book returned
        '404':
          description: Loan not found

components:
  schemas:
    Book:
      type: object
      required:
        - id
        - title
        - author
        - isbn
        - publisher
        - pageCount
        - stock
      properties:
        id:
          type: string
          format: uuid
        title:
          type: string
        author:
          type: string
        isbn:
          type: string
          format: isbn-13
        publisher:
          type: string
        pageCount:
          type: integer
        stock:
          type: integer
    Student:
      type: object
      required:
        - id
        - name
        - studentNumber
        - email
        - isActive
      properties:
        id:
          type: string
          format: uuid
        name:
          type: string
        studentNumber:
          type: string
        email:
          type: string
          format: email
        isActive:
          type: boolean
    Loan:
      type: object
      required:
        - id
        - studentId
        - bookId
        - loanDate
        - status
      properties:
        id:
          type: string
          format: uuid
        studentId:
          type: string
        bookId:
          type: string
        loanDate:
          type: string
          format: date
        returnDate:
          type: string
          format: date
          nullable: true
        status:
          type: string
          enum: ["ongoing", "returned", "late"]
  parameters:
    bookId:
      in: path
      name: id
      required: true
      schema:
        type: string
        format: uuid
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT


```

### 🔗 GitHub Repo Linki
https://github.com/enesscakmak/restApi-acik-kaynak-odev

---

## 📝 API Açıklaması

- books, students ve loans entity bulunur. Books kitap bilgilerini, students öğrenci bilgilerini ve loans ise kitapların ödünç alınma işlemlerini içerir.
- Tüm entityler için tümünü getirmeye yarayan GET ile spesifik bir elementi getirmeye yarayan GET/{id}, yeni element eklemeye yarayan POST bulunur. books ve students için element güncellemeye yarayan PUT ile element silmeye yarayan DELETE bulunurken loans entitysinde iade etme için kullanılan PATCH bulunmaktadır.
- Ortak parametlere components/parameters altında bulunurken sık kullanılan yanıtlar components/responses altında bulunur. Her endpoint bunlara bağlıdır.
- GET/books endpointinde page ve size parametreleri kullanıldı, bunlar components/parameters altında tanımlanarak $ref ile endpointe eklendi. Her endpoint için 400, 404 gibi hata yanıtları örneklendi ve açıklama eklendi.

