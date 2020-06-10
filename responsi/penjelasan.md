Penjelasan pada materi Docker Networking


Section #1 Networking Basics
1. The Docker Network Command
	Adalah perintah utama untuk mengonfigurasi dan mengelola jaringan kontainer. Jalankan perintah jaringan buruh pelabuhan dari terminal pertama.

$docker network

Output perintah menunjukkan bagaimana menggunakan perintah dan juga semua sub-perintah docker network. Seperti output pada gambar 1-1, perintah docker network memungkinkan untuk membuat jaringan baru, mendaftar jaringan yang ada, memeriksa jaringan, dan menghapus jaringan. Ini juga memungkinkan Anda untuk menghubungkan dan memutuskan wadah dari jaringan.

2. List Network

$ docker network ls

Dapat dilihat bahwa setiap jaringan mendapatkan ID dan NAME yang unik. Setiap jaringan juga dikaitkan dengan satu driver. 

3. Inspect a Network
	Perintah docker network inspect command digunakan untuk melihat detail konfigurasi jaringan. Rincian ini meliputi; nama, ID, driver, driver IPAM, info subnet, wadah terhubung, dan banyak lagi.

Gunakan docker network inspect <network> untuk melihat detail konfigurasi dari jaringan kontainer pada host Docker.

$docker network inspect bridge

4. List Network Driver Plungins
	Perintah docker info command menunjukkan banyak informasi menarik tentang pemasangan Docker. Jalankan perintah docker info command dan temukan daftar plugin jaringan.

$docker info


Section #2 Bridge Networking
1. The Basic

$docker network ls

Output di atas menunjukkan bahwa jaringan jembatan dikaitkan dengan bridge driver. Penting untuk dicatat bahwa jaringan dan driver terhubung, tetapi mereka tidak sama.

$ip a

2. Connect a container
	The bride network adalah jaringan default untuk wadah baru. Ini berarti menentukan jaringan yang berbeda, semua kontainer baru akan terhubung ke jaringan jembatan.

$docker run -dt ubuntu sleep infinity

$docker ps

$docker network inspect bridge

3. Test Network Cnnectivity

$ping -c5 172.17.0.2

install ping program dengan perintah
$apt-get update && apt-get install -y iputils-ping

ping ke github 
$ping -c5 www.github.com

keluar
$exit

4. Configurate NAT for External Connectivity

$docker run --name web1 -d -p 8080:80 nginx

$docker ps

$curl 127.0.0.1:8080


Section #3 Overlay Networking
1.The Basics

$docker swarm init --advertise-addr $(hostname -i)

$docker node ls

2. Create an overlay network

$docker network create -d overlay overnet

$docker network ls

$docker network inspect overnet

$docker service create --name myservice \
--network overnet \
--replicas 2 \
ubuntu sleep infinity

$docker service ls

$docker service ps myservice

$docker network ls

$docker network inspect overnet


Section #4 Test the Network

$docker network inspect overnet

$docker ps

$apt-get update && apt-get install -y iputils-ping



Section #5 Test service discovery

$cat /etc/resolv.conf

$docker service inspect myservice


cleaning up

$docker service rm myservice

$docker ps

$docker swarm leave --force
