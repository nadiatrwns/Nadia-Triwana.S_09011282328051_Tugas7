### How to Install and Configure SSH on Ubuntu 22.04
## 1.	Prepare Ubuntu: The first thing you need to do before you start installing SSH on Ubuntu is to update all apt packages to the latest versions. To do this, use the following command:
![1](https://github.com/user-attachments/assets/705d0276-3732-490e-93b0-2d3f1e577a6d)
# •	sudo: Singkatan dari "superuser do", memberikan hak akses administrator (root) untuk menjalankan perintah yang memerlukan izin khusus.
# •	apt: Advanced Package Tool, manajer paket yang digunakan pada distribusi berbasis Debian seperti Ubuntu.
# •	update: Memperbarui daftar paket dari repositori yang terkonfigurasi, memastikan sistem mengetahui versi terbaru dari setiap paket.
# •	&&: Operator logika yang menggabungkan dua perintah. Perintah kedua (apt upgrade) hanya akan dijalankan jika perintah pertama (apt update) berhasil.
# •	upgrade: Menginstal versi terbaru dari semua paket yang sudah terpasang di sistem. Ini memastikan bahwa semua perangkat lunak berada pada versi terbaru dan aman.

## 2.	Install SSH on Ubuntu: OpenSSH is not pre-installed on the system, so let's install it manually. To do this, type in the terminal:
![2](https://github.com/user-attachments/assets/0fa13b47-d1d8-424e-b5c2-6f33bfc67138)
# •	sudo: Memberikan hak akses administrator untuk menjalankan perintah instalasi.
# •	apt install: Perintah untuk menginstal paket perangkat lunak dari repositori.
# •	openssh-server: Nama paket yang akan diinstal. OpenSSH Server adalah implementasi server dari protokol SSH (Secure Shell) yang memungkinkan koneksi SSH ke komputer.

## 3.	Start SSH: Now you need to enable the service you just installed using the command below:
![3](https://github.com/user-attachments/assets/82e8eece-1248-487b-b6d3-6a3b39e5213b)
# •	sudo: Hak akses administrator.
# •	systemctl: Alat baris perintah untuk mengontrol dan mengelola layanan (services) di sistem yang menggunakan systemd.
# •	enable: Mengonfigurasi layanan agar dimulai secara otomatis setiap kali sistem booting.
# •	--now: Memulai layanan segera setelah mengaktifkannya, tanpa perlu reboot.
# •	ssh: Nama layanan yang akan diaktifkan dan dijalankan, yaitu OpenSSH Server.
## On successful startup, you will see the following system message. The key helps you launch the service and simultaneously set it to start when the system boots. To verify that the service is enabled and running successfully, type:
![3 (2)](https://github.com/user-attachments/assets/d2560acf-3e98-49ce-8eda-55c8b7572930)
# •	sudo: Hak akses administrator.
# •	systemctl status: Memeriksa status dari layanan yang disebutkan.
# •	ssh: Nama layanan yang statusnya ingin diperiksa.

## 4.	Configure the firewall: Before connecting to the server via SSH, check the firewall to ensure it is configured correctly. In our case, we have the UFW installed, so we will use the following command:
![4](https://github.com/user-attachments/assets/f7591aac-19d4-4460-a4bb-5c8b878d1a22)
# •	sudo: Hak akses administrator.
# •	ufw: Uncomplicated Firewall, alat manajemen firewall sederhana pada Ubuntu.
# •	status: Memeriksa status saat ini dari firewall, termasuk aturan yang diterapkan.
## In the output, you should see that SSH traffic is allowed. If you don't have it listed, you need to allow incoming SSH connections. This command will help with this:
![4 (2)](https://github.com/user-attachments/assets/1a9b2c00-8db6-44d5-81c6-9d270c8866c1)
# •	sudo: Hak akses administrator.
# •	ufw allow: Menambahkan aturan pada firewall untuk mengizinkan lalu lintas masuk pada port tertentu.
# •	ssh: Menunjukkan bahwa aturan ini berlaku untuk layanan SSH, biasanya melalui port 22.

## 5.	Connect to the server: Once you complete all the previous steps, you can log into the server using the SSH protocol. To do this, you will need the server's IP address or domain name and the name of a user created on the server. In the terminal line, enter the command:
![5](https://github.com/user-attachments/assets/fbd446d8-3d8c-4c58-a43a-7cdd9da9ddd4)
![ga ad](https://github.com/user-attachments/assets/70530f3e-7320-4c5b-8f18-99089912f78f)
# •	ssh: Perintah untuk memulai sesi SSH, memungkinkan koneksi aman ke server.
# •	username: Nama pengguna yang telah dibuat di server. Pengguna ini akan digunakan untuk login ke server.
# •	@: Simbol yang memisahkan nama pengguna dan alamat server.
# •	IP_address: Alamat IP dari server yang ingin dihubungkan, misalnya 192.168.1.100.

## 6.	Configure SSH: Having completed the previous five steps, you can already connect to the server remotely. However, you can further increase the connection's security by changing the default connection port to another or changing the password authentication to key authentication. These and other changes require editing the SSH configuration file. The main OpenSSH server settings are stored in the main configuration file sshd_config (location: /etc/ssh). Before you start editing, you should create a backup of this file:
![6](https://github.com/user-attachments/assets/5c4107a4-e2fd-4927-bcd7-9d41f4ea37ce)
# •	sudo: Memberikan hak akses administrator yang diperlukan untuk menyalin file di direktori sistem.
# •	cp: Perintah untuk menyalin file atau direktori.
# •	/etc/ssh/sshd_config: Lokasi file konfigurasi utama OpenSSH server. File ini berisi pengaturan-pengaturan yang mengontrol perilaku server SSH.
# •	/etc/ssh/sshd_config.initial: Nama file cadangan yang akan dibuat. Dengan menambahkan .initial, Anda menandakan bahwa ini adalah salinan awal dari konfigurasi asli.
## If you get any errors after editing the configuration file, you can restore the original file without problems. After creating the backup, you can proceed to edit the configuration file. To do this, open it using the editor:
![6 (2)](https://github.com/user-attachments/assets/205b8567-f4a8-40d5-bfe4-814942a73230)
# •	sudo: Memberikan hak akses administrator untuk mengedit file sistem.
# •	nano: Perintah ini membuka editor teks Nano di terminal. Nano adalah editor teks berbasis terminal yang sederhana dan mudah digunakan.
# •	/etc/ssh/sshd_config: Lokasi file konfigurasi SSH yang akan diedit.
## In the file, change the port to a more secure one. It is best to set values from the dynamic range of ports (49152 - 65535) and use different numbers for additional security. For example, let's change the port value to 49532. To do this, we uncomment the corresponding line in the file and change the port as shown in the screenshot below.
![6 (3)](https://github.com/user-attachments/assets/7241ddbf-5acd-48c7-ab34-a45ff7d0a4d6)
# •	Menentukan nomor port baru yang akan digunakan oleh layanan SSH. Port ini berada dalam rentang dinamis (49152 - 65535), yang lebih aman dibandingkan port standar (22).
## In addition to this setting, we recommend changing the password authentication mode to a more secure key authentication mode. To do this, uncomment the corresponding line and make sure the value is "Yes", as shown in the screenshot.
![6 (4)](https://github.com/user-attachments/assets/2f337196-787f-40ac-855a-ca735ff581ed)
# •	Mengonfigurasi server SSH untuk hanya menerima otentikasi kunci publik, meningkatkan keamanan dengan mencegah penggunaan kata sandi yang lebih rentan terhadap serangan.
## Now, let's prohibit logging on to the server as a superuser by changing the corresponding line as shown in the picture below.
![6 (5)](https://github.com/user-attachments/assets/97d7a82c-1e70-4a4e-8dd2-1eae86e581f6)
# •	Menonaktifkan login langsung sebagai superuser (root) melalui SSH. Ini memastikan bahwa pengguna harus login menggunakan akun pengguna biasa, kemudian menggunakan perintah sudo untuk menjalankan perintah sebagai superuser.
## After making all the changes in the main configuration file, save them and close the editor. Restart the service to make the changes take effect:
## sudo systemctl restart ssh
# •	sudo: Menjalankan perintah dengan hak akses administratif.
# •	systemctl: Alat manajemen layanan yang digunakan untuk mengontrol layanan pada sistem yang menggunakan systemd.
# •	restart: Tindakan untuk menghentikan dan memulai kembali layanan.
# •	ssh: Nama layanan SSH yang akan direstart.
## If you have changed the port in the configuration file, you should connect using the new port: 
![6 (6)](https://github.com/user-attachments/assets/6bd22eca-af96-4458-879a-9317898dfb9e)
# •	ssh: Perintah untuk memulai sesi SSH (Secure Shell), yang memungkinkan Anda untuk terhubung secara aman ke server jarak jauh.
# •	-p port_number: Opsi -p digunakan untuk menentukan nomor port khusus yang akan digunakan untuk koneksi SSH. port_number harus diganti dengan nomor port baru yang telah dikonfigurasi dalam file konfigurasi SSH server (sshd_config).
# •	Secara default, SSH menggunakan port 22. Jika port ini diubah ke port lain untuk keamanan, perintah ini menginstruksikan SSH untuk menggunakan port yang baru.
# •	username: Nama pengguna yang digunakan untuk masuk ke server jarak jauh. Harus disesuaikan dengan akun pengguna yang ada di server tujuan.
# •	IP_address: Alamat IP dari server yang ingin diakses. Alternatifnya, Anda juga bisa menggunakan nama domain server.

### Download PUTTY
