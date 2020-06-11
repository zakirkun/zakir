---
template: post
title: Pengenalan Strapi
subtitle: Pengenalan dasar tentang strapi dan Headless-cms
date: 2020-06-10T17:00:00Z
thumb_img_path: "/images/strapi.png"
content_img_path: "/images/strapi-1.png"
excerpt: Strapi ini headless-cms. Headless-cms sederhananya itu seperti traditional
  CMS, tapi nggak punya "kepala" alias front-page.  Kalo di traditional CMS kayak
  WordPress itu kan ada halaman depannya atau udah ada tampilannya. Nah, di headless-cms
  ini nggak ada, jadi kita cuma bisa manage content aja di dalamnya.  Headless-cms
  juga di dalamnya –biasanya– terdapat fitur untuk membuat collections, collection
  ini berfungsi untuk kita menyimpan data, seperti artikel, kategori, order, user,
  dan lain sebagainya – kalo di RDBMS itu namanya table.

---
Strapi ini headless-cms. Headless-cms sederhananya itu seperti traditional CMS, tapi nggak punya "kepala" alias front-page.

Kalo di traditional CMS kayak WordPress itu kan ada halaman depannya atau udah ada tampilannya. Nah, di headless-cms ini nggak ada, jadi kita cuma bisa manage content aja di dalamnya.

Headless-cms juga di dalamnya –biasanya– terdapat fitur untuk membuat collections, collection ini berfungsi untuk kita menyimpan data, seperti artikel, kategori, order, user, dan lain sebagainya – kalo di RDBMS itu namanya table.

Katakanlah kamu mau bikin blog dengan headless-cms, berarti kamu setidaknya perlu membuat collections: articles, comments, dan categories.

Kalo di traditional CMS, ketika kita input data artikel di halaman admin, data artikel tadi akan langsung muncul di halaman depan CMS. Nah, berhubung headless-cms itu nggak punya "kepala" atau halaman depan, maka ada cara lain untuk mengkonsumsi data dari artikel tersebut atau dari semua collection yang dibuat.

Untuk mengkonsumsi data dari collections, biasanya headless-cms menyediakan API, bisa REST atau GraphQL. Jadi, masing-masing collections tadi punya endpoint-nya sendiri-sendiri, misal untuk collections articles, maka akan memiliki beberapa endpoint seperti ini:

\- GET /articles – untuk ngambil semua data artikel  
\- GET /articles/{id} – untuk ngambil satu data artikel berdasarkan id  
\- POST /articles – untuk membuat artikel  
\- DELETE /articles/{id} – untuk menghapus data artikel berdasarkan id  
\- PUT /articles/{id} – untuk menyunting data artikel berdasarkan id

Sekarang balik lagi ke Strapi. Strapi ini headless-cms Node. Untuk pakai Strapi sudah tentu harus pakai Node JS. Strapi juga fiturnya lengkap, mulai dari collections, authentication, authorization (roles & permissions), email, social media login, support REST (by default) dan GraphQL, plugin ecosystem, dan yang paling penting powerful documentation.

Yap, documentation-nya bagus banget. Mulai dari quickstart, sampe per fitur dijelasin, bahkan ada step-by-step deployment ke VPS juga.

Strapi ini self-hosted, jadi, kamu butuh server yang mendukung Node untuk dapat meng-install Strapi. Selain itu, Strpai juga database-agnostic. Kamu bisa milih database favorit kamu, seperti Postgre, MySQL, Mongo, SQLite, dan Maria.

Hanya saja masalah saya dengan Strapi soal data migration\[1\]. Saya dan beberapa orang yang memiliki masalah yang sama kesulitan migrasi data yang sudah dibuat di satu environment ke environment lain. Misal, saya install Strapi di local, dan saya sudah masukkan banyak data di di sana, dan saya ingin memindahkan data tadi ke production. Nah, di Strapi cara ini tidak bisa dilakukan (by default). Sampai saat ini juga Strapi belum mengimplementasikan fitur ini. Jadi, saya biasanya langsung install di server production atau stagging dengan setting environment "developemnt" di Strapi-nya.

Tapi, di terlepas masalah barusan, Strapi punya banyak fitur-fitur powerful lainnya yang dapat menjadi pertimbangan kamu.

Soal collections, Strapi mantep banget. Kita bisa bikin collection yang basic sampe collection yang punya banyak relasi. Strapi mendukung beberapa jenis relasi, diantaranya:

\- has one  
\- has and belongs to one  
\- belongs to many  
\- has many  
\- has and belongs to many

Soal API, Strapi punya plugin yang dapat men-generate Swagger atau API documentation secara otomatis untuk setiap collection yang dibuat.

Soal authentication, Strapi pakai JWT dan juga mendukung social media login. Jadi, kita bisa bikin login with social media dengan mudah, hanya perlu provide API key dan secret key dari masing-masing social media provider.

Selain yang saya sebutin di atas, Strapi masih punya banyak fitur lainnya yang powerful. Juga, selain Strapi sebenernya banyak headless-cms lainnya, kalo di Laravel ada Voyager, atau kalo masih mau pake yang versi Node, ada Ghost; atau kalau pengen yang berbayar dan cloud-based, ada Contentful.

Headless-cms ini cocok banget untuk kamu yang pengen bikin app, tapi males bikin back-end dan halaman adminnya. Saya pakai Strapi biasanya dengan React atau NextJS. Jadi, untuk manage datanya itu di Strapi, sedangakan NextJS untuk konsumsi data yang dari Strapi sekaligus ditampilin di UI.

FYI, kalo kamu develop app pake NextJS dan Strapi itu bener-bener bisa cepet banget, mungkin bisa 2x lebih cepet. Apalagi NextJS support export to static app, jadi, tinggal deploy ke Netlify atau Github Pages. Atau kalo kamu mau pake fitur SSR dari NextJS juga bisa, tinggal deploy ke Vercel Now, free pula.

FYI lagi, kalo kamu mau gampang install Strapi, bisa pakai Digital Ocean, mereka punya droplet yang udah ada Strapi di dalamnya\[2\]. Jadi, beberapa software akan otomatis di-install, seperti:

\- NodeJS  
\- Yarn (package manager juga, alternatif NPM)  
\- PM2 (untuk daemonize Node App dan monitoring)  
\- Nginx (ini dibutuhin untuk reverse proxy)  
\- PostgreSQL (ini default database yang dipilih Digital Ocean)

Tulisan ini merupakan jawaban atas permintaan dari [Iqbal Aqaba](https://web.facebook.com/iqbalaqaba?__tn__=%2CdK-R-R&eid=ARDNGf9kA3mzaWbd7ArdCY2pfXkS85hjBxz_JLy8VcQtFAmddDZbDWjlZdSXzCESzhCHnuAVB3Og3it5&fref=mentions "Iqbal Aqaba") soal "Request penjelasan strapi dong".

Semoga bermanfaat.

Referensi : [Muhammad Nauval Azhar](https://web.facebook.com/mhdnauvalazhar/posts/1308561539337080?comment_id=1308574236002477&reply_comment_id=1308577569335477&notif_id=1591842542254501&notif_t=feed_comment_reply)