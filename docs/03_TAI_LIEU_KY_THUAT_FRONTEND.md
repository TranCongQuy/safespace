# TÀI LIỆU NGHIỆP VỤ

# DỰ ÁN: SAFESPACE - NỀN TẢNG PHÒNG CHỐNG TỰ TỬ

**Mã dự án:** SP-2026-001  
**Phiên bản:** 1.0  
**Ngày ban hành:** 09/10/2026  
**Tác giả:** Trần Công Quý  
**Đơn vị:** Đại học Phú Xuân - Khoa Công nghệ Thông tin

---

## MỤC LỤC

1. Tổng quan dự án
2. Vấn đề xã hội và động lực
3. Mục tiêu dự án
4. Phạm vi dự án
5. Đối tượng người dùng
6. Yêu cầu chức năng
7. Yêu cầu phi chức năng
8. Quy tắc nghiệp vụ
9. Use Cases chi tiết
10. Quy trình nghiệp vụ
11. Rủi ro và biện pháp
12. Tiêu chí đánh giá thành công
13. Phụ lục

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1 Giới thiệu

**SafeSpace** là nền tảng web hỗ trợ sức khỏe tâm thần và phòng chống tự tử, cung cấp không gian an toàn, ẩn danh để người dùng có thể:

- Chia sẻ cảm xúc, khó khăn trong cuộc sống
- Tiếp cận đường dây nóng khẩn cấp 24/7
- Kết nối với tình nguyện viên đã được đào tạo
- Theo dõi và cải thiện sức khỏe tinh thần

### 1.2 Bối cảnh

Theo thống kê của Tổ chức Y tế Thế giới (WHO):

- **700.000+ người** tử vong do tự tử mỗi năm trên toàn cầu
- **Tại Việt Nam**: ước tính **5.000-6.000 ca/năm** (chưa đầy đủ do underreport)
- **Độ tuổi 15-29**: Tự tử là nguyên nhân tử vong đứng thứ 4
- **Sau COVID-19**: Tỷ lệ trầm cảm, lo âu ở giới trẻ VN **tăng 30-40%**

### 1.3 Vấn đề cấp thiết

| #   | Vấn đề                           | Hệ quả                                 |
| --- | -------------------------------- | -------------------------------------- |
| 1   | Thiếu không gian chia sẻ ẩn danh | Người gặp khủng hoảng không dám nói ra |
| 2   | Kỳ thị sức khỏe tâm thần         | Không tìm kiếm sự hỗ trợ               |
| 3   | Đường dây nóng chưa phổ biến     | Không biết liên hệ ai khi khủng hoảng  |
| 4   | Chi phí tư vấn cao               | Không đủ khả năng chi trả              |
| 5   | Thiếu công cụ self-help          | Không biết cách tự chăm sóc            |

### 1.4 Giải pháp

Xây dựng nền tảng web SafeSpace với các module:

- **Crisis Support**: Nút SOS, hotline, quiz đánh giá nguy cơ
- **Mood Tracking**: Nhật ký cảm xúc, biểu đồ tâm trạng
- **Community**: Diễn đàn ẩn danh, chat với supporter
- **Resources**: Bài viết, hướng dẫn, bài tập thiền
- **Admin Panel**: Quản lý nội dung, hỗ trợ khẩn cấp

---

## 2. VẤN ĐỀ XÃ HỘI VÀ ĐỘNG LỰC

### 2.1 Số liệu thực tế

| Chỉ số                              | Số liệu      | Nguồn              |
| ----------------------------------- | ------------ | ------------------ |
| Ca tự tử/năm tại VN                 | ~5.000-6.000 | WHO 2023           |
| Người có ý định tự tử               | ~100.000+    | Ước tính           |
| Tỷ lệ trầm cảm ở SV                 | 15-20%       | Nghiên cứu ĐHQG    |
| Người từng có ý định tự tử          | 5-10% dân số | Nghiên cứu tâm lý  |
| Khoảng cách từ ý định đến hành động | 10-30 phút   | Nghiên cứu quốc tế |

### 2.2 Động lực xây dựng

**Câu chuyện người dùng điển hình:**

> _"Minh, 19 tuổi, sinh viên năm 1. Cảm thấy cô đơn, áp lực thi cử, bố mẹ ly hôn. Không dám nói với ai vì sợ bị đánh giá 'yếu đuối'. Lên mạng tìm kiếm, thấy toàn thông tin tiêu cực. Không biết gọi ai. Cuối cùng... đã có lúc nghĩ đến việc kết thúc tất cả."_

