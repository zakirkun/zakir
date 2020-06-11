---
template: post
title: Cara Install Docker CE (Community Edition) pada Ubuntu LTS 16.04
subtitle: Docker merupakan salah satu teknologi container yang paling populer dan
  banyak digunakan oleh perusahaan di seluruh dunia.
date: 2020-06-10T17:00:00Z
thumb_img_path: "/images/1_4jrpzdbzle_ga-mgwecnuq.png"
content_img_path: "/images/1_4jrpzdbzle_ga-mgwecnuq.png"
excerpt: 'Container merupakan unit standar perangkat lunak yang mengemas kode dan
  semua dependensinya sehingga aplikasi berjalan dengan cepat dan andal dari satu
  lingkungan komputasi ke yang lain. Docker container merupakan container yang ringan,
  mandiri, dapat dieksekusi yang dapat mencakup semua yang diperlukan untuk menjalankan
  aplikasi: kode, runtime, alat sistem, pustaka dan pengaturan sistem. '

---
Container merupakan unit standar perangkat lunak yang mengemas kode dan semua dependensinya sehingga aplikasi berjalan dengan cepat dan andal dari satu lingkungan komputasi ke yang lain. Docker container merupakan container yang ringan, mandiri, dapat dieksekusi yang dapat mencakup semua yang diperlukan untuk menjalankan aplikasi: kode, runtime, alat sistem, pustaka dan pengaturan sistem. Docker menggunakan teknologi container untuk dapat mempermudah para developer untuk mengembangkan aplikasi.

![](https://miro.medium.com/max/1400/0*guurlUOHq5C73V-F.png)

Pada tutorial kali ini, admin ingin berbagi dengan kalian mengenai cara instalasi docker CE pada ubuntu LTS 16.04. lalu apa itu docker ce? Docker ce merupakan versi docker gratis yang bersifat open source sehingga bisa digunakan oleh siapa saja. Docker terdiri dari dua versi yaitu versi gratis yang disebut CE (Community Edition) dan versi yang berbayar disebut EE (Enterprise Edition). Masing-masing versi memiliki kelebihan dan kelemahan. Docker CE ideal untuk developer individu dan tim yang ingin memulai belajar Docker dan bereksperimen dengan aplikasi berbasis container. Docker EE dirancang untuk developer perusahaan dan tim TI yang membangun, mengirim, dan menjalankan aplikasi bisnis yang berskala produksi.

Berikut merupakan tahapan-tahapan instalasi docker CE pada ubuntu LTS 16.04:

pertama, lakukan update repository ubuntu dan lakukan upgrade pada ubuntu

    $ sudo apt-get update$ sudo apt-get upgrade

Tunggu beberapa saat sampai proses upgrade selesai. lakukan instalasi package tambahan agar bisa melakukan proses download file repository yang menggunakan protocol https

    $ sudo apt-get install \      apt-transport-https \      ca-certificates \      curl \      gnupg-agent \      software-properties-common

Tambahkan docker official GPG key

    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add –

lakukan veritifikasi apakah kode yang muncul dari eksekusi perintah diatas sesuai dengan 8 karakter terakhir kode dibawah ini:

9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88

kemudian gunakan perintah dibawah ini untuk mempersiapkan repositori yang stabil

    $ sudo add-apt-repository \"deb [arch=amd64] https://download.docker.com/linux/ubuntu \$(lsb_release -cs) \stable"

setelah melakukan set up repositori docker kemudian lakukan lagi update repositori ubuntu dengan perintah sebagai berikut

    $ sudo apt-get update

akhirnya lakukan instalasi docker dengan perintah sebagai berikut

    $ sudo apt-get install docker-ce

Secara otomatis perintah diatas akan melakukan instalasi docker ce versi terbaru. apabila ingin melakukan instalasi docker dengan versi tertentu dapat menggunakan perintah sebagai berikut

    $ sudo apt-get install docker-ce=<VERSION_STRING>

Tunggu beberapa saat sampai proses instalasi selesai. Kemudian cek status dari docker

    $ sudo service docker status

Kemudian akan muncul keterangan seperti dibawah ini yang menandakan docker telah berjalan dengan normal

![](/images/1_cuuamafxrnuh2mkz_ps8xa.png)

Secara default, apabila kita ingin menjalankan docker command memerlukan sudo privilege sehingga kita perlu menambahkan kata sudo setiap kali ingin melakukan eksekusi docker command. Hal tersebut mengakibatkan penggunaan docker menjadi tidak praktis. Maka admin akan berbagi cara untuk menghindari pengetikkan sudo setiap kali akan menjalankan docker command, lankah pertama yaitu tambahkan nama pengguna Anda ke grup docker:

    $ sudo usermod -aG docker ${USER}$ su - ${USER}

Setelah meneksekusi perintah diatas anda akan diperintahkan untuh memasukkan password user anda. Setelah itu maka anda bisa melakukan eksekusi perintah docker tanpa mengetikkan sudo. Anda bisa mengeceknya dengan mengetikkan perintah

    $ docker

Perintah diatas akan menampilkan keterangan seperti dibawah ini:

![](https://miro.medium.com/max/1248/1*Aovt1AOpjZfi_gc4Rsbc3A.png)

![](https://miro.medium.com/max/1248/1*MdSwBePo4IfBXTI5Ljin5Q.png)

![](https://miro.medium.com/max/1248/1*IlzhYj_3m4TemFSomMKgYQ.png)

Cukup sekian dulu ya postingan dari admin kali ini jangan lupa tunggu lanjutan post admin tentang docker yang selanjutnya.

Sumber referensi:

1\. [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/ "https://docs.docker.com/install/linux/docker-ce/ubuntu/")

2\. [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04](https://docs.docker.com/install/linux/docker-ce/ubuntu/ "https://docs.docker.com/install/linux/docker-ce/ubuntu/")