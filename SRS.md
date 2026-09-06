# Software Requirements Specification (SRS) - CAB System

## 1. Business Goals (Mục tiêu Kinh doanh)

- **BG-01 (Tự động hóa phân công):** Chuyển từ phân công thủ công sang hệ thống tự động tìm và gán tài xế phù hợp theo vị trí GPS.
- **BG-02 (Xử lý chuyển tiếp đơn):** Tự động tìm tài xế thay thế khi tài xế được đề xuất từ chối hoặc hết thời gian phản hồi.
- **BG-03 (Theo dõi realtime):** Cho phép khách hàng theo dõi vị trí tài xế, trạng thái chuyến đi và thời gian dự kiến đến (ETA).
- **BG-04 (Minh bạch & Thanh toán):** Tự động tính cước rõ ràng và hỗ trợ cả 2 phương thức: tiền mặt và thanh toán điện tử qua cổng trung gian.
- **BG-05 (Hỗ trợ vận hành):** Cho phép nhân viên theo dõi các chuyến đi đang diễn ra, quản lý thông tin tài xế/khách hàng và hỗ trợ khi có sự cố.
- **BG-06 (Báo cáo cơ bản):** Cung cấp các số liệu thống kê cơ bản về doanh thu, số lượng chuyến đi và tỷ lệ hoàn thành.

## 2. Business Requirements (Yêu cầu Kinh doanh)

### 2.1 Vấn đề & Lý do đầu tư
- Quy trình phân công tài xế cũ làm thủ công, khó theo dõi chuyến đi và không thể mở rộng quy mô.
- Cần xây dựng nền tảng CAB mới tự động hóa hoàn toàn luồng đặt xe và điều phối.

### 2.2 Phạm vi Dự án (Project Scope)
- **In-Scope:** Luồng đặt xe end-to-end cho Khách hàng, Tài xế và Admin; Tích hợp thanh toán điện tử; Hệ thống thông báo; Dashboard báo cáo vận hành.
- **Out-of-Scope:** Dịch vụ giao hàng, giao đồ ăn, tính năng đi chung xe (Ride-sharing).

### 2.3 Ràng buộc Dự án (Constraints)
- **Thời gian:** Hoàn thành triển khai trong 7 tuần.
- **Bảo mật:** Không lưu thông tin thẻ ngân hàng trực tiếp trên hệ thống; phân quyền chi tiết cho Admin.
- **Chịu lỗi:** Lỗi từ dịch vụ thanh toán/thông báo không được làm ảnh hưởng đến luồng đặt xe chính.

## 3. System Modules (Danh sách Module Hệ thống)

1. **User & Auth Module:** Quản lý đăng ký, đăng nhập và phân quyền (Khách hàng, Tài xế, Admin).
2. **Booking & Dispatch Module:** Tiếp nhận yêu cầu đặt xe, tự động tìm và gán chuyến cho tài xế phù hợp.
3. **Tracking & Trip Module:** Cập nhật vị trí GPS realtime và quản lý các trạng thái của chuyến đi.
4. **Fare & Payment Module:** Tính cước tự động và tích hợp cổng thanh toán điện tử.
5. **Notification Module:** Gửi thông báo trạng thái chuyến đi cho khách hàng và tài xế.
6. **Admin & Reporting Module:** Quản lý danh mục, hỗ trợ vận hành và xuất báo cáo thống kê.

## 4. Customer Requirements (Yêu cầu của Khách hàng)

### 4.1 Khách hàng (Customer)
- Đăng ký/đăng nhập, nhập điểm đón/đến, chọn loại xe và gửi yêu cầu.
- Theo dõi vị trí tài xế, xem ETA, xem lịch sử chuyến đi và đánh giá tài xế.

### 4.2 Tài xế (Driver)
- Bật/tắt trạng thái sẵn sàng, nhận/từ chối chuyến, cập nhật tiến trình chuyến đi và chia sẻ vị trí GPS.

### 4.3 Nhân viên Vận hành (Operator)
- Quản lý tài khoản/phương tiện, giám sát các chuyến đi realtime, hỗ trợ xử lý sự cố và xem báo cáo doanh thu/hiệu suất.

### 4.4 Quy tắc Nghiệp vụ Cốt lõi
- Tự động ghép chuyến theo vị trí GPS, tự động chuyển tài xế khác khi bị từ chối/timeout.
- Tính cước tự động, tích hợp cổng thanh toán điện tử an toàn (không lưu thông tin thẻ).
- Hệ thống chịu lỗi tốt, các dịch vụ thanh toán/thông báo hoạt động độc lập với luồng đặt xe.

