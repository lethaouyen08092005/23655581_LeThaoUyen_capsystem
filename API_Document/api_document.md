# API DOCUMENT - CAB System

## 1. Thông tin chung

### 1.1. Tên hệ thống

**CAB System – Nền tảng đặt xe**

### 1.2. Mục đích

API của CAB System cung cấp các dịch vụ để ứng dụng khách hàng, ứng dụng tài xế và giao diện quản trị có thể giao tiếp với hệ thống.

Các API chính hỗ trợ:

- Quản lý tài khoản
- Quản lý thông tin khách hàng và tài xế
- Quản lý phương tiện
- Đặt xe
- Tìm và phân công tài xế
- Theo dõi và cập nhật chuyến đi
- Tính cước
- Thanh toán
- Gửi thông báo
- Xem lịch sử chuyến
- Đánh giá tài xế
- Quản lý vận hành
- Tra cứu giao dịch
- Xem báo cáo

### 1.3. Base URL

```text
https://api.cabsystem.com/api/v1
```

### 1.4. Định dạng dữ liệu

API sử dụng định dạng **JSON**.

Request:

```http
Content-Type: application/json
```

Response:

```http
Content-Type: application/json
```

---

# 2. Quy ước chung

## 2.1. HTTP Method

| Method | Mục đích |
|---|---|
| GET | Lấy dữ liệu |
| POST | Tạo dữ liệu hoặc thực hiện thao tác |
| PUT | Cập nhật dữ liệu |
| DELETE | Xóa dữ liệu |

## 2.2. HTTP Status Code

| Status Code | Ý nghĩa |
|---|---|
| 200 | Thành công |
| 201 | Tạo dữ liệu thành công |
| 400 | Dữ liệu gửi lên không hợp lệ |
| 401 | Chưa đăng nhập hoặc xác thực không hợp lệ |
| 403 | Không có quyền thực hiện |
| 404 | Không tìm thấy dữ liệu |
| 409 | Dữ liệu bị trùng hoặc xung đột |
| 500 | Lỗi hệ thống |

## 2.3. Xác thực

Các API yêu cầu tài khoản sử dụng JWT Token.

Header:

```http
Authorization: Bearer <access_token>
```

---

# 3. API quản lý tài khoản

## 3.1. Đăng ký tài khoản

**Endpoint**

```http
POST /auth/register
```

**Mô tả:**  
Cho phép khách hàng hoặc tài xế đăng ký tài khoản.

**Request Body**

```json
{
  "fullName": "Nguyen Van A",
  "phone": "0901234567",
  "email": "nguyenvana@gmail.com",
  "password": "123456",
  "role": "CUSTOMER"
}
```

**Response**

```json
{
  "success": true,
  "message": "Đăng ký tài khoản thành công",
  "data": {
    "userId": "USR001",
    "fullName": "Nguyen Van A",
    "phone": "0901234567",
    "role": "CUSTOMER"
  }
}
```

---

## 3.2. Đăng nhập

**Endpoint**

```http
POST /auth/login
```

**Mô tả:**  
Cho phép người dùng đăng nhập vào hệ thống.

**Request Body**

```json
{
  "phone": "0901234567",
  "password": "123456"
}
```

**Response**

```json
{
  "success": true,
  "message": "Đăng nhập thành công",
  "data": {
    "accessToken": "jwt-token",
    "userId": "USR001",
    "role": "CUSTOMER"
  }
}
```

---

## 3.3. Xem thông tin tài khoản

**Endpoint**

```http
GET /users/me
```

**Mô tả:**  
Lấy thông tin của tài khoản đang đăng nhập.

**Response**

```json
{
  "success": true,
  "data": {
    "userId": "USR001",
    "fullName": "Nguyen Van A",
    "phone": "0901234567",
    "email": "nguyenvana@gmail.com",
    "role": "CUSTOMER"
  }
}
```

---

## 3.4. Cập nhật thông tin

**Endpoint**

```http
PUT /users/me
```

**Request Body**

```json
{
  "fullName": "Nguyen Van B",
  "email": "nguyenvanb@gmail.com"
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật thông tin thành công"
}
```

---

# 4. API quản lý tài xế

## 4.1. Xem hồ sơ tài xế

**Endpoint**

