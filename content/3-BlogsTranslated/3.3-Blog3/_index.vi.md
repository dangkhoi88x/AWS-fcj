---
title: "Blog 3"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Tối đa hóa giá trị dữ liệu SAP bằng Amazon Q Business và Amazon Bedrock Generative AI – Phần 1

Các tổ chức đã vượt qua giai đoạn khám phá trí tuệ nhân tạo tổng quát (Generative AI - Gen AI) để bắt đầu sử dụng nó một cách tích cực nhằm tạo ra giá trị kinh doanh. Nếu bạn đang làm việc với các hệ thống SAP và tự hỏi nên bắt đầu từ đâu hoặc đang tìm kiếm ý tưởng về các trường hợp sử dụng, bạn không hề đơn độc. Bài viết này khám phá các trường hợp sử dụng kinh doanh phổ biến cho dữ liệu SAP và các cách tiếp cận khác nhau có sẵn khi sử dụng các dịch vụ AWS cho Gen AI.

Đối với Gen AI, các tổ chức có nhiều lựa chọn để tận dụng các phương pháp công nghệ cho dữ liệu ERP của họ:

- AI được tích hợp vào phần mềm (chẳng hạn như SAP Joule)
- Các giải pháp nền tảng như SAP Gen AI Hub, cung cấp quyền truy cập vào các mô hình Amazon Bedrock như Anthropic Claude và Amazon Nova
- Các giải pháp được xây dựng sẵn để phân tích dữ liệu, thường có các giải pháp dành riêng cho ngành như Nhân sự, Pháp lý, v.v.
- Xây dựng và tùy chỉnh các ứng dụng sử dụng công nghệ Gen AI với dữ liệu kinh doanh của riêng họ

Trong loạt bài viết này, chúng tôi tập trung vào việc xây dựng các giải pháp riêng với các trường hợp sử dụng sau, liên quan đến việc sử dụng dữ liệu SAP với AWS:

- Phân tích báo cáo SAP Early Watch Analysis (EWA) bằng ngôn ngữ tự nhiên
- Tự động hóa xử lý hóa đơn bằng xử lý tài liệu thông minh (Intelligent Document Processing - IDP)
- Thu thập thông tin chi tiết kinh doanh từ dữ liệu tài chính có cấu trúc bằng ngôn ngữ tự nhiên

---

## Dịch vụ Generative AI của AWS

Trong bài viết này, chúng tôi sử dụng các dịch vụ Generative AI sau từ AWS:

- **Amazon Q Business**: một trợ lý được quản lý hoàn toàn, hỗ trợ bởi Gen AI, mà bạn có thể cấu hình để trả lời câu hỏi, cung cấp tóm tắt, tạo nội dung và hoàn thành các nhiệm vụ dựa trên dữ liệu doanh nghiệp của bạn.

- **Amazon Bedrock**: một dịch vụ được quản lý hoàn toàn, cung cấp các mô hình nền tảng (Foundation Models - FMs) hiệu suất cao từ các công ty AI hàng đầu và Amazon thông qua một API thống nhất. Tính năng Knowledge Bases của Bedrock cho phép bạn cung cấp thông tin ngữ cảnh từ các nguồn dữ liệu riêng của công ty cho các mô hình nền tảng và tác nhân để mang lại các phản hồi liên quan, chính xác và được tùy chỉnh hơn.

---

## Phân tích báo cáo SAP Early Watch Analysis (EWA) bằng ngôn ngữ tự nhiên

Báo cáo EWA chứa các chi tiết quan trọng về tình trạng của hệ thống SAP và có thể được sử dụng để phân tích xu hướng theo thời gian. Vì các tài liệu này chứa thông tin chi tiết, một báo cáo thường có thể dài hàng chục trang; ví dụ: mẫu báo cáo EWA S/4HANA công khai từ SAP có 213 trang. Việc phân tích báo cáo, kết hợp các khuyến nghị và nắm bắt xu hướng theo thời gian là một nhiệm vụ tẻ nhạt, có thể được tự động hóa bằng Amazon Q Business.

Hình 1 minh họa sơ đồ kiến trúc của một giải pháp tiềm năng, và phần này sẽ trình bày các bước để xây dựng nó.

- Tải báo cáo EWA của tổ chức bạn (từ tất cả các hệ thống SAP) lên Amazon S3 bucket
- Tạo ứng dụng Amazon Q Business, Q Index và thêm S3 làm nguồn dữ liệu
- Sử dụng giao diện web để phân tích báo cáo EWA

> _Hình 1: Kiến trúc của Ứng dụng Phân tích EWA_

---

### 1. Tải báo cáo EWA của tổ chức bạn (từ tất cả các hệ thống SAP) lên Amazon S3 bucket

