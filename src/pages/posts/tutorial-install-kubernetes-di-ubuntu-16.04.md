---
template: post
title: Tutorial install Kubernetes di Ubuntu 16.04
subtitle: Pengenalan dasar tentang Kubernetes dan deploy ke Ubuntu 16.04
date: 2020-06-10T17:00:00Z
thumb_img_path: "/images/kubernetes.png"
content_img_path: "/images/kubernetes.png"
excerpt: Kubernetes adalah sistem manajemen wadah open-source yang tersedia secara
  gratis. Ini menyediakan platform untuk mengotomatiskan penyebaran, penskalaan, dan
  pengoperasian wadah aplikasi di seluruh kelompok host. Kubernetes memberi Anda kebebasan
  untuk mengambil keuntungan dari infrastruktur cloud publik, hibrid, atau publik,
  melepaskan organisasi dari tugas-tugas penyebaran yang membosankan.

---
_By Hitesh Jethva,_ [_Alibaba Cloud Tech Share_](https://www.alibabacloud.com/campaign/techshare) _Author._ [_Tech Share_](https://www.alibabacloud.com/campaign/techshare) _is Alibaba Cloud’s incentive program to encourage the sharing of technical knowledge and best practices within the cloud community._

Kubernetes adalah sistem manajemen wadah open-source yang tersedia secara gratis. Ini menyediakan platform untuk mengotomatiskan penyebaran, penskalaan, dan pengoperasian wadah aplikasi di seluruh kelompok host. Kubernetes memberi Anda kebebasan untuk mengambil keuntungan dari infrastruktur cloud publik, hibrid, atau publik, melepaskan organisasi dari tugas-tugas penyebaran yang membosankan.

Kubernetes awalnya dirancang oleh Google dan dikelola oleh Cloud Native Computing Foundation (CNCF). Dengan cepat menjadi standar baru untuk menyebarkan dan mengelola perangkat lunak di cloud. Kubernetes mengikuti arsitektur master-slave, di mana, ia memiliki master yang menyediakan kontrol terpusat untuk semua agen. Kubernetes memiliki beberapa komponen termasuk, etcd, flanel, kube-apiserver, kube-controller-manager, kube-scheduler, kubelet, kube-proxy, buruh pelabuhan dan banyak lagi.

Dalam tutorial ini, kita akan mengatur multi-node Kubernetes Cluster di server Ubuntu 16.04.

### **Prasyaratan :**

* Minimum 2GB RAM per instance.
* A Root password is setup on each instance.
* Two fresh Alibaba Cloud [Elastic Compute Service (ECS)](https://www.alibabacloud.com/product/ecs) instance with Ubuntu 16.04 server installed.
* A static IP address 192.168.0.103 is configured on the first instance (Master) and 192.168.0.104 is configured on the second instance (Slave).

### Luncurkan Alibaba Cloud ECS Instance

Pertama, Masuk ke [https://ecs.console.aliyun.com/?spm=a3c0i.o25424en.a3.13.388d499ep38szx](https://ecs.console.aliyun.com/?spm=a3c0i.o25424en.a3.13.388d499ep38szx "https://ecs.console.aliyun.com/?spm=a3c0i.o25424en.a3.13.388d499ep38szx") Alibaba Cloud ECS Console. Buat instance ECS baru, pilih Ubuntu 16.04 sebagai sistem operasi dengan minimal 2GB RAM. Sambungkan ke instance ECS Anda dan masuk sebagai pengguna root.

Setelah Anda masuk ke instance Ubuntu 16.04 Anda, jalankan perintah berikut untuk memperbarui sistem basis Anda dengan paket terbaru yang tersedia.

    $ apt-get update -y

### Mengkonfigurasi Server ECS Anda

Sebelum memulai, Anda harus mengkonfigurasi file host dan nama host pada setiap server, sehingga setiap server dapat berkomunikasi satu sama lain menggunakan nama host.

Pertama, buka file / etc / hosts di server pertama:

    $ nano /etc/hosts

Tambahkan baris berikut:

    192.168.0.103 master-node
    192.168.0.104 slave-node

Simpan dan tutup file ketika Anda selesai, lalu atur nama host dengan menjalankan perintah berikut:

    $ hostnamectl set-hostname master-node

Selanjutnya, buka file / etc / hosts di server kedua:

    $ nano /etc/hosts

Tambahkan baris berikut:

    192.168.0.103 master-node
    192.168.0.104 slave-node

Simpan dan tutup file ketika Anda selesai, lalu atur nama host dengan menjalankan perintah berikut:

    $ hostnamectl set-hostname slave-node

Selanjutnya, Anda harus menonaktifkan swap memory pada setiap server. Karena, kubelet tidak mendukung memori swap dan tidak akan berfungsi jika swap aktif atau bahkan ada di file /etc/fstab.

Anda dapat menonaktifkan penggunaan memori swap dengan perintah berikut:

    $ swapoff -a

Anda dapat menonaktifkan ini secara permanen dengan mengomentari file swap di /etc /fstab:

    $ nano /etc/fstab

Komentari baris swap seperti yang ditunjukkan di bawah ini:

    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a
    # device; this may be used with UUID= as a more robust way to name devices
    # that works even if disks are added and removed. See fstab(5).
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    # / was on /dev/sda4 during installation
    UUID=6f612675-026a-4d52-9d02-547030ff8a7e /               ext4    errors=remount-ro 0       1
    # swap was on /dev/sda6 during installation
    #UUID=46ee415b-4afa-4134-9821-c4e4c275e264 none            swap    sw              0       0
    /dev/sda5 /Data               ext4   defaults  0 0

Simpan dan tutup file, ketika Anda selesai.

### Install Docker

Sebelum memulai, Anda harus menginstal Docker di server master dan slave. Secara default, versi terbaru Docker tidak tersedia di repositori Ubuntu 16.04, jadi Anda perlu menambahkan repositori Docker ke sistem Anda.

Pertama, instal paket yang diperlukan untuk menambahkan repositori Docker dengan perintah berikut:

    $ apt-get install apt-transport-https ca-certificates curl software-properties-common -y

Selanjutnya, unduh dan tambahkan kunci GPG Docker dengan perintah berikut:

    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

Selanjutnya, tambahkan repositori Docker dengan perintah berikut:

    $ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

Selanjutnya, perbarui repositori dan instal Docker dengan perintah berikut:

    $ apt-get update -y
    $ apt-get install docker-ce -y

### Install Kubernetes

Selanjutnya, Anda harus menginstal kubeadm, kubectl dan kubelet di kedua server. Pertama, unduh dan kunci GPG dengan perintah berikut:

    $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

Selanjutnya, tambahkan repositori Kubernetes dengan perintah berikut:

    $ echo 'deb http://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee /etc/apt/sources.list.d/kubernetes.list

Akhirnya, perbarui repositori dan instal Kubernetes dengan perintah berikut:

    $ apt-get update -y
    $ apt-get install kubelet kubeadm kubectl -y

### Konfigurasikan Node Master

Semua paket yang diperlukan diinstal pada kedua server. Sekarang, saatnya untuk mengkonfigurasi Kubernetes Master Node.

Pertama, inisialisasi gugus Anda menggunakan alamat IP privatnya dengan perintah berikut:

    $ kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.0.103

Anda akan melihat output berikut:

    Your Kubernetes master has initialized successfully!
    To start using your cluster, you need to run the following as a regular user:
      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
    You should now deploy a pod network to the cluster.
    Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
      https://kubernetes.io/docs/concepts/cluster-administration/addons/
    You can now join any number of machines by running the following on each node
    as root:
    kubeadm join --token 62b281.f819128770e900a3 192.168.0.103:6443 --discovery-token-ca-cert-hash sha256:68ce767b188860676e6952fdeddd4e9fd45ab141a3d6d50c02505fa0d4d44686

Catatan: Catat token dari output di atas. Ini akan digunakan untuk bergabung dengan Slave Node ke Master Node di langkah berikutnya.

Selanjutnya, Anda perlu menjalankan perintah berikut untuk mengkonfigurasi alat kubectl:

    $ mkdir -p $HOME/.kube
    $ cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    $ chown $(id -u):$(id -g) $HOME/.kube/config

Selanjutnya, periksa status Master Node dengan menjalankan perintah berikut:

    $ kubectl get nodes

Anda akan melihat output berikut:

    NAME          STATUS     ROLES     AGE       VERSION
    master-node   NotReady   master    14m       v1.9.4

Dalam output di atas, Anda akan melihat bahwa Master Node terdaftar sebagai tidak siap. Karena cluster tidak memiliki Container Networking Interface (CNI).

Mari kita gunakan Calico CNI untuk Master Node dengan perintah berikut:

    $ kubectl apply -f https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml

Pastikan Calico dikerahkan dengan benar dengan menjalankan perintah berikut:

    $ kubectl get pods --all-namespaces

Anda akan melihat output berikut :

    NAMESPACE     NAME                                      READY     STATUS              RESTARTS   AGE
    kube-system   calico-etcd-p2gbx                         0/1       ContainerCreating   0          35s
    kube-system   calico-kube-controllers-d554689d5-v5lb6   0/1       Pending             0          34s
    kube-system   calico-node-667j2                         0/2       ContainerCreating   0          35s
    kube-system   etcd-master-node                          1/1       Running             0          15m
    kube-system   kube-apiserver-master-node                1/1       Running             0          15m
    kube-system   kube-controller-manager-master-node       1/1       Running             0          15m
    kube-system   kube-dns-6f4fd4bdf-7rl74                  0/3       Pending             0          15m
    kube-system   kube-proxy-hqb98                          1/1       Running             0          15m
    kube-system   kube-scheduler-master-node                1/1       Running    

Sekarang, Jalankan kubectl, dapatkan perintah node lagi, dan Anda akan melihat Master Node sekarang terdaftar sebagai Ready.

    $ kubectl get nodes

Hasilnya:

    NAME          STATUS    ROLES     AGE       VERSION
    master-node   Ready     master    7m        v1.9.4

### Tambahkan Slave Node ke Kubernetes Cluster

Selanjutnya, Anda harus masuk ke Slave Node dan menambahkannya ke Cluster. Ingat perintah join di output dari perintah inisialisasi Master Node dan berikan pada Slave Node seperti yang ditunjukkan di bawah ini:

    $ kubeadm join --token 62b281.f819128770e900a3 192.168.0.103:6443 --discovery-token-ca-cert-hash sha256:68ce767b188860676e6952fdeddd4e9fd45ab141a3d6d50c02505fa0d4d44686

Setelah Node bergabung dengan sukses, Anda akan melihat output berikut:

    [discovery] Trying to connect to API Server "192.168.0.103:6443"
    [discovery] Created cluster-info discovery client, requesting info from "https://192.168.0.104:6443"
    [discovery] Requesting info from "https://192.168.0.104:6443" again to validate TLS against the pinned public key
    [discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "192.168.0.104:6443"
    [discovery] Successfully established connection with API Server "192.168.0.103:6443"
    This node has joined the cluster:
    * Certificate signing request was sent to master and a response
      was received.
    * The Kubelet was informed of the new secure connection details.
    Run 'kubectl get nodes' on the master to see this node join the cluster.

Sekarang, kembali ke Master Node dan keluarkan perintah kubectl get nodes untuk melihat bahwa slave node sekarang siap:

    $ kubectl get nodes

Hasilnya:

    NAME          STATUS    ROLES     AGE       VERSION
    master-node   Ready     master    35m       v1.9.4
    slave-node    Ready     <none>    7m        v1.9.4

### Deploy the Apache Container to the Cluster

Kubernetes Cluster sekarang siap, sekarang saatnya untuk menggunakan kontainer Apache.

Di Master Node, jalankan perintah berikut untuk membuat penyebaran Apache:

    $ kubectl create deployment apache --image=apache

Hasilnya:

    deployment "apache" created

Anda dapat membuat daftar penyebaran dengan perintah berikut:

    $ kubectl get deployments

Hasilnya:

    NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    apache    1         1         1            0           16s

Anda dapat melihat informasi lebih lanjut tentang penyebaran Apache dengan perintah berikut:

    $ kubectl describe deployment apache

Hasilnya:

    Name:                   apache
    Namespace:              default
    CreationTimestamp:      Mon, 19 Mar 2018 19:04:03 +0530
    Labels:                 app=apache
    Annotations:            deployment.kubernetes.io/revision=1
    Selector:               app=apache
    Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  1 max unavailable, 1 max surge
    Pod Template:
      Labels:  app=apache
      Containers:
       apache:
        Image:        apache
        Port:         <none>
        Environment:  <none>
        Mounts:       <none>
      Volumes:        <none>
    Conditions:
      Type           Status  Reason
      ----           ------  ------
      Available      True    MinimumReplicasAvailable
    OldReplicaSets:  <none>
    NewReplicaSet:   apache-5fcc8cd4bf (1/1 replicas created)
    Events:
      Type    Reason             Age   From                   Message
      ----    ------             ----  ----                   -------
      Normal  ScalingReplicaSet  43s   deployment-controller  Scaled up replica set apache-5fcc8cd4bf to 1

Selanjutnya, Anda harus membuat wadah Apache tersedia untuk jaringan dengan perintah:

    $ kubectl create service nodeport apache --tcp=80:80

Sekarang, daftarkan layanan saat ini dengan menjalankan perintah berikut:

    $ kubectl get svc

Anda harus melihat layanan Apache dengan port yang ditugaskan 30267:

    NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
    apache       NodePort    10.107.95.29   <none>        80:30267/TCP   15s
    kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        37m

Sekarang, buka browser web Anda dan ketik URL http://192.168.0.104:30267 (IP Slave Node), Anda akan melihat halaman Selamat Datang Apache default:

![](https://miro.medium.com/max/1400/0*ECogN96XAUvjJY69.png)

Selamat! Wadah Apache Anda telah digunakan di Kubernetes Cluster Anda.

Referensi :

[https://www.alibabacloud.com/blog/how-to-install-and-deploy-kubernetes-on-ubuntu-1604_592719?spm=a2c41.11553776.0.0](https://www.alibabacloud.com/blog/how-to-install-and-deploy-kubernetes-on-ubuntu-1604_592719?spm=a2c41.11553776.0.0 "https://www.alibabacloud.com/blog/how-to-install-and-deploy-kubernetes-on-ubuntu-1604_592719?spm=a2c41.11553776.0.0")