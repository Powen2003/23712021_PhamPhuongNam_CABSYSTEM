# Software Requirements Specification (SRS) - CAB System

## 1. Business Goals (Mục tiêu Kinh doanh)

- **BG-01 (Tự động hóa phân công):** Chuyển từ phân công thủ công sang hệ thống tự động tìm và gán tài xế phù hợp theo vị trí GPS.
- **BG-02 (Xử lý chuyển tiếp đơn):** Tự động tìm tài xế thay thế khi tài xế được đề xuất từ chối hoặc hết thời gian phản hồi.
- **BG-03 (Theo dõi realtime):** Cho phép khách hàng theo dõi vị trí tài xế, trạng thái chuyến đi và thời gian dự kiến đến (ETA).
- **BG-04 (Minh bạch & Thanh toán):** Tự động tính cước rõ ràng và hỗ trợ cả 2 phương thức: tiền mặt và thanh toán điện tử qua cổng trung gian.
- **BG-05 (Hỗ trợ vận hành):** Cho phép nhân viên theo dõi các chuyến đi đang diễn ra, quản lý thông tin tài xế/khách hàng và hỗ trợ khi có sự cố.
- **BG-06 (Báo cáo cơ bản):** Cung cấp các số liệu thống kê cơ bản về doanh thu, số lượng chuyến đi và tỷ lệ hoàn thành.

## 2. Danh sách các Module Hệ thống (System Modules)

1. **User & Auth Module:** Quản lý đăng ký, đăng nhập và phân quyền (Khách hàng, Tài xế, Admin).
2. **Booking & Dispatch Module:** Tiếp nhận yêu cầu đặt xe, tự động tìm và gán chuyến cho tài xế phù hợp.
3. **Tracking & Trip Module:** Cập nhật vị trí GPS realtime và quản lý các trạng thái của chuyến đi.
4. **Fare & Payment Module:** Tính cước tự động và tích hợp cổng thanh toán điện tử.
5. **Notification Module:** Gửi thông báo trạng thái chuyến đi cho khách hàng và tài xế.
6. **Admin & Reporting Module:** Quản lý danh mục, hỗ trợ vận hành và xuất báo cáo thống kê.

## Business Requirements (Yêu cầu Kinh doanh)

### 1. Vấn đề & Lý do đầu tư
- Quy trình phân công tài xế cũ làm thủ công, khó theo dõi chuyến đi và không thể mở rộng quy mô.
- Cần xây dựng nền tảng CAB mới tự động hóa hoàn toàn luồng đặt xe và điều phối.

### 2. Phạm vi Dự án (Project Scope)
- **In-Scope:** Luồng đặt xe end-to-end cho Khách hàng, Tài xế và Admin; Tích hợp thanh toán điện tử; Hệ thống thông báo; Dashboard báo cáo vận hành.
- **Out-of-Scope:** Dịch vụ giao hàng, giao đồ ăn, tính năng đi chung xe (Ride-sharing).

### 3. Ràng buộc Dự án (Constraints)
- **Thời gian:** Hoàn thành triển khai trong 7 tuần.
- **Bảo mật:** Không lưu thông tin thẻ ngân hàng trực tiếp trên hệ thống; phân quyền chi tiết cho Admin.
- **Chịu lỗi:** Lỗi từ dịch vụ thanh toán/thông báo không được làm ảnh hưởng đến luồng đặt xe chính.

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

## Quy trình Nghiệp vụ Chi tiết từ Đặt xe đến Kết thúc Chuyến đi

### 1. Giai đoạn 1: Đặt xe & Phân công tự động
- **Khởi tạo:** Khách hàng nhập điểm đón, điểm đến và chọn loại dịch vụ.
- **Tính cước:** Hệ thống hiển thị giá cước dự kiến và thời gian chờ.
- **Ghép chuyến:** Hệ thống tự động tìm và gửi thông báo đến tài xế rảnh gần nhất.
- **Chuyển tiếp đơn:** Nếu tài xế từ chối hoặc quá thời gian phản hồi (timeout), đơn hàng tự động được chuyển sang tài xế tiếp theo mà không làm gián đoạn trải nghiệm của khách.

### 2. Giai đoạn 2: Di chuyển & Theo dõi Realtime
- **Xác nhận:** Khách nhận được thông tin tài xế, biển số xe .
- **Cập nhật tiến trình:** Tài xế thao tác lần lượt các trạng thái: *Đã đón khách -> Hoàn thành chuyến đi*.
- **Định vị:** Vị trí di chuyển được cập nhật liên tục qua GPS thời gian thực.

### 3. Giai đoạn 3: Thanh toán & Kết thúc
- **Xử lý thanh toán:** Hỗ trợ thanh toán bằng Tiền mặt hoặc Điện tử qua cổng trung gian. Trường hợp lỗi thanh toán điện tử, hệ thống cho phép chọn lại phương thức khác.
- **Đánh giá:** Khách hàng chấm điểm và gửi phản hồi chất lượng sau khi hoàn thành.
- **Khôi phục trạng thái:** Tài xế trở lại trạng thái sẵn sàng nhận đơn mới.
##  System Architecture & Diagrams

###  Use Case Diagram

```mermaid
graph TD
    %% Actors
    C[Khách hàng]
    D[Tài xế]
    O[Nhân viên vận hành]
    P[Hệ thống Thanh toán]

    subgraph CAB System
        UC1(Đăng ký / Đăng nhập)
        UC2(Đặt xe & Theo dõi vị trí)
        UC3(Thanh toán & Đánh giá)
        
        UC4(Cập nhật trạng thái sẵn sàng / Vị trí)
        UC5(Nhận / Từ chối chuyến)
        
        UC6(Quản lý người dùng & Phương tiện)
        UC7(Theo dõi chuyến đi & Xử lý sự cố)
        UC8(Xem báo cáo & Thống kê)
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