```http
GET /drivers/me
```

**Mô tả:**  
Tài xế xem thông tin hồ sơ cá nhân.

**Response**

```json
{
  "success": true,
  "data": {
    "driverId": "DRV001",
    "fullName": "Tran Van B",
    "phone": "0912345678",
    "status": "AVAILABLE"
  }
}
```

---

## 4.2. Cập nhật hồ sơ tài xế

**Endpoint**

```http
PUT /drivers/me
```

**Request Body**

```json
{
  "fullName": "Tran Van B",
  "phone": "0912345678"
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật hồ sơ tài xế thành công"
}
```

---

## 4.3. Cập nhật trạng thái hoạt động

**Endpoint**

```http
PUT /drivers/me/status
```

**Request Body**

```json
{
  "status": "AVAILABLE"
}
```

Giá trị trạng thái:

```text
AVAILABLE
UNAVAILABLE
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật trạng thái thành công",
  "data": {
    "status": "AVAILABLE"
  }
}
```

---

## 4.4. Cập nhật vị trí tài xế

**Endpoint**

```http
PUT /drivers/me/location
```

**Request Body**

```json
{
  "latitude": 10.7769,
  "longitude": 106.7009
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật vị trí thành công"
}
```

---

# 5. API quản lý phương tiện

## 5.1. Xem thông tin phương tiện

**Endpoint**

```http
GET /drivers/me/vehicle
```

**Response**

```json
{
  "success": true,
  "data": {
    "vehicleId": "VEH001",
    "licensePlate": "51A-12345",
    "vehicleType": "CAR_4"
  }
}
```

---

## 5.2. Thêm phương tiện

**Endpoint**

```http
POST /drivers/me/vehicle
```

**Request Body**

```json
{
  "licensePlate": "51A-12345",
  "vehicleType": "CAR_4"
}
```

**Response**

```json
{
  "success": true,
  "message": "Thêm phương tiện thành công",
  "data": {
    "vehicleId": "VEH001"
  }
}
```

---

## 5.3. Cập nhật phương tiện

**Endpoint**

```http
PUT /drivers/me/vehicle
```

**Request Body**

```json
{
  "licensePlate": "51A-67890",
  "vehicleType": "CAR_7"
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật phương tiện thành công"
}
```

---

# 6. API đặt xe

## 6.1. Tạo yêu cầu đặt xe

**Endpoint**

```http
POST /bookings
```

**Mô tả:**  
Khách hàng tạo yêu cầu đặt xe bằng cách nhập điểm đón, điểm đến và loại xe.

**Request Body**

```json
{
  "pickupLocation": "Trường Đại học ABC",
  "dropoffLocation": "Chợ Bến Thành",
  "vehicleType": "CAR_4"
}
```

**Response**

```json
{
  "success": true,
  "message": "Yêu cầu đặt xe đã được tiếp nhận",
  "data": {
    "bookingId": "BOOK001",
    "status": "SEARCHING_DRIVER"
  }
}
```

---

## 6.2. Xem trạng thái yêu cầu đặt xe

**Endpoint**

```http
GET /bookings/{bookingId}
```

**Response**

```json
{
  "success": true,
  "data": {
    "bookingId": "BOOK001",
    "pickupLocation": "Trường Đại học ABC",
    "dropoffLocation": "Chợ Bến Thành",
    "vehicleType": "CAR_4",
    "status": "DRIVER_ASSIGNED",
    "driverId": "DRV001"
  }
}
```

---

# 7. API tìm và phân công tài xế

## 7.1. Tìm tài xế phù hợp

**Endpoint**

```http
POST /bookings/{bookingId}/matching
```

**Mô tả:**  
Hệ thống tìm tài xế dựa trên vị trí, trạng thái sẵn sàng và tiêu chí vận hành.

**Response**

```json
{
  "success": true,
  "data": {
    "bookingId": "BOOK001",
    "status": "DRIVER_SEARCHING",
    "message": "Đang tìm tài xế phù hợp"
  }
}
```

---

## 7.2. Tài xế nhận chuyến

**Endpoint**

```http
POST /bookings/{bookingId}/accept
```

**Mô tả:**  
Tài xế chấp nhận yêu cầu chuyến.

**Response**

