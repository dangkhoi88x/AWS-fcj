---
title: "Nhật ký Tuần 6"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu Tuần 6

- Tìm hiểu và thực hành các dịch vụ **Data & Analytics** của AWS.
- Làm quen với các dịch vụ **Machine Learning (ML)** như **SageMaker** và **Rekognition**.
- Nâng cao kỹ năng **giám sát, kiểm tra, và tối ưu hiệu suất hệ thống** bằng CloudWatch, CloudTrail, và Trusted Advisor.
- Tìm hiểu về **quản lý danh tính nâng cao** trong AWS bao gồm IAM Roles, Federated Access, và SSO.

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo                         |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------- | ------------------------------------------ |
| 1    | - Nghiên cứu **Data & Analytics Services** <br> + Làm quen với **Amazon Athena** để truy vấn dữ liệu trực tiếp từ S3 <br> + Tìm hiểu **AWS Glue** cho ETL (Extract – Transform – Load) <br> + Giới thiệu về **Amazon EMR** và xử lý dữ liệu lớn (Big Data) | 13/10/2025   | 13/10/2025    | <https://aws.amazon.com/athena/>           |
| 2    | - Thực hành với **AWS Glue & Kinesis** <br> + Tạo Glue crawler để tự động thu thập metadata <br> + Thiết lập **Kinesis Data Stream** để xử lý dữ liệu theo thời gian thực <br> + Kết hợp Glue và Athena để phân tích dữ liệu động                          | 14/10/2025   | 14/10/2025    | <https://aws.amazon.com/glue/>             |
| 3    | - Bắt đầu học về **Machine Learning trên AWS** <br> + Làm quen với **Amazon SageMaker** <br> + Tạo và huấn luyện mô hình đơn giản trên dataset mẫu <br> + Khám phá dịch vụ **Amazon Rekognition** để nhận diện hình ảnh                                    | 15/10/2025   | 15/10/2025    | <https://aws.amazon.com/sagemaker/>        |
| 4    | - Tiếp tục phần **Machine Learning** <br> + Triển khai mô hình SageMaker lên endpoint inference <br> + Kiểm thử và đánh giá độ chính xác mô hình <br> + Tìm hiểu các dịch vụ ML khác như **Comprehend**, **Translate**, và **Textract**                    | 16/10/2025   | 16/10/2025    | <https://aws.amazon.com/machine-learning/> |
| 5    | - Thực hành **Monitoring, Audit & Performance** <br> + Cấu hình **Amazon CloudWatch** để thu thập metrics <br> + Kích hoạt **AWS CloudTrail** để theo dõi hoạt động API <br> + Sử dụng **Trusted Advisor** để phát hiện rủi ro và tối ưu chi phí           | 17/10/2025   | 17/10/2025    | <https://aws.amazon.com/cloudwatch/>       |
| 6    | - Học về **Advanced Identity in AWS** <br> + Tạo và quản lý **IAM Roles** nâng cao <br> + Tìm hiểu **Federated Access** để đăng nhập qua tài khoản ngoài <br> + Cấu hình **AWS SSO (Single Sign-On)** và thử nghiệm truy cập đa tài khoản                  | 18/10/2025   | 18/10/2025    | <https://aws.amazon.com/iam/>              |

---

### Thành tựu Tuần 6

- Nắm vững kiến thức về **AWS Data & Analytics**:

  - Hiểu quy trình phân tích dữ liệu sử dụng **Athena**, **Glue**, **EMR**, và **Kinesis**.
  - Thực hành truy vấn dữ liệu S3 bằng **Athena** và xây dựng ETL pipeline cơ bản bằng **Glue**.
  - Tạo pipeline xử lý dữ liệu thời gian thực với **Kinesis Data Stream**.

- Tiếp cận và thực hành **Machine Learning trên AWS**:

  - Huấn luyện và triển khai mô hình mẫu bằng **SageMaker**.
  - Sử dụng **Rekognition** để nhận diện khuôn mặt và đối tượng trong hình ảnh.
  - Làm quen với các dịch vụ ML khác: **Comprehend**, **Translate**, **Textract**.

- Thành thạo kỹ năng **giám sát và kiểm tra hệ thống**:

  - Theo dõi metric và log bằng **CloudWatch**.
  - Ghi nhận và kiểm tra hoạt động API qua **CloudTrail**.
  - Phân tích báo cáo và khuyến nghị từ **Trusted Advisor** để tối ưu hiệu suất.

- Tăng cường hiểu biết về **bảo mật và quản lý danh tính nâng cao**:

  - Tạo **IAM Roles** và áp dụng nguyên tắc least privilege.
  - Tìm hiểu **Federated Access** cho phép đăng nhập bằng tài khoản tổ chức.
  - Cấu hình **AWS SSO** để truy cập nhiều tài khoản AWS một cách an toàn và thuận tiện.

- Hoàn thiện kỹ năng tổng hợp giữa **Data, Machine Learning, Monitoring**, và **Security**, chuẩn bị cho tuần tiếp theo về **Automation & Infrastructure as Code (IaC)**.
