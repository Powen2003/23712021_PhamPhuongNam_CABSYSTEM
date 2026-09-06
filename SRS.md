# Software Requirements Specification (SRS) - CAB System

## 1. Business Goals (Mục tiêu Kinh doanh)
- **BG-01 (Tự động hóa):** Chuyển đổi từ phân công thủ công sang hệ thống ghép chuyến tự động dựa trên vị trí GPS để tối ưu thời gian chờ.
- **BG-02 (Trải nghiệm người dùng):** Cho phép theo dõi hành trình realtime, minh bạch cước phí và hỗ trợ thanh toán không dùng tiền mặt.
- **BG-03 (Quản trị tập trung):** Xây dựng công cụ quản lý vận hành theo dõi trạng thái tài xế, chuyến đi và báo cáo doanh thu theo thời gian thực.
- **BG-04 (Khả năng mở rộng):** Đảm bảo hệ thống chịu tải cao vào giờ cao điểm và dễ dàng tích hợp thêm các dịch vụ/đối tác mới trong tương lai.
- **BG-05 (Bảo mật & Tuân thủ):** Bảo vệ dữ liệu cá nhân, không lưu thông tin thẻ nhạy cảm và duy trì log hệ thống để kiểm toán.
- **BG-06 (Độ tin cậy & Chịu lỗi):** Đảm bảo hệ thống đạt độ sẵn sàng cao vào giờ cao điểm, sự cố ở tính năng thanh toán/thông báo không làm sập luồng đặt xe.
- **BG-07 (Tối ưu hiệu suất tài xế):** Giảm thời gian xe chạy rỗng, tự động điều phối chuyến để tăng thu nhập cho tài xế và giảm tỷ lệ hủy chuyến.
## 2. System Architecture & Diagrams

### 2.1 Use Case Diagram
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

    %% System Interactions
    UC3 .-> P
