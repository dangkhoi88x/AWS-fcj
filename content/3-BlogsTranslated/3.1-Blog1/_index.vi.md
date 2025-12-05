---
title: "Blog 1"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
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

- Bắt đầu từ camera, firmware của thiết bị đã tích hợp Realtek SDK để truy cập các mô-đun camera thông qua các API được xác định.

- Các đoạn video được truyền đến các mô hình học máy của Plumerai để thực hiện phát hiện đối tượng.

- Ứng dụng mẫu thêm kết quả phát hiện dưới dạng lớp phủ hộp giới hạn (bonding box) trên các đoạn video gốc. Ứng dụng mẫu này liên tục tải các đoạn video lên đám mây thông qua Kinesis Video Streams Producer SDK. (Lưu ý thêm, bạn cũng có thể thiết lập kết quả phát hiện để kích hoạt tải lên các đoạn video 20 giây.)

- Kinesis Video Streams Producer SDK sử dụng PutMedia API với kết nối HTTPS lâu dài để tải các đoạn MKV lên một cách liên tục theo kiểu truyền tải.

- Dữ liệu truyền thông sẽ được thu nhận và dịch vụ lưu trữ tất cả dữ liệu truyền thông một cách bền vững để phân tích sau này.

- Một ứng dụng giao diện người dùng thực hiện phát lại video trực tiếp hoặc các video đã ghi trước đó, dựa vào các giao thức HLS hoặc DASH từ Kinesis Video Streams.

- Giải pháp này cung cấp dữ liệu video và âm thanh vào các Mô hình Ngôn ngữ Lớn (Large Language Models - LLMs) để thu thập thông tin chi tiết từ Agentic AI. (Chúng tôi sẽ đề cập đến tìm kiếm video ngữ nghĩa trong bài viết tiếp theo).

---

## Điểm nổi bật của tích hợp

### Amazon Kinesis Video Streams

Kinesis Video Streams thay đổi cách các doanh nghiệp xử lý các giải pháp video cho camera IP, robot và ô tô. Các lợi ích chính bao gồm:

- **Kiến trúc được quản lý hoàn toàn**: Điều này giúp các nhóm kỹ thuật tập trung vào đổi mới thay vì cơ sở hạ tầng, lý tưởng cho các công ty có nguồn lực hạn chế.

- **AWS SDKs mã nguồn mở**: Các thương hiệu hàng đầu đặc biệt đánh giá cao sự độc lập khỏi các ràng buộc nền tảng.

- **Mô hình định giá trả theo sử dụng linh hoạt**: Trong khi việc phát triển thiết bị có thể mất hàng tháng hoặc hàng năm, bạn không phải trả phí cho đến khi camera hoạt động. Với tỷ lệ kích hoạt lưu trữ đám mây điển hình dưới 30% và giảm hàng năm, chi phí thấp hơn đáng kể so với phí cấp phép cố định.

---

### Plumerai

Công ty Plumerai chuyên về các giải pháp AI nhúng, tập trung vào việc làm cho học sâu trở nên nhỏ gọn và hiệu quả. Mô hình Plumerai giúp cung cấp suy luận trên phần cứng nhỏ, giá cả phải chăng và tiết kiệm năng lượng. Công ty cũng tối ưu hóa các mô hình AI cho nền tảng Realtek Ameba Pro2 thông qua:

- **Tối ưu hóa cấp độ lắp ráp**: Tối đa hóa hiệu suất CPU Arm Cortex-M và tận dụng các lệnh DSP để nâng cao khả năng xử lý tín hiệu.

- **Tìm kiếm Kiến trúc Thần kinh (Neural Architecture Search - NAS)**: Chọn các mô hình AI tối ưu cho NPU và kiến trúc bộ nhớ của Realtek để đạt được gia tốc NPU 0.4 TOPS.

- Mô hình Plumerai sử dụng bộ tăng tốc phần cứng trên chip của Realtek (bộ chia tỷ lệ, bộ chuyển đổi định dạng) để giảm tải tính toán.