```json
{
  "success": true,
  "message": "Tài xế đã nhận chuyến",
  "data": {
    "bookingId": "BOOK001",
    "driverId": "DRV001",
    "status": "DRIVER_ASSIGNED"
  }
}
```

---

## 7.3. Tài xế từ chối chuyến

**Endpoint**

```http
POST /bookings/{bookingId}/reject
```

**Response**

```json
{
  "success": true,
  "message": "Đã ghi nhận từ chối chuyến",
  "data": {
    "bookingId": "BOOK001",
    "status": "SEARCHING_DRIVER"
  }
}
```

---

# 8. API quản lý chuyến đi

## 8.1. Cập nhật trạng thái chuyến

**Endpoint**

```http
PUT /trips/{tripId}/status
```

**Request Body**

```json
{
  "status": "ARRIVED_PICKUP"
}
```

Các trạng thái chính:

```text
DRIVER_ASSIGNED
ARRIVED_PICKUP
PICKED_UP
IN_PROGRESS
COMPLETED
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật trạng thái chuyến thành công",
  "data": {
    "tripId": "TRIP001",
    "status": "ARRIVED_PICKUP"
  }
}
```

---

## 8.2. Xem trạng thái chuyến

**Endpoint**

```http
GET /trips/{tripId}
```

**Response**

```json
{
  "success": true,
  "data": {
    "tripId": "TRIP001",
    "bookingId": "BOOK001",
    "driverId": "DRV001",
    "status": "IN_PROGRESS",
    "pickupLocation": "Trường Đại học ABC",
    "dropoffLocation": "Chợ Bến Thành"
  }
}
```

---

# 9. API tính cước

## 9.1. Tính cước chuyến đi

**Endpoint**

```http
POST /trips/{tripId}/fare
```

**Mô tả:**  
Hệ thống tính số tiền khách hàng phải trả sau khi chuyến đi hoàn thành.

**Response**

```json
{
  "success": true,
  "data": {
    "tripId": "TRIP001",
    "fare": 85000,
    "currency": "VND"
  }
}
```

---

# 10. API thanh toán

## 10.1. Tạo thanh toán

**Endpoint**

```http
POST /payments
```

**Request Body**

```json
{
  "tripId": "TRIP001",
  "method": "E_WALLET"
}
```

Các phương thức:

```text
CASH
E_WALLET
```

**Response**

```json
{
  "success": true,
  "data": {
    "paymentId": "PAY001",
    "tripId": "TRIP001",
    "amount": 85000,
    "method": "E_WALLET",
    "status": "PENDING"
  }
}
```

---

## 10.2. Xem trạng thái thanh toán

**Endpoint**

```http
GET /payments/{paymentId}
```

**Response**

```json
{
  "success": true,
  "data": {
    "paymentId": "PAY001",
    "amount": 85000,
    "method": "E_WALLET",
    "status": "SUCCESS"
  }
}
```

---

# 11. API thông báo

## 11.1. Gửi thông báo

**Endpoint**

```http
POST /notifications
```

**Mô tả:**  
Hệ thống gửi thông báo thông qua nhà cung cấp thông báo bên ngoài.

**Request Body**

```json
{
  "userId": "USR001",
  "type": "DRIVER_ASSIGNED",
  "message": "Tài xế đã nhận chuyến của bạn."
}
```

**Response**

```json
{
  "success": true,
  "message": "Gửi thông báo thành công"
}
```

---

## 11.2. Xem thông báo

**Endpoint**

```http
GET /notifications
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "notificationId": "NOTI001",
      "type": "DRIVER_ASSIGNED",
      "message": "Tài xế đã nhận chuyến của bạn.",
      "isRead": false
    }
  ]
}
```

---

# 12. API lịch sử chuyến

## 12.1. Xem lịch sử chuyến

**Endpoint**

```http
GET /trips/history
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "tripId": "TRIP001",
      "pickupLocation": "Trường Đại học ABC",
      "dropoffLocation": "Chợ Bến Thành",
      "fare": 85000,
      "status": "COMPLETED"
    }
  ]
}
```

---

## 12.2. Xem chi tiết chuyến

**Endpoint**

```http
GET /trips/{tripId}/detail
```

**Response**

