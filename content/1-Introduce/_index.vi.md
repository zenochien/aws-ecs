---
title : "Giới thiệu ECS"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
#### Amazon Elastic Container Service (ECS)

Amazon Elastic Container Service (Amazon ECS) là dịch vụ điều phối bộ chứa được quản lý toàn phần giúp bạn dễ dàng triển khai, quản lý và mở rộng quy mô ứng dụng trong bộ chứa. Là một dịch vụ được quản lý toàn phần, Amazon ECS được tích hợp sẵn các biện pháp thực hành tốt nhất về vận hành và cấu hình AWS. Nó được tích hợp với cả AWS và các công cụ của bên thứ ba, chẳng hạn như Docker và Docker của Amazon Elastic Container. Sự tích hợp này giúp các nhóm dễ dàng tập trung vào việc xây dựng ứng dụng hơn là môi trường. Bạn có thể chạy và thay đổi quy mô khối lượng công việc trong bộ chứa của mình trên các Khu vực AWS trên đám mây và tại chỗ mà không gặp phải sự phức tạp trong việc quản lý mặt phẳng điều khiển.

#### Thuật ngữ và thành phần của Amazon ECS

 Có ba lớp trong Amazon ECS:

- Capacity - Cơ sở hạ tầng nơi container của bạn chạy

- Controller - Triển khai và quản lý các ứng dụng chạy trên vùng chứa của bạn

- Provisioning  - Các công cụ mà bạn có thể sử dụng để giao tiếp với bộ lập lịch nhằm triển khai và quản lý các ứng dụng cũng như vùng chứa của mình

Sơ đồ sau đây hiển thị các lớp Amazon ECS.

![Tạo ECS](/images/1/1.png?featherlight=false&width=90pc)

#### Dung lượng Amazon ECS

 Dung lượng Amazon ECS là cơ sở hạ tầng nơi các container của bạn chạy. Sau đây là tổng quan về các tùy chọn công suất:

- Phiên bản Amazon EC2 trên đám mây AWS. Bạn chọn loại phiên bản, số lượng phiên bản và quản lý dung lượng.

- Serverless (AWS Fargate (Fargate)) trên đám mây AWS

- Fargate là công cụ tính toán không có máy chủ, trả tiền theo mức sử dụng. Với Fargate, bạn không cần quản lý máy chủ, xử lý việc lập kế hoạch công suất hoặc tách biệt khối lượng công việc của bộ chứa để bảo mật.

- On-premises virtual machines hoặc servers

- Amazon ECS Anywhere cung cấp hỗ trợ đăng ký một phiên bản bên ngoài như máy chủ tại chỗ hoặc máy ảo (VM) vào cụm Amazon ECS của bạn.

Dung lượng có thể được đặt trong bất kỳ tài nguyên AWS nào sau đây:


- Availability Zones

- Local Zones

- Wavelength Zones

- AWS Regions

- AWS Outposts

#### Amazon ECS controller

Bộ lập lịch Amazon ECS là phần mềm quản lý các ứng dụng của bạn.

#### Cung cấp Amazon ECS

Có nhiều tùy chọn để cung cấp Amazon ECS:

- AWS Management Console — Cung cấp giao diện web mà bạn có thể sử dụng để truy cập tài nguyên Amazon ECS của mình.

- AWS Command Line Interface (AWS CLI) — Cung cấp các lệnh cho một loạt dịch vụ AWS, bao gồm cả Amazon ECS. Nó được hỗ trợ trên Windows, Mac và Linux. Để biết thêm thông tin, hãy xem Giao diện dòng lệnh AWS

- AWS SDK — Cung cấp các API dành riêng cho ngôn ngữ và xử lý nhiều chi tiết kết nối. Chúng bao gồm tính toán chữ ký, xử lý các lần thử lại yêu cầu và xử lý lỗi. Để biết thêm thông tin, hãy xem AWS SDK

- Copilot — Cung cấp công cụ nguồn mở để các nhà phát triển xây dựng, phát hành và vận hành các ứng dụng được đóng gói sẵn sàng sản xuất trên Amazon ECS. Để biết thêm thông tin, hãy xem Copilot trên trang web GitHub.