- **Mô hình AI hỗ trợ RTOS**: Tích hợp liền mạch với hệ điều hành thời gian thực của SoC.

- Ứng dụng tích hợp với khung truyền thông của Realtek.

- **Thiết kế khởi động nhanh**: Hỗ trợ thời gian khởi động nhanh, cải thiện tuổi thọ pin và đảm bảo tốc độ cao trong phát hiện đối tượng hoạt động.

- Mô hình AI biên được huấn luyện trên 30 triệu hình ảnh và video có gắn nhãn.

Những cải tiến này mang lại hiệu suất thực tế như sau:

- Cung cấp độ chính xác mà không lãng phí bộ nhớ.

- Ghi lại các cảnh rộng thông qua ống kính có trường nhìn 180°.

- Phát hiện cá nhân ở khoảng cách hơn 20m (65ft).

- Xử lý đám đông bằng cách theo dõi đồng thời 20 người.

- Duy trì theo dõi cá nhân với hệ thống ID duy nhất.

- Hoạt động ổn định trong ánh sáng ban ngày và bóng tối hoàn toàn.

---

### Realtek Ameba Pro2

Hình trên minh họa kiến trúc dữ liệu của Realtek Ameba Pro2. Nó bao gồm Bộ mã hóa Video Tích hợp (Integrated Video Encoder - IVE) và Bộ xử lý Tín hiệu Hình ảnh (Image Signal Processor - ISP) xử lý dữ liệu thô của phương tiện và truyền kết quả đến Bộ xử lý Tải Video (Video Offload Engine - VOE). VOE sau đó quản lý nhiều kênh video và luồng video đồng thời để hỗ trợ thuật toán phát hiện chuyển động. Đơn vị Xử lý Thần kinh (Neural Processing Unit - NPU) thực hiện suy luận trên các hình ảnh hoặc vùng hình ảnh. Đơn vị Xử lý Song song (Parallel Processing Unit - PPU) xử lý các công việc đa nhiệm như cắt Vùng Quan tâm (Regions of Interests - ROIs) từ hình ảnh độ phân giải cao, thay đổi kích thước đầu vào suy luận NPU và lấy đầu ra cuối cùng từ các kênh độ phân giải cao. Kiến trúc này mở khóa các khả năng mạnh mẽ để hỗ trợ phân tích video tại biên, bao gồm:

- Hoạt động với công suất CPU tối thiểu để đạt hiệu quả tối đa.

- Phản hồi gần như thời gian thực với chuyển động.

- Bắt đầu xử lý video ngay cả trong quá trình khởi động.

- Truyền tải đến cả thẻ SD và đám mây thông qua WiFi hoặc Ethernet bảo mật.

- Tận dụng NPU để cung cấp hiệu suất AI vượt trội.

- Tích hợp với các mô hình Plumerai và Kinesis Video Streams thông qua SDK khung đa phương tiện.

---

## Hướng dẫn từng bước

Phần này phác thảo các bước xây dựng giải pháp để chạy AI biên và truyền tải các đoạn video.

### Điều kiện tiên quyết

- Tài khoản AWS với quyền cho:

  - Đăng nhập vào AWS Console.
  - API Kinesis Video Streams (GetDataEndpoint, DescribeStream, PutMedia).
  - Tạo phiên bản Amazon Elastic Compute Cloud (Amazon EC2) để xây dựng SDK và tệp nhị phân.

- Một tài nguyên luồng có tên "kvs-plumerai-realtek-stream" được tạo trên Kinesis Video Streams Console.

- Realtek Ameba Pro2 Mini MCU.

- Kiến thức cơ bản về hệ thống nhúng và làm việc trong môi trường Linux.

- Kết nối internet để tải SDK và tải video lên AWS.

- Tệp thư viện và mô hình học máy từ Plumerai. (Vui lòng gửi yêu cầu của bạn trên Trang web Plumerai.)

---

## Thiết lập môi trường xây dựng