**SafeSpace sẽ giúp:**

- Minh có nơi để **nói ra** mà không sợ bị phán xét
- Tiếp cận **ngay** hotline khi khủng hoảng
- **Theo dõi** cảm xúc để thấy pattern
- Kết nối với **supporter** đã được đào tạo
- Đọc **câu chuyện** của người đã vượt qua

### 2.3 Tính cấp thiết

- **Can thiệp sớm** giảm 60-80% nguy cơ tự tử
- **Đường dây nóng** giảm 40-50% ca tự tử
- **Peer support** (hỗ trợ đồng cảnh) hiệu quả cao

---

## 3. MỤC TIÊU DỰ ÁN

### 3.1 Mục tiêu tổng quát

Xây dựng nền tảng web hỗ trợ sức khỏe tâm thần, giúp **giảm tỷ lệ tự tử** và **cải thiện chất lượng sống** của người dùng thông qua việc cung cấp:

- Không gian an toàn, ẩn danh
- Kết nối khẩn cấp với chuyên gia
- Công cụ tự chăm sóc sức khỏe tinh thần

### 3.2 Mục tiêu cụ thể (SMART)

| #   | Mục tiêu                            | Chỉ số đo lường                      | Thời hạn |
| --- | ----------------------------------- | ------------------------------------ | -------- |
| 1   | Cung cấp không gian chia sẻ ẩn danh | 100% user có thể post ẩn danh        | 4 tuần   |
| 2   | Kết nối hotline khẩn cấp            | Nút SOS hiển thị trên 100% trang     | 4 tuần   |
| 3   | Hỗ trợ theo dõi cảm xúc             | User có thể log mood hàng ngày       | 4 tuần   |
| 4   | Kết nối cộng đồng                   | ≥ 50 bài viết hỗ trợ trong tháng đầu | 8 tuần   |
| 5   | Kết nối supporter                   | ≥ 10 tình nguyện viên đăng ký        | 8 tuần   |
| 6   | Tài liệu hóa đầy đủ                 | 4 file tài liệu hoàn chỉnh           | 4 tuần   |

### 3.3 Mục tiêu dài hạn

- Trở thành nền tảng **top đầu VN** về hỗ trợ sức khỏe tâm thần
- Hợp tác với **tổ chức phi chính phủ** (NGOs)
- Tích hợp với **bệnh viện tâm thần** trên toàn quốc
- Mở rộng ra **các nước Đông Nam Á**

---

## 4. PHẠM VI DỰ ÁN

### 4.1 Trong phạm vi (In-scope)

- ✅ Web app responsive (mobile + desktop)
- ✅ Anonymous authentication
- ✅ Crisis support (SOS, hotline, quiz)
- ✅ Mood tracking với biểu đồ
- ✅ Diễn đàn ẩn danh
- ✅ Chat 1-1 với supporter (realtime)
- ✅ Resources (bài viết, thiền)
- ✅ Admin panel

### 4.2 Ngoài phạm vi (Out-of-scope)

- ❌ Mobile app native (iOS/Android)
- ❌ Tư vấn video call
- ❌ Kê đơn thuốc
- ❌ Chẩn đoán y khoa
- ❌ Thay thế dịch vụ y tế chuyên nghiệp

### 4.3 Giới hạn

- Chỉ hỗ trợ **tiếng Việt**
- Chỉ hỗ trợ **web platform**
- **KHÔNG** phải dịch vụ y tế chính thức
- **KHÔNG** thu thập thông tin định danh thật

---

## 5. ĐỐI TƯỢNG NGƯỜI DÙNG

### 5.1 Personas chi tiết

#### Persona 1: **Người cần hỗ trợ (User)**

- **Tên:** Minh, 19 tuổi
- **Nghề:** Sinh viên năm 1
- **Vấn đề:** Stress thi cử, cô đơn, lo âu
- **Mục tiêu:** Có nơi chia sẻ không bị phán xét
- **Nhu cầu:** Ẩn danh, dễ tiếp cận, có hotline
- **Thiết bị:** Điện thoại (mobile-first)

#### Persona 2: **Tình nguyện viên (Supporter)**

