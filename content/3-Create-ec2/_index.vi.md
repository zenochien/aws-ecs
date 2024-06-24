---
title : "Tạo EC2 instance"
date :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Tạo một Instance trên AWS

Bạn có thể tạo một instance Linux bằng cách sử dụng AWS Management Console theo hướng dẫn sau đây. Hướng dẫn này được thiết kế để giúp bạn tạo instance đầu tiên một cách nhanh chóng, nên nó không bao gồm tất cả các tùy chọn có thể có. Để biết thông tin về các tùy chọn nâng cao, hãy xem Hướng dẫn tạo instance bằng cách sử dụng hướng dẫn mới về Launch Instance. Để biết thông tin về cách khác để tạo instance của bạn, hãy xem Hướng dẫn tạo instance của bạn.

1. Truy cập AWS Console

   - Mở trình duyệt và truy cập vào Amazon EC2 console tại [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).

2. Chọn **Launch instance**

   - Tại màn hình dashboard của EC2 console, trong hộp **Launch instance**, chọn **Launch instance**, sau đó chọn **Launch instance** từ các tùy chọn xuất hiện.

   ![Create a VPC](/images/3/0001.png?featherlight=false&width=90pc)

3. Đặt tên cho instance

   - Dưới phần **Name and tags**, cho **Name**, nhập tên mô tả cho instance của bạn.

   ![Create a VPC](/images/3/0002.png?featherlight=false&width=90pc)

4. Chọn hình ảnh (Amazon Machine Image - AMI)

   - Dưới phần **Application and OS Images (Amazon Machine Image)**, làm theo các bước sau đây:
     - Chọn **Quick Start**, sau đó chọn **Amazon Linux**. Đây là hệ điều hành (OS) cho instance của bạn.
     - Từ **Amazon Machine Image (AMI)**, chọn một phiên bản HVM của **Amazon Linux 2023**. Lưu ý rằng các AMI này được đánh dấu là **Free tier eligible**. Một Amazon Machine Image (AMI) là một cấu hình cơ bản dùng làm mẫu cho instance của bạn.

   ![Create a VPC](/images/3/0003.png?featherlight=false&width=90pc)

5. Chọn loại instance

   - Dưới phần **Instance type**, từ danh sách **Instance type**, bạn có thể chọn cấu hình phần cứng cho instance của bạn. Chọn loại instance **t2.micro**, mà mặc định đã được chọn. Loại instance **t2.micro** đủ điều kiện để sử dụng miễn phí trong AWS Free Tier. Tại các khu vực mà loại **t2.micro** không có sẵn, bạn có thể sử dụng loại **t3.micro** trong AWS Free Tier. Để biết thêm thông tin, hãy xem AWS Free Tier.

   ![Create a VPC](/images/3/0004.png?featherlight=false&width=90pc)

6. Chọn Key Pair

   - Dưới phần **Key pair (login)**, cho **Key pair name**, chọn key pair mà bạn đã tạo khi cài đặt.
   
   - **Cảnh báo**: Đừng chọn **Proceed without a key pair (Không được khuyến nghị)**. Nếu bạn tạo instance mà không có key pair, bạn sẽ không thể kết nối vào nó.

   ![Create a VPC](/images/3/0005.png?featherlight=false&width=90pc)

7. Cấu hình Security Group

   - Bên cạnh **Network settings**, chọn **Edit**. Đối với **Security group name**, bạn sẽ thấy rằng hướng dẫn đã tạo và chọn một security group cho bạn. Bạn có thể sử dụng security group này hoặc bạn có thể chọn security group mà bạn đã tạo khi cài đặt bằng các bước sau đây:

     - Chọn **Select existing security group**.
     - Từ **Common security groups**, chọn security group của bạn từ danh sách các security group hiện có.

   ![Create a VPC](/images/3/0006.png?featherlight=false&width=90pc)

   ![Create a VPC](/images/3/0007.png?featherlight=false&width=90pc)

