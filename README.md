## 🚀 Langkah-langkah Setup

### 🧱 1. Install MySQL di Kedua Server

```bash
sudo apt update
sudo apt install mysql-server -y
```

### 🧭 2. Konfigurasi Master

### Edit file konfigurasi MySQL:

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
### Cari dan ubah:

```
server-id = 1
bind-address = 0.0.0.0
binlog_do_db = nama_database
log_bin = /var/log/mysql/mysql-bin.log
```

### Restart MySQL:

```
sudo systemctl restart mysql
```
### Lalu masuk MySQL:

```
sudo mysql
```
