# Monitoring dan Automatisasi Infrastruktur CI/CD dengan Prometheus, Grafana, dan Ansible di AWS 

![diagram-export-5-26-2025-2_18_43-PM](https://github.com/user-attachments/assets/bb6f9459-fcc5-4f3f-aa28-69d410576322)

## Agenda 
## Prerequisites

### Step 1 : Install dan Konfigurasi Node-Exporter
1. Install node-exporter pada komputer lokal, server jenkins, sonarqube, dan docker
2. Konfigurasi node-exporter membuat unit serviced agar dapat berjalan sebagai service

Kunjungi website berikut untuk mendownload Node-Exporter 
https://prometheus.io/download/

Salin link download node-exporter sesuai dengan sesuai dengan OS yang digunakan disini saya menggunakan Linux

![Screenshot from 2025-05-26 14-40-33](https://github.com/user-attachments/assets/864ee78a-599b-4498-806b-b2304d6290e7)

Setelah salin link download, selanjutnya download Node-Exporter di terminal dan simpan di folder yang diinginkan lalu extract file node exporter dan jalankan dengan perintah berikut

![Screenshot from 2025-05-26 14-49-22](https://github.com/user-attachments/assets/5bf94826-4c74-4aec-bf68-1b160622bf45)

Selanjutnya adalah buat unit service systemd untuk node-exporter agar dapat berjalan sebagai service. langkah yang pertama adalah buat user untuk node exporter dengan perintah berikut 
```
sudo useradd --no-create-home --shell /bin/false node_exporter
```
![Screenshot from 2025-05-26 14-54-14](https://github.com/user-attachments/assets/15808031-8f6a-47f7-8b86-756112f86adb)

Selanjutnya buat file service systemd
```
sudo nano /etc/systemd/system/node_exporter.service
```
lalu masukkan isi file berikut ini dan control x + s untuk save file
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```
![Screenshot from 2025-05-26 14-54-35](https://github.com/user-attachments/assets/d3d099b9-247f-4c75-880f-85902d35a179)

Set izin dan reload system untuk node-exporter
```
# Set ownership kalau pakai user khusus
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
# Reload systemd
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```
![Screenshot from 2025-05-26 14-57-08](https://github.com/user-attachments/assets/3ab75084-356c-4d07-a7c1-2fe1ee24ec92)

Selanjutnya aktifkan node-exporter dengan perintah berikut ini
```
# Menjalankan node-exporter
sudo systemctl start node-exporter
# Mengecek status node-exporter
sudo systemctl status node-exporter
```
![Screenshot from 2025-05-26 14-58-58](https://github.com/user-attachments/assets/b064ac44-b8dd-440b-b0d6-7ec388033a60)

Selanjutnya adalah menginstall node-exporter pada server jenkins, sonarqube, dan docker

Remote server jenkins, sonarqube, dan docker menggunakan SSH key dan download node exporter di website berikut ini: https://prometheus.io/download/

![Screenshot from 2025-05-26 17-09-25](https://github.com/user-attachments/assets/41b5feae-8849-4ec9-aedd-f3a94e68719a)

Setelah berhasil di download, extract file node-exporter dengan menggunakan perintah 
```
tar -xf <file node-exporter.tar.gz>
```
Setelah itu masuk ke dalam folder node-exporter yang sudah di extract dan jalankan node-exporter

![Screenshot from 2025-05-26 17-10-19](https://github.com/user-attachments/assets/9b132db8-02a1-4352-9b2f-c0eacb4e7c1d)

Selanjutnya adalah buat unit service systemd untuk node-exporter agar dapat berjalan sebagai service. 

Buat user untuk node-exporter dengan menggunakan perintah berikut ini: 
```
sudo useradd --no-create-home --shell /bin/false node_exporter

```
![Screenshot from 2025-05-26 17-10-51](https://github.com/user-attachments/assets/cfb31c11-9275-47a9-b3bf-80f87ef6de44)

Lalu buat file systemd dan isi file berikut ini:
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

```
![Screenshot from 2025-05-26 17-11-08](https://github.com/user-attachments/assets/550d7be0-1b37-48db-9b82-3d728b55c880)

Setelah diisii tekan control x + s untuk save file

Selanjutnya pindahkan bin node-exporter pada file node-exporter yang sudah di extract ke folder /usr/local/bin dengan menggunakan perintah
```
sudo mv node_exporter /usr/local/bin
```
![Screenshot from 2025-05-26 17-11-54](https://github.com/user-attachments/assets/22fa9e00-e3cc-4b2d-b402-6591819f8d7c)

Selanjutnya set izin ownership menggunakan perintah berikut ini
```
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```
![Screenshot from 2025-05-26 17-12-17](https://github.com/user-attachments/assets/9d8ba64c-f1d1-4fde-b79f-0296af0b2871)

Setelah itu reload systemd menggunakan perintah berikut ini
```
# Reload systemd
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```
![Screenshot from 2025-05-26 17-12-40](https://github.com/user-attachments/assets/5fdbe823-f5c1-4662-b741-1449a6916f78)

Selanjutnya, aktifkan node exporter dan enable service menggunakan perintah berikut ini
```
# Start dan Enable 
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```
![Screenshot from 2025-05-26 17-13-01](https://github.com/user-attachments/assets/9906bd60-fdd2-4a01-b336-3d5fa7619317)

Lalu cek apakah node-exporter sudah berjalan dengan normal menggunakan perintah berikut
```
sudo systemctl status node_exporter
```
![Screenshot from 2025-05-26 17-13-38](https://github.com/user-attachments/assets/1f7bab77-ff0c-4dba-b28e-57d1718cc830)

Pastikan untuk mengizinkan PORT 9100 di server yang akan di monitoring

![image](https://github.com/user-attachments/assets/a9f465a0-b738-4d06-855a-8333367609b2)

Selanjutnya ulangi langkah-langkah diatas untuk konfigurasi node-exporter pada server sonarqube dan docker


### Step 2 : Install dan Konfigurasi Prometheus di Komputer Lokal

Kunjungi website berikut untuk mendownload Prometheus
https://prometheus.io/download/

Copy link download dari Prometheus dan pastikan OS yang digunakan sesuai 

![Screenshot from 2025-05-26 14-35-12](https://github.com/user-attachments/assets/cdd1faab-2e7f-42aa-aa15-37fd471d7441)

Download prometheus dengan menggunakan perintah wget + link download prometheus

![Screenshot from 2025-05-26 14-35-45](https://github.com/user-attachments/assets/b02f7065-2d25-4173-b1fc-0aa22599b4d1)

Extract file Prometheus

![image](https://github.com/user-attachments/assets/74be305a-cfa8-4908-a527-8f8b6ef59cee)

Copy bin prometheus kedalam project

![image](https://github.com/user-attachments/assets/a88aa281-d4d0-4938-831f-20b8446ec420)

![image](https://github.com/user-attachments/assets/f0ce2828-d9ff-4980-b75e-d01e541f8e2c)

Selanjutnya buat file prometheus.yml dan isi dengan perintah berikut : 
```
global:
  # Scrape data pada server dalam waktu 5 detik
  scrape_interval: "5s"
  # Waktu interval antara setiap pengambilan data
  evaluation_interval: "3s"
scrape_configs:
  - job_name: 'exporter-prometheus-node'
    static_configs:
      # Menentukan target yang akan di-scrape
      - targets:
        - '18.143.75.200:9100' # jenkins instance
        # - 'localhost:8080' # sonarque instance
        # - 'localhost:9090' # docker instance        
```
Untuk menjalankan prometheus cukup dengan menggunakan perintah berikut ini 
```
./prometheus pada folder project
```
![image](https://github.com/user-attachments/assets/7f50b4ac-5070-47bd-97eb-bb8b470f5ead)

Secara default Prometheus berjalan di PORT 9090. yang dimana port tempat web ui dan api dari prometheus di ekspos. sekarang buka web browser dan masukkan ip dari komputer lokal dan tambah port 9090 

![image](https://github.com/user-attachments/assets/8cc7bf61-bf40-4831-9ecc-68586ff8af1f)

Cara untuk melihat metrics dari server yang di monitoring bisa menuju menu Status > Targets 

![image](https://github.com/user-attachments/assets/20266687-4622-4381-8708-3587a73ce982)

Berikut adalah hasil dari scaping data dari server aws 

![image](https://github.com/user-attachments/assets/b725aa7c-5900-4f1e-bf5b-4ea73fcbaa66)