Bạn có thể tạo một Amazon S3 bucket mới hoặc sử dụng một bucket trống hiện có. Tải xuống tất cả các báo cáo EWA và lưu trữ chúng trong Amazon S3 bucket (ghi chú lại tên bucket S3, ví dụ: sap-early-watch).

{{% notice info %}}
**Lưu ý:** Nếu bạn muốn thử nghiệm giải pháp này với các báo cáo công khai, bạn có thể sử dụng các báo cáo mẫu công khai có sẵn.
{{% /notice %}}

---

### 2. Tạo ứng dụng Amazon Q Business, Q Index và thêm S3 làm nguồn dữ liệu

Hãy làm theo các bước sau để tạo Ứng dụng Amazon Q Business:

- Điều hướng đến Amazon Q Business trong AWS Console và nhấp vào nút Get Started.
- Nhấp vào Create Application và sử dụng biểu mẫu để tạo ứng dụng như minh họa trong Hình 2.
- Nên sử dụng AWS IAM Identity Center theo các thực tiễn tốt nhất của AWS, nhưng bạn cũng có thể sử dụng Nhà cung cấp danh tính AWS IAM nếu bạn quản lý danh tính người dùng bên ngoài AWS.
- Chọn một người dùng để bắt đầu nhanh, bạn cũng có thể chỉnh sửa sau (tùy chọn).
- Sau khi hoàn tất biểu mẫu, nhấp vào nút create, ứng dụng web sẽ được tạo trong vài giây.

> _Hình 2: Tạo ứng dụng trong Amazon Q Business_

Hãy làm theo các bước sau để thêm Amazon Q Index:

- Điều hướng đến ứng dụng bạn vừa tạo (ABC-EWA-Analyzer) và đi đến Data Sources như minh họa trong Hình 3.

> _Hình 3: Nguồn dữ liệu cho Amazon Q Business_

- Điều hướng đến tùy chọn "Add an index" và tạo một chỉ mục mới (như minh họa trong Hình 4).
- Hoàn tất biểu mẫu và thêm chỉ mục (quá trình này có thể mất đến 20 phút để hoàn tất).

> _Hình 4: Thêm chỉ mục vào ứng dụng ABC-EWA-Analyzer_

Hãy làm theo các bước sau để thêm S3 làm nguồn dữ liệu:

- Sau khi tạo chỉ mục, nhấp vào add data source (như minh họa trong Hình 5) và chọn bucket S3 nơi lưu trữ báo cáo EWA (sap-early-watch trong trường hợp này).

> _Hình 5: Thêm Amazon S3 làm nguồn dữ liệu cho ứng dụng_

- Chọn loại đồng bộ và lịch trình bạn muốn.
- Hoàn tất biểu mẫu nguồn dữ liệu và nhấp vào nút "add data source" để hoàn thành nhiệm vụ.
- Sau khi nguồn dữ liệu được thêm, bạn có thể sử dụng "sync now" để thực hiện đồng bộ ban đầu, có thể mất đến 20 phút tùy thuộc vào số lượng và độ dài của các báo cáo.

---

### 3. Sử dụng giao diện web để phân tích báo cáo EWA

Sau khi hoàn tất đồng bộ, sử dụng URL được triển khai (hiển thị trong ứng dụng) để truy cập ứng dụng EWA Analyzer. Bạn có thể tùy chỉnh trải nghiệm web bằng cách bao gồm tiêu đề riêng, lời chào và logo công ty để hiển thị trong ứng dụng.

Hình 6 minh họa giao diện của ứng dụng tùy chỉnh có thể trông như thế nào (đảm bảo bạn đã chọn kiến thức công ty trong nút chuyển đổi cho các nguồn kiến thức).

> _Hình 6: Ứng dụng Early Watch Analyzer_

Giờ đây, bạn có thể sử dụng ứng dụng để thực hiện phân tích (như các phân tích được đề cập dưới đây), điều này không chỉ tăng năng suất mà còn giúp bạn đào sâu hơn vào việc điều tra.

- Tìm các truy vấn cơ sở dữ liệu chạy lâu nhất.
- Kiểm tra xem cơ sở dữ liệu và hệ điều hành của bạn có các bản cập nhật mới nhất không.
- Nhận các gợi ý cụ thể để cải thiện hiệu suất như được đề cập trong báo cáo.

Hình 7 cho thấy một ví dụ về tương tác với ứng dụng để đặt các câu hỏi liên quan về báo cáo EWA được tải lên bucket S3. Cũng lưu ý khía cạnh minh bạch của Amazon Q, hiển thị các nguồn được sử dụng để tạo câu trả lời.

> _Hình 7: Phân tích EWA bằng ứng dụng Amazon Q Business_