- **Tên:** Lan, 25 tuổi
- **Nghề:** Sinh viên tâm lý học năm cuối
- **Vấn đề:** Muốn giúp người khác
- **Mục tiêu:** Lắng nghe, hỗ trợ người khác
- **Nhu cầu:** Công cụ chat, hướng dẫn training
- **Thiết bị:** Máy tính

#### Persona 3: **Quản trị viên (Admin)**

- **Tên:** Anh Hùng, 30 tuổi
- **Nghề:** Chuyên gia tâm lý
- **Vấn đề:** Cần kiểm soát nội dung nguy hiểm
- **Mục tiêu:** Đảm bảo môi trường an toàn
- **Nhu cầu:** Dashboard, công cụ moderation
- **Thiết bị:** Máy tính

### 5.2 Bảng phân quyền

| Vai trò       | Xem | Đăng bài | Comment | Chat | Mood Log | Admin           |
| ------------- | --- | -------- | ------- | ---- | -------- | --------------- |
| **Guest**     | ✅  | ❌       | ❌      | ❌   | ❌       | ❌              |
| **User**      | ✅  | ✅       | ✅      | ✅   | ✅       | ❌              |
| **Supporter** | ✅  | ✅       | ✅      | ✅   | ✅       | ❌              |
| **Moderator** | ✅  | ✅       | ✅      | ✅   | ✅       | ✅ (một phần)   |
| **Admin**     | ✅  | ✅       | ✅      | ✅   | ✅       | ✅ (toàn quyền) |

---

## 6. YÊU CẦU CHỨC NĂNG

### 6.1 Module 1: Xác thực ẩn danh

| ID    | Chức năng         | Mô tả                                  | Ưu tiên      |
| ----- | ----------------- | -------------------------------------- | ------------ |
| F-1.1 | Đăng nhập ẩn danh | Tạo tài khoản tự động, không cần email | BẮT BUỘC     |
| F-1.2 | Đặt nickname      | User chọn biệt danh ẩn danh            | BẮT BUỘC     |
| F-1.3 | Avatar tự động    | Sinh avatar từ seed                    | BẮT BUỘC     |
| F-1.4 | Đăng xuất         | Clear session                          | BẮT BUỘC     |
| F-1.5 | Xóa tài khoản     | Xóa toàn bộ data của user              | KHUYẾN KHÍCH |

**Lưu ý bảo mật:**

- KHÔNG yêu cầu email, số điện thoại
- KHÔNG lưu IP
- KHÔNG track user across sites

### 6.2 Module 2: Crisis Support (HỖ TRỢ KHẨN CẤP)

| ID    | Chức năng                | Mô tả                                                | Ưu tiên      |
| ----- | ------------------------ | ---------------------------------------------------- | ------------ |
| F-2.1 | Nút SOS nổi bật          | Hiển thị mọi trang, click hiện hỗ trợ ngay           | BẮT BUỘC     |
| F-2.2 | Banner hotline           | Hiển thị hotline 24/7 trên header                    | BẮT BUỘC     |
| F-2.3 | Crisis Quiz              | Đánh giá mức độ nguy hiểm (low/medium/high/critical) | BẮT BUỘC     |
| F-2.4 | Breathing Exercise       | Hít thở 4-7-8 có animation                           | BẮT BUỘC     |
| F-2.5 | Emergency Modal          | Modal hiện hotline + chat ngay                       | BẮT BUỘC     |
| F-2.6 | Detect từ khóa nguy hiểm | Tự động cảnh báo khi post chứa từ khóa               | BẮT BUỘC     |
| F-2.7 | Gợi ý hành động          | Đề xuất cụ thể theo mức độ nguy hiểm                 | BẮT BUỘC     |
| F-2.8 | Danh sách hotline        | Trang riêng với tất cả hotline VN                    | KHUYẾN KHÍCH |

### 6.3 Module 3: Mood Tracking

| ID    | Chức năng          | Mô tả                             | Ưu tiên      |
| ----- | ------------------ | --------------------------------- | ------------ |
| F-3.1 | Log mood hàng ngày | Chọn mức độ 1-5 + cảm xúc         | BẮT BUỘC     |
| F-3.2 | Ghi chú            | Note riêng tư (chỉ user xem)      | BẮT BUỘC     |
| F-3.3 | Biểu đồ tâm trạng  | Chart.js hiển thị theo tuần/tháng | BẮT BUỘC     |
| F-3.4 | Calendar view      | Lịch với màu mood                 | KHUYẾN KHÍCH |
| F-3.5 | Streak counter     | Đếm ngày liên tục log             | KHUYẾN KHÍCH |
| F-3.6 | Export data        | Tải về JSON/PDF                   | TÙY CHỌN     |
| F-3.7 | Phân tích pattern  | AI phát hiện xu hướng tiêu cực    | NÂNG CAO     |

