---
title : "Bắt đầu với Amazon RDS"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Amazon Relational Database Service (Amazon RDS)

- **Amazon Relational Database Service (Amazon RDS)** là một dịch vụ quản lý cho phép bạn triển khai và quản lý các cơ sở dữ liệu quan hệ trên AWS.

- Amazon RDS là loại cơ sở dữ liệu xử lý giao dịch trực tuyến (OLTP).

- Trường hợp sử dụng chính là cơ sở dữ liệu giao dịch (thay vì cơ sở dữ liệu phân tích).

- Nó phù hợp nhất với các yêu cầu lưu trữ dữ liệu có cấu trúc và quan hệ.

- Nó nhằm mục tiêu là một tùy chọn thay thế dễ dàng cho các instance trên nơi làm việc truyền thống của cùng loại cơ sở dữ liệu.

- Các bản sao lưu tự động và việc vá lỗ được thực hiện trong các khung giờ bảo trì được xác định bởi khách hàng.

- Việc mở rộng, sao chép và tính sẵn có chỉ cần một nút nhấn.

![Create a VPC](/images/0001.png?featherlight=false&width=90pc)

- Amazon RDS hỗ trợ các hệ thống cơ sở dữ liệu sau:

  - Amazon Aurora.
  - MySQL.
  - MariaDB.
  - Oracle.
  - SQL Server.
  - PostgreSQL.

- RDS là một dịch vụ được quản lý và bạn không có quyền truy cập vào máy chủ EC2 cơ bản (không có quyền truy cập root).

- Ngoại lệ đối với quy tắc trên là **Amazon RDS Custom**, cho phép truy cập vào hệ điều hành cơ bản. Điều này mới và có sẵn cho một số DB Engine giới hạn và không xuất hiện trong bài kiểm tra (exam) hiện tại.

- Dịch vụ quản lý Amazon RDS bao gồm các tính năng sau:

  - Bảo mật và vá lỗ cho các DB instances.
  - Sao lưu tự động cho các DB instances.
  - Cập nhật phần mềm cho DB engine.
  - Mở rộng dễ dàng cho lưu trữ và tính toán.
  - Tùy chọn Multi-AZ với sao chép đồng bộ.
  - Tự động chuyển giao cho tùy chọn Multi-AZ.
  - Tùy chọn sao chép đọc cho các tải công việc nặng đọc.
  - Một DB instance là môi trường cơ sở dữ liệu trong đám mây với tài nguyên tính toán và lưu trữ mà bạn chỉ định.

- Các DB instance được truy cập thông qua các điểm cuối (endpoints).

- Các điểm cuối có thể được truy xuất thông qua mô tả DB instance trong AWS Management Console, API DescribeDBInstances hoặc lệnh describe-db-instances.

- Mặc định, khách hàng được phép có tối đa 40 DB instances Amazon RDS (chỉ có 10 trong số này có thể là Oracle hoặc MS SQL trừ khi bạn có giấy phép riêng của mình).

- Cửa sổ bảo trì được cấu hình để cho phép thực hiện các sửa đổi DB instances như mở rộng và cài đặt phần mềm vá (một số hoạt động yêu cầu DB instance phải tắt một thời gian ngắn).

- Bạn có thể xác định cửa sổ bảo trì hoặc AWS sẽ lên lịch cho một cửa sổ 30 phút.

- Xác thực tích hợp với Windows chỉ hoạt động với SQL khi sử dụng các miền được tạo bằng dịch vụ thư mục AWS - cần thiết lập một mối tin tưởng với thư mục AD trên nơi làm việc truyền thống.


#### Sự kiện và Thông báo:

- Amazon RDS sử dụng AWS SNS để gửi sự kiện RDS qua thông báo SNS.
- Bạn có thể sử dụng các cuộc gọi API đến dịch vụ Amazon RDS để liệt kê các sự kiện RDS trong 14 ngày qua (API DescribeEvents).
- Bạn có thể xem các sự kiện trong 14 ngày qua bằng dòng lệnh CLI.
- Sử dụng AWS Console, bạn chỉ có thể xem các sự kiện RDS trong 1 ngày qua.

