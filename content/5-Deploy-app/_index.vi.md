---
title : "Triển khai ứng dụng"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

#### Triển khai ứng dụng

1. Để clone repository từ GitHub của AWS-First-Cloud-Journey, bạn có thể sử dụng lệnh sau:

```
git clone https://github.com/AWS-First-Cloud-Journey/AWS-FCJ-Management
```

![Create a VPC](/images/5/0001.png?featherlight=false&width=90pc)

2. #Hướng dẫn cài đặt Node.js trên Amazon Linux 2023

Dưới đây là một tập lệnh Bash để cài đặt Node.js trên Amazon Linux. Hãy sao chép và thực thi các bước sau:

```
#!/bin/bash

# Các màu cho định dạng
GREEN='\033[0;32m'
NC='\033[0m' # Không màu

# Kiểm tra xem NVM đã được cài đặt chưa
if ! command -v nvm &> /dev/null; then
  # Bước 1: Cài đặt nvm
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
  source ~/.nvm/nvm.sh
fi

# Xác minh việc cài đặt nvm
nvm --version

# Cài đặt phiên bản LTS của Node.js
nvm install --lts

# Sử dụng phiên bản LTS đã cài đặt
nvm use --lts

# Xác minh cài đặt Node.js và npm
node -v
npm -v

# Bước 4: Tạo tệp package.json (nếu nó chưa tồn tại)
if [ ! -f package.json ]; then
  npm init -y
  echo -e "${GREEN}Đã tạo tệp package.json.${NC}"
fi

# Bước 5: Cài đặt các gói npm cần thiết
echo -e "Đang cài đặt các gói npm cần thiết..."
npm install express dotenv express-handlebars body-parser mysql

# Bước 6: Cài đặt nodemon như một phần phát triển
echo -e "Đang cài đặt nodemon như một phần phát triển..."
npm install --save-dev nodemon
npm install -g nodemon

# Bước 7: Thêm script npm start vào package.json
if ! grep -q '"start":' package.json; then
  npm set-script start "index.js"  # Thay thế "your-app.js" bằng tệp điểm nhập của bạn
  echo -e "${GREEN}Đã thêm script npm start vào package.json.${NC}"
fi

echo -e "${GREEN}Cài đặt hoàn tất. Bây giờ bạn có thể bắt đầu xây dựng và chạy ứng dụng Node.js của mình bằng 'npm start'.${NC}"

```

![Create a VPC](/images/5/0002.png?featherlight=false&width=90pc)

3. Đây là một đoạn script Bash dùng để cài đặt và cấu hình máy chủ MySQL trên một hệ thống . Script này thực hiện các bước sau:

- Đặt các biến với đường dẫn RPM của MySQL và thông tin cơ sở dữ liệu như địa chỉ RDS, tên cơ sở dữ liệu, tên người dùng và mật khẩu.

- Kiểm tra xem RPM của kho cộng đồng MySQL đã tồn tại trong thư mục hiện tại chưa. Nếu chưa tồn tại, nó sẽ tải về RPM từ URL đã chỉ định.

- Cài đặt RPM của kho cộng đồng MySQL và MySQL Server.

- Khởi động máy chủ MySQL và cấu hình nó để tự động khởi động cùng hệ thống.

- Kiểm tra phiên bản MySQL đã cài đặt.

- Bảo mật máy chủ MySQL bằng lệnh mysql_secure_installation.

- Tạo hoặc cập nhật tệp .env với thông tin cơ sở dữ liệu (địa chỉ, tên cơ sở dữ liệu, tên người dùng và mật khẩu).

- Kết nối đến máy chủ MySQL với thông tin xác thực và bạn có thể thêm các lệnh SQL cụ thể tại đây.

> Lưu ý: Để thực hiện script này, bạn cần có quyền sudo và phải chắc chắn rằng bạn đã cung cấp đúng thông tin cơ sở dữ liệu (RDS Endpoint, tên cơ sở dữ liệu, tên người dùng và mật khẩu) trước khi chạy script.


```
#!/bin/bash

# Set variables for MySQL RPM and database information
MYSQL_RPM_URL="https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm"
DB_HOST="RDS Endpoint"
DB_NAME="Database name"
DB_USER="Database username"
DB_PASS="Database password"


# Check if MySQL Community repository RPM already exists
if [ ! -f mysql80-community-release-el9-1.noarch.rpm ]; then
  sudo wget $MYSQL_RPM_URL
fi

# Install MySQL Community repository
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm

# Install MySQL server
sudo dnf install -y mysql-community-server

# Start MySQL server
sudo systemctl start mysqld

# Enable MySQL to start on boot
sudo systemctl enable mysqld

# Check MySQL version
mysql -V

# Secure the MySQL server
sudo mysql_secure_installation

# Create or update the .env file with database information
echo "DB_HOST=$DB_HOST" >> .env
echo "DB_NAME=$DB_NAME" >> .env
echo "DB_USER=$DB_USER" >> .env
echo "DB_PASS=$DB_PASS" >> .env

# Connect to MySQL and create a new database (you might want to add specific SQL commands here)
mysql -h $DB_HOST -P 3306 -u $DB_USER -p$DB_PASS
```

