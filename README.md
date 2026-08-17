# 23637531_TranQuocThai_capsytem
# CAB System - Dự Án Nền Tảng Đặt Xe Trực Tuyến

> ** Bước 1: Tìm hiểu Nghiệp vụ**
---
## 1. Vấn đề là gì? (Problem Statement)

Công ty ABC hiện đang vận hành dịch vụ đặt xe trực tuyến nhưng hệ thống hiện tại (tổng đài kết hợp ứng dụng đơn giản) đang bộc lộ nhiều điểm nghẽn nghiêm trọng, làm cản trở sự phát triển kinh doanh:

* **Vận hành thủ công:** Việc phân công tài xế chủ yếu dựa vào thao tác thủ công của nhân viên, gây mất thời gian và thiếu tối ưu hóa lộ trình.
* **Trải nghiệm khách hàng kém:** Khách hàng gặp khó khăn trong việc theo dõi trạng thái chuyến đi theo thời gian thực và thiếu sự minh bạch thông tin.
* **Quản lý tài chính phân mảnh:** Thông tin thanh toán chưa được quản lý tập trung, thiếu tích hợp chuẩn hóa với các cổng thanh toán điện tử hiện đại.
* **Khả năng mở rộng hạn chế:** Hệ thống cũ không đáp ứng được khi số lượng người dùng tăng đột biến, khó tích hợp thêm tính năng hoặc dịch vụ mới trong tương lai.
* **Thiếu kiểm soát bảo mật và phân quyền:** Thiếu cơ chế phân quyền rõ ràng cho nhân viên vận hành, dữ liệu cá nhân, vị trí và giao dịch chưa được bảo vệ toàn diện.

---

## 2. Mục tiêu của dự án (Project Objectives)

* **Xây dựng nền tảng CAB mới** trong thời hạn **7 tuần** với kiến trúc linh hoạt, cho phép mở rộng độc lập từng thành phần (Decoupled/Microservices-ready) nhằm dễ dàng bổ sung dịch vụ, cổng thanh toán hoặc nhà cung cấp thông báo mới mà không phải làm lại từ đầu.
* **Tự động hóa quy trình phân công chuyến đi:** Tối ưu hóa việc tìm kiếm và ghép nối tài xế dựa trên vị trí gần nhất và trạng thái sẵn sàng, kèm theo cơ chế dự phòng (fallback) tự động khi tài xế từ chối hoặc không phản hồi.
* **Nâng cao trải nghiệm toàn diện cho các bên:**
  * **Khách hàng:** Dễ dàng đặt xe, theo dõi hành trình trực tuyến, nhận thông báo đa kênh, xem lịch sử và thanh toán linh hoạt.
  * **Tài xế:** Nhận cuốc xe nhanh chóng, cập nhật trạng thái hành trình dễ dàng và minh bạch thu nhập.
  * **Nhân viên vận hành:** Cung cấp giao diện quản trị toàn diện, hệ thống báo cáo kinh doanh chi tiết (doanh thu, số lượng chuyến, tỷ lệ hoàn thành/hủy).
* **Đảm bảo tính ổn định và bảo mật cao:** Hệ thống chịu tải tốt vào giờ cao điểm, phân lập lỗi (lỗi thanh toán hay thông báo không làm sập hệ thống đặt xe), đồng thời thực hiện xác thực, phân quyền và lưu vết (audit log) đầy đủ.

---

## 3. Ai là người tham gia sử dụng hệ thống? (Actors / Users)

Hệ thống phục vụ **3 nhóm người dùng chính** cùng với **các hệ thống bên thứ ba**:

### 👤 1. Khách hàng (Customer)
* **Vai trò:** Người có nhu cầu di chuyển, tạo yêu cầu đặt xe và sử dụng dịch vụ vận chuyển.
* **Chức năng chính:**
  * Đăng ký, đăng nhập và quản lý thông tin cá nhân.
  * Nhập điểm đón, điểm đến, lựa chọn loại xe/dịch vụ.
  * Gửi yêu cầu đặt xe và theo dõi hành trình theo thời gian thực (đang tìm tài xế, tài xế nhận chuyến, thời gian đến dự kiến).
  * Xem lịch sử chuyến đi, chi tiết cước phí.
  * Thực hiện thanh toán (tiền mặt hoặc thanh toán điện tử).
  * Nhận thông báo trạng thái chuyến đi và đánh giá tài xế sau khi hoàn thành.