####  Sử Dụng, Các Lựa Chọn Thay Thế và Anti-Patterns

Bảng dưới đây cung cấp hướng dẫn về khi nào nên sử dụng RDS và một số dịch vụ lưu trữ dữ liệu cơ sở dữ liệu AWS khác:

**Dịch Vụ Lưu Trữ Dữ Liệu** | **Khi Nào Sử Dụng**
------------------------|---------------------------
Cơ Sở Dữ Liệu trên EC2 | Kiểm soát tối đa về cơ sở dữ liệu, ưa chuộng DB không có sẵn trên RDS
Amazon RDS | Cần cơ sở dữ liệu quan hệ truyền thống cho giao dịch trực tiếp (OLTP), dữ liệu của bạn được hình thành và có cấu trúc tốt, ứng dụng hiện có yêu cầu RDBMS
Amazon DynamoDB | Dữ liệu cặp tên/giá trị hoặc cấu trúc dữ liệu không dự đoán được, hiệu suất trong bộ nhớ với tính bền vững, nhu cầu I/O cao, mở rộng một cách động
Amazon RedShift | Khối lượng lớn dữ liệu, chủ yếu là tải làm phân tích trực quan (OLAP)
Amazon Neptune | Mối quan hệ giữa các đối tượng là một phần quan trọng của giá trị dữ liệu
Amazon Elasticache | Lưu trữ tạm thời nhanh cho lượng dữ liệu nhỏ, dữ liệu biến động cao
Amazon S3 | Đối tượng nhị phân lớn (BLOBs), các trang web tĩnh

**Tùy Chọn Thay Thế cho Amazon RDS:**

Nếu trường hợp sử dụng của bạn không được hỗ trợ trên RDS, bạn có thể chạy cơ sở dữ liệu trên Amazon EC2.

Xem xét những điểm sau khi xem xét DB trên EC2:

- Bạn có thể chạy bất kỳ cơ sở dữ liệu nào bạn thích với sự kiểm soát đầy đủ và tính linh hoạt tối đa.
- Bạn phải quản lý mọi thứ như sao lưu, dự phòng, cập nhật và mở rộng.
- Lựa chọn tốt nếu bạn cần một cơ sở dữ liệu mà RDS chưa hỗ trợ, chẳng hạn như IBM DB2 hoặc SAP HANA.
- Lựa chọn tốt nếu việc di chuyển sang cơ sở dữ liệu do AWS quản lý không khả thi.

**Anti-Patterns:**

Anti-patterns là những mẫu thiết kế hoặc phát triển cụ thể trong kiến trúc hoặc phát triển mà được xem xét là không tốt hoặc thực hành không tối ưu - tức là có thể có một dịch vụ hoặc phương pháp tốt hơn để đạt được kết quả tốt nhất.

Bảng dưới đây mô tả các yêu cầu không phù hợp cho RDS:

**Yêu Cầu** | **Dịch Vụ Thích Hợp Hơn**
----------------|---------------------------
Nhiều đối tượng nhị phân lớn (BLOBs) | S3
Khả năng tự động mở rộng | DynamoDB
Cấu trúc Dữ Liệu Tên/Giá Trị | DynamoDB
Dữ liệu không có cấu trúc hoặc không dự đoán được | DynamoDB
Các nền tảng cơ sở dữ liệu khác như IBM DB2 hoặc SAP HANA | EC2
Kiểm soát hoàn toàn trên cơ sở dữ liệu | EC2

**Mã Hóa:**

Bạn có thể mã hóa các trường hợp và bản chụp Amazon RDS của bạn khi nghỉ bằng cách bật tùy chọn mã hóa cho trường hợp Amazon RDS DB của bạn.

Mã hóa khi nghỉ được hỗ trợ cho tất cả các loại DB và sử dụng AWS KMS.

Khi sử dụng mã hóa khi nghỉ, các yếu tố sau cũng được mã hóa:

- Tất cả bản chụp DB.
- Sao lưu.
- Lưu trữ trường hợp DB.
- Read Replicas.