8. Xác nhận và khởi động instance

   - Giữ nguyên các lựa chọn mặc định cho các cài đặt khác của instance của bạn.
   - Xem lại bản tóm tắt về cấu hình instance của bạn trong **Summary panel**, và khi bạn đã sẵn sàng, chọn **Launch instance**.

   ![Create a VPC](/images/3/0008.png?featherlight=false&width=90pc)

9. Xác nhận và kiểm tra

   - Một trang xác nhận sẽ thông báo rằng instance của bạn đang được khởi động. Chọn **View all instances** để đóng trang xác nhận và trở lại giao diện console.
   - Trên màn hình Instances, bạn có thể xem trạng thái của quá trình khởi động. Có một thời gian ngắn để instance được khởi động. Khi bạn khởi động instance, trạng thái ban đầu của nó là **pending**. Sau khi instance khởi động, trạng thái của nó sẽ thay đổi thành **running** và nó sẽ nhận được một tên DNS công cộng. Nếu cột **Public IPv4 DNS** bị ẩn, hãy chọn biểu tượng cài đặt (Biểu tượng cài đặt) ở góc trên bên phải, bật **Public IPv4 DNS** và chọn **Confirm**.
   - Có thể mất vài phút để instance sẵn sàng để bạn kết nối vào. Hãy kiểm tra xem instance của bạn đã vượt qua kiểm tra trạng thái; bạn có thể xem thông tin này trong cột **Status check**.

   ![Create a VPC](/images/3/0009.png?featherlight=false&width=90pc)

   ![Create a VPC](/images/3/00010.png?featherlight=false&width=90pc)

#### Kết nối vào EC2 Instance bằng SSH sử dụng Mobaxterm

Để kết nối vào một Amazon Elastic Compute Cloud (EC2) instance thông qua SSH sử dụng Mobaxterm, bạn cần thực hiện các bước sau:

1. **Tải và Cài Đặt Mobaxterm**:

   - Đầu tiên, tải Mobaxterm từ trang web chính thức: [Mobaxterm Website](https://mobaxterm.mobatek.net/download-home-edition.html).
   - Sau khi tải xong, cài đặt Mobaxterm trên máy tính của bạn.

2. **Mở Mobaxterm**:

   - Khởi động Mobaxterm sau khi cài đặt xong.

3. **Tạo Kết Nối SSH Mới**:

   - Trong Mobaxterm, bạn có thể tạo kết nối SSH mới bằng cách nhấp vào biểu tượng **Session** ở góc trên bên trái của giao diện.

4. **Cấu Hình Kết Nối SSH**:

   - Trong cửa sổ cấu hình, điền các thông tin sau:
     - **Remote Host**: Điền địa chỉ IP hoặc tên miền của EC2 instance bạn muốn kết nối.
     - **Port**: Mặc định là 22 (port SSH).
     - **User**: Tên người dùng trên EC2 instance (thông thường là **ec2-user** hoặc **ubuntu**).
     - **Advanced SSH settings**: Ở đây, bạn có thể cung cấp khóa SSH (private key) nếu bạn sử dụng khóa thay vì mật khẩu.

   ![Create a VPC](/images/3/00011.png?featherlight=false&width=90pc)

5. **Kết Nối vào EC2 Instance**:

   - Nhấp vào nút **OK** để lưu cấu hình và sau đó nhấp vào biểu tượng kết nối để mở kết nối SSH vào EC2 instance của bạn.

6. **Nhập Mật Khẩu (Nếu Có)**:

   - Nếu bạn sử dụng mật khẩu thay vì khóa SSH, Mobaxterm sẽ yêu cầu bạn nhập mật khẩu người dùng trên EC2 instance.

7. **Kết Nối Thành Công**:

   - Nếu tất cả thông tin đúng, bạn sẽ kết nối thành công vào EC2 instance và có thể thực hiện các tác vụ trên máy chủ từ xa.

   - Nhớ rằng bạn cần phải có quyền truy cập vào EC2 instance và đã cấu hình Security Group của nó cho phép kết nối SSH từ địa chỉ IP của bạn.

   ![Create a VPC](/images/3/00012.png?featherlight=false&width=90pc)
