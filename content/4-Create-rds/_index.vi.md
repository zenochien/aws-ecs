---
title : "Tạo RDS database instance"
date :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

#### Cài đặt Git trên Amazon EC2 2023

Dưới đây là hướng dẫn cài đặt Git trên một máy ảo Amazon EC2 chạy Amazon Linux năm 2023 sử dụng các bước cơ bản.

1. Cập nhật Gói Hệ thống

Trước tiên, hãy cập nhật các gói hệ thống của bạn để đảm bảo rằng bạn đang sử dụng phiên bản mới nhất:

```
sudo dnf update -y
```

2. Tìm Gói Git. Sử dụng lệnh sau để tìm gói Git trong kho lưu trữ:

```
sudo dnf search git
```

3. Cài đặt Git. Sau khi tìm được gói Git, bạn có thể cài đặt nó bằng lệnh sau:

```
sudo dnf install git -y
```

4. Xác minh Cài đặt Git. Cuối cùng, hãy kiểm tra phiên bản Git đã được cài đặt thành công:

```
git --version
```

Nếu bạn thấy phiên bản Git xuất hiện, điều đó có nghĩa là cài đặt đã hoàn tất.

![Create a VPC](/images/4/0001.png?featherlight=false&width=90pc)

5. Cài đặt Node.js trên Amazon EC2 Linux 2023

- Dưới đây là một tập lệnh Bash để cài đặt Node.js trên Amazon EC2 Linux:

```
#!/bin/bash

# Màu sắc cho định dạng
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

# Bước 4: Tạo tệp package.json (nếu chưa tồn tại)
if [ ! -f package.json ]; then
  npm init -y
  echo -e **${GREEN}Đã tạo tệp package.json.${NC}**
fi

# Bước 5: Cài đặt các gói npm cần thiết
echo -e **Đang cài đặt các gói npm cần thiết...**
npm install express dotenv express-handlebars body-parser mysql

# Bước 6: Cài đặt nodemon như một phụ thuộc phát triển
echo -e **Đang cài đặt nodemon như một phụ thuộc phát triển...**
npm install --save-dev nodemon
npm install -g nodemon

# Bước 7: Thêm kịch bản npm start vào package.json
if ! grep -q '**start**:' package.json; then
  npm set-script start **index.js**  # Thay thế **your-app.js** bằng tệp điểm nhập của bạn
  echo -e **${GREEN}Đã thêm kịch bản npm start vào package.json.${NC}**
fi

echo -e **${GREEN}Cài đặt hoàn tất. Bây giờ bạn có thể bắt đầu xây dựng và chạy ứng dụng Node.js của mình bằng 'npm start'.${NC}**
```

![Create a VPC](/images/4/0002.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/0003.png?featherlight=false&width=90pc)

#### Tạo một DB Instance trên AWS

Bạn có thể tạo một DB Instance bằng cách sử dụng AWS Management Console với tùy chọn Easy create được bật hoặc không được bật. Khi Easy create được bật, bạn chỉ cần chỉ định loại DB engine, kích thước DB Instance và định danh DB Instance. Easy create sử dụng các thiết lập mặc định cho các tùy chọn cấu hình khác. Khi Easy create không được bật, bạn phải chỉ định nhiều tùy chọn cấu hình hơn khi bạn tạo một cơ sở dữ liệu, bao gồm các tùy chọn về khả dụng, bảo mật, sao lưu và bảo trì.

**Lưu ý:** Trong thủ tục dưới đây, tùy chọn Standard create được bật và Easy create không được bật. Thủ tục này sử dụng MySQL làm ví dụ.

Đối với ví dụ sử dụng Easy create để hướng dẫn bạn tạo và kết nối các DB Instance mẫu cho từng engine, xem **Bắt đầu với Amazon RDS**.

**Để tạo một DB Instance:**

