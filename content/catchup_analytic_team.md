# First Meeting – Analytic Team Catchup

## 1. Tổng quan về team & con người

**Số thành viên:** 04

| Thành viên | Vai trò | Thời gian làm việc |
|---|---|---|
| Đoàn Trung Hiếu | Team Lead | 1.5 năm |
| Nguyễn Thu An | Data Analyst | 9 tháng |
| Nguyễn Nhật Tường | Data Scientist | 7 tháng |
| Phạm Thị Nhung | Thực tập sinh | 5 tháng |

**Văn hóa team:** Rất autonomy, thích experimentation, giao tiếp thẳng thắn.

**Kỳ vọng với cơ cấu team mới:**
- Có định hướng rõ ràng
- Có cơ chế truy cập dữ liệu nhiều hơn để thỏa sức phân tích và ứng dụng công nghệ mới

---

## 2. Các dự án / task đã và đang thực hiện

### CPM – Customer Profiling
- Xây dựng dữ liệu nền tảng khách hàng
- Phân tích tìm insight từ dữ liệu, xây dựng báo cáo, dashboard
- Quản lý yêu cầu thu thập event tracking trên web, app (qua Countly – open source tool)

### CMS – Campaign Management System
- Hệ thống quản lý phân phối thông báo cho khách hàng (notification, toast message, popup trên App SmartOne) theo danh sách khách hàng, theo lịch và kịch bản tùy biến
- Đã golive nhưng chưa apply case thực tế nào
- Đề xuất: bàn giao về TTTK sản phẩm nếu tiếp tục phát triển (không gắn chặt với chức năng Data)

### CRM – Salesforce
- Đã trải qua giai đoạn Discovery (tìm hiểu giải pháp, thu thập yêu cầu sơ bộ business) và POC
- Đã pass round đánh giá giải pháp (chức năng nghiệp vụ, kỹ thuật, bảo mật)
- Hiện tạm dừng do định hướng tự build giải pháp CRM nội bộ

### AI & Automation – Vibe Code, n8n
- Ứng dụng AI vào tự động hóa task thủ công cho team Design & Research (CX cũ)
- Đã trình bày workshop cho các team
- Hiện không còn critical do đã restructure team

### AI cho DA/DS – Claude Code, Vibe Code
- Nghiên cứu ứng dụng AI vào: EDA dữ liệu, báo cáo data quality, xử lý data quality cơ bản, generate code bằng NLQ (Natural Language Query)
- Task tự phát sinh trong giai đoạn restructure, đang bị pending

### Adhoc – Báo cáo luồng nạp tiền (Deeplink)
- Yêu cầu thống kê, làm báo cáo cho luồng nạp tiền qua Deeplink dựa trên dữ liệu hành vi khách hàng trên App
- Liên quan đến dự án cải tiến luồng nạp tiền qua SmartOne App (đầu mối: TTTK sản phẩm)

---

## 3. Thách thức & Pain Points

- **Thiếu analytic platform chuẩn:** Chưa có môi trường đầy đủ (PySpark, dbt, Python/Notebook, …)
- **Hạn chế truy cập dữ liệu thật:** Gần như toàn bộ dữ liệu production liên quan đến khách hàng chưa được truy cập (ngoại trừ dữ liệu hành vi qua event tracking)
- **Thiếu knowledge sharing:** Không có business glossary hay data dictionary; mỗi lần tìm hiểu dữ liệu phải làm việc trực tiếp với business và IT → tốn rất nhiều effort
- **Tư duy "quá cầu toàn":** Các task dữ liệu thường bị kéo dài do kỳ vọng hoàn hảo ngay từ đầu (nhận xét cá nhân)

---

## 4. Quy trình làm việc cũ

```
Yêu cầu từ Head of CX (hoặc bên khác qua Head of CX)
  → Team DA/DS phân tích yêu cầu
  → Lên yêu cầu dữ liệu
  → Gửi HAV develop
  → Team Analytic phân tích, tạo báo cáo/dashboard
    (nếu chỉ liên quan event data)
    HOẶC team HAV develop báo cáo
    (nếu không phải dữ liệu hành vi)
  → Team Analytic test, nghiệm thu
  → Golive production
```

---

## 5. Ideas – Hướng tạo value cho business

- **Truy cập dữ liệu giao dịch (production):** Khai phá dữ liệu, đưa ra insight (hiện team chưa có điều kiện thực hiện)
- **Automation cho Broker:**
  - Tự động thu thập thông tin thị trường (trong nước & nước ngoài)
  - Phân tích cơ bản & kỹ thuật: thị trường, ngành, mã cổ phiếu
  - Tự động gửi báo cáo cho broker/khách hàng
  - Giao tiếp với broker qua Telegram/WhatsApp
- **NLQ – Natural Language Query:**
  - Build luồng tự động xử lý data cơ bản
  - Trả lời câu hỏi nghiệp vụ bằng ngôn ngữ tự nhiên
