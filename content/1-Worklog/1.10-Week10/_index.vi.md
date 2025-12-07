---
title: "Nhật ký Tuần 10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu Tuần 10

- **Triển khai thực tế hệ thống Mini-Market** trên môi trường AWS.
- **Hoàn thiện code và tích hợp** các dịch vụ AWS đã thiết kế.
- **Thực hiện testing và tối ưu hóa** hiệu suất hệ thống.
- **Chuẩn bị demo** và video trình bày các tính năng chính của hệ thống.
- **Rà soát cuối cùng** toàn bộ tài liệu, báo cáo và proposal.
- **Hoàn thiện presentation slides** và chuẩn bị nội dung thuyết trình cuối cùng.
- **Tổng kết toàn bộ chương trình** và đánh giá kết quả đạt được.

**Thời gian:** 01/12/2025 - 07/12/2025

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo                         |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | ------------- | ------------------------------------------ |
| 1    | - **Triển khai Infrastructure trên AWS** <br> + Tạo VPC với public và private subnets <br> + Cấu hình Security Groups và Network ACLs <br> + Thiết lập NAT Gateway và Internet Gateway <br> + Tạo RDS SQL Server instance trong private subnet                                                                                                                                                                                                                                                         | 01/12/2025   | 01/12/2025    | <https://aws.amazon.com/vpc/>              |
| 2    | - **Deploy Application Layer** <br> + Tạo Elastic Beanstalk environment cho ứng dụng .NET <br> + Cấu hình Auto Scaling và Load Balancer <br> + Kết nối ứng dụng với RDS database <br> + Thiết lập environment variables và connection strings                                                                                                                                                                                                                                                          | 02/12/2025   | 02/12/2025    | <https://aws.amazon.com/elasticbeanstalk/> |
| 3    | - **Tích hợp Storage và CDN** <br> + Tạo S3 bucket cho lưu trữ ảnh sản phẩm <br> + Cấu hình CloudFront distribution để phân phối nội dung <br> + Thiết lập ElastiCache Redis cluster cho caching <br> + Kết nối ứng dụng với S3 và Redis                                                                                                                                                                                                                                                               | 03/12/2025   | 03/12/2025    | <https://aws.amazon.com/s3/>               |
| 4    | - **Cấu hình Security và Monitoring** <br> + Thiết lập IAM Roles và Policies cho các dịch vụ <br> + Cấu hình AWS WAF để bảo vệ ứng dụng <br> + Thiết lập CloudWatch alarms và dashboards <br> + Kích hoạt CloudTrail để ghi log hoạt động                                                                                                                                                                                                                                                              | 04/12/2025   | 04/12/2025    | <https://aws.amazon.com/iam/>              |
| 5    | - **Testing và Tối ưu hóa** <br> + Thực hiện load testing để kiểm tra hiệu suất <br> + Kiểm tra tính sẵn sàng cao (High Availability) <br> + Tối ưu hóa database queries và caching strategy <br> + Điều chỉnh Auto Scaling policies dựa trên metrics                                                                                                                                                                                                                                                  | 05/12/2025   | 05/12/2025    | <https://aws.amazon.com/cloudwatch/>       |
| 6    | - **Chuẩn bị Demo và Documentation** <br> + Ghi video demo các tính năng chính: Đăng nhập, Xem sản phẩm, Thêm vào giỏ hàng, Thanh toán <br> + Tạo user guide và technical documentation <br> + Chuẩn bị script demo cho thuyết trình <br> + Tạo screenshots và diagrams cho documentation                                                                                                                                                                                                              | 06/12/2025   | 06/12/2025    | -                                          |
| 7    | - **Rà soát cuối cùng và Kết thúc Chương trình** <br> + Rà soát cuối cùng toàn bộ tài liệu, báo cáo và proposal <br> + Hoàn thiện presentation slides và nội dung thuyết trình <br> + Đánh giá kết quả triển khai và testing <br> + Ghi nhận các vấn đề đã gặp và cách giải quyết <br> + Cập nhật Proposal với kết quả thực tế <br> + Tổng kết toàn bộ hành trình AWS Cloud Journey <br> + Đánh giá kết quả đạt được và các kỹ năng đã phát triển <br> + Chuẩn bị các deliverables cuối cùng để submit | 07/12/2025   | 07/12/2025    | -                                          |

---

### Thành tựu Tuần 10

- **Triển khai thành công hệ thống Mini-Market trên AWS**:

  - Tạo và cấu hình đầy đủ VPC với public/private subnets, NAT Gateway, và Internet Gateway.
  - Deploy ứng dụng .NET trên Elastic Beanstalk với Auto Scaling và Load Balancer.
  - Thiết lập RDS SQL Server trong private subnet với Multi-AZ cho High Availability.
  - Tích hợp S3 cho storage và CloudFront cho CDN.

- **Tối ưu hóa hiệu suất và bảo mật**:

  - Cấu hình ElastiCache Redis để cache dữ liệu và giảm tải database.
  - Thiết lập AWS WAF để bảo vệ ứng dụng khỏi các tấn công phổ biến.
  - Cấu hình IAM Roles và Policies theo nguyên tắc least privilege.
  - Thiết lập CloudWatch monitoring và CloudTrail logging.

- **Hoàn thiện testing và validation**:

  - Thực hiện load testing và xác nhận hệ thống có thể xử lý traffic cao.
  - Kiểm tra tính sẵn sàng cao bằng cách simulate failover scenarios.
  - Tối ưu hóa database queries và caching strategy dựa trên kết quả testing.
  - Điều chỉnh Auto Scaling policies để đảm bảo hiệu quả chi phí.

- **Chuẩn bị demo và documentation chuyên nghiệp**:

  - Ghi video demo đầy đủ các tính năng chính của hệ thống.
  - Tạo user guide và technical documentation chi tiết.
  - Chuẩn bị script demo và slides cho thuyết trình.
  - Tạo screenshots và diagrams minh họa cho documentation.

- **Ghi nhận kinh nghiệm thực tế**:

  - Hiểu rõ quy trình triển khai hệ thống trên AWS từ thiết kế đến production.
  - Nắm vững cách troubleshoot các vấn đề thường gặp trong quá trình deployment.
  - Học được các best practices về security, monitoring, và cost optimization.
  - Phát triển kỹ năng làm việc với multiple AWS services cùng lúc.

- **Hoàn thành chương trình AWS Cloud Journey**:

  - Hệ thống đã được triển khai và tested thành công.
  - Documentation và demo materials đã được chuẩn bị đầy đủ.
  - Rà soát cuối cùng tất cả deliverables đã hoàn tất.
  - Presentation slides và nội dung đã được hoàn thiện.
  - Tổng kết chương trình và đánh giá đã hoàn thành.
  - Tất cả deliverables đã sẵn sàng để submit.
  - Đã hoàn thành thành công 10 tuần học tập và triển khai dự án AWS Cloud Journey.