## 5. Quy trình Nghiệp vụ Chi tiết từ Đặt xe đến Kết thúc Chuyến đi

### 5.1 Giai đoạn 1: Đặt xe & Phân công tự động
- **Khởi tạo:** Khách hàng nhập điểm đón, điểm đến và chọn loại dịch vụ.
- **Tính cước:** Hệ thống hiển thị giá cước dự kiến và thời gian chờ.
- **Ghép chuyến:** Hệ thống tự động tìm và gửi thông báo đến tài xế rảnh gần nhất.
- **Chuyển tiếp đơn:** Nếu tài xế từ chối hoặc quá thời gian phản hồi (timeout), đơn hàng tự động được chuyển sang tài xế tiếp theo mà không làm gián đoạn trải nghiệm của khách.

### 5.2 Giai đoạn 2: Di chuyển & Theo dõi Realtime
- **Xác nhận:** Khách nhận được thông tin tài xế, biển số xe và thời gian dự kiến tài xế đến (ETA).
- **Cập nhật tiến trình:** Tài xế thao tác lần lượt các trạng thái: *Đã đến điểm đón -> Đã đón khách -> Đang di chuyển -> Hoàn thành chuyến đi*.
- **Định vị:** Vị trí di chuyển được cập nhật liên tục qua GPS thời gian thực.

### 5.3 Giai đoạn 3: Thanh toán & Kết thúc
- **Xử lý thanh toán:** Hỗ trợ thanh toán bằng Tiền mặt hoặc Điện tử qua cổng trung gian. Trường hợp lỗi thanh toán điện tử, hệ thống cho phép chọn lại phương thức khác.
- **Đánh giá:** Khách hàng chấm điểm và gửi phản hồi chất lượng sau khi hoàn thành.
- **Khôi phục trạng thái:** Tài xế trở lại trạng thái sẵn sàng nhận đơn mới.

## 6. Functional Requirements (Yêu cầu Chức năng)

### 6.1 User & Auth Module
- **FR-01:** Khách hàng/Tài xế đăng ký, đăng nhập và cập nhật hồ sơ cá nhân.
- **FR-02:** Tài xế/Admin cập nhật thông tin phương tiện và bằng lái.
- **FR-03:** Phân quyền hệ thống chặt chẽ cho Nhân viên vận hành.

### 6.2 Booking & Dispatch Module
- **FR-04:** Khách nhập điểm đón/đến, chọn loại xe và xác nhận đặt xe.
- **FR-05:** Tự động quét GPS và gán đơn cho tài xế gần nhất.
- **FR-06:** Tài xế có quyền Chấp nhận hoặc Từ chối chuyến đi.
- **FR-07:** Tự động chuyển tiếp đơn sang tài xế khác nếu bị từ chối/timeout.
- **FR-08:** Thông báo cho khách hàng nếu không tìm thấy xe khả dụng.

### 6.3 Tracking & Trip Module
- **FR-09:** Tài xế bật/tắt trạng thái sẵn sàng làm việc.
- **FR-10:** Tài xế cập nhật trạng thái: *Đã đến điểm đón -> Đã đón khách -> Đang di chuyển -> Hoàn thành*.
- **FR-11:** Lưu trữ tọa độ GPS và hiển thị vị trí xe di chuyển Realtime cho khách.
- **FR-12:** Tính toán và hiển thị thời gian dự kiến tài xế đến (ETA).

### 6.4 Fare & Payment Module
- **FR-13:** Tự động tính cước phí chuyến đi dựa trên khoảng cách và loại dịch vụ.
- **FR-14:** Hỗ trợ thanh toán Tiền mặt và Thanh toán điện tử (qua cổng bên thứ 3).
- **FR-15:** Không lưu trực tiếp thông tin thẻ/tài khoản ngân hàng nhạy cảm.
- **FR-16:** Xử lý ngoại lệ và cho phép chọn lại phương thức khi thanh toán lỗi.

### 6.5 Notification Module
- **FR-17:** Gửi thông báo realtime cho Khách hàng qua từng giai đoạn của chuyến đi.
- **FR-18:** Gửi thông báo chuyến mới và thay đổi thông tin cho Tài xế.

### 6.6 Admin & Reporting Module
- **FR-19:** Dashboard quản trị giám sát chuyến đi đang chạy và hỗ trợ sự cố.
- **FR-20:** Cho phép Khách hàng xem lịch sử chuyến đi và đánh giá tài xế.
- **FR-21:** Xuất báo cáo thống kê: Doanh thu, tổng chuyến, tỷ lệ hoàn thành/hủy.
- **FR-22:** Lưu nhật ký thao tác (Audit Logs) để phục vụ kiểm toán sự cố.

