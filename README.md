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

### Buat user replikasi:

```
CREATE USER 'repluser'@'%' IDENTIFIED BY 'repl_password' REQUIRE NONE; GRANT REPLICATION SLAVE ON *.* TO 'repluser'@'%'; FLUSH PRIVILEGES;
```

### Cek status binary log:

```
SHOW MASTER STATUS;
```

### Catat hasilnya(contoh):

```
File: mysql-bin.000001
Position:  1481
```

### 🧭 3. Konfigurasi Slave

### Edit file konfigurasi:

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

### Ubah jadi:

```
server-id = 2
relay-log = /var/log/mysql/mysql-relay-bin.log
```

### Restart MySQL:

```
sudo systemctl restart mysql
```

### Masuk MySQL di slave:

```
sudo mysql
```

### Hubungkan ke master:

```
STOP SLAVE;
CHANGE MASTER TO
  MASTER_HOST='10.0.0.1',
  MASTER_USER='repl',
  MASTER_PASSWORD='passwordku',
  MASTER_LOG_FILE='mysql-bin.000001',
  MASTER_LOG_POS=1481;
START SLAVE;
```

### ✅ 4. Verifikasi Replikasi

### Cek status di slave:

```
SHOW SLAVE STATUS\G
```

### Pastikan berjalan:

```
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
```

### Kalau dua-duanya Yes → berarti replikasi berhasil! 🎉