### 6.4 Module 4: Community (Diễn đàn ẩn danh)

| ID    | Chức năng         | Mô tả                        | Ưu tiên      |
| ----- | ----------------- | ---------------------------- | ------------ |
| F-4.1 | Đăng bài ẩn danh  | Tạo post với title + content | BẮT BUỘC     |
| F-4.2 | Danh mục bài viết | share/question/support/story | BẮT BUỘC     |
| F-4.3 | Comment           | Bình luận ẩn danh            | BẮT BUỘC     |
| F-4.4 | Reactions         | Like/hug/support             | BẮT BUỘC     |
| F-4.5 | Report nội dung   | Báo cáo vi phạm              | BẮT BUỘC     |
| F-4.6 | Filter bài viết   | Theo category, sort          | KHUYẾN KHÍCH |
| F-4.7 | Tag người dùng    | @mention (ẩn danh)           | TÙY CHỌN     |
| F-4.8 | Follow user       | Theo dõi user khác           | TÙY CHỌN     |

### 6.5 Module 5: Chat với Supporter

| ID    | Chức năng           | Mô tả                          | Ưu tiên      |
| ----- | ------------------- | ------------------------------ | ------------ |
| F-5.1 | Danh sách supporter | Xem list supporter online      | BẮT BUỘC     |
| F-5.2 | Bắt đầu chat        | Bấm để bắt đầu session         | BẮT BUỘC     |
| F-5.3 | Chat realtime       | Supabase Realtime              | BẮT BUỘC     |
| F-5.4 | Kết thúc chat       | Đánh giá + feedback            | BẮT BUỘC     |
| F-5.5 | Escalate khẩn cấp   | Chuyển lên admin nếu nguy hiểm | BẮT BUỘC     |
| F-5.6 | Lịch sử chat        | Xem lại (30 ngày gần nhất)     | KHUYẾN KHÍCH |
| F-5.7 | Chat ẩn danh        | Supporter không thấy identity  | BẮT BUỘC     |

### 6.6 Module 6: Resources (Tài nguyên)

| ID    | Chức năng           | Mô tả                     | Ưu tiên      |
| ----- | ------------------- | ------------------------- | ------------ |
| F-6.1 | Bài viết giáo dục   | Về trầm cảm, lo âu, tự tử | BẮT BUỘC     |
| F-6.2 | Câu chuyện vượt qua | Testimonial               | BẮT BUỘC     |
| F-6.3 | Bài tập thiền       | Audio + hướng dẫn         | BẮT BUỘC     |
| F-6.4 | Hướng dẫn giúp đỡ   | Cách giúp người khác      | KHUYẾN KHÍCH |
| F-6.5 | Danh sách hotline   | Đầy đủ, phân vùng         | BẮT BUỘC     |
| F-6.6 | Tìm kiếm tài nguyên | Search + tag              | KHUYẾN KHÍCH |

### 6.7 Module 7: Admin Panel

| ID    | Chức năng         | Mô tả                 | Ưu tiên      |
| ----- | ----------------- | --------------------- | ------------ |
| F-7.1 | Dashboard         | Thống kê tổng quan    | BẮT BUỘC     |
| F-7.2 | Quản lý bài viết  | Duyệt/xóa/hide post   | BẮT BUỘC     |
| F-7.3 | Quản lý user      | Ban/unban user        | BẮT BUỘC     |
| F-7.4 | Xử lý report      | Xem, resolve, dismiss | BẮT BUỘC     |
| F-7.5 | Quản lý hotline   | CRUD hotline          | BẮT BUỘC     |
| F-7.6 | Quản lý resources | CRUD bài viết         | BẮT BUỘC     |
| F-7.7 | Quản lý supporter | Verify, đào tạo       | BẮT BUỘC     |
| F-7.8 | Audit log         | Xem log hoạt động     | KHUYẾN KHÍCH |
| F-7.9 | Export báo cáo    | Excel/PDF             | TÙY CHỌN     |

---

## 7. YÊU CẦU PHI CHỨC NĂNG

### 7.1 Hiệu năng