- AWS CDK — Cung cấp khung phát triển phần mềm nguồn mở mà bạn có thể sử dụng để lập mô hình và cung cấp tài nguyên ứng dụng đám mây bằng các ngôn ngữ lập trình quen thuộc. AWS CDK cung cấp tài nguyên của bạn một cách an toàn, có thể lặp lại thông qua AWS CloudFormation.

#### Vòng đời ứng dụng

![Tạo ECS](/images/1/2.png?featherlight=false&width=90pc)

Bạn phải kiến ​​trúc các ứng dụng của mình để chúng có thể chạy trên các vùng chứa. Vùng chứa là một đơn vị phát triển phần mềm được tiêu chuẩn hóa chứa mọi thứ mà ứng dụng phần mềm của bạn yêu cầu để chạy. Điều này bao gồm mã có liên quan, thời gian chạy, công cụ hệ thống và thư viện hệ thống. Vùng chứa được tạo từ mẫu chỉ đọc được gọi là hình ảnh. Hình ảnh thường được xây dựng từ Dockerfile. Dockerfile là một tệp văn bản gốc chứa các hướng dẫn xây dựng vùng chứa. Sau khi được tạo, những hình ảnh này được lưu trữ trong sổ đăng ký như Amazon ECR nơi chúng có thể được tải xuống từ đó.

Sau khi tạo và lưu trữ hình ảnh, bạn tạo định nghĩa tác vụ Amazon ECS. Định nghĩa nhiệm vụ là một bản thiết kế chi tiết cho ứng dụng của bạn. Đó là một tệp văn bản ở định dạng JSON mô tả các tham số và một hoặc nhiều vùng chứa tạo thành ứng dụng của bạn. Ví dụ: bạn có thể sử dụng nó để chỉ định hình ảnh và tham số cho hệ điều hành, bộ chứa nào sẽ sử dụng, cổng nào sẽ mở cho ứng dụng của bạn và khối lượng dữ liệu nào sẽ sử dụng với bộ chứa trong tác vụ. Các tham số cụ thể có sẵn cho việc xác định nhiệm vụ của bạn tùy thuộc vào nhu cầu của ứng dụng cụ thể của bạn.

Sau khi xác định định nghĩa nhiệm vụ của mình, bạn triển khai nó dưới dạng dịch vụ hoặc nhiệm vụ trên cụm của mình. Cụm là một nhóm logic các nhiệm vụ hoặc dịch vụ chạy trên cơ sở hạ tầng năng lực được đăng ký vào một cụm.

Một tác vụ là sự khởi tạo của một định nghĩa tác vụ trong một cụm. Bạn có thể chạy một tác vụ độc lập hoặc có thể chạy một tác vụ như một phần của dịch vụ. Bạn có thể sử dụng dịch vụ Amazon ECS để chạy và duy trì đồng thời số lượng tác vụ mong muốn trong một cụm Amazon ECS. Nó như thế nào hoạt động là nếu bất kỳ tác vụ nào của bạn không thành công hoặc dừng vì bất kỳ lý do gì, bộ lập lịch dịch vụ Amazon ECS sẽ khởi chạy một phiên bản khác dựa trên định nghĩa tác vụ của bạn. Nó thực hiện điều này để thay thế nó và do đó duy trì số lượng nhiệm vụ mong muốn của bạn trong dịch vụ.

Tác nhân bộ chứa chạy trên từng phiên bản bộ chứa trong cụm Amazon ECS. Tác nhân gửi thông tin về các tác vụ đang chạy hiện tại và mức sử dụng tài nguyên của bộ chứa của bạn tới Amazon ECS. Nó bắt đầu và dừng các tác vụ bất cứ khi nào nó nhận được yêu cầu từ Amazon ECS.

Sau khi triển khai tác vụ hoặc dịch vụ, bạn có thể sử dụng bất kỳ công cụ nào sau đây để giám sát việc triển khai và ứng dụng của mình:

- CloudWatch

- Giám sát thời gian chạy