Bài viết này sử dụng một Amazon EC2 với Ubuntu LTS 22.04 làm môi trường xây dựng. Bạn cũng có thể sử dụng máy tính Ubuntu của riêng mình để biên dịch chéo SDK.

### Thiết lập phiên bản Amazon EC2:

1. Đăng nhập vào AWS Management Console và điều hướng đến Amazon EC2.

2. Khởi chạy một phiên bản với cấu hình sau:

   - Tên phiên bản: KVS_AmebaPlumerAI_poc
   - Hình ảnh Ứng dụng và Hệ điều hành: Ubuntu Server 22.04 LTS (HVM)
   - Loại phiên bản: t3.large
   - Tạo một cặp khóa mới để đăng nhập: kvs-plumerai-realtek-keypair
   - Cấu hình lưu trữ: 100GiB
   - Thực hiện các điều kiện tiên quyết kết nối SSH để cho phép lưu lượng SSH đến.

3. Tải script mẫu từ Github:

Sử dụng lệnh sau để đăng nhập vào phiên bản Amazon EC2 của bạn (hãy đảm bảo thay xxx.yyy.zzz bằng địa chỉ IP của phiên bản). Để biết hướng dẫn chi tiết, xem Kết nối với phiên bản Linux của bạn bằng ứng dụng khách SSH.

```bash
ssh -o ServerAliveInterval=60 -i kvs-plumerai-realtek-keypair.pem ubuntu@54.xxx.yyy.zzz
```

```bash
git clone https://github.com/aws-samples/sample-kvs-edge_ai-video-streaming-solution.git
cd ./sample-kvs-edge_ai-video-streaming-solution/KVS_Ameba_Plumerai
```

### Lấy thư viện Plumerai:

Sử dụng biểu mẫu liên hệ với chúng tôi của Plumerai, gửi yêu cầu để nhận một bản sao gói demo của họ. Khi bạn đã có gói, thay thế thư mục "plumerai" bằng nó trong phiên bản Amazon EC2. Cấu trúc thư mục được cập nhật nên là như sau:

```
KVS_Ameba_Plumerai/
├── plumerai/
│   ├── include/
│   ├── lib/
│   └── models/
└── ...
```

### Lấy Ameba SDK:

Vui lòng liên hệ với Realtek để lấy Ameba Pro2 SDK mới nhất. Trong cấu trúc thư mục, thay thế "ambpro2_sdk" trong Amazon EC2. Cấu trúc thư mục nên là như sau:

```
KVS_Ameba_Plumerai/
├── ambpro2_sdk/
│   ├── component/
│   ├── project/
│   └── ...
└── ...
```

### Cài đặt các phụ thuộc và cấu hình môi trường

Chạy script `setup_kvs_ameba_plumerai.sh` trong thư mục `sample-kvs-edge_ai-video-streaming-solution` từ kho lưu trữ Github:

```bash
chmod +x setup_kvs_ameba_plumerai.sh
./setup_kvs_ameba_plumerai.sh
```

Script sẽ tự động cài đặt các phụ thuộc Linux, xây dựng toolchain Realtek, chạy các bản vá cần thiết của Plumerai, sao chép tệp mô hình và tải xuống Kinesis Video Streams Producer SDK. Nếu bạn gặp lỗi trong quá trình này, vui lòng liên hệ với Realtek hoặc Plumerai để được hỗ trợ kỹ thuật.

---

### Cấu hình mẫu trong Kinesis Video Streams Producer SDK

Sử dụng các thông tin sau để cấu hình thông tin xác thực AWS, tên luồng và khu vực AWS. Những thông tin này có thể được tìm thấy trong tệp `component/example/kvs_producer_mmf/sample_config.h`.