| Yêu cầu                 | Chỉ số                    |
| ----------------------- | ------------------------- |
| Thời gian tải trang chủ | < 2s                      |
| Thời gian phản hồi API  | < 500ms (95th percentile) |
| Concurrent users        | ≥ 1.000                   |
| Uptime                  | ≥ 99%                     |
| Chat realtime latency   | < 200ms                   |

### 7.2 Bảo mật

| Yêu cầu            | Giải pháp                       |
| ------------------ | ------------------------------- |
| Ẩn danh tuyệt đối  | Anonymous auth, không lưu email |
| Mã hóa dữ liệu     | HTTPS, encryption at rest       |
| Row Level Security | Supabase RLS policies           |
| Chống spam         | Rate limiting                   |
| Content moderation | AI + manual review              |
| Data retention     | Auto-delete sau 30 ngày (chat)  |

### 7.3 Khả năng sử dụng

| Yêu cầu               | Chi tiết                      |
| --------------------- | ----------------------------- |
| Mobile responsive     | 320px - 1920px                |
| Accessibility         | WCAG 2.1 AA                   |
| Ngôn ngữ              | Tiếng Việt                    |
| Trình duyệt           | Chrome, Firefox, Safari, Edge |
| Thời gian học sử dụng | < 5 phút                      |

### 7.4 Khả năng mở rộng

- Horizontal scaling
- Database indexing
- CDN cho static assets
- Microservices-ready (future)

### 7.5 Độ tin cậy

- Auto backup hàng ngày
- Error logging tập trung
- Monitoring + alerting
- Graceful degradation

---

## 8. QUY TẮC NGHIỆP VỤ

### 8.1 Quy tắc nội dung

| Mã    | Quy tắc                                         | Hành động vi phạm  |
| ----- | ----------------------------------------------- | ------------------ |
| BR-01 | Không được khuyến khích tự tử                   | Hide + ban 7 ngày  |
| BR-02 | Không được chia sẻ phương pháp tự tử            | Hide + ban 30 ngày |
| BR-03 | Không được quấy rối, xúc phạm                   | Cảnh cáo + hide    |
| BR-04 | Không được spam quảng cáo                       | Xóa + ban          |
| BR-05 | Không được tiết lộ thông tin cá nhân người khác | Hide + cảnh cáo    |
| BR-06 | Không được giả mạo supporter                    | Ban vĩnh viễn      |

### 8.2 Quy tắc chat

| Mã    | Quy tắc                                      |
| ----- | -------------------------------------------- |
| BR-07 | Supporter không được hỏi thông tin định danh |
| BR-08 | Supporter không được khuyên dùng thuốc       |
| BR-09 | Chat tự động xóa sau 30 ngày                 |
| BR-10 | Nếu có dấu hiệu khủng hoảng → escalate ngay  |

### 8.3 Quy tắc dữ liệu

| Mã    | Quy tắc                             |
| ----- | ----------------------------------- |
| BR-11 | Mood logs CHỈ user xem được         |
| BR-12 | Không chia sẻ dữ liệu với bên thứ 3 |
| BR-13 | User có quyền xóa toàn bộ data      |
| BR-14 | Không lưu IP, không track           |
| BR-15 | Backup mã hóa                       |

### 8.4 Quy tắc khẩn cấp

| Mã    | Quy tắc                                        |
| ----- | ---------------------------------------------- |
| BR-16 | Nút SOS luôn hiển thị, không được ẩn           |
| BR-17 | Khi detect từ khóa nguy hiểm → hiện alert ngay |
| BR-18 | Khi user chọn level "critical" → gọi admin     |
| BR-19 | Admin phải phản hồi trong 5 phút               |
| BR-20 | Lưu crisis assessment (ẩn danh) để phân tích   |

---

## 9. USE CASES CHI TIẾT

### UC-01: User gặp khủng hoảng, cần hỗ trợ ngay

**Actor:** User  
**Precondition:** User đã mở app  
**Trigger:** User bấm nút SOS

**Main Flow:**

1. User bấm nút SOS (nổi bật góc phải)
2. Hệ thống mở Emergency Modal
3. Modal hiển thị:
   - Hotline 1800 599 920 (miễn phí, 24/7)
   - Nút "Chat với supporter"
   - Nút "Hít thở 4-7-8"
   - Danh sách hotline khác
4. User chọn 1 trong 3:
   - **A.** Gọi hotline → hệ thống log + close
   - **B.** Chat với supporter → tạo session
   - **C.** Hít thở → mở Breathing Exercise