Bạn không thể mã hóa một DB hiện có, bạn cần tạo một bản chụp, sao chép nó, mã hóa bản sao, sau đó xây dựng một DB đã được mã hóa từ bản chụp.

Dữ liệu được mã hóa khi nghỉ bao gồm lưu trữ cơ bản cho một trường hợp DB, sao lưu tự động của nó, Read Replicas và các bản chụp.

Một bản sao Read của một trường hợp RDS được mã hóa cũng bằng cách sử dụng cùng một khóa với trường hợp chính khi cả hai ở trong cùng một AZ.

Nếu trường hợp chính và Read Replica ở trong các AZ khác nhau, bạn sẽ mã hóa bằng cách sử dụng khóa mã hóa cho AZ đó.

Bạn không thể có một bản sao Read đã mã hóa của một trường hợp DB chưa mã hóa hoặc một bản sao Read chưa mã hóa của một trường hợp DB đã mã hóa.

Mã hóa/giải mã được xử lý một cách trong suốt.

RDS hỗ trợ mã hóa SSL giữa các ứng dụng và trường hợp DB RDS.

RDS tạo chứng chỉ cho trường hợp.

**Nhóm DB Subnet group:**

Nhóm DB Subnet group là một tập hợp các subnet group (thường là riêng tư) mà bạn tạo trong một VPC và sau đó bạn chỉ định cho các trường hợp DB của bạn.

Mỗi nhóm DB Subnet group nên có các subnet group trong ít nhất hai AZ khả dụng trong một AZ cụ thể.

Đề xuất cấu hình một nhóm subnet group với các subnet group trong mỗi AZ (thậm chí cho các trường hợp độc lập).

Trong quá trình tạo trường hợp RDS, bạn có thể chọn nhóm DB Subnet group và AZ trong nhóm để đặt trường hợp RDS DB vào đó.

Bạn không thể chọn địa chỉ IP trong subnet group được cấp.

**Thanh Toán và Cung Cấp**

AWS tính phí cho:

- Giờ trường hợp DB (giờ phần làm tròn lên giờ đầy đủ).
- Lưu trữ GB/tháng.
- Yêu cầu I/O/tháng - cho lưu trữ từ tính.
- Provisioned IOPS/tháng - cho RDS SSD IOPS được cung cấp.
- Truyền dữ liệu ra ngoài.
- Lưu trữ sao lưu (sao lưu DB và bản chụp thủ công).

Lưu trữ sao lưu cho sao lưu tự động RDS là miễn phí cho đến kích thước ổ EBS đã được cung cấp.

Tuy nhiên, AWS sao chép dữ liệu qua nhiều AZ và do đó bạn phải trả tiền cho không gian lưu trữ thêm trên S3.

Đối với đa-AZ, bạn phải trả tiền cho:

- Giờ DB đa-AZ.
- Lưu trữ được cung cấp.
- Ghi I/O hai lần.
- Đối với đa-AZ, bạn không phải trả phí cho truyền dữ liệu DB trong quá trình sao chép từ trường hợp chính sang trạng thái đứng.

Giấy phép Oracle và Microsoft SQL được bao gồm, hoặc bạn có thể tự mang (BYO).

Giá ứng dụng dự phòng và giá ứng dụng dự phòng có sẵn.

Giấy phép ứng dụng dự phòng được định nghĩa dựa trên các thuộc tính sau đây không được thay đổi:

- DB engine.
- Lớp trường hợp DB.
- Loại triển khai (độc lập, đa-AZ).
- Mô hình giấy phép.
- AZ.

Giấy phép ứng dụng dự phòng:

- Có thể di chuyển giữa các AZ trong cùng một AZ.
- Có sẵn cho các triển khai đa-AZ.
- Có thể áp dụng cho các bản sao Read nếu lớp trường hợp DB và AZ giống nhau.

Tính khả năng mở rộng được đạt được thông qua việc thay đổi lớp trường hợp để tính toán và điều chỉnh dung lượng lưu trữ bổ sung.

**Khả Năng Mở Rộng**