![Create a VPC](/images/5/0003.png?featherlight=false&width=90pc)

4. Tạo Database và Bảng trong AWS RDS

Sau khi kết nối thành công vào RDS (Relational Database Service) trên AWS, chúng ta có thể tạo một database mới và định nghĩa một bảng trong đó bằng cách sử dụng SQL script sau đây.

#### Tạo Database

Đầu tiên, chúng ta sẽ tạo một database mới nếu nó chưa tồn tại. Sử dụng lệnh sau:

```
CREATE DATABASE IF NOT EXISTS first_cloud_users;
```

Lệnh này kiểm tra xem database "first_cloud_users" đã tồn tại hay chưa. Nếu chưa tồn tại, nó sẽ tạo một database mới có tên "first_cloud_users".

#### Sử dụng Database
Tiếp theo, chúng ta sử dụng database "first_cloud_users" bằng cách sử dụng lệnh:

```
USE first_cloud_users;
```

Lệnh này cho biết rằng tất cả các lệnh SQL sau đó sẽ được thực thi trong database "first_cloud_users".

#### Tạo Bảng "user"

Chúng ta đã tạo database và sử dụng nó. Bây giờ, chúng ta sẽ định nghĩa một bảng "user" trong database này bằng cách sử dụng SQL script sau:

```
CREATE TABLE `user`
(
    `id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `first_name` VARCHAR(45) NOT NULL,
    `last_name` VARCHAR(45) NOT NULL,
    `email` VARCHAR(100) NOT NULL UNIQUE,
    `phone` VARCHAR(15) NOT NULL,
    `comments` TEXT NOT NULL,
    `status` ENUM('active', 'inactive') NOT NULL DEFAULT 'active'
) ENGINE = InnoDB;

```
Lệnh này định nghĩa cấu trúc của bảng "user" với các cột như "id", "first_name", "last_name", "email", "phone", "comments", và "status". Các cột này đại diện cho thông tin về người dùng, và cột "id" được đặt là khóa chính tự động tăng.

#### Thêm Dữ liệu vào Bảng "user"
Cuối cùng, chúng ta có thể thêm dữ liệu vào bảng "user" bằng cách sử dụng lệnh INSERT INTO. Dưới đây là ví dụ thêm một số bản ghi vào bảng:

```
INSERT INTO `user`
(`first_name`, `last_name`, `email`, `phone`, `comments`, `status`)
VALUES
('Amanda', 'Nunes', 'anunes@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Alexander', 'Volkanovski', 'avolkanovski@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Khabib', 'Nurmagomedov', 'knurmagomedov@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Kamaru', 'Usman', 'kusman@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Israel', 'Adesanya', 'iadesanya@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Henry', 'Cejudo', 'hcejudo@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Valentina', 'Shevchenko', 'vshevchenko@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Tyron', 'Woodley', 'twoodley@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Rose', 'Namajunas', 'rnamajunas@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Tony', 'Ferguson', 'tferguson@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Jorge', 'Masvidal', 'jmasvidal@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Nate', 'Diaz', 'ndiaz@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Conor', 'McGregor', 'cmcGregor@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Cris', 'Cyborg', 'ccyborg@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Tecia', 'Torres', 'ttorres@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Ronda', 'Rousey', 'rrousey@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Holly', 'Holm', 'hholm@ufc.com', '012345 678910', 'I love AWS FCJ', 'active'),
('Joanna', 'Jedrzejczyk', 'jjedrzejczyk@ufc.com', '012345 678910', 'I love AWS FCJ', 'active');
```
Lệnh này thêm các bản ghi của người dùng vào bảng "user" với thông tin như tên, email, số điện thoại, bình luận và trạng thái mặc định là "active".

Đây là cách tạo và quản lý một database và bảng trong AWS RDS sử dụng SQL script.

#### một số lệnh SQL để kiểm tra thông tin về cơ sở dữ liệu (database) trong hệ thống quản lý cơ sở dữ liệu (DBMS) như MySQL hoặc PostgreSQL:

#### Hiển thị danh sách tất cả các database:

```
SHOW DATABASES;
```

Lệnh này sẽ liệt kê tất cả các database có sẵn trong hệ thống.

#### Chọn một database cụ thể để làm việc:

```
USE database_name;
```
Lệnh này sẽ chuyển bạn từ database hiện tại sang database có tên là "database_name". Sau khi sử dụng lệnh này, tất cả các lệnh SQL tiếp theo sẽ áp dụng cho database này.


#### Hiển thị các bảng trong database hiện tại:

```
SHOW TABLES;
```

Lệnh này sẽ liệt kê tất cả các bảng có trong database hiện tại.

#### Hiển thị cấu trúc của một bảng cụ thể:

```
DESCRIBE table_name;
```

Lệnh này sẽ cho bạn biết cấu trúc của bảng có tên là "table_name", bao gồm tên cột, kiểu dữ liệu và các thuộc tính khác của cột.

#### Hiển thị thông tin về kích thước của database:

```
SELECT table_schema "Database Name", SUM(data_length + index_length) / 1024 / 1024 "Database Size (MB)"
FROM information_schema.tables
GROUP BY table_schema;
```

Lệnh này sẽ hiển thị thông tin về kích thước của các database trong hệ thống, tính bằng đơn vị Megabyte (MB).

Nhớ thay thế "database_name" và "table_name" bằng tên cụ thể của database và bảng mà bạn muốn kiểm tra. Các lệnh này giúp bạn quản lý và kiểm tra thông tin về cơ sở dữ liệu của bạn.

![Create a VPC](/images/5/0004.png?featherlight=false&width=90pc)

5. Sau khi bạn đã ở trong thư mục ứng dụng, chạy lệnh sau để khởi động ứng dụng bằng npm start:

```
npm start
```

![Create a VPC](/images/5/0005.png?featherlight=false&width=90pc)


6. **Kiểm tra trạng thái EC2 Instance:** Đảm bảo rằng EC2 Instance của bạn đang chạy và hoạt động bình thường.

- **Kiểm tra ứng dụng trên trình duyệt:** Mở trình duyệt web và nhập địa chỉ IP hoặc tên miền của EC2 Instance, kèm theo cổng 5000 (ví dụ: `http://<địa chỉ IP hoặc tên miền>:5000`). Điều này sẽ thực hiện kết nối đến ứng dụng của bạn chạy trên cổng 5000.