5. Nếu chọn chat → hệ thống tìm supporter online
6. Nếu không có supporter → hiển thị message động viên + hotline

**Alternative Flow:**

- Nếu user chọn "critical" trong quiz → gọi admin

**Postcondition:** User đã được kết nối với hỗ trợ

---

### UC-02: User log mood hàng ngày

**Actor:** User  
**Precondition:** Đã đăng nhập ẩn danh

**Main Flow:**

1. User vào trang "Mood Tracker"
2. Chọn mức độ mood (1-5 với emoji)
3. Chọn emotions (nhiều lựa chọn)
4. Ghi chú (optional)
5. Bấm "Lưu"
6. Hệ thống lưu vào DB
7. Hiển thị confirmation + streak
8. Update biểu đồ

**Postcondition:** Mood log đã được lưu (chỉ user xem)

---

### UC-03: User đăng bài chia sẻ

**Actor:** User  
**Precondition:** Đã đăng nhập

**Main Flow:**

1. User vào "Community"
2. Bấm "Đăng bài"
3. Chọn category (share/question/support/story)
4. Nhập title + content
5. **Hệ thống tự động scan từ khóa nguy hiểm**
6. Nếu phát hiện:
   - Hiển thị warning: "Bài viết của bạn có dấu hiệu cần hỗ trợ"
   - Hiện hotline + nút chat
   - Cho phép user tiếp tục hoặc hủy
7. User submit
8. Post hiển thị với status "pending review"
9. Admin duyệt

**Alternative Flow:**

- Nếu user chọn ẩn danh hoàn toàn → không hiện nickname

---

### UC-04: Supporter chat với user

**Actor:** Supporter  
**Precondition:** Đã được verify

**Main Flow:**

1. Supporter online (bật status)
2. Nhận notification có user cần hỗ trợ
3. Chấp nhận session
4. Chat realtime với user (ẩn danh 2 chiều)
5. Nếu phát hiện nguy hiểm → bấm "Escalate"
6. Kết thúc → đánh giá session

---

### UC-05: Admin xử lý report

**Actor:** Admin  
**Precondition:** Có report mới

**Main Flow:**

1. Admin vào Dashboard
2. Xem danh sách report pending
3. Click vào report → xem chi tiết
4. Xem xét nội dung
5. Chọn hành động:
   - **Hide post** + cảnh cáo
   - **Ban user** (7/30 ngày/vĩnh viễn)
   - **Dismiss** (không vi phạm)
6. Ghi lý do
7. Update status report

---

## 10. QUY TRÌNH NGHIỆP VỤ

### 10.1 Flow đăng ký ẩn danh

User mở app
↓
Click "Bắt đầu ẩn danh"
↓
Hệ thống tạo tài khoản (Supabase Anonymous)
↓
Sinh nickname ngẫu nhiên (VD: "Mèo con 123")
↓
Sinh avatar từ seed
↓
User có thể đổi nickname nếu muốn
↓
Vào trang chủ

text

### 10.2 Flow phát hiện khủng hoảng

User type content (post/comment/chat)
↓
System scan keywords
↓
Nếu có keyword nguy hiểm?
├─ CÓ → Hiển thị CrisisAlert modal
│ ├─ Hotline
│ ├─ Nút chat
│ └─ Quiz đánh giá
└─ KHÔNG → Tiếp tục bình thường

text

### 10.3 Flow escalation

Supporter đang chat
↓
Phát hiện user có dấu hiệu nguy hiểm
↓
Bấm nút "Escalate khẩn cấp"
↓
System:

Gửi notification cho admin

Đánh dấu session = "escalated"

Hiển thị hotline cho user

Lưu vào crisis log
↓
Admin nhận notification
↓
Admin tham gia chat (với role special)
↓
Đánh giá + quyết định

text

---

## 11. RỦI RO VÀ BIỆN PHÁP

### 11.1 Rủi ro nghiệp vụ

| Rủi ro                           | Mức độ   | Biện pháp              |
| -------------------------------- | -------- | ---------------------- |
| User hiểu nhầm app thay thế y tế | HIGH     | Disclaimer rõ ràng     |
| Thông tin sai lệch về tâm lý     | HIGH     | Review bởi chuyên gia  |
| User giả danh supporter          | MEDIUM   | Verify + audit log     |
| Bị lợi dụng để lừa đảo           | MEDIUM   | Moderation chặt        |
| Data breach lộ danh tính         | CRITICAL | Anonymous, encryption  |
| Không đủ supporter online        | MEDIUM   | Auto-respond + hotline |