Bạn chỉ có thể mở rộng RDS lên (tính toán và lưu trữ).

Bạn không thể giảm lưu trữ được cấp cho trường hợp RDS.

Bạn có thể mở rộng lưu trữ và thay đổi loại lưu trữ cho tất cả các DB engine ngoại trừ MS SQL.

Đối với MS SQL, giải pháp tạm thời là tạo một trường hợp mới từ một bản chụp có cấu hình mới.

Việc mở rộng lưu trữ có thể xảy ra trong khi trường hợp RDS đang chạy mà không gây ra sự cố, tuy nhiên có thể có sự suy giảm hiệu suất.

Việc mở rộng tính toán sẽ gây ra thời gian ngừng hoạt động.

Bạn có thể chọn để thay đổi có hiệu lực ngay lập tức, tuy nhiên mặc định là trong cửa sổ bảo trì.

Yêu cầu mở rộng được áp dụng trong cửa sổ bảo trì được chỉ định trừ khi sử dụng "áp dụng ngay lập tức".

Tất cả các loại DB RDS hỗ trợ kích thước DB tối đa là 64 TiB ngoại trừ Microsoft SQL Server (16 TiB).

**Hiệu Năng**

Amazon RDS sử dụng ổ đĩa EBS (không sử dụng lưu trữ trường hợp) cho lưu trữ DB và log.

Có ba loại lưu trữ có sẵn: Mục Đích Chung (SSD), Provisioned IOPS (SSD), và từ tính (Magnetic).

Mục Đích Chung (SSD):

- Sử dụng cho các tải công việc cơ sở dữ liệu có nhu cầu I/O trung bình.
- Hiệu quả về chi phí.
- Còn được gọi là gp2.
- 3 IOPS/GB.
- Burst lên đến 3000 IOPS.

Provisioned IOPS (SSD):

- Sử dụng cho công việc có yêu cầu I/O cao.
- Độ trễ thấp và I/O đều đặn.
- Số IOPS được chỉ định bởi người dùng (xem bảng bên dưới).
- Đối với lưu trữ IOPS được cấp, bảng dưới đây cho thấy phạm vi IOPS được cấp và phạm vi kích thước lưu trữ cho mỗi động cơ cơ sở dữ liệu.

**Tên Động Cơ Cơ Sở Dữ Liệu** | **Phạm Vi IOPS Được Cấp** | **Phạm Vi Kích Thước Lưu Trữ**
-----------------------|--------------------------|--------------------------
MariaDB | 1.000-80.000 IOPS | 100 GiB-64TiB
SQL Server | 1.000-64.000 IOPS | 20 GiB-16TiB
MySQL | 1.000-80.000 IOPS | 100 GiB-64TiB
Oracle | 1.000-256.000 IOPS | 100 GiB-64TiB
PostgreSQL | 1.000-80.000 IOPS | 100 GiB-64TiB

Từ Tính (Magnetic):

- Không còn được khuyến nghị nữa, có sẵn cho tích hợp ngược.
- Không cho phép bạn mở rộng lưu trữ khi sử dụng động cơ cơ sở dữ liệu SQL Server.
- Không hỗ trợ ổ đĩa co giãn.
- Giới hạn tối đa 4 TiB.
- Giới hạn tối đa 1.000 IOPS.


####  Multi-AZ và Read Replicas

Multi-AZ và Read Replicas được sử dụng để đảm bảo tính sẵn có cao, khả năng chịu lỗi và mở rộng hiệu suất.

Bảng dưới đây so sánh các triển khai Multi-AZ và Read Replicas:

| Multi-AZ Deployments (Triển khai Multi-AZ) | Read Replicas (Read Replicas) |
|--------------------------------------------|--------------------------------|
| Replication đồng bộ - bền vững cao         | Replication không đồng bộ - mở rộng cao |
| Chỉ có cơ sở dữ liệu trên primary instance là hoạt động | Tất cả read replicas đều có thể truy cập và được sử dụng để mở rộng đọc |
| Sao lưu tự động được thực hiện từ standby | Không có sao lưu được cấu hình mặc định |
| Luôn luôn bao gồm hai vùng khả dụng trong một AZ duy nhất | Có thể nằm trong một Availability Zone, Cross-AZ hoặc Cross-Region |
| Các instance của cơ sở dữ liệu được nâng cấp trên primary | Việc nâng cấp instance của cơ sở dữ liệu độc lập với instance nguồn |
| Tự động chuyển đổi sang standby khi phát hiện sự cố | Có thể được thăng cấp thủ công thành một instance cơ sở dữ liệu độc lập |


####  Multi-AZ

- Multi-AZ RDS tạo một bản sao ở AZ (AZ) khác và sao chép đồng bộ đến đó (chỉ dành cho DR).
- Có tùy chọn để chọn Multi-AZ trong quá trình tạo.
- AWS khuyên nên sử dụng lưu trữ provisioned IOPS cho các trường hợp DB RDS Multi-AZ.
- Mỗi AZ chạy trên cơ sở hạ tầng riêng biệt, độc lập về vật lý và được thiết kế để đảm bảo tính tin cậy cao.
- Bạn không thể chọn AZ nào trong AZ sẽ được chọn để tạo bản sao DB dự phòng.
- Bạn có thể xem AZ nào mà bản sao DB dự phòng được tạo ra.
- Failover có thể được kích hoạt trong các trường hợp sau:
  - Mất AZ chính hoặc lỗi trạng thái DB chính.
  - Mất kết nối mạng với DB chính.
  - Lỗi đơn vị tính toán (EC2) trên DB chính.
  - Lỗi đơn vị lưu trữ (EBS) trên DB chính.
  - Thay đổi DB chính.
  - Cập nhật hệ điều hành trên DB chính.
  - Failover thủ công (khởi động lại với lựa chọn failover được chọn trên DB chính).
- Trong quá trình failover, RDS tự động cập nhật cấu hình (bao gồm điểm cuối DNS) để sử dụng nút thứ hai.
- Tùy thuộc vào lớp instance, có thể mất từ 1 đến vài phút để failover đến bản sao DB dự phòng.
- Được đề xuất triển khai việc thử lại kết nối DB trong ứng dụng của bạn.Read Replicas 
- Được đề xuất sử dụng điểm cuối thay vì địa chỉ IP để chỉ định ứng dụng đến DB RDS.
- Cách để khởi đầu failover của DB RDS dự phòng thủ công là khởi động lại và chọn tùy chọn failover.
- Yêu cầu khởi động lại DB để thay đổi cấu hình nhóm tham số DB hoặc khi bạn thay đổi tham số DB tĩnh.
- Nhóm tham số DB là một hộp chứa cấu hình cho động cơ DB.
- Bạn sẽ nhận được thông báo từ sự kiện DB khi failover xảy ra.
- Bản sao DB thứ cấp trong cấu hình Multi-AZ không thể được sử dụng như một nút đọc độc lập (đọc hoặc ghi).
- Không tính phí cho việc truyền dữ liệu giữa các trường hợp RDS chính và dự phòng.
- Các nâng cấp hệ thống như việc cập nhật hệ điều hành, thay đổi kích thước DB Instance và các nâng cấp hệ thống được áp dụng trước tiên trên replicas trước khi failover và sửa đổi DB Instance khác.
- Trong cấu hình Multi-AZ, các bản snapshot và sao lưu tự động được thực hiện trên replicas để tránh tạm dừng I/O trên trường hợp DB chính.

#### Hỗ trợ Read Replicas  cho Multi-AZ:

- Amazon RDS Read Replicas  cho MySQL, MariaDB, PostgreSQL và Oracle hỗ trợ các triển khai Multi-AZ.
- Kết hợp Read Replicas  với Multi-AZ cho phép bạn xây dựng một chiến lược phục hồi thảm họa chắc chắn và đơn giản hóa quá trình nâng cấp động cơ cơ sở dữ liệu của bạn.
- Một read snapshot ở một AZ khác so với cơ sở dữ liệu nguồn có thể được sử dụng như một cơ sở dữ liệu dự phòng và được thúc đẩy để trở thành cơ sở dữ liệu sản xuất mới trong trường hợp có sự cố vùng.
- Điều này cho phép bạn mở rộng khả năng đọc đồng thời cũng như có Multi-AZ cho DR.