- **Kiểm tra kết quả:** Trình duyệt sẽ hiển thị ứng dụng của bạn nếu mọi thứ được cấu hình đúng và EC2 Instance đang hoạt động. Nếu không, bạn cần kiểm tra lại các bước trước đó để xác định vấn đề và sửa chữa nó.

```
http://<địa chỉ IP hoặc tên miền>:5000
```

![Create a VPC](/images/5/00011.png?featherlight=false&width=90pc)

#### Giám sát AWS RDS

1. Trên giao diện AWS RDS, bạn có thể thực hiện các bước sau để theo dõi:

- Chọn **Databases** (Cơ sở dữ liệu).
- Chọn DB instance mà bạn đã tạo.
- Chọn **Monitoring** (Theo dõi).

![Create a VPC](/images/5/00014.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00015.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00016.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00017.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00018.png?featherlight=false&width=90pc)

2. Để xem thông tin về sao lưu của DB instance trong AWS RDS, làm theo các bước sau:

- Đăng nhập vào AWS Management Console.

- Chọn dịch vụ "Amazon RDS" trong danh sách các dịch vụ.

- Trong bảng điều khiển RDS, chọn DB instance bạn muốn kiểm tra.

- Trong trang quản lý DB instance, điều hướng đến tab "Maintenance & backups" (Bảo trì và sao lưu).

- Tại đây, bạn có thể xem thông tin về sao lưu tự động và sao lưu thủ công. Bạn cũng có thể cấu hình và quản lý các thiết lập sao lưu.

![Create a VPC](/images/5/00019.png?featherlight=false&width=90pc)

3. Xem thông tin Snapshot.

![Create a VPC](/images/5/00020.png?featherlight=false&width=90pc)

4. Chọn DB snapshot mà bạn muốn khôi phục.

- Trong phần Actions, chọn Restore snapshot.

![Create a VPC](/images/5/00021.png?featherlight=false&width=90pc)

5. Trên trang Restore snapshot, điền tên cho DB instance bạn muốn khôi phục vào ô DB instance identifier.

- Chọn các thiết lập khác như kích thước bộ nhớ được cấp phát.

- Để biết thêm thông tin về mỗi thiết lập, hãy xem [Settings for DB instances](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Limits.html#Limits.DBInstance).

- Cuối cùng, chọn Restore DB instance.

![Create a VPC](/images/5/00022.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00023.png?featherlight=false&width=90pc)


![Create a VPC](/images/5/00024.png?featherlight=false&width=90pc)

![Create a VPC](/images/5/00025.png?featherlight=false&width=90pc)

![Create a VPC](/images/5/00026.png?featherlight=false&width=90pc)

6. Hoàn thành restore snapshot.

![Create a VPC](/images/5/00027.png?featherlight=false&width=90pc)

7.Kiểm tra database instance đã restore.

![Create a VPC](/images/5/00028.png?featherlight=false&width=90pc)
