---
title: "Blog 2"
date: 2025-11-13
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Quản lý Hồ sơ và Tùy chỉnh Amazon Q Developer trong Các Tổ chức Lớn

Khi các tổ chức mở rộng nỗ lực phát triển phần mềm, việc sử dụng các trợ lý lập trình AI hiểu được mô hình và tiêu chuẩn nội bộ sẽ giúp quy trình phát triển hiệu quả hơn, đồng thời nâng cao chất lượng phần mềm được triển khai. Amazon Q Developer Pro giúp giải quyết thách thức này bằng cách cho phép tổ chức tùy chỉnh trợ lý AI dựa trên mã nguồn và quy trình phát triển độc quyền của họ. Thông qua Amazon Q Developer Profiles, các nhóm có thể quản lý quyền truy cập và phân phối tùy chỉnh Amazon Q một cách hiệu quả trên nhiều vùng AWS (regions) và AWS Identity Centers khác nhau.

Trong bài đăng này, chúng tôi sẽ khám phá các cách tiếp cận khác nhau để triển khai và quản lý các Amazon Q Developer profiles và Amazon Q customizations trên các tổ chức lớn.

Sử dụng một ví dụ với nhiều business units, chúng tôi sẽ khám phá các phương pháp để quản lý access controls và customization governance đồng thời đáp ứng các yêu cầu về security và compliance.

Amazon Q customization hiện đã có sẵn ở cả hai vùng US East (N. Virginia) và EU Central (Frankfurt), mang lại cho các đội ngũ sự linh hoạt hơn trong việc tạo và triển khai các customizations gần hơn với các operational hubs của họ đồng thời đáp ứng các yêu cầu về regional data residency.

Bài blog này không nhằm mục đích cung cấp các khuyến nghị về cách cấu trúc AWS accounts của bạn hoặc phân chia Q Developer subscriptions.

Thay vào đó, mục tiêu của chúng tôi là khám phá toàn bộ khả năng của Q Developer Customizations trong một kịch bản toàn diện thể hiện the current art of the possible.

---

## Tình huống phân phối đăng ký Amazon Q Developer Pro

Sơ đồ sau minh họa một cấu trúc mẫu của AWS Organizations với một Tài khoản Quản lý (Management Account) và bốn Đơn vị Tổ chức (Organizational Units - OUs). Đây là một kịch bản doanh nghiệp phổ biến với ba đơn vị kinh doanh, mỗi đơn vị yêu cầu đăng ký Amazon Q Developer Pro riêng và các tùy chỉnh riêng.

> _Hình 1: Cấu trúc AWS Organizations và Hệ thống phân cấp Tài nguyên_

OU Infrastructure có một Tài khoản Quản trị Ủy quyền (Delegated Admin Account) với quyền truy cập được ủy quyền vào AWS IAM Identity Center. Có ba OU bổ sung: Alpha, Bravo và Charlie, mỗi OU có ít nhất một đăng ký Amazon Q Developer Pro. Tài khoản Alpha có đăng ký Amazon Q Developer ở cả khu vực US East (N. Virginia) và EU Central (Frankfurt).

Hãy nghĩ về mỗi đơn vị kinh doanh như một hệ sinh thái riêng trong tổ chức của bạn. Khi bạn cung cấp các đăng ký Amazon Q Developer Pro riêng cho các OU khác nhau, bạn thực chất đang cung cấp cho mỗi đơn vị một trợ lý AI cá nhân hóa riêng. Sự tách biệt này có giá trị vì nó cho phép mỗi nhóm làm việc độc lập trong khi vẫn duy trì các yêu cầu và quy trình làm việc cụ thể của họ.

OU Charlie duy trì một phiên bản tài khoản riêng của IAM Identity Center cho Amazon Q Developer Pro. Trong hầu hết các trường hợp, chúng tôi khuyến nghị sử dụng một phiên bản tổ chức của IAM Identity Center với Amazon Q Developer Pro, nhưng có một vài tình huống mà các phiên bản tài khoản thành viên có thể phù hợp, chẳng hạn như: khi bạn không có một nhà cung cấp danh tính duy nhất, hoặc khi bạn chưa quyết định triển khai cho toàn bộ tổ chức và muốn sử dụng Amazon Q chỉ cho tài khoản AWS mà bạn kiểm soát.

{{% notice info %}}
**Lưu ý:** Khi một nhà phát triển có một người dùng trong một hồ sơ Amazon Q được liên kết với hai phiên bản IAM Identity Center khác nhau (Bravo và Charlie), họ sẽ có hai đăng ký người dùng và bị tính phí hai lần. Tuy nhiên, nếu họ thuộc hai hồ sơ Amazon Q khác nhau trong hai tài khoản khác nhau (Alpha và Bravo) nhưng dưới cùng một IAM Identity Center, họ sẽ chỉ bị tính phí một lần.
{{% /notice %}}

Trong ví dụ của chúng ta, OU Charlie yêu cầu chi phí vận hành bổ sung trong việc quản lý thông tin xác thực và quy trình xác thực riêng biệt. Ngoài ra, bảng điều khiển và cài đặt quản trị sẽ chỉ được liên kết với người dùng và nhóm trong tài khoản này.

