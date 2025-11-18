---
title: "Nhật ký Tuần 9"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu Tuần 9

- Hoàn thành **báo cáo tổng kết toàn bộ hành trình AWS Cloud Journey**.
- Xây dựng và trình bày **kiến trúc hệ thống hoàn chỉnh** dựa trên các dịch vụ AWS đã học.
- Ôn tập và củng cố toàn bộ kiến thức từ Week 1 → Week 8.
- Chuẩn bị báo cáo, tài liệu và nội dung thuyết trình cuối kỳ.

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động                                                                                                                                                                  | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo                                      |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------- | ------------------------------------------------------- |
| 1    | - Hoàn thiện **Solution Architecture Diagram** <br> + Bổ sung chi tiết các dịch vụ: Beanstalk, RDS, ElastiCache, S3, CloudFront, NAT Gateway, WAF, Route 53 <br> + Xác định rõ luồng dữ liệu & giao tiếp giữa các tầng hệ thống | 03/11/2025   | 03/11/2025    | <https://aws.amazon.com/architecture/> |
| 2    | - Viết nội dung chi tiết cho **Solution Architecture** trong Proposal <br> + Mô tả thành phần, cơ chế hoạt động, bảo mật, scaling <br> + Chuẩn hóa bản vẽ kiến trúc để đưa vào báo cáo                                           | 04/11/2025   | 04/11/2025    | - |
| 3    | - Tiếp tục hoàn thiện **Application Layer** <br> + Tối ưu service logic, tách interface rõ ràng <br> + Rà soát và tối ưu Repository & Unit of Work <br> + Kiểm tra lại migration và seed data                                     | 05/11/2025   | 05/11/2025    | - |
| 4    | - Phát triển **WebShop MVC** <br> + Hoàn thiện trang sản phẩm, chi tiết sản phẩm và giỏ hàng <br> + Kết nối dữ liệu thật từ database & cache Redis <br> + Tối ưu controller + view                                                      | 06/11/2025   | 06/11/2025    | - |
| 5    | - Tích hợp **Upload hình ảnh sản phẩm lên S3** trong WebShop <br> + Kiểm tra phân quyền bucket & policy S3 <br> + Tối ưu tốc độ tải ảnh qua CloudFront                                                                            | 07/11/2025   | 07/11/2025    | <https://aws.amazon.com/s3/> |
| 6    | - Chuẩn bị cho phần triển khai AWS ở tuần tiếp theo <br> + Rà soát lại cấu trúc project <br> + Viết checklist triển khai lên Elastic Beanstalk <br> + Kiểm tra lại connection string RDS & cấu hình appsettings                    | 08/11/2025   | 08/11/2025    | - |

---

### Thành tựu Tuần 9

- Hoàn thành **bản kiến trúc hệ thống hoàn chỉnh**, bao gồm:

  - VPC với public/private subnets
  - EC2 + ALB + Auto Scaling
  - RDS + S3 + CloudFront
  - IAM roles/policies + Security Groups
  - CloudWatch + CloudTrail

- Viết và hoàn thiện **báo cáo tổng kết AWS Cloud Journey**, thể hiện đầy đủ:

  - Kiến thức đã học
  - Dịch vụ đã triển khai
  - Giải thích kiến trúc và lý do lựa chọn

- Củng cố kiến thức về **Security, Networking, Compute, Storage, Serverless, Monitoring**.

- Chuẩn bị đầy đủ **slide thuyết trình**, hình ảnh, sơ đồ kiến trúc, và demo nhỏ.

- Sẵn sàng cho buổi **báo cáo Final Presentation** của chương trình.