{{% notice tip %}}
**Mẹo:** Bạn cũng có thể sử dụng ứng dụng Amazon Q Business để phân tích báo cáo kiểm tra sẵn sàng SAP, báo cáo định cỡ, v.v.
{{% /notice %}}

Điều này kết thúc trường hợp sử dụng phân tích EWA, nơi chúng tôi đã thảo luận cách bạn có thể:

- Giảm thời gian xem xét thủ công các báo cáo EWA nhiều trang.
- Cho phép phân tích nhanh tình trạng hệ thống trên nhiều hệ thống SAP.
- Theo dõi xu hướng và khuyến nghị theo thời gian.

---

## Tự động hóa xử lý hóa đơn bằng xử lý tài liệu thông minh (IDP)

Việc xử lý thủ công truyền thống đối với hóa đơn giấy và kỹ thuật số không chỉ tốn thời gian mà còn dễ xảy ra lỗi, dẫn đến việc thanh toán bị trì hoãn, rủi ro về tuân thủ và sử dụng không hiệu quả các nguồn lực con người quý giá. Khi doanh nghiệp mở rộng quy mô, khối lượng hóa đơn tăng theo cấp số nhân, khiến việc xử lý thủ công ngày càng không bền vững. Đây là lúc bạn có thể tận dụng giải pháp trí tuệ nhân tạo tổng quát (Generative AI) để giảm bớt gánh nặng. Bạn cũng có thể sử dụng giải pháp tự động hóa quy trình SAP Build cùng với các công nghệ AWS.

### Xử lý và phân loại hóa đơn

Nếu tổ chức của bạn làm việc với hóa đơn giấy hoặc hóa đơn dạng PDF/email, bạn sẽ hiểu được sự đánh đổi giữa hiệu quả và độ chính xác. Xử lý thủ công cũng không thể mở rộng. Phần này trình bày cách sử dụng tính năng Tự động hóa Dữ liệu của Amazon Bedrock cho các trường hợp sử dụng sau:

- Tự động hóa xử lý hóa đơn giấy/PDF trong hệ thống SAP hoặc hệ thống bên thứ ba.
- Phân loại hóa đơn thành các nhóm khác nhau để phân tích thêm.

{{% notice info %}}
**Lưu ý:** Nếu bạn đăng hóa đơn trực tiếp trong SAP, bạn cũng có thể sử dụng một cách tiếp cận thay thế như sử dụng SAP Build Process Automation (một dịch vụ Business Technology Platform có sẵn trên AWS) để tự động hóa xử lý.
{{% /notice %}}

Tự động hóa Dữ liệu Bedrock (Bedrock Data Automation - BDA) đơn giản hóa việc trích xuất dữ liệu từ các tài liệu không có cấu trúc (như hóa đơn) và phương tiện truyền thông bằng cách tận dụng Generative AI để tự động chuyển đổi dữ liệu đa phương thức thành các định dạng có cấu trúc. Bạn có thể sử dụng BDA để xử lý và phân loại hóa đơn như sau:

- Trích xuất, chuẩn hóa và xác thực dữ liệu từ hóa đơn.
- Tùy chỉnh đầu ra của BDA và tích hợp với các quy trình làm việc hiện có để xử lý.
- Sử dụng đầu ra chuẩn hóa để phân loại hóa đơn (chẳng hạn theo đơn vị kinh doanh).

Hình 8 minh họa sơ đồ kiến trúc của giải pháp, và phần này trình bày các bước để xây dựng nó.

- Tải hóa đơn của tổ chức bạn lên Amazon S3 bucket.
- Tạo dự án BDA và thêm một blueprint.
- Tích hợp kết quả vào quy trình làm việc hiện có để xử lý.

> _Hình 8: Kiến trúc sử dụng BDA để xử lý và phân loại hóa đơn_

---

### 1. Tải hóa đơn của tổ chức bạn lên Amazon S3 bucket

Bạn có thể tạo một Amazon S3 bucket mới hoặc sử dụng một bucket trống hiện có. Tải tất cả các hóa đơn (PDF, hình ảnh quét, v.v.) lên Amazon S3 bucket.

---

### 2. Tạo dự án BDA và thêm một blueprint

Hãy làm theo các bước sau để tạo dự án BDA và blueprint:

- Điều hướng đến Data Automation trong Amazon Bedrock trên AWS Console và tạo một dự án như minh họa trong Hình 9.

> _Hình 9: Tạo dự án BDA mới_

- Sau khi dự án được tạo, sử dụng tùy chọn kiểm tra để nhập dữ liệu từ bucket S3 nơi lưu trữ hóa đơn, như minh họa trong Hình 10.

> _Hình 10: Kiểm tra xử lý hóa đơn từ AWS Console_