Từ góc độ quản trị, thay vì cố gắng quản lý một cấu hình tập trung duy nhất nhằm đáp ứng nhu cầu của mọi người, bạn có thể phân phối quyền quản trị cho từng đơn vị kinh doanh và ủy quyền trách nhiệm cho các nhóm riêng lẻ.

Điều này giống như việc có các phòng ban chuyên môn khác nhau trong một bệnh viện – mặc dù tất cả đều thuộc cùng một tổ chức và có thể hợp tác khi cần thiết, mỗi phòng ban có các công cụ và giao thức chuyên biệt riêng giúp họ thực hiện các chức năng cụ thể một cách hiệu quả hơn.

---

## Cách tiếp cận chiến lược đối với Tùy chỉnh thông qua Hồ sơ Q Developer

> _Hình 2: Liên kết của các nhà phát triển với Đăng ký Amazon Q Developer Pro, Tùy chỉnh và IAM Identity Center_

Hồ sơ Amazon Q Developer là cách các nhà phát triển kết nối với các đăng ký Amazon Q Developer khác nhau thông qua IDE của họ. Mỗi hồ sơ đại diện cho một sự kết hợp độc đáo giữa một đăng ký Amazon Q Developer và các tùy chỉnh liên quan. Sau khi xác thực, các nhà phát triển có thể dễ dàng chọn hoặc chuyển đổi giữa các hồ sơ trong IDE của họ để truy cập vào các tùy chỉnh khác nhau.

Hãy cùng xem xét một số kịch bản trong kiến trúc này.

---

### Kịch bản 1 – Người dùng truy cập hai tùy chỉnh khác nhau được liên kết với một Phiên bản IAM Identity Center duy nhất trong tài khoản quản lý

Các nhà phát triển từ Nhóm Orange có quyền truy cập vào các tùy chỉnh của tài khoản Alpha có thể cấu hình hai hồ sơ Amazon Q Developer khác nhau trong IDE của họ:

- Một "Hồ sơ US" kết nối với đăng ký US East trong tài khoản Alpha.
- Một "Hồ sơ EU" kết nối với đăng ký EU Central trong tài khoản Alpha.

Việc chuyển đổi giữa các bộ tùy chỉnh khác nhau chỉ đơn giản là chọn hồ sơ phù hợp trong IDE của họ.

> _Hình 3: Giao diện IDE hiển thị bảng tùy chỉnh Amazon Q Developer. Các nhà phát triển chuyển đổi giữa Hồ sơ US và EU cùng với các tùy chỉnh của chúng_

{{% notice info %}}
**Lưu ý:** Mặc dù các nhà phát triển có thể truy cập nhiều tùy chỉnh thông qua các hồ sơ Amazon Q Developer khác nhau, họ chỉ phải chịu một chi phí đăng ký người dùng duy nhất vì họ đang sử dụng phiên bản tổ chức của IAM Identity Center. Điều này là do đăng ký được gắn với danh tính người dùng trong phiên bản tổ chức IAM Identity Center, không phụ thuộc vào số lượng hồ sơ hoặc tùy chỉnh mà họ truy cập.
{{% /notice %}}

---

### Kịch bản 2 – Người dùng truy cập hai tùy chỉnh khác nhau được liên kết với một Phiên bản IAM Identity Center duy nhất trong tài khoản quản lý

Tương tự, các nhà phát triển từ Nhóm Blue cũng có thể cấu hình nhiều hồ sơ:

- Một hồ sơ để truy cập các tùy chỉnh của Alpha và Bravo thông qua phiên bản AWS IAM Identity Center của tài khoản quản lý.
- Một hồ sơ riêng để truy cập các tùy chỉnh của Charlie thông qua phiên bản tài khoản thành viên AWS IAM Identity Center.

Khi các nhà phát triển có quyền truy cập vào nhiều tùy chỉnh trong cùng một cấu hình IAM Identity Center và khu vực, họ có thể chuyển đổi giữa các hồ sơ trong IDE mà không cần xác thực lại.

> _Hình 4: Giao diện IDE hiển thị bảng tùy chỉnh Amazon Q Developer. Khi được xác thực thông qua AWS IAM Identity Center Tổ chức, các nhà phát triển Blue có thể thấy cả tùy chỉnh của Alpha và Bravo._

Tuy nhiên, như trường hợp của các nhà phát triển Blue đã thể hiện, việc chuyển đổi giữa các hồ sơ sử dụng các cấu hình IAM Identity Center khác nhau (Tổ chức so với Phiên bản Tài khoản) vẫn yêu cầu xác thực lại.

> _Hình 5: Giao diện IDE hiển thị bảng tùy chỉnh Amazon Q Developer. Khi được xác thực thông qua Phiên bản AWS IAM Identity Center của Tài khoản, các nhà phát triển Blue chỉ có thể thấy các tùy chỉnh của Charlie._

