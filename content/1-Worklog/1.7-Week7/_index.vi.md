---
title: "Nhật ký Tuần 7"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu Tuần 7

- Hiểu và áp dụng các phương pháp **bảo mật dữ liệu và hệ thống** trong AWS.
- Quản lý, cấu hình và tối ưu **Amazon VPC (Virtual Private Cloud)** cho hệ thống mạng riêng.
- Học cách **sao lưu, phục hồi và triển khai kế hoạch khôi phục thảm họa (Disaster Recovery)** với các công cụ AWS.

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động                                                                                                                                                                                                                                                                                     | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo                 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------- | ---------------------------------- |
| 1    | - Tìm hiểu về **AWS Security & Encryption** <br> + Làm quen với **AWS KMS (Key Management Service)** để mã hóa dữ liệu <br> + Khám phá **CloudHSM** cho bảo mật khóa phần cứng (Hardware Security Module) <br> + Tìm hiểu **AWS Shield** và **WAF** để bảo vệ ứng dụng web khỏi tấn công DDoS | 20/10/2025   | 20/10/2025    | <https://aws.amazon.com/security/> |
| 2    | - Thực hành **KMS và Shield/WAF** <br> + Tạo và quản lý **Customer Managed Keys (CMK)** trong KMS <br> + Thiết lập **AWS WAF rules** để chặn truy cập độc hại <br> + Kiểm thử bảo mật ứng dụng bằng **Shield Standard**                                                                       | 21/10/2025   | 21/10/2025    | <https://docs.aws.amazon.com/kms/> |
| 3    | - Nghiên cứu **Amazon VPC cơ bản** <br> + Tạo **VPC, Subnet, Route Table, và Internet Gateway** <br> + Thiết lập **Security Group** và **Network ACL** để kiểm soát truy cập <br> + Kết nối VPC với các dịch vụ khác như EC2 và RDS                                                           | 22/10/2025   | 22/10/2025    | <https://aws.amazon.com/vpc/>      |
| 4    | - Tìm hiểu nâng cao về **VPC Networking** <br> + Thiết lập **VPN Connection** giữa on-premise và VPC <br> + Giới thiệu **AWS Direct Connect** để kết nối vật lý <br> + Mô phỏng mô hình phân vùng mạng an toàn (Private/Public Subnets)                                                       | 23/10/2025   | 23/10/2025    | <https://docs.aws.amazon.com/vpc/> |
| 5    | - Tìm hiểu về **Disaster Recovery & Backup** <br> + Sử dụng **AWS Backup** để sao lưu tài nguyên tự động <br> + Cấu hình **RDS Read Replicas** để tăng khả năng chịu lỗi <br> + Thực hành **Cross-Region Failover** cho kế hoạch DR                                                           | 24/10/2025   | 24/10/2025    | <https://aws.amazon.com/backup/>   |
| 6    | - Ôn tập và tổng hợp kiến thức tuần 7 <br> + Kiểm thử lại các thiết lập **VPC**, **Backup**, và **WAF** <br> + Viết tài liệu đánh giá bảo mật hệ thống <br> + Tổng hợp bài học về bảo mật, mạng, và phục hồi thảm họa                                                                         | 25/10/2025   | 25/10/2025    | -                                  |

---

### Thành tựu Tuần 7

- Nắm vững **bảo mật dữ liệu và mã hóa trong AWS**:

  - Thực hành tạo và quản lý **KMS keys** để bảo vệ dữ liệu.
  - Hiểu cơ chế hoạt động của **CloudHSM** trong quản lý khóa phần cứng.
  - Cấu hình **AWS Shield** và **WAF** để bảo vệ ứng dụng khỏi tấn công DDoS và truy cập độc hại.

- Làm chủ **mạng riêng ảo Amazon VPC**:

  - Tạo và cấu hình **VPC**, **subnets**, **route tables**, **Internet Gateway**, và **NAT Gateway**.
  - Áp dụng **Security Groups** và **Network ACLs** để kiểm soát truy cập hiệu quả.
  - Thiết lập **VPN Connection** và hiểu khái niệm **Direct Connect** cho kết nối bảo mật cao.

- Hiểu rõ quy trình **Disaster Recovery (DR)** và sao lưu dữ liệu:

  - Sử dụng **AWS Backup** để quản lý sao lưu định kỳ.
  - Cấu hình **RDS Read Replicas** để đảm bảo tính sẵn sàng dữ liệu.
  - Thực hiện **Cross-Region Failover** để đảm bảo khôi phục nhanh sau thảm họa.

- Tăng cường khả năng thiết kế hệ thống an toàn, đáng tin cậy và có khả năng phục hồi cao.
  - Biết cách kết hợp giữa **Security**, **Networking**, và **Resilience** trong kiến trúc AWS.
  - Chuẩn bị kiến thức cho tuần tiếp theo về **Automation, Infrastructure as Code (IaC), và DevOps Practices**.
