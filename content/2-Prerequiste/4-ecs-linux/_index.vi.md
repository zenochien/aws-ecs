---
title : "Tạo DB Subnet Group"
date :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---


#### Tạo DB Subnet Group trên AWS

Để tạo DB Subnet Group trên AWS, làm theo các bước sau:

1. Truy cập vào AWS Management Console.

2. Tìm và chọn dịch vụ Amazon RDS.

3. Trong menu điều hướng, chọn Subnet groups.

4. Chọn Create DB Subnet Group.

![Create a VPC](/images/2/0005.png?featherlight=false&width=90pc)

5. Trong giao diện Create DB Subnet Group:

   - Đặt tên cho subnet group của bạn trong trường Name.

   - Nhập mô tả cho subnet group của bạn trong trường Description.

   - Chọn Virtual Private Cloud (VPC) mặc định hoặc VPC bạn đã tạo.

![Create a VPC](/images/2/0006.png?featherlight=false&width=90pc)

6. Trong phần **Add subnets**, chọn các Availability Zones (AZ) chứa các subnets trong mục Availability Zones, sau đó chọn các subnets trong mục Subnets.

7. Nhấn nút Create để hoàn thành quá trình tạo DB Subnet Group.

![Create a VPC](/images/2/0007.png?featherlight=false&width=90pc)

Lưu ý: Nếu bạn đã bật Local Zone, bạn có thể chọn một nhóm Availability Zone trên trang Create DB Subnet Group. Trong trường hợp này, hãy chọn nhóm Availability Zone, các Availability Zones và Subnets tương ứng.

Sau khi hoàn thành, DB Subnet Group mới của bạn sẽ xuất hiện trong danh sách các DB Subnet Group trên giao diện RDS console. Bạn có thể chọn DB Subnet Group để xem chi tiết, bao gồm danh sách các subnets được kết nối với nhóm này, trong phần chi tiết ở dưới cùng của cửa sổ.

![Create a VPC](/images/2/0008.png?featherlight=false&width=90pc)