{{% notice warning %}}
**Lưu ý:** Trong kịch bản này, các nhà phát triển sẽ phải chịu hai chi phí đăng ký người dùng riêng biệt vì họ đang truy cập các tùy chỉnh thông qua hai cấu hình IAM Identity Center khác nhau (tổ chức và phiên bản tài khoản). Như đã đề cập ở trên, kịch bản này không được khuyến nghị trừ khi nó phù hợp với một số tình huống cụ thể và được trình bày ở đây chỉ để minh họa cách thức hoạt động của cơ chế xác thực và chuyển đổi hồ sơ giữa các cấu hình IAM Identity Center khác nhau.
{{% /notice %}}

Một kịch bản để tạo các tùy chỉnh mã cụ thể cho mỗi hồ sơ là các nhà phát triển trong Nhóm Alpha có thể cần Q hiểu các thư viện cụ thể và quy ước mã hóa nội bộ cho Java, trong khi các nhà phát triển Nhóm Bravo có thể cần Q thông thạo các công nghệ độc quyền và tiêu chuẩn phát triển với Python. Với các hồ sơ và tùy chỉnh riêng biệt, mỗi nhóm sẽ có phiên bản "đặc trưng" riêng của Q, hiểu được bối cảnh của họ.

Đối với các nhà phát triển Blue có quyền truy cập vào các tùy chỉnh của Alpha, Bravo và Charlie, họ cần thiết lập các hồ sơ riêng biệt vì các tùy chỉnh này thuộc về các cấu hình IAM Identity Center và khu vực AWS khác nhau. Việc chuyển đổi giữa các hồ sơ này yêu cầu xác thực lại do liên quan đến các cấu hình IAM Identity Center khác nhau.

---

## Tóm tắt quyền truy cập

| Nhóm Phát triển | AWS IAM Identity Center | Tùy chỉnh                                                                           |
| --------------- | ----------------------- | ----------------------------------------------------------------------------------- |
| Orange          | Phiên bản tổ chức       | Tùy chỉnh Alpha ở US East (N. Virginia)<br>Tùy chỉnh Alpha ở EU Central (Frankfurt) |
| Blue            | Phiên bản tổ chức       | Tùy chỉnh Alpha ở US East (N. Virginia)<br>Tùy chỉnh Bravo                          |
| Blue            | Phiên bản tài khoản     | Tùy chỉnh Charlie                                                                   |
| Grey            | Phiên bản tổ chức       | Tùy chỉnh Bravo                                                                     |

Bạn có thể quản lý quyền truy cập vào các tùy chỉnh cụ thể của Amazon Q Developer Pro bằng cách thêm các người dùng và nhóm được chọn đã có quyền truy cập vào các đăng ký Amazon Q Developer Pro trong cùng một Identity Center. Kiểm soát quyền truy cập chi tiết này cho phép bạn tạo ra các tùy chỉnh được nhắm mục tiêu, chỉ có thể truy cập bởi các thành viên hoặc nhóm cụ thể trong tổ chức của bạn.

---

## Kết luận

Trong bài viết này, chúng tôi đã khám phá các chiến lược toàn diện để triển khai các tùy chỉnh Amazon Q Developer trong các tổ chức lớn. Chúng tôi đã trình bày cách các hồ sơ Amazon Q Developer cung cấp một cách linh hoạt để quản lý quyền truy cập vào các tùy chỉnh khác nhau trên các khu vực AWS và cấu hình IAM Identity Center. Bằng cách tích hợp các kho mã độc quyền, thiết lập quản trị tùy chỉnh và triển khai các vòng phản hồi liên tục, các doanh nghiệp có thể tối đa hóa giá trị của trợ lý phát triển hỗ trợ AI trong khi duy trì chất lượng mã và tiêu chuẩn phát triển.

Con đường phía trước phụ thuộc vào vị trí của bạn trong hành trình tùy chỉnh Amazon Q Developer. Nếu bạn mới bắt đầu, hãy bắt đầu với việc đánh giá rõ ràng codebase của bạn và lập kế hoạch cho phương pháp tùy chỉnh trước khi triển khai. Đối với những người dùng hiện tại, hãy xem xét lại các tùy chỉnh và cấu hình hồ sơ hiện có để xác định cơ hội tối ưu hóa.

Trong cả hai trường hợp, hãy triển khai quản trị tùy chỉnh mà chúng tôi đã thảo luận, điều chỉnh chúng theo các mô hình phát triển và cấu trúc nhóm cụ thể của bạn. Hãy nhớ rằng tùy chỉnh phát triển cùng với codebase của bạn – việc tinh chỉnh thường xuyên giúp đảm bảo trợ lý AI của bạn vẫn hiệu quả khi ứng dụng của bạn phát triển và các thực tiễn phát triển trưởng thành. Dù bạn mới bắt đầu với các tùy chỉnh Amazon Q Developer hay đang tối ưu hóa các triển khai hiện có, những thực tiễn này có thể giúp phát triển một trợ lý AI thực sự hiểu và phù hợp với môi trường phát triển độc đáo của tổ chức bạn.
