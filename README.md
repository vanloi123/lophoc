# Lớp Học Vẫy Tay

Sân chơi ôn tập điều khiển bằng cử chỉ tay qua webcam: 23 trò chơi, soạn bộ đề, nhập từ Excel, bảng điểm lớp.
Toàn bộ ứng dụng nằm trong một tệp `index.html`. Có thể đồng bộ dữ liệu giữa nhiều máy qua Supabase (không bắt buộc).

| Tệp | Dùng để |
|---|---|
| `index.html` | Ứng dụng |
| `supabase-schema.sql` | Tạo bảng dữ liệu trên Supabase (chạy một lần) |
| `Mau-cau-hoi-Lop-Hoc-Vay-Tay.xlsx` | File Excel mẫu để soạn câu hỏi |
| `.nojekyll` | Để GitHub Pages phục vụ tệp nguyên trạng |

---

## Bước 1 — Tạo cơ sở dữ liệu trên Supabase (khoảng 5 phút)

1. Vào <https://supabase.com>, đăng nhập, bấm **New project**. Đặt tên (ví dụ `lop-hoc-vay-tay`), đặt mật khẩu database, chọn vùng gần Việt Nam (ví dụ Singapore), bấm **Create**.
2. Khi project tạo xong: menu trái → **SQL Editor** → **New query** → mở tệp `supabase-schema.sql`, dán toàn bộ nội dung vào → bấm **Run**. Thấy “Success” là xong.
3. Menu trái → **Project Settings** → **API** (hoặc **Data API / API Keys**). Chép 2 giá trị:
   - **Project URL**, dạng `https://abcdxyz.supabase.co`
   - Khóa **anon public** (chuỗi dài bắt đầu bằng `eyJ…`, hoặc khóa *publishable* `sb_publishable_…`)

   > Khóa anon được phép công khai trên web: dữ liệu vẫn an toàn vì mỗi giáo viên chỉ đọc/ghi được dữ liệu của chính mình (đã bật RLS trong file SQL).
   > **Tuyệt đối không** dùng khóa `service_role` / `secret` trong trang web.

## Bước 2 — Điền thông tin Supabase vào trang

Mở `index.html` bằng Notepad (hoặc sửa trực tiếp trên GitHub), tìm ở gần đầu tệp dòng:

```js
window.LHVT_CLOUD = { url: "", anonKey: "" };
```

và điền 2 giá trị vừa chép:

```js
window.LHVT_CLOUD = { url: "https://abcdxyz.supabase.co", anonKey: "eyJhbGciOi..." };
```

(Nếu để trống, giáo viên vẫn có thể nhập 2 giá trị này trong **Cài đặt → Tài khoản & lưu trữ đám mây** trên từng máy.)

## Bước 3 — Đưa lên GitHub và bật GitHub Pages

1. Vào <https://github.com/new>, đặt tên repository (ví dụ `lop-hoc-vay-tay`), chọn **Public**, bấm **Create repository**.
2. Bấm **uploading an existing file**, kéo thả các tệp trong thư mục này vào (`index.html`, `supabase-schema.sql`, `README.md`, `.nojekyll`, file Excel mẫu) → **Commit changes**.
   *(Tệp `.nojekyll` bị ẩn trên một số máy; không có cũng chạy được.)*
3. Vào **Settings → Pages**. Ở **Build and deployment**, chọn **Source: Deploy from a branch**, **Branch: main**, thư mục **/(root)** → **Save**.
4. Sau 1–2 phút, trang có địa chỉ dạng: `https://<tên-tài-khoản>.github.io/lop-hoc-vay-tay/`

GitHub Pages chạy bằng HTTPS nên trình duyệt cho phép bật camera.

## Bước 4 — Cho Supabase biết địa chỉ trang (để đường link trong email hoạt động)

Supabase → **Authentication → URL Configuration**:

- **Site URL**: dán địa chỉ GitHub Pages ở bước 3, ví dụ `https://<tên-tài-khoản>.github.io/lop-hoc-vay-tay/`
- **Redirect URLs**: thêm cùng địa chỉ đó.

Tùy chọn: nếu không muốn giáo viên phải bấm link xác nhận email khi tạo tài khoản, vào **Authentication → Sign In / Providers → Email** và tắt **Confirm email**.
Gói miễn phí của Supabase giới hạn số email gửi đi mỗi giờ; nếu nhiều giáo viên đăng ký cùng lúc, nên tắt xác nhận email hoặc cấu hình SMTP riêng.

## Sử dụng

- Mở trang → **Cài đặt** → **Tài khoản & lưu trữ đám mây** → nhập email, mật khẩu → **Tạo tài khoản mới** (lần đầu) hoặc **Đăng nhập**.
- Biểu tượng đám mây ở góc trên cho biết trạng thái: *Đã đồng bộ*, *Đang đồng bộ…*, *Mất mạng – lưu trên máy*.
- Mọi thay đổi được lưu ngay trên máy và tự đẩy lên Supabase sau khoảng 1–2 giây. Mất mạng vẫn dạy bình thường; có mạng lại sẽ tự đồng bộ.
- Máy dùng chung (phòng máy): khi xong, dùng **Đăng xuất và xóa dữ liệu trên máy này**.

Dữ liệu được đồng bộ: bộ đề, danh sách lớp và điểm buổi học, kỷ lục, thiết lập từng trò chơi, trò vừa chơi.
Thiết lập camera, nhận diện tay và hiệu chỉnh tay là riêng cho từng máy nên không đồng bộ.

## Lưu ý

- Project Supabase miễn phí có thể bị tạm dừng nếu lâu không có ai dùng; khi đó vào Supabase và bấm **Restore project**. Dữ liệu trên máy vẫn còn, trang vẫn chạy được trong lúc chờ.
- Muốn cập nhật phiên bản mới: chỉ cần thay tệp `index.html` trên GitHub (nhớ điền lại `window.LHVT_CLOUD`).
