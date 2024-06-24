---
title : "Tạo RDS Security group"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### Tạo Security Group cho DB instance trong AWS

Chúng ta có thể sử dụng Security Group để kiểm soát truy cập vào DB instance riêng tư trong môi trường AWS. Dưới đây là cách tạo một Security Group cho DB instance trong giao diện VPC:

1. Trong giao diện VPC, chọn **Security group**.

2. Chọn **Create security group** để tạo một Security Group mới cho DB instance riêng tư.

3. Đặt tên cho Security Group:
   - Security group name: Nhập tên Security Group.
   - Description: Nhập mô tả cho Security Group.

![Create a VPC](/images/2/0001.png?featherlight=false&width=90pc)

4. Chọn VPC đã tạo để liên kết Security Group với VPC.

5. Tiến hành cấu hình các Inbound rules để quyết định nguồn nào được phép truy cập DB instance. Ví dụ:
   - Chọn **MYSQL/Aurora** và cổng **3306**.
   - Custom source: Nhập ID của Security Group của Amazon EC2 instance mà bạn muốn kết nối với DB instance.

![Create a VPC](/images/2/0002.png?featherlight=false&width=90pc)

6. Sau khi cấu hình xong, chọn **Create security group** để hoàn thành quá trình tạo Security Group cho DB instance.

![Create a VPC](/images/2/0003.png?featherlight=false&width=90pc)

Như vậy, bạn đã tạo thành công một Security Group cho DB instance riêng tư trong môi trường AWS.

> Lưu ý: Không khuyến khích chia sẻ Security Group giữa DB instance và Amazon EC2 instance để đảm bảo an toàn và quản lý riêng biệt cho từng tài nguyên.

![Create a VPC](/images/2/0004.png?featherlight=false&width=90pc)