1. Đăng nhập vào AWS Management Console và mở Amazon RDS console tại [https://console.aws.amazon.com/rds/](https://console.aws.amazon.com/rds/).

2. Ở góc trên bên phải của Amazon RDS console, chọn khu vực AWS mà bạn muốn tạo DB Instance.

3. Trong khung điều hướng, chọn **Databases.**

4. Chọn **Create database,** sau đó chọn **Standard create.**

![Create a VPC](/images/4/0004.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/0005.png?featherlight=false&width=90pc)

5. Đối với **Engine type,** chọn MariaDB, Microsoft SQL Server, MySQL, Oracle, hoặc PostgreSQL. Trong ví dụ này, chúng ta sử dụng Microsoft SQL Server.

6. Đối với **Database management type,** nếu bạn sử dụng Oracle hoặc SQL Server, chọn **Amazon RDS** hoặc **Amazon RDS Custom.**

7. Đối với **Edition,** nếu bạn sử dụng Oracle hoặc SQL Server, chọn phiên bản của DB engine mà bạn muốn sử dụng.

8. Đối với **Version,** chọn phiên bản của engine.

9. Trong phần **Templates,** chọn mẫu phù hợp với trường hợp sử dụng của bạn. Nếu bạn chọn **Production,** các tùy chọn sau sẽ được chọn trước trong bước sau:
   - Tùy chọn Multi-AZ failover
   - Tùy chọn lưu trữ Provisioned IOPS SSD (io1)
   - Tùy chọn bảo vệ khỏi xóa

   Chúng tôi khuyên bạn nên sử dụng những tính năng này cho môi trường sản xuất.

**Lưu ý:** Các lựa chọn mẫu có thể thay đổi theo phiên bản.

10. Để nhập mật khẩu chính của bạn, làm theo các bước sau:

    - Trong phần **Settings,** mở **Credential Settings.**
    - Nếu bạn muốn chỉ định một mật khẩu, hãy bỏ chọn hộp kiểm **Auto generate a password** nếu nó đã được chọn.
    - (Tùy chọn) Thay đổi giá trị **Master username.**
    - Nhập cùng mật khẩu trong **Master password** và **Confirm password.**
    - (Tùy chọn) Cài đặt kết nối với một tài nguyên tính toán cho DB Instance này.

![Create a VPC](/images/4/0006.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/0007.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/0008.png?featherlight=false&width=90pc)

11. Bạn có thể cấu hình kết nối giữa một Amazon EC2 instance và DB Instance mới trong quá trình tạo DB Instance. Để biết thêm thông tin, xem **Cấu hình kết nối mạng tự động với một EC2 instance.**

12. Trong phần **Connectivity** dưới **VPC security group (firewall),** nếu bạn chọn **Create new,** một nhóm bảo mật VPC sẽ được tạo ra với một luật đăng nhập cho phép địa chỉ IP máy tính cục bộ của bạn truy cập vào cơ sở dữ liệu.

![Create a VPC](/images/4/0009.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00010.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00011.png?featherlight=false&width=90pc)

13. Đối với các phần còn lại, chỉ định các thiết lập DB Instance của bạn. Để biết thêm thông tin về mỗi thiết lập, xem **Settings for DB instances.**

14. Chọn **Create database.**

![Create a VPC](/images/4/00012.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00013.png?featherlight=false&width=90pc)

15. Nếu bạn chọn sử dụng mật khẩu được tạo tự động, nút **View credential details** sẽ xuất hiện trên trang **Databases.**

16. Để xem tên người dùng chính và mật khẩu cho DB Instance, chọn **View credential details.**

17. Để kết nối vào DB Instance dưới tên người dùng chính, sử dụng tên người dùng và mật khẩu được hiển thị.

**Quan trọng:** Bạn không thể xem lại mật khẩu người dùng chính. Nếu bạn không ghi lại nó, bạn có thể phải thay đổi nó. Nếu bạn cần thay đổi mật khẩu người dùng chính sau khi DB Instance đã sẵn sàng, bạn có thể sửa đổi DB Instance để làm điều này. Để biết thêm thông tin về việc sửa đổi một DB Instance, xem **Modifying an Amazon RDS DB Instance.**

18. Đối với **Databases,** chọn tên của DB Instance mới.

![Create a VPC](/images/4/00014.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00015.png?featherlight=false&width=90pc)

19. Trên console RDS, thông tin về DB Instance mới sẽ xuất hiện. DB Instance có trạng thái **Creating** cho đến khi nó được tạo và sẵn sàng sử dụng. Khi trạng thái chuyển sang **Available,** bạn có thể kết nối vào DB Instance. Tùy thuộc vào lớp DB Instance và lưu trữ được cấp phát, có thể mất vài phút để DB Instance mới có sẵn.

![Create a VPC](/images/4/00016.png?featherlight=false&width=90pc)

20. Kiểm tra RDS
   - Trong trang chi tiết của instance RDS, bạn có thể tìm thấy các thông tin liên quan đến kết nối như **Endpoint** (điểm kết nối), **Port** (cổng), và **Username** (tên người dùng).
   - Điểm kết nối (Endpoint) là URL hoặc địa chỉ IP mà bạn sử dụng để kết nối tới cơ sở dữ liệu RDS.

![Create a VPC](/images/4/00017.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00018.png?featherlight=false&width=90pc)

#### Xem Log và Sự kiện trên AWS RDS

Để theo dõi Log và Sự kiện trên Amazon RDS (Relational Database Service), bạn có thể thực hiện các bước sau:

1. Đăng nhập vào [AWS Management Console](https://aws.amazon.com/console/).

2. Chọn dịch vụ **Amazon RDS** từ bảng điều khiển AWS.

3. Chọn instance RDS mà bạn muốn xem Log và Sự kiện.

4. Trong trang chi tiết của instance, bạn sẽ thấy các tab sau:

   - **DB instance details**: Hiển thị thông tin cơ bản về instance.
   - **Configuration**: Cho phép bạn xem và thay đổi cấu hình của instance.
   - **Log & events**: Đây là nơi bạn có thể xem Log và Sự kiện.

5. Nhấp vào tab **Log & events**. Tại đây, bạn có thể xem các log như:

   - **Error log**: Ghi lại các lỗi xảy ra trên instance.
   - **General log**: Ghi lại các hoạt động chung trên instance.
   - **Slow query log**: Ghi lại các truy vấn chậm.
   - **Event log**: Hiển thị các sự kiện quan trọng liên quan đến instance.

6. Bạn có thể tùy chỉnh các thiết lập xem Log và Sự kiện tại đây, chẳng hạn như khoảng thời gian bạn muốn xem log hoặc cài đặt thông báo qua email cho các sự kiện quan trọng.

Nhớ duyệt qua các log và sự kiện thường xuyên để theo dõi tình trạng của Amazon RDS instance của bạn và phát hiện sớm bất kỳ vấn đề nào.

![Create a VPC](/images/4/00019.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00020.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00021.png?featherlight=false&width=90pc)

![Create a VPC](/images/4/00022.png?featherlight=false&width=90pc)

#### Xem Bảo Trì và Sao Lưu AWS RDS

Trong dịch vụ Amazon Web Services (AWS), Amazon Relational Database Service (RDS) cung cấp một cơ sở dữ liệu quan hệ quản lý dễ dàng và có khả năng tự động hóa nhiều tác vụ, bao gồm bảo trì và sao lưu cơ sở dữ liệu. Dưới đây là cách bạn có thể xem thông tin về bảo trì và sao lưu trong AWS RDS:

#### Xem Thông Tin Bảo Trì

Để xem thông tin về bảo trì của một DB instance trong RDS, bạn có thể thực hiện các bước sau:

1. Đăng nhập vào AWS Management Console.

2. Chọn dịch vụ **Amazon RDS** trong danh sách các dịch vụ.

3. Trong bảng điều khiển RDS, chọn DB instance bạn quan tâm.

4. Trong trang quản lý DB instance, điều hướng đến tab **Maintenance & backups** (Bảo trì và sao lưu).

5. Tại đây, bạn sẽ thấy thông tin về lịch trình bảo trì, bao gồm các thời gian khi DB instance sẽ được tự động sao lưu và thực hiện công việc bảo trì. Bạn cũng có thể xem lịch sử các sự kiện bảo trì trước đó.

![Create a VPC](/images/4/00023.png?featherlight=false&width=90pc)

#### Xem Thông Tin Sao Lưu

Để xem thông tin về sao lưu của DB instance trong AWS RDS, làm theo các bước sau:

1. Đăng nhập vào AWS Management Console.

2. Chọn dịch vụ **Amazon RDS** trong danh sách các dịch vụ.

3. Trong bảng điều khiển RDS, chọn DB instance bạn muốn kiểm tra.

4. Trong trang quản lý DB instance, điều hướng đến tab **Maintenance & backups** (Bảo trì và sao lưu).

5. Tại đây, bạn có thể xem thông tin về sao lưu tự động và sao lưu thủ công. Bạn cũng có thể cấu hình và quản lý các thiết lập sao lưu.

Nhớ luôn tuân theo các quy tắc và chính sách liên quan khi thực hiện bất kỳ tác vụ nào trên AWS RDS để đảm bảo an toàn và bảo mật dữ liệu của bạn.

![Create a VPC](/images/4/00024.png?featherlight=false&width=90pc)