```c
// KVS general configuration
#define AWS_ACCESS_KEY "xxxxx"
#define AWS_SECRET_KEY "xxxxx"

// KVS stream configuration
#define KVS_STREAM_NAME "kvs-plumerai-realtek-stream"
#define AWS_KVS_REGION "us-east-1"
#define AWS_KVS_SERVICE "kinesisvideo"
#define AWS_KVS_HOST AWS_KVS_SERVICE "." AWS_KVS_REGION ".amazonaws.com"
```

Bình luận bỏ `example_kvs_producer_mmf();` và `example_kvs_producer_with_object_detection();` trong tệp `/home/ubuntu/KVS_Ameba_Plumerai/ambpro2_sdk/component/example/kvs_producer_mmf/app_example`.

```c
//example_kvs_producer_mmf();
//example_kvs_producer_with_object_detection();
```

---

## Biên dịch và xây dựng

Sử dụng mã sau để điều hướng đến thư mục KVS_Ameba_Plumerai và biên dịch, xây dựng các cập nhật.

```bash
cd ./KVS_Ameba_Plumerai
cmake -DVIDEO_EXAMPLE=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=../ambpro2_sdk/project/realtek_amebapro2_v0_example/GCC-RELEASE/toolchain.cmake -Sambpro2_sdk/project/realtek_amebapro2_v0_example/GCC-RELEASE -Bbuild
cmake --build build --target flash_nn
```

---

## Chạy mẫu trong Ameba Pro2

### Tải xuống và flash hình ảnh nhị phân:

1. Tải xuống hình ảnh nhị phân `flash_ntz.nn.bin` về máy cục bộ từ phiên bản Amazon EC2. Ví dụ, chạy lệnh sau trên máy cục bộ (hãy đảm bảo cập nhật lệnh để bao gồm địa chỉ IP của bạn và đường dẫn thư mục chính xác):

```bash
scp -i kvs-keypair.pem ubuntu@54.64.xxx.xxx:/home/ubuntu/sample-kvs-edge_ai-video-streaming-solution/KVS_Ameba_Plumerai/build/flash_ntz.nn.bin ./flash_ntz.nn.bin
```

2. Kết nối Ameba Pro2 MCU với máy cục bộ qua USB và nhấn các nút ở cả hai bên để vào chế độ tải xuống. Sử dụng công cụ hình ảnh Ameba Pro2 từ Realtek để flash hình ảnh nhị phân và khởi động lại nó.

Ví dụ, sử dụng lệnh sau trên Microsoft Windows (vui lòng cập nhật đường dẫn của riêng bạn đến công cụ và số cổng COM):

```bash
C:\Users\Administrator\Desktop\Pro2_PG_tool_v1.3.0>.\uartfwburn.exe -p COM3 -f flash_ntz.nn.bin -b 2000000 -U
```

Hoặc sử dụng lệnh sau trên macOS:

```bash
./uartfwburn.arm.darwin -p /dev/cu.usbserial-110 -f ./flash_ntz.nn.bin -b 3000000
```

Khi quá trình hoàn tất, bạn sẽ nhận được nhật ký đầu ra tương tự như sau:

```
[INFO] Flash firmware successfully!
```

---

### Cấu hình WiFi:

1. Nhấn nút reset, nằm cạnh chỉ báo LED đỏ.

2. Sử dụng công cụ serial và cấu hình nó như sau:

   - Tốc độ baud = 115200
   - Bit dữ liệu = 8
   - Parity = None
   - Bit dừng = 1, XON_OFF

3. Dán thông tin cấu hình WiFi (hãy đảm bảo thay đổi thông tin cụ thể cho mạng của bạn):

```
ATW0=myHotspotName
ATW1=myPassword
ATWC
```

Khi bạn hoàn tất, nhấn Enter.

Khi quá trình hoàn tất, bạn sẽ nhận được nhật ký đầu ra tương tự như sau:

```
[INFO] WiFi connected successfully!
```

---

## Xác minh video trên AWS Management Console

Giữ Ameba Pro2 được kết nối với cổng USB và hướng camera để ghi lại các chuyển động của con người.

