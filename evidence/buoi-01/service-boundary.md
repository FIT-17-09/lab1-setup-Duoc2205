# Service Boundary của nhóm

## 1. Thông tin nhóm

- Tên nhóm: nhóm 7
- Lớp: CNTT 17-09
- Thành viên:
  - Nguyễn Hữu Hưng
  - Nguyễn Văn Được
  - Đỗ Quốc Khánh
- Service nhóm phụ trách:
  - Dịch vụ tiếp nhận và xử lý luồng camera
- Sản phẩm tổng thể của lớp:
  - Hệ thống giám sát và phân tích dữ liệu thông minh sử dụng IoT và AI
## 2. Actor

Ai tương tác với hệ thống/service?
- Quản trị viên hệ thống
- Nhân viên giám sát
- Camera IP
- Service AI phân tích hình ảnh
- Service xử lý nghiệp vụ trung tâm

## 3. System Boundary

Nhóm em xây phần nào?

Phần nhóm kiểm soát:
- Tiếp nhận luồng video từ camera IP
- Xử lý và truyền dữ liệu video
- Quản lý trạng thái camera
- Chuyển frame hình ảnh sang service AI để phân tích
- Lưu thông tin camera và nhật ký hoạt động
- ...

Phần nhóm chỉ tích hợp:
- Service AI phân tích hình ảnh
- Service tổng hợp dữ liệu
- Service gửi cảnh báo
- Database trung tâm
- Dashboard giao diện người dùng
- ...

## 4. Service Boundary

Service của nhóm có trách nhiệm gì?
- Kết nối với camera IP
- Tiếp nhận và xử lý luồng video realtime
- Kiểm tra trạng thái hoạt động của camera
- Trích xuất frame từ video
- Gửi dữ liệu hình ảnh đến service AI
- Ghi log hoạt động camera
- Quản lý danh sách camera

Service KHÔNG làm gì?
- Không thực hiện nhận diện AI trực tiếp
- Không xử lý nghiệp vụ cảnh báo
- Không gửi email hoặc SMS
- Không quản lý người dùng hệ thống
- Không trực tiếp hiển thị giao diện dashboard

---
## 5. Input / Output

### Input
- Luồng video RTSP từ camera
- Thông tin cấu hình camera
- Yêu cầu từ service trung tâm
- Request kiểm tra trạng thái camera
- ...

### Output
- Frame hình ảnh đã xử lý
- Metadata camera
- Trạng thái hoạt động camera
- Dữ liệu gửi sang AI service
- Nhật ký hoạt động
- ...

## 6. API dự kiến

| Method | Endpoint | Mục đích |
|---|---|---|
| GET | /health | Kiểm tra trạng thái service |
| GET | /cameras | Lấy danh sách camera |
| POST | /cameras | Thêm camera mới |
| PUT | /cameras/{id} | Cập nhật thông tin camera |
| DELETE | /cameras/{id} | Xóa camera |
| GET | /cameras/{id}/status | Kiểm tra trạng thái camera |
| POST | /stream/start | Bắt đầu luồng camera |
| POST | /stream/stop | Dừng luồng camera |
| POST | /frames/send-ai | Gửi frame sang AI service |

## 7. Phụ thuộc service khác

Service này gọi đến service nào?
- AI Image Analysis Service
- Central Business Service
- Database Service

Service nào gọi đến service này?
- Dashboard Service
- Central Business Service
- Alert Service


## 8. Sơ đồ minh họa

Có thể vẽ bằng Mermaid, draw.io, Ludichart hoặc ảnh chụp sơ đồ.

```mermaid
flowchart LR
    Camera[Camera IP] --> StreamService[Camera Stream Service]

    StreamService --> DB[(Database)]

    StreamService --> AI[AI Image Analysis Service]

    StreamService --> Core[Central Business Service]

    Dashboard[Dashboard] --> StreamService

    Core --> Alert[Alert Service]