- Hình 11 cho thấy một ví dụ về đầu ra tiêu chuẩn từ BDA, có thể được tải xuống ở định dạng JSON.

> _Hình 11: Ví dụ về đầu ra tiêu chuẩn của BDA_

- Đánh giá đầu ra tiêu chuẩn và nếu bạn cần kiểm soát thêm, sử dụng đầu ra tùy chỉnh (như minh họa trong Hình 12) và tạo một blueprint bằng "blueprint prompt"; bạn cũng có thể chọn một blueprint do AWS cung cấp từ danh sách.

> _Hình 12: Đầu ra tùy chỉnh và blueprint của BDA_

---

### 3. Tích hợp kết quả vào quy trình làm việc hiện có để xử lý

Sau khi kiểm tra kỹ lưỡng, khi bạn chắc chắn rằng blueprint trích xuất dữ liệu theo cách tổ chức của bạn sử dụng, tích hợp dự án BDA vào quy trình làm việc hiện có để xử lý hóa đơn trong SAP, chẳng hạn như một ứng dụng được tạo bằng các dịch vụ SAP Business Technology Platform (BTP).

**AWS SDK cho SAP ABAP**: Trong quy trình làm việc của bạn, bạn có thể sử dụng AWS SDK cho SAP ABAP để tích hợp liền mạch giữa SAP ABAP và hơn 200 dịch vụ AWS trong các môi trường RISE with SAP, GROW with SAP và tự quản lý.

{{% notice tip %}}
**Mẹo:** Bạn cũng có thể sử dụng đầu ra như một phần của quy trình làm việc Retrieval Augmented Generation (RAG) để truy vấn thông tin đã trích xuất bằng ngôn ngữ tự nhiên.
{{% /notice %}}

Điều này kết thúc trường hợp sử dụng xử lý hóa đơn bằng IDP, nơi chúng tôi đã thảo luận cách bạn có thể:

- Chuyển đổi dữ liệu hóa đơn không có cấu trúc thành các định dạng có cấu trúc.
- Tối ưu hóa việc xử lý hóa đơn giấy và kỹ thuật số.
- Giảm lỗi xử lý thủ công và cải thiện hiệu quả.
- Cho phép phân loại hóa đơn để phân tích thêm.

---

## Phân tích chi phí mẫu

Bảng sau cung cấp một phân tích chi phí mẫu để triển khai giải pháp này trong tài khoản AWS của bạn với các tham số mặc định tại khu vực US East 1 (N. Virginia).

| Dịch vụ AWS                          | Kích thước                      | Chi phí (USD) |
| ------------------------------------ | ------------------------------- | ------------- |
| Amazon Q Business Pro                | Mỗi người dùng mỗi tháng        | $20           |
| Enterprise Index                     | Một đơn vị Index mỗi tháng      | $192.72       |
| Amazon Bedrock Data Automation (BDA) | Xử lý 1000 trang bằng blueprint | $40           |
| Amazon S3                            | 1 TB lưu trữ cho EWA và hóa đơn | $23.55        |

{{% notice tip %}}
**Mẹo:** Chúng tôi khuyên bạn nên tạo một Ngân sách thông qua AWS Cost Explorer để giúp quản lý chi phí. Để biết chi tiết đầy đủ, hãy tham khảo trang web định giá cho từng dịch vụ AWS được sử dụng trong bài viết này.
{{% /notice %}}

---

## Kết luận

Trong bài viết này, chúng tôi đã thảo luận về cách bạn có thể tăng hiệu quả nhân sự và giảm lỗi đối với dữ liệu SAP bằng cách sử dụng các dịch vụ Generative AI của AWS. Trong Phần 2 của bài viết, chúng tôi sẽ thảo luận chi tiết về việc thu thập thông tin chi tiết kinh doanh từ dữ liệu tài chính có cấu trúc bằng ngôn ngữ tự nhiên.

Tìm hiểu thêm về các dịch vụ Generative AI của AWS và bắt đầu với các trường hợp sử dụng của riêng bạn với Amazon Q.

---

## Tham gia Thảo luận về SAP trên AWS

Ngoài nhóm tài khoản khách hàng và các kênh Hỗ trợ AWS của bạn, AWS cung cấp các diễn đàn Hỏi & Đáp công khai trên trang re:Post của chúng tôi. Nhóm Kiến trúc Giải pháp SAP trên AWS của chúng tôi thường xuyên theo dõi chủ đề SAP trên AWS để thảo luận và trả lời các câu hỏi nhằm hỗ trợ bạn. Nếu câu hỏi của bạn không liên quan đến hỗ trợ, hãy cân nhắc tham gia thảo luận tại re:Post và đóng góp vào cơ sở kiến thức cộng đồng.