#### Quy trình thực hiện các hoạt động bảo trì như sau:

1. Thực hiện các hoạt động trên replicas.
2. Thúc đẩy replicas để trở thành primary .
3. Thực hiện các hoạt động trên replicas mới (primary  bị hạ cấp).

##### Bạn có thể nâng cấp thủ công một DB instance lên phiên bản động cơ DB được hỗ trợ từ AWS Console.

- Theo mặc định, nâng cấp sẽ có hiệu lực trong cửa sổ bảo trì tiếp theo.
- Bạn có thể tùy chọn buộc nâng cấp ngay lập tức.
- Trong các triển khai Multi-AZ, nâng cấp phiên bản sẽ được thực hiện trên cả primary  và replicas cùng một lúc, gây ra sự cố cho cả hai DB Instance.
- Đảm bảo các SG và NACL sẽ cho phép máy chủ ứng dụng của bạn giao tiếp với cả primary và replicas.

#### Read Replicas
Read Replicas được sử dụng cho các cơ sở dữ liệu có tải đọc nhiều và sao chép là không đồng bộ.

Read Replicas được sử dụng để chia sẻ và giảm tải công việc làm.

Read Replicas cung cấp khả năng khôi phục dữ liệu ở chế độ chỉ đọc.

Read Replicas được tạo ra từ một bản snapshot của bản chính.

Phải bật tính năng sao lưu tự động trên bản chính (thời gian lưu trữ > 0).

Chỉ được hỗ trợ cho các động cơ lưu trữ cơ sở dữ liệu giao dịch (InnoDB không phải InnoDB).

Read Replicas có sẵn cho MySQL, PostgreSQL, MariaDB, Oracle, Aurora và SQL Server.

Đối với các động cơ cơ sở dữ liệu MySQL, MariaDB, PostgreSQL và Oracle, Amazon RDS tạo ra một bản thứ hai sử dụng bản snapshot của bản chính.

Sau đó, nó sử dụng sao chép không đồng bộ tự nhiên của các động cơ để cập nhật Read Replicas mỗi khi có sự thay đổi đối với bản chính.

Amazon Aurora sử dụng một lớp lưu trữ ảo được hỗ trợ bởi SSD, được thiết kế đặc biệt cho tải công việc làm cơ sở dữ liệu.

Bạn có thể tạo snapshot của các Read Replicas PostgreSQL nhưng không thể bật sao lưu tự động.

Bạn có thể bật sao lưu tự động trên các Read Replicas MySQL và MariaDB.

Bạn có thể bật viết trên các Read Replicas MySQL và MariaDB.

Bạn có thể có tối đa 5 Read Replicas của một cơ sở dữ liệu sản xuất.

Bạn không thể có nhiều hơn bốn bản sao tham gia trong chuỗi sao chép.

Bạn có thể có Read Replicas của các Read Replicas cho MySQL và MariaDB nhưng không cho PostgreSQL.

Cấu hình Read Replicas có thể được thiết lập từ Giao diện AWS hoặc API.

Bạn có thể chỉ định khu vực ra đời của Read Replicas.

Loại lưu trữ và lớp bản chính của các Read Replicas có thể khác nhau so với nguồn, nhưng tính toán phải ít nhất là hiệu suất của nguồn.

Bạn không thể thay đổi động cơ cơ sở dữ liệu.

Trong trường hợp mất kết nối đa khu vực, các Read Replicas được chuyển sang bản chính mới.

Read Replicas phải được xóa một cách tường tận.

Nếu một bản chính bị xóa mà không xóa các bản sao, mỗi bản sao trở thành một bản chính độc lập tại một khu vực một khu vực.

Bạn có thể thăng cấp một Read Replicas thành bản chính.

Việc thăng cấp Read Replicas mất vài phút.

Các Read Replicas được thăng cấp giữ lại:

