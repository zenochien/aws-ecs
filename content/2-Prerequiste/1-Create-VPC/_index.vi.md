<!-- ---
title : "Tạo VPC"
date :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Để tạo một VPC, các subnet và các tài nguyên VPC khác bằng cách sử dụng giao diện điều khiển

Sử dụng thủ tục sau để tạo một VPC cùng với các tài nguyên VPC bổ sung mà bạn cần để chạy ứng dụng của bạn, chẳng hạn như các subnet, route table, Internet gateway và cổng NAT. Ví dụ về cấu hình VPC, xem trong các ví dụ VPC.

1. Mở giao diện điều khiển Amazon VPC tại [đây](https://console.aws.amazon.com/vpc/).

![Create a VPC](/images/1/0001.png?featherlight=false&width=90pc)

2. Trên bảng điều khiển VPC, chọn **Create VPC.**

3. Cho phần **Resources to create,** chọn **VPC and more.**

![Create a VPC](/images/1/0002.png?featherlight=false&width=90pc)

4. Giữ nguyên tùy chọn **Name tag auto-generation** để tạo các nhãn Tên cho các tài nguyên VPC, hoặc bỏ chọn nó để cung cấp các nhãn Tên của bạn cho các tài nguyên VPC.

5. Đối với dãy địa chỉ IPv4 CIDR, nhập một dãy địa chỉ IPv4 cho VPC. VPC phải có một dãy địa chỉ IPv4.

6. (Tùy chọn) Để hỗ trợ lưu lượng IPv6, chọn **IPv6 CIDR block,** dãy IPv6 do Amazon cung cấp.

7. Chọn một tùy chọn Tenancy. Tùy chọn này xác định xem các máy EC2 mà bạn khởi chạy vào VPC sẽ chạy trên phần cứng được chia sẻ với các tài khoản AWS khác hay trên phần cứng được dành riêng cho bạn. Nếu bạn chọn Tenancy của VPC là **Default,** các máy EC2 được khởi chạy vào VPC này sẽ sử dụng thuộc tính Tenancy được chỉ định khi bạn khởi chạy máy -- Xem thêm thông tin trong Hướng dẫn người dùng Amazon EC2 cho các phiên bản Linux. Nếu bạn chọn Tenancy của VPC là **Dedicated,** các máy sẽ luôn chạy như Các Máy Dành Riêng trên phần cứng được dành riêng cho bạn. Nếu bạn đang sử dụng AWS Outposts, Outpost của bạn cần kết nối riêng tư; bạn phải sử dụng Tenancy mặc định.

8. Đối với **Number of Availability Zones (AZs),** chúng tôi đề xuất bạn cung cấp các subnet trong ít nhất hai Availability Zones cho môi trường sản xuất. Để chọn AZ cho các subnet của bạn, mở rộng **Customize AZs.** Nếu không, hãy để AWS chọn giúp bạn.

![Create a VPC](/images/1/0003.png?featherlight=false&width=90pc)

9. Để cấu hình các subnet của bạn, chọn các giá trị cho **Number of public subnets** và **Number of private subnets.** Để chọn các dãy địa chỉ IP cho các subnet của bạn, mở rộng **Customize subnets CIDR blocks.** Nếu không, hãy để AWS chọn giúp bạn.

10. (Tùy chọn) Nếu các tài nguyên trong một subnet riêng cần truy cập Internet công cộng qua IPv4, cho cổng NAT gateways, chọn số AZ mà bạn muốn tạo NAT gateways. Trong môi trường sản xuất, chúng tôi đề xuất bạn triển khai một NAT gateway trong mỗi AZ với các tài nguyên cần truy cập Internet công cộng. Lưu ý rằng có một chi phí liên quan đến NAT gateways. Để biết thêm thông tin, xem Giá cả.

11. (Tùy chọn) Nếu các tài nguyên trong một subnet riêng cần truy cập Internet công cộng qua IPv6, cho cổng Egress only internet gateway, chọn **Yes.**

12. (Tùy chọn) Nếu bạn cần truy cập Amazon S3 trực tiếp từ VPC của bạn, chọn **VPC endpoints, S3 Gateway.** Điều này tạo ra một điểm cuối VPC cổng cho Amazon S3. Để biết thêm thông tin, xem Cổng kết nối VPC trong Hướng dẫn AWS PrivateLink.

13. (Tùy chọn) Đối với tùy chọn DNS, cả hai tùy chọn về giải quyết tên miền đều được kích hoạt mặc định. Nếu mặc định không đáp ứng nhu cầu của bạn, bạn có thể vô hiệu hóa các tùy chọn này.

14. (Tùy chọn) Để thêm một nhãn cho VPC của bạn, mở rộng **Additional tags,** chọn **Add new tag,** và nhập một khóa nhãn và giá trị nhãn.

15. Trong bảng **Preview,** bạn có thể xem biểu đồ quan hệ giữa các tài nguyên VPC mà bạn đã cấu hình. Các đường mass nét đại diện cho mối quan hệ giữa các tài nguyên. Đường nét đứt đoạn đại diện cho lưu lượng mạng đến NAT gateways, cổng Internet và điểm cuối cổng. Sau khi bạn tạo VPC, bạn có thể xem các tài nguyên trong VPC của mình ở định dạng này bất kỳ lúc nào bằng cách sử dụng tab Bản đồ tài nguyên. Để biết thêm thông tin, xem Biểu đồ các tài nguyên trong VPC của bạn.

16. Khi bạn hoàn tất cấu hình VPC của mình, hãy chọn **Create VPC.**

![Create a VPC](/images/1/0004.png?featherlight=false&width=90pc)

![Create a VPC](/images/1/0005.png?featherlight=false&width=90pc)


#### Thay đổi thuộc tính địa chỉ Public IPv4 cho subnet của bạn
Mặc định, subnet không mặc định có thuộc tính địa chỉ Public IPv4 được đặt thành sai (false), và subnet mặc định có thuộc tính này được đặt thành đúng (true). Một ngoại lệ là một subnet không mặc định được tạo ra bằng cách sử dụng hướng dẫn của Amazon EC2 trong trình tạo thể hiện - trình tạo sẽ đặt thuộc tính này thành đúng. Bạn có thể thay đổi thuộc tính này bằng cách sử dụng giao diện Amazon VPC.

**Để thay đổi hành vi địa chỉ Public IPv4 của subnet của bạn**
1. Mở giao diện Amazon VPC tại [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).

2. Trong bảng điều hướng, chọn subnet (Subnets).

![Create a VPC](/images/1/0006.png?featherlight=false&width=90pc)

3. Chọn subnet của bạn và chọn Hành động (Actions), chỉnh sửa cài đặt subnet (Edit subnet settings).

![Create a VPC](/images/1/0007.png?featherlight=false&width=90pc)

4. Ô kiểm Bật tự động gán địa chỉ Public IPv4 (Enable auto-assign public IPv4 address), nếu được chọn, sẽ yêu cầu một địa chỉ Public IPv4 cho tất cả các thể hiện được khởi chạy vào subnet được chọn. Chọn hoặc bỏ chọn ô kiểm theo yêu cầu, sau đó chọn Lưu (Save).

![Create a VPC](/images/1/0008.png?featherlight=false&width=90pc)
 -->
