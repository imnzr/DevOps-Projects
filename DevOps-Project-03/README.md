# Monitoring dan Automatisasi Infrastruktur CI/CD dengan Prometheus, Grafana, dan Ansible di AWS 

![diagram-export-5-26-2025-2_18_43-PM](https://github.com/user-attachments/assets/bb6f9459-fcc5-4f3f-aa28-69d410576322)

## Agenda 
## Prerequisites

### Step 1 : Install dan Konfigurasi Node-Exporter
1. Install node-exporter pada komputer lokal
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

### Step 2 : Install dan Konfigurasi Prometheus 

Kunjungi website berikut untuk mendownload Prometheus
https://prometheus.io/download/

Copy link download dari Prometheus dan pastikan OS yang digunakan sesuai 

![Screenshot from 2025-05-26 14-35-12](https://github.com/user-attachments/assets/cdd1faab-2e7f-42aa-aa15-37fd471d7441)

Download prometheus dengan menggunakan perintah wget + link download prometheus

![Screenshot from 2025-05-26 14-35-45](https://github.com/user-attachments/assets/b02f7065-2d25-4173-b1fc-0aa22599b4d1)

Extract file Prometheus

![image](https://github.com/user-attachments/assets/74be305a-cfa8-4908-a527-8f8b6ef59cee)