### 11.2 Rủi ro kỹ thuật

| Rủi ro                        | Mức độ   | Biện pháp                  |
| ----------------------------- | -------- | -------------------------- |
| Server down khi user cần nhất | CRITICAL | Multi-region, uptime 99.9% |
| Chat không realtime           | HIGH     | Supabase Realtime fallback |
| Database quá tải              | MEDIUM   | Indexing + caching         |
| Spam/DDoS                     | MEDIUM   | Rate limiting + CDN        |

### 11.3 Rủi ro pháp lý

| Rủi ro                          | Mức độ | Biện pháp                         |
| ------------------------------- | ------ | --------------------------------- |
| Bị coi là hành nghề y trái phép | HIGH   | Disclaimer + không chẩn đoán      |
| Vi phạm quyền riêng tư          | HIGH   | Tuân thủ Nghị định 13/2023        |
| Trách nhiệm nếu user tự tử      | HIGH   | Terms rõ ràng + emergency contact |

---

## 12. TIÊU CHÍ ĐÁNH GIÁ THÀNH CÔNG

### 12.1 Tiêu chí kỹ thuật

| #   | Tiêu chí            | Mục tiêu          |
| --- | ------------------- | ----------------- |
| 1   | Thời gian tải       | < 2s              |
| 2   | Uptime              | ≥ 99%             |
| 3   | Bug critical        | 0                 |
| 4   | Accessibility score | ≥ 90 (Lighthouse) |
| 5   | Mobile responsive   | 100% trang        |

### 12.2 Tiêu chí nghiệp vụ

| #   | Tiêu chí                  | Mục tiêu  |
| --- | ------------------------- | --------- |
| 1   | User đăng ký tháng đầu    | ≥ 100     |
| 2   | Bài viết hỗ trợ           | ≥ 50      |
| 3   | Supporter đăng ký         | ≥ 10      |
| 4   | Thời gian phản hồi crisis | < 5 phút  |
| 5   | User hài lòng             | ≥ 4/5 sao |

### 12.3 Tiêu chí học thuật

| #   | Tiêu chí                | Trạng thái |
| --- | ----------------------- | ---------- |
| 1   | 4 file tài liệu đầy đủ  | ✅         |
| 2   | Source code trên GitHub | ✅         |
| 3   | Deploy production       | ✅         |
| 4   | Demo hoạt động          | ✅         |
| 5   | Báo cáo phản ánh        | ✅         |

---

## 13. PHỤ LỤC

### 13.1 Danh sách hotline khẩn cấp

| Tổ chức                          | Hotline       | Giờ    | Khu vực   |
| -------------------------------- | ------------- | ------ | --------- |
| Tổng đài Quốc gia Bảo vệ Trẻ em  | 111           | 24/7   | Toàn quốc |
| Đường dây nóng Sức khỏe Tâm thần | 1800 599 920  | 24/7   | Toàn quốc |
| BV Tâm thần TW 1                 | 024 3825 2240 | Giờ HC | Miền Bắc  |
| BV Tâm thần TP.HCM               | 028 3822 3470 | Giờ HC | Miền Nam  |
| Tổ chức Good Neighbors           | 024 3221 2345 | Giờ HC | Miền Bắc  |

### 13.2 Từ khóa cảnh báo nguy hiểm

tự tử, tự sát, kết thúc cuộc đời, không muốn sống,
chết đi, biến mất, suicide, kill myself,
không còn lý do để sống, mệt mỏi lắm rồi,
muốn ngủ mãi, không ai cần tôi, vô dụng

text

### 13.3 Tài liệu tham khảo

- WHO. (2023). _Suicide Worldwide in 2019_.
- Bộ Y tế VN. (2022). _Báo cáo Sức khỏe Tâm thần_.
- Nghị định 13/2023/NĐ-CP về Bảo vệ dữ liệu cá nhân.
- Supabase Documentation: https://supabase.com/docs
- React Documentation: https://react.dev

---

**PHÊ DUYỆT**

| Vai trò    | Họ tên          | Chữ ký | Ngày       |
| ---------- | --------------- | ------ | ---------- |
| Tác giả    | Trần Công Quý   |        | 09/10/2026 |
| Giảng viên | Trần Thanh Bình |        |            |
