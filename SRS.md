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

## 3. System Architecture & Diagrams

### 3.1 Use Case Diagram

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