Mở Kinesis Video Streams Console và chọn luồng video bạn đã tạo. Bạn sẽ thấy video với các hộp giới hạn (bounding boxes) hiển thị kết quả phát hiện.

Các đoạn video với các hộp giới hạn cho con người và khuôn mặt của họ giờ đây đã được thu nhận thành công và lưu trữ bền vững bởi dịch vụ.

---

## Tóm tắt hiệu suất

Theo kết quả kiểm tra trong phòng thí nghiệm, ứng dụng tại biên chỉ yêu cầu 1,5MB dung lượng ROM và được tối ưu hóa cho NPU của Ameba Pro2. Firmware đạt tốc độ xử lý khoảng 10 khung hình mỗi giây trong khi chỉ tiêu tốn 20% CPU. Điều này để lại dung lượng cho các ứng dụng bổ sung.

---

## Chi phí và dọn dẹp

Thông thường, bạn sẽ hoàn thành toàn bộ các bước biên dịch và kiểm tra trong 10 giờ. Tổng chi phí nên dưới 5 USD. Để biết chi tiết về giá cho Amazon EC2, xem [giá của phiên bản theo yêu cầu Amazon EC2](https://aws.amazon.com/ec2/pricing/on-demand/). Để biết chi tiết về giá cho Kinesis Video Streams, xem [giá của Kinesis Video Streams](https://aws.amazon.com/kinesis/video-streams/pricing/). Ứng dụng mẫu của chúng tôi bao gồm ba phần sau:

- Dữ liệu được thu nhận vào Kinesis Video Streams
- Dữ liệu được tiêu thụ từ Kinesis Video Streams bằng HLS
- Dữ liệu được lưu trữ trong Kinesis Video Streams

Để tiết kiệm chi phí, vui lòng xóa các tài nguyên bạn đã tạo:

- Mở Amazon EC2 Console và chấm dứt phiên bản `KVS_AmebaPlumerAI_poc`.
- Mở Kinesis Video Streams Console và xóa luồng `kvs-plumerai-realtek-stream`.

---

## Kết luận

Để biết thêm hướng dẫn về các ứng dụng video, xem:

- [Tiêu thụ dữ liệu truyền thông từ Kinesis Video Streams](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/consuming-streams.html)
- [Các ví dụ về Amazon Kinesis Video Streams](https://github.com/aws-samples/amazon-kinesis-video-streams-examples)
- [amazon-kinesis-video-streams-media-viewer](https://github.com/aws-samples/amazon-kinesis-video-streams-media-viewer) để thử nghiệm và kiểm tra nhanh.
- Xem video [Kết nối camera của bạn bằng Amazon Kinesis Video Streams cho WebRTC trong 10 phút](https://www.youtube.com/watch?v=example) trên kênh YouTube AWS IoT.

Sự hợp tác giữa Amazon Kinesis Video Streams, Realtek và Plumerai mang đến một giải pháp bảo mật gia đình tiên tiến, tận dụng AI thị giác tại biên và khả năng truyền tải video nâng cao. Hệ thống tích hợp này đáp ứng nhu cầu ngày càng tăng về các giải pháp video AI/ML phức tạp trong cả thị trường nhà thông minh và giám sát doanh nghiệp. Giải pháp hợp tác này cũng minh họa tiềm năng cho các cải tiến dựa trên AI trong bảo mật gia đình và doanh nghiệp bằng cách cung cấp khả năng giám sát nâng cao, xử lý hiệu quả và tích hợp đám mây liền mạch.

Với khả năng phát hiện AI trực tiếp trên thiết bị, cách tiếp cận ưu tiên biên này có nghĩa là dữ liệu video của bạn vẫn ở cục bộ cho đến khi cần thiết, bảo vệ quyền riêng tư trong khi giảm đáng kể việc sử dụng băng thông. Khi nhu cầu về các giải pháp video thông minh tiếp tục tăng, công nghệ này thiết lập một tiêu chuẩn mới cho những gì có thể đạt được trong lĩnh vực giám sát thông minh và hệ thống giám sát video.