```json
{
  "success": true,
  "data": {
    "tripId": "TRIP001",
    "driverId": "DRV001",
    "pickupLocation": "Trường Đại học ABC",
    "dropoffLocation": "Chợ Bến Thành",
    "fare": 85000,
    "paymentStatus": "SUCCESS",
    "status": "COMPLETED"
  }
}
```

---

# 13. API đánh giá tài xế

## 13.1. Đánh giá tài xế

**Endpoint**

```http
POST /trips/{tripId}/reviews
```

**Mô tả:**  
Khách hàng đánh giá tài xế sau khi chuyến đi hoàn thành.

**Request Body**

```json
{
  "rating": 5,
  "comment": "Tài xế thân thiện và chạy xe an toàn."
}
```

**Response**

```json
{
  "success": true,
  "message": "Đánh giá thành công",
  "data": {
    "reviewId": "REV001",
    "rating": 5
  }
}
```

---

# 14. API quản trị khách hàng

## 14.1. Xem danh sách khách hàng

**Endpoint**

```http
GET /admin/customers
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "userId": "USR001",
      "fullName": "Nguyen Van A",
      "phone": "0901234567",
      "status": "ACTIVE"
    }
  ]
}
```

---

## 14.2. Xem thông tin khách hàng

**Endpoint**

```http
GET /admin/customers/{customerId}
```

---

## 14.3. Cập nhật thông tin khách hàng

**Endpoint**

```http
PUT /admin/customers/{customerId}
```

**Request Body**

```json
{
  "fullName": "Nguyen Van A",
  "status": "ACTIVE"
}
```

---

# 15. API quản trị tài xế

## 15.1. Xem danh sách tài xế

**Endpoint**

```http
GET /admin/drivers
```

---

## 15.2. Xem thông tin tài xế

**Endpoint**

```http
GET /admin/drivers/{driverId}
```

---

## 15.3. Cập nhật thông tin tài xế

**Endpoint**

```http
PUT /admin/drivers/{driverId}
```

**Request Body**

```json
{
  "fullName": "Tran Van B",
  "status": "ACTIVE"
}
```

---

# 16. API quản trị phương tiện

## 16.1. Xem danh sách phương tiện

**Endpoint**

```http
GET /admin/vehicles
```

---

## 16.2. Xem thông tin phương tiện

**Endpoint**

```http
GET /admin/vehicles/{vehicleId}
```

---

## 16.3. Cập nhật phương tiện

**Endpoint**

```http
PUT /admin/vehicles/{vehicleId}
```

**Request Body**

```json
{
  "licensePlate": "51A-12345",
  "vehicleType": "CAR_4"
}
```

---

# 17. API theo dõi chuyến đang diễn ra

## 17.1. Xem danh sách chuyến đang diễn ra

**Endpoint**

```http
GET /admin/trips/active
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "tripId": "TRIP001",
      "driverId": "DRV001",
      "customerId": "USR001",
      "status": "IN_PROGRESS"
    }
  ]
}
```

---

## 17.2. Xem chi tiết chuyến đang diễn ra

**Endpoint**

```http
GET /admin/trips/{tripId}
```

---

# 18. API xử lý chuyến lỗi

## 18.1. Ghi nhận sự cố chuyến

**Endpoint**

```http
POST /admin/trips/{tripId}/issues
```

**Request Body**

```json
{
  "issueType": "TRIP_ERROR",
  "description": "Khách hàng không liên lạc được với tài xế."
}
```

**Response**

```json
{
  "success": true,
  "message": "Đã ghi nhận sự cố"
}
```

---

## 18.2. Cập nhật kết quả xử lý sự cố

**Endpoint**

```http
PUT /admin/trips/{tripId}/issues
```

**Request Body**

```json
{
  "status": "RESOLVED",
  "resolution": "Đã hỗ trợ kết nối lại khách hàng và tài xế."
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật xử lý sự cố thành công"
}
```

---

# 19. API tra cứu giao dịch

## 19.1. Tra cứu giao dịch

**Endpoint**

```http
GET /admin/payments
```

**Query Parameters**

```text
paymentId
tripId
status
fromDate
toDate
```

**Ví dụ**

```http
GET /admin/payments?status=SUCCESS
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "paymentId": "PAY001",
      "tripId": "TRIP001",
      "amount": 85000,
      "method": "E_WALLET",
      "status": "SUCCESS"
    }
  ]
}
```

