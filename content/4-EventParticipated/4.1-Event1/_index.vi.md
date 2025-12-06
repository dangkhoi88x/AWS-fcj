---
title: "Event 1"
date: 2025-11-13
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Bài thu hoạch "AWS Perimeter Protection Workshop"

### Mục Đích Của Sự Kiện

- Cung cấp kỹ năng thực tế để bảo vệ chống lại các mối đe dọa bảo mật đang phát triển.
- Giới thiệu các framework bảo mật AWS đã được chứng minh để luôn đi trước các mối đe dọa mới.
- Học cách thiết kế và tối ưu hóa phân phối nội dung toàn cầu với Amazon CloudFront.
- Thực hành bảo vệ ứng dụng web khỏi các tấn công phổ biến bằng AWS WAF.
- Tham gia hands-on workshop để trải nghiệm thực tế các giải pháp bảo mật.

**Thời gian:** 19/11/2025  
**Địa điểm:** Thành phố Hồ Chí Minh

---

### Danh Sách Diễn Giả

- **Nguyễn Gia Hưng** - Head of Solutions Architect - Vietnam & Cambodia

  - Hưng là Head of Solutions Architect cho Vietnam & Cambodia với hơn 17 năm kinh nghiệm thiết kế và triển khai kiến trúc cloud an toàn, có khả năng phục hồi cao.

- **Julian Ju** - Senior Edge Services Specialist Solutions Architect

  - Julian là AWS Edge services SA tại ASEAN với hơn 25 năm kinh nghiệm IT, giúp khách hàng áp dụng AWS Edge services để có trải nghiệm an toàn và nhanh hơn.

- **Kevin Lim** - Senior Edge Services Specialist GTM
  - Kevin là Edge Specialist tại APJ với 15 năm kinh nghiệm trong IT như SRE, security ops, và infrastructure engineer, giúp khách hàng xây dựng kiến trúc an toàn.

---

### Lịch Trình Sự Kiện

| Thời gian     | Hoạt động                                              | Diễn giả                              |
| ------------- | ------------------------------------------------------ | ------------------------------------- |
| 09:00 - 09:45 | Đăng ký và Networking                                  | -                                     |
| 09:45 - 10:00 | Khai mạc & Chào mừng                                   | -                                     |
| 10:00 - 11:00 | Từ Edge đến Origin: CloudFront làm Nền tảng            | Nguyễn Gia Hưng                       |
| 11:00 - 12:00 | Bảo vệ Attack Surface: WAF & Application Protection    | Julian Ju                             |
| 12:00 - 13:00 | Ăn trưa & Networking                                   | -                                     |
| 13:00 - 15:00 | Hands-On Workshop: Tối ưu hóa Internet Web Application | Nguyễn Gia Hưng, Julian Ju, Kevin Lim |
| 15:00 - 15:15 | Nghỉ giải lao                                          | -                                     |
| 15:15 - 17:15 | Hands-On Workshop: Bảo mật Internet Web Application    | Nguyễn Gia Hưng, Julian Ju, Kevin Lim |

---

### Nội Dung Nổi Bật

#### Bối cảnh và Thách thức Bảo mật

- **Mối đe dọa mạng đang leo thang nhanh chóng**: Các cuộc tấn công DDoS tăng theo từng năm và các cuộc tấn công ở lớp ứng dụng nhắm vào các chức năng kinh doanh quan trọng.
- **Chi phí vi phạm bảo mật**: Các tổ chức không có bảo vệ perimeter mạnh mẽ phải đối mặt với chi phí vi phạm trung bình $4.88M và khả năng gián đoạn kinh doanh.
- **Tầm quan trọng của Perimeter Protection**: Cần có các framework bảo mật AWS đã được chứng minh để luôn đi trước các mối đe dọa mới.

#### Từ Edge đến Origin: CloudFront làm Nền tảng

**Amazon CloudFront - CDN toàn cầu**:

- **Edge Locations**: Hiểu cách CloudFront sử dụng mạng lưới edge locations toàn cầu để phân phối nội dung gần người dùng hơn.
- **Core CDN Capabilities**: Học các khả năng cốt lõi của CDN từ edge locations đến security controls.
- **Security Controls**: Tích hợp các biện pháp bảo mật vào CloudFront distribution.
- **Architecture & Optimization**: Cách thiết kế và tối ưu hóa phân phối nội dung toàn cầu.
- **Scalability**: Xây dựng nền tảng vững chắc để phục vụ nội dung ở quy mô lớn.

**Đối tượng**: Cloud architects và developers.

#### Bảo vệ Attack Surface: WAF & Application Protection

**AWS WAF - Web Application Firewall**:

- **Block Malicious Traffic**: Sử dụng AWS WAF để chặn lưu lượng độc hại trước khi đến ứng dụng.
- **Mitigate OWASP Top 10**: Bảo vệ chống lại các lỗ hổng OWASP Top 10 phổ biến như SQL injection, XSS, CSRF.
- **Defend Against Bot Attacks**: Phát hiện và chặn các cuộc tấn công bot tự động.
- **Custom Rules**: Tạo các quy tắc tùy chỉnh để bảo vệ ứng dụng cụ thể.
- **Integration với CloudFront**: Tích hợp WAF với CloudFront để bảo vệ ở edge.

#### Hands-On Workshop: Tối ưu hóa Internet Web Application

- **Thực hành với CloudFront**: Cấu hình CloudFront distribution cho ứng dụng web.
- **Optimization Techniques**: Học các kỹ thuật tối ưu hóa hiệu suất và giảm độ trễ.
- **Caching Strategies**: Thiết lập caching strategies hiệu quả.
- **Real-world Scenarios**: Thảo luận các tình huống thực tế và cách triển khai.
- **Best Practices**: Áp dụng best practices từ các chuyên gia AWS.

#### Hands-On Workshop: Bảo vệ Internet Web Application

- **Thực hành với AWS WAF**: Cấu hình AWS WAF rules để bảo vệ ứng dụng.
- **Security Configuration**: Thiết lập các biện pháp bảo mật cho web application.
- **Attack Simulation**: Trải nghiệm cách WAF chặn các cuộc tấn công thực tế.
- **Monitoring & Logging**: Thiết lập monitoring và logging cho security events.
- **Integration Patterns**: Học cách tích hợp WAF với các dịch vụ AWS khác.

---

### Những Gì Học Được

#### Kiến trúc Bảo mật Edge

- **CloudFront Architecture**: Hiểu cách thiết kế kiến trúc CloudFront để tối ưu hiệu suất và bảo mật.
- **Edge Security**: Tầm quan trọng của bảo mật ở edge để giảm tải cho origin servers.
- **CDN Best Practices**: Các best practices về caching, compression, và content delivery.
- **Geographic Distribution**: Tận dụng edge locations để giảm độ trễ và cải thiện trải nghiệm người dùng.

#### Bảo vệ Ứng dụng Web

- **WAF Fundamentals**: Nắm vững các khái niệm cơ bản về Web Application Firewall.
- **OWASP Top 10 Protection**: Hiểu cách bảo vệ chống lại các lỗ hổng OWASP Top 10.
- **Bot Management**: Kỹ thuật phát hiện và quản lý bot traffic.
- **Custom Rules Development**: Cách tạo và quản lý custom WAF rules.
- **Security Monitoring**: Thiết lập monitoring và alerting cho security events.

#### Tối ưu hóa Hiệu suất và Bảo mật

- **Performance Optimization**: Các kỹ thuật tối ưu hóa hiệu suất với CloudFront.
- **Security vs Performance Trade-offs**: Hiểu sự đánh đổi giữa bảo mật và hiệu suất.
- **Cost Optimization**: Cách tối ưu hóa chi phí khi sử dụng CloudFront và WAF.
- **Scalability Patterns**: Thiết kế hệ thống có khả năng mở rộng với edge services.