- Thời gian lưu trữ sao lưu.
- Cửa sổ sao lưu.
- Nhóm tham số cơ sở dữ liệu.
- Các Read Replicas hiện có vẫn hoạt động như bình thường.

Mỗi Read Replicas có một điểm cuối DNS riêng.

Các Read Replicas có thể được bật đa khu vực và bạn có thể tạo các Read Replicas từ các cơ sở dữ liệu nguồn đa khu vực.

Các Read Replicas có thể nằm ở khu vực khác (sử dụng sao chép không đồng bộ).

Cấu hình này có thể được sử dụng để tập trung dữ liệu từ các khu vực khác nhau cho mục đích phân tích.

####  DB Snapshots
DB Snapshots là các tình huống do người dùng khởi tạo và cho phép bạn sao lưu DB instance của bạn ở trong một trạng thái xác định bất kỳ thường xuyên như bạn muốn, sau đó khôi phục về trạng thái cụ thể đó.

Không thể sử dụng để khôi phục tại điểm thời gian cụ thể.

Các Snapshot được lưu trữ trên S3.

Snapshot sẽ tồn tại trên S3 cho đến khi bị xóa bằng cách thủ công.

Sao lưu được thực hiện trong một khoảng thời gian đã được định.

I/O tạm ngừng trong thời gian sao lưu và có thể làm tăng độ trễ (áp dụng cho RDS chỉ có một vùng khu vực).

Các DB Snapshot được thực hiện bằng cách thủ công sẽ được lưu trữ ngay cả sau khi DB instance RDS bị xóa.

Các DB được khôi phục sẽ luôn là một DB instance RDS mới với một điểm cuối DNS mới.

Có thể khôi phục lên đến 5 phút trước.

Chỉ có các tham số DB mặc định và các nhóm bảo mật được khôi phục - bạn phải liên kết các tham số DB và SG khác một cách thủ công.

Nên nên chụp một Snapshot cuối cùng trước khi xóa một DB instance RDS.

Snapshot có thể chia sẻ với các tài khoản AWS khác.

####  Các phương pháp Có sẵn cho Sự có sẵn cao cấp cho Cơ sở dữ liệu
Nếu có thể, hãy chọn DynamoDB thay vì RDS vì tính chịu lỗi tích hợp.

Nếu không thể sử dụng DynamoDB, hãy chọn Aurora vì tính dự phòng và tính năng khôi phục tự động.

Nếu không thể sử dụng Aurora, hãy chọn Multi-AZ RDS.

Các sao lưu RDS thường xuyên có thể bảo vệ trước sự hỏng hoặc sự cố dữ liệu và chúng không ảnh hưởng đến hiệu suất triển khai Multi-AZ.

Sao lưu dự phòng khu vực cũng là một lựa chọn nhưng không được đảm bảo sẽ nhất quán mạnh mẽ.

Nếu cơ sở dữ liệu chạy trên EC2, bạn phải tự thiết kế tính sẵn có.

####  Di chuyển dữ liệu
Dịch vụ Di chuyển Cơ sở dữ liệu AWS giúp bạn di chuyển cơ sở dữ liệu vào AWS một cách nhanh chóng và an toàn.

Sử dụng kèm với Công cụ Chuyển đổi Schema (SCT) để di chuyển cơ sở dữ liệu vào RDS AWS hoặc cơ sở dữ liệu dựa trên EC2.

Cơ sở dữ liệu nguồn vẫn hoạt động hoàn toàn trong quá trình di chuyển, giảm thiểu thời gian chết cho các ứng dụng phụ thuộc vào cơ sở dữ liệu.

Dịch vụ Di chuyển Cơ sở dữ liệu AWS có thể di chuyển dữ liệu của bạn tới và từ hầu hết các cơ sở dữ liệu thương mại và nguồn mở phổ biến.

Công cụ Chuyển đổi Schema có thể sao chép các schema cơ sở dữ liệu cho các di chuyển đồng nhất (cùng loại cơ sở dữ liệu) và chuyển đổi các schema cho các di chuyển không đồng nhất (khác loại cơ sở dữ liệu).

