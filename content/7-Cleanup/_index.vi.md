---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

**Dọn dẹp tài nguyên**

1. Để xóa VPC và các tài nguyên liên quan:
   - Xóa DB subnet group.
   - Mở bảng điều khiển Amazon RDS.
   - Trong ngăn dẫn hướng, chọn Subnet groups.
   - Chọn DB subnet group liên quan bài lab.
   - Chọn Delete, sau đó chọn Delete trong cửa sổ xác nhận.
   - Xóa security groups.
   - Mở bảng điều khiển Amazon VPC.
   - Chọn VPC Dashboard, sau đó chọn Security Groups.
   - Chọn security group liên quan bài lab.
   - Chọn Actions, chọn Delete security groups và sau đó chọn Delete để xác nhận.
   - Xóa NAT gateway.
   - Mở bảng điều khiển Amazon VPC.
   - Chọn VPC Dashboard, sau đó chọn NAT Gateways.
   - Chọn NAT Gateway liên quan bài lab.
   - Chọn Actions, chọn Delete NAT gateway.
   - Xác nhận xóa và chọn Delete.
   - Xóa VPC.
   - Mở bảng điều khiển Amazon VPC.
   - Chọn Bảng điều khiển VPC, sau đó chọn VPC.
   - Chọn VPC bạn muốn xóa.
   - Từ Actions, chọn Delete VPC.
   - Trên trang xác nhận, hãy nhập delete, rồi chọn Delete.
   
2. **Release the Elastic IP addresses**
   - Mở bảng điều khiển Amazon EC2.
   - Chọn Amazon EC2 Dashboard, sau đó chọn Elastic IPs.
   - Chọn Elastic IP address liên quan bài lab.
   - Từ Actions, chọn Release Elastic IP addresses.
   - Trên trang xác nhận, hãy chọn Release.
   
3. **Terminate EC2 instance**
   - Truy cập EC2 Management Console.
   - Trên thanh điều hướng bên trái, chọn Instances.
   - Chọn tất cả EC2 Instance liên quan tới bài lab.
   - Click Actions.
   - Click Manage Instance State.
   - Chọn Terminate.
   - Click Change State.
   
4. **Xóa DB Instance**
   - Truy cập RDS Management Console.
   - Trên thanh điều hướng bên trái, chọn Databases.
   - Chọn tất cả DB Instance liên quan tới bài lab.
   - Click Actions.
   - Click Delete.
   - Bỏ chọn Create final snapshot? và chọn I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available.
   - Gõ delete me vào ô trống.
   - Click Delete.
   
5. **Xóa DB Snapshots**
   - Truy cập RDS Management Console.
   - Trên thanh điều hướng bên trái, chọn Snapshots.
   - Chọn tất cả snapshot liên quan tới bài lab.
   - Click Actions.
   - Click Delete snapshot.
   - Click Delete.