## 7. Business Exceptions (Trường hợp Ngoại lệ Nghiệp vụ)

- **EX-01 (Không tìm thấy tài xế):** Hệ thống chuyển trạng thái chuyến đi sang CANCELLED, hiển thị thông báo lỗi rõ ràng cho Khách hàng và cung cấp nút "Thử lại" hoặc gợi ý đổi loại phương tiện.
- **EX-02 (Thanh toán điện tử thất bại):** Hệ thống gửi thông báo cho Khách hàng, yêu cầu thực hiện lại giao dịch hoặc chuyển sang phương thức thanh toán Tiền mặt.
- **EX-03 (Tài xế hủy chuyến sau khi đã nhận):** Hệ thống ghi nhận lý do hủy của tài xế và tự động đưa đơn hàng quay lại luồng tìm tài xế mới cho Khách hàng mà không hủy toàn bộ đơn.
- **EX-04 (Mất kết nối GPS/Mạng trong chuyến đi):** Lưu tọa độ hành trình vào bộ nhớ tạm trên máy (Local Cache) và tự động đồng bộ lại lên Server ngay khi có kết nối mạng.

## 8. Non-Functional Requirements (Yêu cầu Phi chức năng)

### 8.1 Hiệu năng & Khả năng chịu tải (Performance & Scalability)
- **NFR-01:** Thời gian phản hồi tính cước < 2 giây; thời gian phát thông báo đặt xe < 3 giây.
- **NFR-02:** Hệ thống vận hành ổn định trong giờ cao điểm khi lượng truy cập tăng đột biến.
- **NFR-03:** Các module có khả năng mở rộng (Scale) độc lập theo nhu cầu tải.

### 8.2 Độ tin cậy & Chịu lỗi (Reliability & Fault Tolerance)
- **NFR-04:** Sự cố ở Module Thanh toán/Thông báo không làm ảnh hưởng đến luồng Đặt xe cốt lõi.
- **NFR-05:** Tự động lưu bộ nhớ tạm (Cache) khi mất mạng/GPS và đồng bộ lại khi có kết nối.

### 8.3 Bảo mật (Security & Compliance)
- **NFR-06:** Bắt buộc xác thực người dùng và phân quyền chặt chẽ cho Admin.
- **NFR-07:** Không lưu trực tiếp thông tin thẻ ngân hàng nhạy cảm trên hệ thống CAB.
- **NFR-08:** Lưu log hệ thống (Audit Logs) đối với các thao tác quan trọng để kiểm tra sự cố.

### 8.4 Khả năng mở rộng tương lai (Extensibility)
- **NFR-09:** Cấu trúc linh hoạt, sẵn sàng tích hợp thêm cổng thanh toán, kênh thông báo hoặc loại hình dịch vụ mới.

## 9. System Architecture & Use Case Diagrams

### 9.1 Danh sách Use Case (Use Case List)

| Mã UC | Tên Use Case | Actor chính | Mô tả |
| :--- | :--- | :--- | :--- |
| **UC01** | Đăng ký / Đăng nhập | Khách hàng, Tài xế, Operator | Xác thực người dùng truy cập hệ thống. |
| **UC02** | Đặt xe & Theo dõi vị trí | Khách hàng | Nhập lộ trình, xem cước, đặt xe và theo dõi hành trình realtime. |
| **UC03** | Thanh toán & Đánh giá | Khách hàng | Thực hiện thanh toán tiền mặt/điện tử và đánh giá tài xế. |
| **UC04** | Cập nhật Trạng thái & Vị trí | Tài xế | Bật/tắt chế độ sẵn sàng và chia sẻ vị trí GPS realtime. |
| **UC05** | Nhận / Từ chối chuyến | Tài xế / Hệ thống | Tiếp nhận thông báo chuyến xe mới, chấp nhận/từ chối hoặc tự chuyển đơn khi timeout. |
| **UC06** | Quản lý Người dùng & Phương tiện | Operator | Quản lý thông tin tài khoản khách hàng, tài xế và hồ sơ xe. |
| **UC07** | Theo dõi Chuyến đi & Xử lý sự cố | Operator | Giám sát các chuyến đi đang diễn ra và can thiệp hỗ trợ khi có lỗi. |
| **UC08** | Xem Báo cáo & Thống kê | Operator | Tra cứu báo cáo doanh thu, tổng số chuyến, tỷ lệ hoàn thành/hủy. |

