<!-- ---
title : "Tạo EC2 Security Group"
date :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---


#### Để tạo một Security Group trên AWS với các cổng 80, 443, 5000 và 22, bạn có thể sử dụng các bước sau:

1. Đăng nhập vào bảng điều khiển AWS.

2. Trong menu chính, chọn **Services** (Dịch vụ) và sau đó chọn **EC2** trong phần **Compute** (Máy tính).

3. Trong bên trái, bạn sẽ thấy **Network & Security** (Mạng và Bảo mật). Chọn **Security Groups** (Các nhóm bảo mật).

4. Nhấp vào nút **Create Security Group** (Tạo nhóm bảo mật) để bắt đầu quá trình tạo.

![Create a VPC](/images/1/0009.png?featherlight=false&width=90pc)

5. Điền các thông tin cơ bản như tên và mô tả cho Security Group của bạn.

![Create a VPC](/images/1/00010.png?featherlight=false&width=90pc)

6. Trong phần **Inbound rules** (Luật đến), thêm các luật sau đây để cho phép truy cập vào các cổng cụ thể:
   - HTTP (80): Chọn **HTTP** trong danh sách hoặc nhập cổng 80.
   - HTTPS (443): Chọn **HTTPS** trong danh sách hoặc nhập cổng 443.
   - Custom TCP Rule (5000): Chọn **Custom TCP Rule** và nhập cổng 5000.
   - SSH (22): Chọn **SSH** trong danh sách hoặc nhập cổng 22.

![Create a VPC](/images/1/00011.png?featherlight=false&width=90pc)

7. Sau khi thêm các luật, nhấp vào nút **Create security group** (Tạo nhóm bảo mật).

![Create a VPC](/images/1/00012.png?featherlight=false&width=90pc)

8. Security Group của bạn đã được tạo và bạn có thể gán nó cho các tài nguyên EC2 của bạn để quản lý quyền truy cập vào các cổng này.

![Create a VPC](/images/1/00013.png?featherlight=false&width=90pc) -->