DMS được sử dụng cho các chuyển đổi nhỏ hơn, đơn giản hơn và hỗ trợ MongoDB và DynamoDB.

SCT được sử dụng cho các bộ dữ liệu lớn hơn, phức tạp hơn như các kho dữ liệu.

DMS có chức năng sao chép từ nơi trên cơ sở vào AWS hoặc vào Snowball hoặc S3.

#### Theo dõi, Ghi log và Báo cáo
Bạn có thể sử dụng các công cụ theo dõi tự động sau để theo dõi Amazon RDS và báo cáo khi có điều gì đó sai:

- Sự kiện Amazon RDS - Đăng ký sự kiện Amazon RDS để được thông báo khi có thay đổi với DB instance, DB snapshot, nhóm tham số DB hoặc nhóm bảo mật DB.
- Các tệp ghi cơ sở dữ liệu - Xem, tải xuống hoặc xem các tệp ghi cơ sở dữ liệu bằng cách sử dụng giao diện Amazon RDS hoặc các thao tác API Amazon RDS. Bạn cũng có thể truy vấn một số tệp ghi cơ sở dữ liệu được nạp vào các bảng cơ sở dữ liệu.
- Amazon RDS Enhanced Monitoring - Xem các số liệu thống kê thời gian thực cho hệ điều hành.
- Amazon RDS Performance Insights - Đánh giá tải trên cơ sở dữ liệu của bạn và xác định khi nào và ở đâu cần thực hiện.
- Amazon RDS Recommendations - Xem các khuyến nghị tự động cho các tài nguyên cơ sở dữ liệu, chẳng hạn như các DB instance, bản sao đọc và nhóm tham số DB.

Ngoài ra, Amazon RDS tích hợp với Amazon CloudWatch, Amazon EventBridge và AWS CloudTrail cho khả năng theo dõi bổ sung:

- Số liệu CloudWatch - Amazon RDS tự động gửi số liệu đến CloudWatch mỗi phút cho mỗi cơ sở dữ liệu hoạt động. Bạn không phải trả thêm phí cho số liệu Amazon RDS trong CloudWatch.
- Các cảnh báo CloudWatch - Bạn có thể theo dõi một số liệu Amazon RDS duy nhất trong một khoảng thời gian cụ thể. Sau đó, bạn có thể thực hiện một hoặc nhiều hành động dựa trên giá trị của số liệu đối với ngưỡng bạn đặt.
- Các tệp ghi CloudWatch - Hầu hết các động cơ DB cho phép bạn theo dõi, lưu trữ và truy cập các tệp ghi cơ sở dữ liệu của bạn trong CloudWatch Logs.
- Các sự kiện CloudWatch và Amazon EventBridge - Bạn có thể tự động hóa các dịch vụ AWS và phản ứng với các sự kiện hệ thống như vấn đề sẵn sàng ứng dụng hoặc thay đổi tài nguyên. Sự kiện từ các dịch vụ AWS được gửi đến CloudWatch Events và EventBridge gần như trong thời gian thực. Bạn có thể viết các quy tắc đơn giản để chỉ định các sự kiện mà bạn quan tâm và các hành động tự động nào nên thực hiện khi một sự kiện phù hợp với quy tắc.
- AWS CloudTrail - Bạn có thể xem một hồ sơ các hành động được thực hiện bởi người dùng, vai trò hoặc dịch vụ AWS trong Amazon RDS. CloudTrail ghi lại tất cả các cuộc gọi API cho Amazon RDS như sự kiện. Các cuộc gọi này bao gồm cuộc gọi từ giao diện Amazon RDS và từ các cuộc gọi mã vào các thao tác API Amazon RDS. Nếu bạn tạo một dãy, bạn có thể bật giao hàng liên tục của các sự kiện CloudTrail đến một xô Amazon S3, bao gồm các sự kiện cho Amazon RDS. Nếu bạn không cấu hình một dãy, bạn vẫn có thể xem các sự kiện gần đây nhất trong bảng điều khiển CloudTrail trong Lịch sử sự kiện.