### 🚗 2. Tài xế (Driver)
* **Vai trò:** Người trực tiếp cung cấp dịch vụ vận chuyển cho khách hàng.
* **Chức năng chính:**
  * Đăng ký tài khoản hoặc nhận tài khoản từ nhân viên vận hành, cập nhật hồ sơ cá nhân và thông tin phương tiện.
  * Chuyển trạng thái hoạt động (Sẵn sàng nhận chuyến / Offline).
  * Nhận thông báo khi có yêu cầu phù hợp, thực hiện chấp nhận hoặc từ chối chuyến đi.
  * Cập nhật trạng thái chuyến trong quá trình thực hiện (đã đến điểm đón, đã đón khách, đang di chuyển, hoàn thành).
  * Cung cấp dữ liệu vị trí liên tục để hệ thống hỗ trợ định vị và điều phối gần nhất.

### 👩‍💻 3. Nhân viên vận hành / Quản trị viên (Operator / Admin)
* **Vai trò:** Quản trị hệ thống, giám sát vận hành và hỗ trợ xử lý sự cố.
* **Chức năng chính:**
  * Quản lý thông tin khách hàng, tài xế, phương tiện và chuyến đi.
  * Theo dõi các chuyến xe đang diễn ra và trạng thái hoạt động của tài xế trên giao diện quản trị.
  * Xử lý ngoại lệ (chuyến bị lỗi, hủy chuyến, tranh chấp thanh toán).
  * Tra cứu lịch sử giao dịch và xem hệ thống báo cáo (số lượng chuyến, doanh thu, tỷ lệ hoàn thành/hủy, hiệu quả tài xế).
  * Thực hiện các thao tác quản trị được kiểm soát chặt chẽ qua phân quyền (RBAC).

### 🔌 4. Hệ thống bên thứ ba (External Systems)
* **Cổng thanh toán điện tử (Payment Gateway):** Xử lý giao dịch thẻ/ví điện tử độc lập, đảm bảo không lưu trữ trực tiếp thông tin nhạy cảm tại hệ thống CAB.
* **Nhà cung cấp dịch vụ thông báo (Notification Provider):** Cung cấp hạ tầng gửi thông báo đa kênh (SMS, Push Notification, Email) đến khách hàng và tài xế.

> ** Bước 2: Stakeholders**

> **Tài liệu Phân tích Nghiệp vụ (Business Analysis) - Bảng Stakeholders**

---

| Stakeholders | Vai trò | Tương tác với hệ thống |
| :--- | :--- | :--- |
| **Ban Giám Đốc & Chủ Đầu Tư**<br>*(Executive Management / Business Sponsors)* | Định hướng chiến lược, phê duyệt ngân sách và quản lý tổng thể dự án trong 7 tuần. | • Theo dõi các báo cáo tổng quan (doanh thu, số lượng chuyến, tỷ lệ hoàn thành/hủy, hiệu quả tài xế).<br>• Giám sát các chỉ số vận hành cốt lõi qua dashboard quản trị. |
| **Khách hàng**<br>*(Customers)* | Người sử dụng dịch vụ đặt xe trực tuyến để di chuyển. | • Đăng ký, đăng nhập và quản lý tài khoản cá nhân.<br>• Nhập điểm đón/đến, chọn loại xe và gửi yêu cầu đặt xe.<br>• Theo dõi trạng thái chuyến đi thời gian thực (tìm tài xế, tài xế nhận chuyến, thời gian đến).<br>• Thanh toán cước phí (tiền mặt/điện tử), xem lịch sử và đánh giá tài xế. |
| **Tài xế**<br>*(Drivers)* | Đối tác trực tiếp cung cấp dịch vụ vận chuyển cho khách hàng. | • Đăng ký tài khoản hoặc nhận tài khoản từ vận hành, cập nhật hồ sơ và phương tiện.<br>• Chuyển trạng thái hoạt động (Sẵn sàng / Offline) và chia sẻ dữ liệu vị trí.<br>• Nhận thông báo cuốc xe, thực hiện chấp nhận hoặc từ chối.<br>• Cập nhật trạng thái chuyến đi (đến điểm đón, đón khách, đang di chuyển, hoàn thành). |
| **Nhân viên vận hành / Quản trị viên**<br>*(Operators / Admins)* | Giám sát vận hành hàng ngày, hỗ trợ xử lý sự cố và ngoại lệ của hệ thống. | • Quản lý thông tin khách hàng, tài xế, phương tiện và chuyến đi trên giao diện quản trị.<br>• Theo dõi chuyến xe đang diễn ra và trạng thái tài xế theo thời gian thực.<br>• Xử lý ngoại lệ (hủy chuyến, lỗi thanh toán, tranh chấp) và tra cứu lịch sử giao dịch.<br>• Thực hiện các thao tác quản trị được kiểm soát bằng phân quyền (RBAC). |
| **Đội ngũ phát triển kỹ thuật**<br>*(Development & Technical Team)* | Phân tích, thiết kế, lập trình, kiểm thử và triển khai hệ thống nền tảng CAB. | • Tương tác qua môi trường phát triển (Dev/Staging/Production), hệ thống quản lý mã nguồn, CI/CD.<br>• Giám sát hiệu năng hệ thống và cấu hình các dịch vụ tích hợp. |
| **Nhà cung cấp cổng thanh toán**<br>*(Payment Gateway Providers)* | Bên thứ ba cung cấp hạ tầng xử lý giao dịch điện tử (thẻ/ví) an toàn. | • Nhận yêu cầu thanh toán qua API từ hệ thống CAB.<br>• Xử lý giao dịch và trả kết quả (thành công/thất bại) về hệ thống mà không lưu dữ liệu nhạy cảm tại CAB. |
| **Nhà cung cấp dịch vụ thông báo**<br>*(Notification Providers)* | Bên thứ ba cung cấp hạ tầng gửi tin nhắn và thông báo đa kênh. | • Nhận yêu cầu từ hệ thống CAB để gửi tin nhắn (SMS, Push Notification, Email) đến khách hàng và tài xế. |