---

# 20. API quản lý tài khoản và phân quyền

## 20.1. Xem danh sách tài khoản

**Endpoint**

```http
GET /admin/users
```

---

## 20.2. Cập nhật quyền người dùng

**Endpoint**

```http
PUT /admin/users/{userId}/role
```

**Request Body**

```json
{
  "role": "OPERATOR"
}
```

**Response**

```json
{
  "success": true,
  "message": "Cập nhật quyền thành công"
}
```

---

# 21. API báo cáo

## 21.1. Báo cáo số lượng chuyến

**Endpoint**

```http
GET /reports/trips
```

**Query Parameters**

```text
fromDate
toDate
```

**Response**

```json
{
  "success": true,
  "data": {
    "totalTrips": 1250,
    "completedTrips": 1100,
    "cancelledTrips": 150
  }
}
```

---

## 21.2. Báo cáo doanh thu

**Endpoint**

```http
GET /reports/revenue
```

**Query Parameters**

```text
fromDate
toDate
```

**Response**

```json
{
  "success": true,
  "data": {
    "totalRevenue": 125000000,
    "currency": "VND"
  }
}
```

---

## 21.3. Báo cáo hiệu quả tài xế

**Endpoint**

```http
GET /reports/drivers
```

**Query Parameters**

```text
fromDate
toDate
```

**Response**

```json
{
  "success": true,
  "data": {
    "totalDrivers": 100,
    "activeDrivers": 80,
    "averageCompletedTrips": 15
  }
}
```

---

# 22. API Audit Log

## 22.1. Tra cứu Audit Log

**Endpoint**

```http
GET /admin/audit-logs
```

**Mô tả:**  
Cho phép người có quyền quản trị xem lại các thao tác quan trọng trên hệ thống.

**Query Parameters**

```text
userId
action
fromDate
toDate
```

**Response**

```json
{
  "success": true,
  "data": [
    {
      "logId": "LOG001",
      "userId": "OP001",
      "action": "UPDATE_DRIVER",
      "timestamp": "2026-08-24T10:30:00"
    }
  ]
}
```

---

# 23. API xử lý lỗi chung

Khi API xảy ra lỗi, hệ thống trả về cấu trúc:

```json
{
  "success": false,
  "message": "Thông tin không hợp lệ",
  "errorCode": "INVALID_REQUEST"
}
```

Một số mã lỗi:

| Error Code | Ý nghĩa |
|---|---|
| INVALID_REQUEST | Dữ liệu gửi lên không hợp lệ |
| UNAUTHORIZED | Chưa đăng nhập |
| FORBIDDEN | Không có quyền |
| NOT_FOUND | Không tìm thấy dữ liệu |
| DRIVER_NOT_FOUND | Không tìm được tài xế |
| PAYMENT_FAILED | Thanh toán thất bại |
| INVALID_STATUS | Trạng thái không hợp lệ |
| SERVER_ERROR | Lỗi hệ thống |

---

# 24. Tổng hợp API

