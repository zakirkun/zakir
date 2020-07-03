---
template: post
title: Pengelan Singkat Tentang Api & Rest Api
subtitle: Dengan contoh source code rest api covid19
date: 2020-07-02T17:00:00Z
thumb_img_path: "/images/200416-rest-api-713x285.jpg"
content_img_path: "/images/200416-rest-api-713x285.jpg"
excerpt: Jika kalian belum tau soal apa itu rest api, disini akan di jelaskan secara
  singkat soal rest api.

---
Assalamualaikum Warahmatullahi Wabaraktuh,

Dalam Postingan ini saya akan memberikan pengertian soal Rest Api jika anda belum mengetahui apabila sudah paham boleh di skip, jadi kita harus tau dulu apa itu API ( _Application Programming Interface )_ merupakan sebuah perangkat lunak yang dapat menerima panggilan atau permintaan dari perangkat lunak lainnya seperti aplikasi dan _website_ yang memberikan pelayanan.

Sesuai penjelasan diatas jika **API** adalah sebuah software yang mengintegrasikan antara aplikasi yang kita buat dengan aplikasi yang lain. Tujuan pembuatannya yaitu untuk saling berbagi data antar aplikasi yang sudah diintegrasikan tersebut.

Sedangkan **REST API** merupakan salah satu dari desain arsitektur yang terdapat di dalam API itu sendiri. Dan cara kerja dari **RESTful API** yaitu REST client akan Melakukan akses pada data/resource pada REST server dimana masing-masing resource. Atau data/resource tersebut akan dibedakan oleh sebuah global ID atau URIs (Universal Resource Identifiers).

Jadi, Nantinya data yang diberikan oleh REST server itu bisa berupa format text, JSON atau XML. Dan saat ini format yang paling populer dan paling banyak digunakan adalah format JSON.

Adapun metode HTTP yang secara umum dipakai dalam REST api adalah:

* GET, berfungsi untuk membaca data/resource dari REST server
* POST, berfungsi untuk membuat sebuah data/resource baru di REST server
* PUT, berfungsi untuk memperbaharui data/resource di REST server
* DELETE, berfungsi untuk menghapus data/resource dari REST serve
* OPTIONS, berfungsi untuk mendapatkan operasi yang disupport pada resource dari REST server.

Contoh sourcecode Rest Api Covid19 Menggunakan Programan PHP silahkan [Download](https://github.com/Bekasi-Dev-Community/Rest-Api-Info-Covid19-Indonesia).

Referensi :

* [https://medium.com/jagoanhosting/perbedaan-antara-api-rest-api-dan-restful-api-6a66d655a6c2](https://medium.com/jagoanhosting/perbedaan-antara-api-rest-api-dan-restful-api-6a66d655a6c2 "https://medium.com/jagoanhosting/perbedaan-antara-api-rest-api-dan-restful-api-6a66d655a6c2")
* [https://netmonk.id/apa-itu-api/](https://netmonk.id/apa-itu-api/ "https://netmonk.id/apa-itu-api/")
* [https://www.codepolitan.com/mengenal-apa-itu-web-api-5a0c2855799c8#:\~:text=API%20adalah%20singkatan%20dari%20Application,aplikasi%20yang%20berbeda%20secara%20bersamaan.](https://www.codepolitan.com/mengenal-apa-itu-web-api-5a0c2855799c8#:\~:text=API%20adalah%20singkatan%20dari%20Application,aplikasi%20yang%20berbeda%20secara%20bersamaan. "https://www.codepolitan.com/mengenal-apa-itu-web-api-5a0c2855799c8#:~:text=API%20adalah%20singkatan%20dari%20Application,aplikasi%20yang%20berbeda%20secara%20bersamaan.")