> **Sơ đồ Mermaid (Quy trình Đặt xe và Phân công Tài xế)**

---

## 🗺️ Sơ đồ Quy trình Đặt xe & Phân công (Mermaid Diagram)


```mermaid
flowchart TD
    Start([Bắt đầu]) --> Input[Khách hàng nhập điểm đón, điểm đến và chọn loại xe]
    Input --> CreateReq[Khách hàng gửi yêu cầu đặt xe]
    CreateReq --> SystemProcess[Hệ thống ghi nhận yêu cầu và tính cước dự kiến]
    
    SystemProcess --> FindDriver{Hệ thống tìm tài xế phù hợp gần nhất}
    
    FindDriver -->|Tìm thấy tài xế| SendNoti[Gửi thông báo yêu cầu chuyến đi cho Tài xế]
    FindDriver -->|Không tìm thấy tài xế| NotifyFail[Thông báo rõ ràng cho khách hàng: Không tìm thấy tài xế] --> End([Kết thúc])
    
    SendNoti --> DriverAction{Tài xế phản hồi?}
    
    DriverAction -->|Từ chối hoặc Hết thời gian| CheckMore{Còn tài xế phù hợp khác?}
    CheckMore -->|Còn| FindDriver
    CheckMore -->|Hết| NotifyFail
    
    DriverAction -->|Chấp nhận chuyến| UpdateTrip[Hệ thống cập nhật: Tài xế đã nhận chuyến]
    UpdateTrip --> NotiCustomer[Thông báo cho khách hàng: Tài xế đang đến]
    
    NotiCustomer --> DriverArrive[Tài xế đến điểm đón và đón khách]
    DriverArrive --> InProgress[Chuyến đi đang di chuyển đến điểm đến]
    InProgress --> CompleteTrip[Tài xế hoàn thành chuyến đi]
    
    CompleteTrip --> CalcFinal[Hệ thống tính cước phí chính thức]
    CalcFinal --> PaymentChoice{Phương thức thanh toán?}
    
    PaymentChoice -->|Tiền mặt| CashPay[Khách hàng thanh toán tiền mặt cho tài xế]
    PaymentChoice -->|Điện tử| GatewayPay[Gọi API Cổng thanh toán bên thứ ba]
    
    GatewayPay --> PayResult{Giao dịch thành công?}
    PayResult -->|Thất bại| PayFailNoti[Thông báo lỗi thanh toán và cho phép xử lý lại] --> GatewayPay
    PayResult -->|Thành công| FinishPay[Xác nhận thanh toán hoàn tất]
    
    CashPay --> Finalize[Hệ thống đóng chuyến đi]
    FinishPay --> Finalize
    
    Finalize --> Rate[Khách hàng đánh giá tài xế và nhận thông báo hoàn thành] --> End
