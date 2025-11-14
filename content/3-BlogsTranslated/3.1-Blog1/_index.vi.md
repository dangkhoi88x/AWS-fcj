---
title: "Blog 1"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Truyền tải video hiệu quả và AI thị giác tại biên với Realtek, Plumerai và Amazon Kinesis Video Streams

Trí tuệ nhân tạo (AI) tại biên đang trở nên phổ biến trong các thiết bị video thông minh. Ví dụ, các camera Nhà thông minh và chuông cửa video đã cách mạng hóa việc giám sát tại nhà. Từ những công cụ ghi hình đơn giản và xem từ xa, các thiết bị này đã tiến hóa thành những người quan sát thông minh. Với sự tích hợp AI, các camera ngày nay có thể chủ động phân tích cảnh, cảnh báo người dùng về các sự kiện chuyển động, nhận diện khuôn mặt quen thuộc, phát hiện việc giao hàng gói hàng và điều chỉnh hành vi ghi hình một cách linh hoạt. Các camera giám sát doanh nghiệp là một ví dụ khác. Những camera này có độ phân giải vượt trội, khả năng tính toán nâng cao và có thể hỗ trợ các mô hình AI phức tạp hơn. Những khả năng nâng cao này mang lại khả năng phát hiện sắc nét hơn ở khoảng cách xa hơn.

Như đã minh họa, khách hàng đòi hỏi các hệ thống giám sát thông minh có thể xử lý dữ liệu cục bộ trong khi vẫn duy trì quyền riêng tư và giảm chi phí băng thông. Để đáp ứng những nhu cầu này, nhóm AWS Internet of Things (AWS IoT) đã phát triển một giải pháp camera thông minh cùng với các đối tác AWS, kết hợp Amazon Kinesis Video Streams, vi điều khiển Ameba Pro2 tiết kiệm năng lượng của Realtek và các mô hình học máy hiệu quả từ Plumerai. Bài viết này cung cấp hướng dẫn cho việc tải video kích hoạt theo sự kiện kết hợp với xử lý thuật toán phát hiện con người tại biên.

---

## Kiến trúc giải pháp

Hình dưới đây minh họa kiến trúc giải pháp mà bài viết này sử dụng:

- Bắt đầu từ camera, firmware của thiết bị đã tích hợp **Realtek SDK** để truy cập các mô-đun camera thông qua các API được xác định.
- Các đoạn video được truyền đến **các mô hình học máy của Plumerai** để thực hiện phát hiện đối tượng.
- Ứng dụng mẫu thêm kết quả phát hiện dưới dạng **lớp phủ bounding box** trên các đoạn video gốc. Ứng dụng này liên tục tải các đoạn video lên đám mây thông qua **Kinesis Video Streams Producer SDK**.  
  _(Bạn cũng có thể thiết lập để chỉ tải lên các đoạn video 20 giây khi có phát hiện.)_
- **Producer SDK** sử dụng **PutMedia API** với kết nối HTTPS lâu dài để tải liên tục các mảnh MKV.
- Dữ liệu media được thu nhận và lưu trữ bền vững để phân tích về sau.
- Một **ứng dụng front-end** có thể phát lại video trực tiếp hoặc video đã ghi thông qua **HLS hoặc DASH**.
- Giải pháp cung cấp dữ liệu video và âm thanh vào **Large Language Models (LLMs)** để khai thác insight từ Agentic AI.  
  _(Tìm kiếm video ngữ nghĩa sẽ được trình bày trong bài tiếp theo.)_

---

## Điểm nổi bật của tích hợp

### **Amazon Kinesis Video Streams**

Kinesis Video Streams mang lại nhiều lợi ích cho camera IP, robot và ô tô:

- **Kiến trúc được quản lý hoàn toàn** → đội kỹ thuật chỉ cần tập trung vào phát triển tính năng.
- **AWS SDK mã nguồn mở**, tránh phụ thuộc nền tảng.
- **Mô hình trả phí theo sử dụng linh hoạt**, không tốn chi phí cho đến khi camera hoạt động thật.

### **Plumerai**

Plumerai chuyên về AI nhúng và tối ưu mô hình để chạy trên phần cứng nhỏ, hiệu năng thấp:

- Tối ưu hoá CPU Arm Cortex-M ở mức **assembly**, tận dụng lệnh DSP.
- **Neural Architecture Search (NAS)** chọn kiến trúc AI tối ưu cho Realtek NPU.
- Tận dụng **hardware accelerators** (scaler, format converter) của Realtek.
- Tích hợp tốt với **RTOS** và khung truyền thông Realtek.
- Khởi động nhanh, mô hình huấn luyện trên **30 triệu video & ảnh**.

**Hiệu suất thực tế:**

- Độ chính xác cao, bộ nhớ thấp.
- Góc nhìn rộng **180°**.
- Phát hiện người trong phạm vi **20m+**.
- Theo dõi **20 người cùng lúc**.
- Hoạt động tốt cả ban ngày và đêm tối.

---

## Realtek Ameba Pro2

Realtek Ameba Pro2 bao gồm:

- **Integrated Video Encoder (IVE)**
- **Image Signal Processor (ISP)**
- **Video Offload Engine (VOE)**
- **Neural Processing Unit (NPU)**
- **Parallel Processing Unit (PPU)**

Khả năng chính:

- Tốn rất ít CPU.
- Phản hồi gần thời gian thực.
- Xử lý video ngay cả khi đang khởi động.
- Truyền tới SD Card và Cloud qua WiFi/Ethernet.
- Hỗ trợ mạnh mẽ inference AI tại biên.

---

## Hướng dẫn từng bước

### **Điều kiện tiên quyết**

Bạn cần:

- Tài khoản AWS có quyền:
  - Đăng nhập AWS Console
  - API Kinesis Video Streams  
    (_GetDataEndpoint, DescribeStream, PutMedia_)
  - Tạo EC2 instance để build SDK
- Stream có tên `kvs-plumerai-realtek-stream`
- Realtek Ameba Pro2 MCU
- Kinh nghiệm Linux basic
- SDK Realtek & thư viện Plumerai
- Kết nối internet

---

## Thiết lập môi trường xây dựng

### **Tạo EC2 Instance**

1. Vào AWS Console → EC2.
2. Launch instance:
   - **Tên:** `KVS_AmebaPlumerAI_poc`
   - **OS:** Ubuntu 22.04 LTS
   - **Loại máy:** t3.large
   - **Key pair:** `kvs-plumerai-realtek-keypair`
   - **Storage:** 100 GiB
3. Mở port SSH.
4. SSH vào EC2:

```bash
ssh -o ServerAliveInterval=60 -i kvs-plumerai-realtek-keypair.pem ubuntu@54.xxx.yyy.zzz
```