---

### Ứng Dụng Vào Công Việc

- **Triển khai CloudFront** cho dự án Mini-Market để cải thiện hiệu suất và giảm tải cho origin servers.
- **Cấu hình AWS WAF** để bảo vệ ứng dụng web khỏi các cuộc tấn công phổ biến.
- **Áp dụng Edge Security** vào kiến trúc hệ thống để tăng cường bảo mật.
- **Tối ưu hóa CDN** với caching strategies và compression để giảm chi phí và cải thiện trải nghiệm người dùng.
- **Thiết lập Security Monitoring** với CloudWatch và CloudTrail để theo dõi các sự kiện bảo mật.

---

### Trải nghiệm trong event

Tham gia **"AWS Perimeter Protection Workshop"** tại Thành phố Hồ Chí Minh là một trải nghiệm rất giá trị, giúp tôi hiểu sâu hơn về cách bảo vệ hệ thống khỏi các mối đe dọa bảo mật hiện đại. Một số trải nghiệm nổi bật:

#### Học hỏi từ các chuyên gia hàng đầu

- **Nguyễn Gia Hưng** đã chia sẻ kiến thức sâu về CloudFront architecture và cách tối ưu hóa phân phối nội dung toàn cầu.
- **Julian Ju** trình bày chi tiết về AWS WAF và các kỹ thuật bảo vệ ứng dụng web khỏi các cuộc tấn công.
- **Kevin Lim** chia sẻ kinh nghiệm thực tế về security operations và infrastructure engineering.

#### Trải nghiệm kỹ thuật thực tế

- **Hands-on với CloudFront**: Trực tiếp cấu hình CloudFront distribution và trải nghiệm cách CDN hoạt động.
- **Thực hành AWS WAF**: Cấu hình WAF rules và xem cách chúng chặn các cuộc tấn công thực tế.
- **Tối ưu hóa Performance**: Học các kỹ thuật caching, compression, và content optimization.
- **Security Configuration**: Thiết lập các biện pháp bảo mật và monitoring cho web applications.

#### Hiểu rõ về Edge Security

- Tầm quan trọng của **bảo mật ở edge** để giảm tải cho origin servers và cải thiện hiệu suất.
- Cách **CloudFront và WAF** làm việc cùng nhau để tạo lớp bảo vệ mạnh mẽ.
- **Best practices** về edge security và cách áp dụng vào các dự án thực tế.

#### Kết nối và trao đổi

- Workshop tạo cơ hội trao đổi trực tiếp với các chuyên gia AWS và các bạn tham gia khác.
- Thảo luận về **real-world scenarios** và cách giải quyết các thách thức bảo mật.
- Học hỏi từ kinh nghiệm của các tổ chức khác trong việc triển khai perimeter protection.

#### Bài học rút ra

- **Perimeter Protection** là lớp phòng thủ đầu tiên và quan trọng nhất trong kiến trúc bảo mật.
- **CloudFront** không chỉ cải thiện hiệu suất mà còn cung cấp các tính năng bảo mật mạnh mẽ.
- **AWS WAF** là công cụ hiệu quả để bảo vệ ứng dụng web khỏi các cuộc tấn công phổ biến.
- **Edge Security** giúp giảm tải cho origin servers và cải thiện khả năng phục hồi của hệ thống.
- Việc kết hợp **CloudFront và WAF** tạo ra giải pháp bảo mật toàn diện và hiệu quả.

#### Một số hình ảnh khi tham gia sự kiện

- Thêm các hình ảnh của các bạn tại đây

> Tổng thể, sự kiện không chỉ cung cấp kiến thức về bảo mật edge mà còn giúp tôi hiểu cách áp dụng các giải pháp AWS để bảo vệ hệ thống khỏi các mối đe dọa hiện đại. Workshop đã trang bị cho tôi các kỹ năng thực tế để thiết kế và triển khai perimeter protection hiệu quả.