| Nhóm chức năng | Method | Endpoint |
|---|---|---|
| Đăng ký | POST | `/auth/register` |
| Đăng nhập | POST | `/auth/login` |
| Xem tài khoản | GET | `/users/me` |
| Cập nhật tài khoản | PUT | `/users/me` |
| Hồ sơ tài xế | GET | `/drivers/me` |
| Cập nhật hồ sơ tài xế | PUT | `/drivers/me` |
| Trạng thái tài xế | PUT | `/drivers/me/status` |
| Vị trí tài xế | PUT | `/drivers/me/location` |
| Xem phương tiện | GET | `/drivers/me/vehicle` |
| Thêm phương tiện | POST | `/drivers/me/vehicle` |
| Cập nhật phương tiện | PUT | `/drivers/me/vehicle` |
| Đặt xe | POST | `/bookings` |
| Xem yêu cầu đặt xe | GET | `/bookings/{bookingId}` |
| Tìm tài xế | POST | `/bookings/{bookingId}/matching` |
| Nhận chuyến | POST | `/bookings/{bookingId}/accept` |
| Từ chối chuyến | POST | `/bookings/{bookingId}/reject` |
| Cập nhật trạng thái chuyến | PUT | `/trips/{tripId}/status` |
| Xem chuyến | GET | `/trips/{tripId}` |
| Tính cước | POST | `/trips/{tripId}/fare` |
| Thanh toán | POST | `/payments` |
| Xem thanh toán | GET | `/payments/{paymentId}` |
| Gửi thông báo | POST | `/notifications` |
| Xem thông báo | GET | `/notifications` |
| Lịch sử chuyến | GET | `/trips/history` |
| Chi tiết chuyến | GET | `/trips/{tripId}/detail` |
| Đánh giá tài xế | POST | `/trips/{tripId}/reviews` |
| Quản lý khách hàng | GET | `/admin/customers` |
| Quản lý tài xế | GET | `/admin/drivers` |
| Quản lý phương tiện | GET | `/admin/vehicles` |
| Theo dõi chuyến | GET | `/admin/trips/active` |
| Xử lý chuyến lỗi | POST | `/admin/trips/{tripId}/issues` |
| Tra cứu giao dịch | GET | `/admin/payments` |
| Quản lý tài khoản | GET | `/admin/users` |
| Phân quyền | PUT | `/admin/users/{userId}/role` |
| Báo cáo chuyến | GET | `/reports/trips` |
| Báo cáo doanh thu | GET | `/reports/revenue` |
| Báo cáo tài xế | GET | `/reports/drivers` |
| Audit Log | GET | `/admin/audit-logs` |

---

# 25. Mối liên hệ giữa API và Functional Requirements

| Functional Requirement | API liên quan |
|---|---|
| FR-01 Đăng ký tài khoản | `POST /auth/register` |
| FR-02 Đăng nhập | `POST /auth/login` |
| FR-03 Cập nhật thông tin | `PUT /users/me` |
| FR-05 Quản lý hồ sơ tài xế | `/drivers/me` |
| FR-06 Quản lý phương tiện | `/drivers/me/vehicle` |
| FR-07 Trạng thái hoạt động | `PUT /drivers/me/status` |
| FR-08 Cập nhật vị trí | `PUT /drivers/me/location` |
| FR-09 Nhập điểm đón | `POST /bookings` |
| FR-10 Nhập điểm đến | `POST /bookings` |
| FR-11 Chọn loại xe | `POST /bookings` |
| FR-12 Tạo yêu cầu đặt xe | `POST /bookings` |
| FR-13 Theo dõi yêu cầu | `GET /bookings/{bookingId}` |
| FR-14 Tìm tài xế | `POST /bookings/{bookingId}/matching` |
| FR-17 Chấp nhận chuyến | `POST /bookings/{bookingId}/accept` |
| FR-18 Từ chối chuyến | `POST /bookings/{bookingId}/reject` |
| FR-22 Cập nhật trạng thái | `PUT /trips/{tripId}/status` |
| FR-27 Theo dõi chuyến | `GET /trips/{tripId}` |
| FR-28 Tính cước | `POST /trips/{tripId}/fare` |
| FR-29 Thanh toán tiền mặt | `POST /payments` |
| FR-30 Thanh toán điện tử | `POST /payments` |
| FR-32 Tra cứu giao dịch | `GET /admin/payments` |
| FR-33–FR-38 Thông báo | `/notifications` |
| FR-39 Xem lịch sử chuyến | `GET /trips/history` |
| FR-40 Xem chi tiết chuyến | `GET /trips/{tripId}/detail` |
| FR-41 Đánh giá tài xế | `POST /trips/{tripId}/reviews` |
| FR-42 Quản lý khách hàng | `/admin/customers` |
| FR-43 Quản lý tài xế | `/admin/drivers` |
| FR-44 Quản lý phương tiện | `/admin/vehicles` |
| FR-45 Theo dõi chuyến | `/admin/trips/active` |
| FR-46 Xử lý chuyến lỗi | `/admin/trips/{tripId}/issues` |
| FR-47–FR-48 Quản lý tài khoản / phân quyền | `/admin/users` |
| FR-49–FR-53 Báo cáo | `/reports/*` |
| FR-54 Xác thực người dùng | `/auth/*` |
| FR-55 Kiểm soát quyền | JWT + Role |
| FR-56 Ghi nhận thao tác | `/admin/audit-logs` |