### 9.2 Sơ đồ Use Case Tổng quan (Use Case Diagram)

```mermaid
graph TD
    %% Actors
    C[Khách hàng]
    D[Tài xế]
    O[Nhân viên vận hành]
    P[Hệ thống Thanh toán]

    subgraph CAB System
        UC1(UC01: Đăng ký / Đăng nhập)
        UC2(UC02: Đặt xe & Theo dõi vị trí)
        UC3(UC03: Thanh toán & Đánh giá)
        
        UC4(UC04: Cập nhật Trạng thái & Vị trí)
        UC5(UC05: Nhận / Từ chối chuyến)
        
        UC6(UC06: Quản lý Người dùng & Phương tiện)
        UC7(UC07: Theo dõi Chuyến đi & Xử lý sự cố)
        UC8(UC08: Xem Báo cáo & Thống kê)
    end

    %% Customer Relations
    C --> UC1
    C --> UC2
    C --> UC3

    %% Driver Relations
    D --> UC1
    D --> UC4
    D --> UC5

    %% Operator Relations
    O --> UC6
    O --> UC7
    O --> UC8

    %% External System Interactions
    UC3 .-> P
```
## 10. Acceptance Criteria (Tiêu chí Chấp nhận)

### 10.1 Quản lý Đặt xe & Ghép chuyến
- **AC-01 (Đặt xe):** Khách chọn xong điểm đi/đến -> Hiển thị cước phí -> Bấm xác nhận -> Đơn chuyển sang `SEARCHING`.
- **AC-02 (Chuyển tiếp đơn):** Tài xế từ chối hoặc quá 20 giây không nhận -> Tự động chuyển đơn sang tài xế tiếp theo.
- **AC-03 (Hết xe):** Quét hết tài xế không ai nhận -> Thông báo lỗi cho khách và hiển thị nút "Thử lại".

### 10.2 Định vị & Tiến trình
- **AC-04 (Trạng thái):** Tài xế bấm thao tác -> Hệ thống cập nhật đúng luồng `ASSIGNED` -> `ARRIVED` -> `IN_PROGRESS` -> `COMPLETED`.
- **AC-05 (Realtime):** Tọa độ GPS của xe được đồng bộ lên bản đồ ứng dụng khách hàng mỗi 3-5 giây.

### 10.3 Thanh toán & Đánh giá
- **AC-06 (Thanh toán thành công):** Trừ tiền qua cổng thanh toán thành công -> Đơn thành `PAID` -> Bật màn hình đánh giá sao.
- **AC-07 (Lỗi thanh toán):** Cổng thanh toán báo lỗi -> Cho phép khách hàng chuyển sang trả Tiền mặt.

## 11. Requirements Traceability Matrix (Bảng truy vết yêu cầu)

| Mã FR | Tên Yêu cầu Chức năng | Business Goals | Module | Use Case | Acceptance Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FR-01** | Đăng ký / Đăng nhập | BG-05 | User & Auth | UC01 | AC-01 |
| **FR-04** | Tạo đơn & Đặt xe | BG-03, BG-04 | Booking & Dispatch | UC02 | AC-01 |
| **FR-05** | Tự động ghép chuyến theo GPS | BG-01 | Booking & Dispatch | UC02, UC05 | AC-01 |
| **FR-07** | Tự động chuyển đơn khi bị từ chối/timeout | BG-02 | Booking & Dispatch | UC05 | AC-02 |
| **FR-08** | Xử lý khi hết xe (EX-01) | BG-02 | Booking & Dispatch | UC02, UC05 | AC-03 |
| **FR-10** | Cập nhật tiến trình chuyến đi | BG-03 | Tracking & Trip | UC03, UC06 | AC-04 |
| **FR-11** | Theo dõi vị trí Realtime | BG-03 | Tracking & Trip | UC02, UC04 | AC-05 |
| **FR-14** | Thanh toán Tiền mặt / Ví điện tử | BG-04 | Fare & Payment | UC03 | AC-06 |
| **FR-16** | Xử lý lỗi thanh toán (EX-02) | BG-04 | Fare & Payment | UC03 | AC-07 |
| **FR-19** | Dashboard quản trị & Giám sát | BG-05 | Admin & Reporting | UC07 | AC-04 |
| **FR-21** | Báo cáo doanh thu & Số chuyến | BG-06 | Admin & Reporting | UC08 | AC-06 |
