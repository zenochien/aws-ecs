---
title : "Backup và restore"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

#### Giám sát AWS RDS - Backup và restore

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

7. Kiểm tra database instance đã restore.

   ![Create a VPC](/images/5/00028.png?featherlight=false&width=90pc)

