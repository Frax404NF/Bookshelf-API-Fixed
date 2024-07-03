# Bookshelf API - Implementasi CRUD di Node.js
- Project Akhir Belajar Membuat Aplikasi Back-End untuk Pemula - Dicoding Indonesia

# Dokumentasi Project Criteria Bookshelf API 

## Kriteria 1: API dapat menyimpan buku
API ini dapat menyimpan buku dengan:

- **Method:** POST  
- **URL:** `/books`

**Body Request:**
```json
{
    "name": "string",
    "year": "number",
    "author": "string",
    "summary": "string",
    "publisher": "string",
    "pageCount": "number",
    "readPage": "number",
    "reading": "boolean"
}
```

Objek buku yang disimpan pada server memiliki struktur sebagai berikut:
```json
{
    "id": "Qbax5Oy7L8WKf74l",
    "name": "Buku A",
    "year": 2010,
    "author": "John Doe",
    "summary": "Lorem ipsum dolor sit amet",
    "publisher": "Dicoding Indonesia",
    "pageCount": 100,
    "readPage": 25,
    "finished": false,
    "reading": false,
    "insertedAt": "2021-03-04T09:11:44.598Z",
    "updatedAt": "2021-03-04T09:11:44.598Z"
}
```

Properti yang **ditebalkan** diolah dan didapatkan di sisi server. Berikut penjelasannya:

- **id**: Nilai id haruslah unik. Untuk membuat nilai unik, saya memanfaatkan nanoid.
- **finished**: Merupakan properti boolean yang menjelaskan apakah buku telah selesai dibaca atau belum. Nilai finished didapatkan dari observasi `pageCount === readPage`.
- **insertedAt**: Merupakan properti yang menampung tanggal dimasukkannya buku. Saya menggunakan `new Date().toISOString()` untuk menghasilkan nilainya.
- **updatedAt**: Merupakan properti yang menampung tanggal diperbarui buku. Ketika buku baru dimasukkan, nilai properti ini sama dengan `insertedAt`.

Server akan merespons gagal bila:

1. Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, server akan merespons dengan:
   - **Status Code:** 400
   - **Response Body:**
     ```json
     {
         "status": "fail",
         "message": "Gagal menambahkan buku. Mohon isi nama buku"
     }
     ```

2. Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, server akan merespons dengan:
   - **Status Code:** 400
   - **Response Body:**
     ```json
     {
         "status": "fail",
         "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
     }
     ```

Bila buku berhasil dimasukkan, server akan mengembalikan respons dengan:

- **Status Code:** 201
- **Response Body:**
  ```json
  {
      "status": "success",
      "message": "Buku berhasil ditambahkan",
      "data": {
          "bookId": "1L7ZtDUFeGs7VlEt"
      }
  }
  ```

## Kriteria 2: API dapat menampilkan seluruh buku
API ini dapat menampilkan seluruh buku yang disimpan dengan:

- **Method:** GET  
- **URL:** `/books`

Server akan mengembalikan respons dengan:

- **Status Code:** 200
- **Response Body:**
  ```json
  {
      "status": "success",
      "data": {
          "books": [
              {
                  "id": "Qbax5Oy7L8WKf74l",
                  "name": "Buku A",
                  "publisher": "Dicoding Indonesia"
              },
              {
                  "id": "1L7ZtDUFeGs7VlEt",
                  "name": "Buku B",
                  "publisher": "Dicoding Indonesia"
              },
              {
                  "id": "K8DZbfI-t3LrY7lD",
                  "name": "Buku C",
                  "publisher": "Dicoding Indonesia"
              }
          ]
      }
  }
  ```

Jika belum terdapat buku yang dimasukkan, server bisa merespons dengan array books kosong:
```json
{
    "status": "success",
    "data": {
        "books": []
    }
}
```

## Kriteria 3: API dapat menampilkan detail buku
API ini dapat menampilkan detail buku berdasarkan id dengan:

- **Method:** GET  
- **URL:** `/books/{bookId}`

Bila buku dengan id yang dilampirkan oleh client tidak ditemukan, server akan mengembalikan respons dengan:

- **Status Code:** 404
- **Response Body:**
  ```json
  {
      "status": "fail",
      "message": "Buku tidak ditemukan"
  }
  ```

Bila buku dengan id yang dilampirkan ditemukan, server akan mengembalikan respons dengan:

- **Status Code:** 200
- **Response Body:**
  ```json
  {
      "status": "success",
      "data": {
          "book": {
              "id": "aWZBUW3JN_VBE-9I",
              "name": "Buku A Revisi",
              "year": 2011,
              "author": "Jane Doe",
              "summary": "Lorem Dolor sit Amet",
              "publisher": "Dicoding",
              "pageCount": 200,
              "readPage": 26,
              "finished": false,
              "reading": false,
              "insertedAt": "2021-03-05T06:14:28.930Z",
              "updatedAt": "2021-03-05T06:14:30.718Z"
          }
      }
  }
  ```

## Kriteria 4: API dapat mengubah data buku
API ini dapat mengubah data buku berdasarkan id dengan:

- **Method:** PUT  
- **URL:** `/books/{bookId}`

**Body Request:**
```json
{
    "name": "string",
    "year": "number",
    "author": "string",
    "summary": "string",
    "publisher": "string",
    "pageCount": "number",
    "readPage": "number",
    "reading": "boolean"
}
```

Server akan merespons gagal bila:

1. Client tidak melampirkan properti name pada request body. Bila hal ini terjadi, server akan merespons dengan:
   - **Status Code:** 400
   - **Response Body:**
     ```json
     {
         "status": "fail",
         "message": "Gagal memperbarui buku. Mohon isi nama buku"
     }
     ```

2. Client melampirkan nilai properti readPage yang lebih besar dari nilai properti pageCount. Bila hal ini terjadi, server akan merespons dengan:
   - **Status Code:** 400
   - **Response Body:**
     ```json
     {
         "status": "fail",
         "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
     }
     ```

3. Id yang dilampirkan oleh client tidak ditemukan oleh server. Bila hal ini terjadi, server akan merespons dengan:
   - **Status Code:** 404
   - **Response Body:**
     ```json
     {
         "status": "fail",
         "message": "Gagal memperbarui buku. Id tidak ditemukan"
     }
     ```

Bila buku berhasil diperbarui, server akan mengembalikan respons dengan:

- **Status Code:** 200
- **Response Body:**
  ```json
  {
      "status": "success",
      "message": "Buku berhasil diperbarui"
  }
  ```

## Kriteria 5: API dapat menghapus buku
API ini dapat menghapus buku berdasarkan id dengan:

- **Method:** DELETE  
- **URL:** `/books/{bookId}`

Bila id yang dilampirkan tidak dimiliki oleh buku manapun, server akan mengembalikan respons berikut:

- **Status Code:** 404
- **Response Body:**
  ```json
  {
      "status": "fail",
      "message": "Buku gagal dihapus. Id tidak ditemukan"
  }
  ```

Bila id dimiliki oleh salah satu buku, maka buku tersebut akan dihapus dan server mengembalikan respons berikut:

- **Status Code:** 200
- **Response Body:**
  ```json
  {
      "status": "success",
      "message": "Buku berhasil dihapus"
  }
  ```
