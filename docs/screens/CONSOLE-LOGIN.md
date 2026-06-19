# Console Login Screen Spec

| Trường | Giá trị |
|---|---|
| **Screen ID** | SOO-LG01 (SOO) / chung cho tất cả Tenant Console |
| **Tenant** | All (SOO, 39 Drug, FK Drug, Hashi Drug, Universal Drug, Uchiyamasasaki, Maruto Drug) |
| **Platform** | CONSOLE |
| **Category** | COMMON UI |
| **Spec source** | `COMMON UI` sheet — [1716][SOO] SCREEN SPECIFICATION.xlsx |
| **Last updated** | 2025-12-30 |
| **Author** | ANHTTT |

---

## Spec Overview

Sheet COMMON UI bao gồm các tính năng sau (tất cả đều là COMMON cho mọi Tenant):

1. **Login** — hỗ trợ 3 mode: No 2FA / 2FA via Mail / 2FA via Authenticator App
2. **Forgot Password** — reset qua mail
3. **Site Menu** — menu trái, khác nhau theo Tenant
4. **Header** — tên console, role, notification icon, user account button
5. **Footer** — phân trang
6. **User Setting** — đổi Login ID / Mail / Password / cài đặt 2FA
7. **Logout**
8. **Favicon & Tab Name**

---

## (1) Login Screen

### UI Components

| No. | Item (JP) | Item (EN) | Component Type | Required | Expected Behavior |
|---|---|---|---|---|---|
| 1.0 | Logo | Logo | ICON | — | Hiển thị logo của từng Tenant. SOO / 39 Drug / FK Drug mỗi Tenant có logo riêng |
| 2.0 | Console Name | Console Name | LABEL | — | Hiển thị tên Console theo Tenant:<br>① SOO: `SOOアプリコンソール`<br>② 39 Drug: `サンキュードラッグアプリコンソール`<br>③ FK Drug: `FKアプリコンソール` |
| 3.0 | ログインID | Login ID | TEXTBOX | ◯ | Nhập Login ID đã đăng ký |
| 4.0 | パスワード | Password | TEXTBOX | ◯ | Nhập Password đã đăng ký |
| 5.0 | ログイン | Login | BUTTON | — | Chỉ enable khi có đủ data ở Login ID & Password. Khi click → validate và đăng nhập |
| 6.0 | パスワードをお忘れの方 | Forgot Pass | HYPERLINK | — | Khi click → di chuyển đến màn hình (2) Forgot Password |

---

### Login Flow — 3 Cases theo cài đặt 2FA

#### Case a: LOGIN_2FA = NULL (Không bật 2FA)

Khi click button Login → validate thành công → di chuyển thẳng đến màn hình đầu tiên của Tenant:
- SOO: màn hình Tenant Management
- 39 Drug / FK Drug / các Tenant khác: màn hình Dashboard

---

#### Case b: LOGIN_2FA = YES = Via MAIL (First Login)

Khi click button Login → hiển thị màn hình chọn phương thức 2FA:

| No. | Item (EN) | Component Type | Behavior |
|---|---|---|---|
| ① | 2FA via Mail | RADIO | Default được chọn. Sau khi chọn và click Setting → hệ thống gửi 2FA code đến mail đã đăng ký (theo template mail). Nhập đúng code → đăng nhập thành công |
| ①.1 | Setting | BUTTON | Click → gửi 2FA code đến mail đã đăng ký |

---

#### Case b: LOGIN_2FA = YES = Via AUTHENTICATOR APP (First Login)

| No. | Item (EN) | Component Type | Behavior |
|---|---|---|---|
| ② | 2FA via Authenticator App | RADIO | — |
| ②.1 | Display QR Code | BUTTON | Click → hiển thị QR code phía dưới để scan bằng Authenticator App |
| ②.2 | QR Code | QR CODE | Hiển thị sau khi click ②.1. Dùng điện thoại cài sẵn Authenticator App để scan |
| ②.3 | 2FA Code & Setting | TEXTBOX + BUTTON | Nhập 2FA code nhận từ Authenticator App → click Setting để xác thực |

---

#### Case b_1: INPUT 2FA CODE (áp dụng cho cả Via Mail & Via Authenticator)

| No. | Item (EN) | Component Type | Behavior |
|---|---|---|---|
| ① | 2FA Code | TEXTBOX | Nhập 2FA code nhận qua App/Mail → authen. Sau khi authen thành công → vào màn hình chính |
| ② | Back to Login | BUTTON | Click → hủy authen, quay lại màn hình (1) Login |

---

## (2) Forgot Password

**Screen Flow:** SOO/39/FK_Console_Login

| No. | Item (JP) | Item (EN) | Component Type | Required | Format | Behavior |
|---|---|---|---|---|---|---|
| ① | メールアドレス | Mail Address | TEXTBOX | ◯ | `<username>@<domain>` | Nhập mail đã đăng ký để nhận URL reset password. Validate format mail |
| ② | 送信 | Send | BUTTON | — | — | Click → gửi URL reset password đến mail nhập ở ①. Từ mail, click URL → di chuyển đến màn hình Reset Password (④⑤⑥⑦) |
| ③ | ログインに戻る | Back to Login | BUTTON | — | — | Click → quay lại màn hình (1) Login |
| ④ | PIN | PIN | TEXTBOX | ◯ | — | Nhập mã PIN đính kèm trong mail. Nếu nhập sai → không hiển thị message validate (silent) |
| ⑤ | 新しいパスワード | New Password | TEXTBOX | ◯ | 8 ký tự trở lên, bao gồm: số, chữ hoa, chữ thường, ký tự đặc biệt (`#` v.v.) | Nhập password mới |
| ⑥ | 更新 | Update | BUTTON | — | — | Click → update password mới và quay lại màn hình (1) Login |
| ⑦ | ログインに戻る | Back to Login | BUTTON | — | — | Click → hủy, quay lại màn hình (1) Login |

---

## (3) Site Menu

| No. | Item (EN) | Behavior |
|---|---|---|
| 1.0 | Site Menu | Mỗi Tenant có danh sách menu khác nhau. Xem chi tiết trong spec từng Tenant |

---

## (4) Header

| No. | Item (JP) | Item (EN) | Component Type | Behavior |
|---|---|---|---|---|
| 1.0 | Console Name | Console Name | LABEL | Tên Console theo Tenant (SOO / 39 Drug / FK Drug...) |
| 2.0 | 権限 | Role | LABEL | Hiển thị Role của User đang login:<br>- ルート管理者 \| Root Admin<br>- 管理者 \| Admin<br>- 店舗担当 \| Store Staff |
| 3.0 | 通知 | Notice | ICON | Hiển thị thông báo hệ thống gửi đến Admin. Chỉ hiển thị với role Admin, không hiển thị với Root Admin |
| 4.0 | User Account | User Account | BUTTON | Hiển thị Account_ID (Login_ID). Khi click → 2 option:<br>① User Setting → màn hình (6) User Setting<br>② Logout → màn hình (7) Logout |

---

## (5) Footer

| No. | Item (EN) | Behavior |
|---|---|---|
| 1.0 | Footer / Pagination | Phân chia danh sách dữ liệu thành nhiều trang để tối ưu hiển thị và hiệu năng |

---

## (6) User Setting

| No. | Item (JP) | Item (EN) | Component Type | Behavior |
|---|---|---|---|---|
| 1.0 | ユーザー設定 | User Setting Screen | LABEL | Tên màn hình |
| 2.0 | ユーザーID | User ID | LABEL | Hiển thị ID của User (auto-gen sau khi tạo account, không cho edit) |
| 3.0 | 権限 | Permission / Role | LABEL | Hiển thị role name:<br>- SOO Admin: ROOT<br>- Tenant Admin: ADMIN |
| 4.0 ① | Current Login ID | Current Login ID | LABEL (disabled) | Hiển thị Login ID hiện tại ở trạng thái view only. Muốn edit → nhập ở ② |
| 4.0 ② | New Login ID | New Login ID | TEXTBOX | Nhập Login ID mới |
| 4.0 ③ | Update | Update | BUTTON | Update Login ID mới. Button chỉ enable khi có data ở ② |
| 5.0 ① | Current Mail | Current Mail | LABEL | Hiển thị mail hiện tại, không cho edit trực tiếp |
| 5.0 ② | Send PIN | Send PIN | BUTTON | Gửi mã PIN đến current mail (theo template `TEMPLATE MAIL_2_CHANGE_MAIL`) |
| 5.0 ③ | New Mail | New Mail | TEXTBOX | Nhập mail mới |
| 5.0 ④ | PIN Code | PIN Code | TEXTBOX | Nhập PIN nhận được từ current mail |
| 5.0 ⑤ | Update | Update | BUTTON | Update mail mới. Button chỉ enable khi có đủ data ở ③ & ④ |
| 6.0 ① | Current Pass | Current Pass | LABEL (disabled) | View only. Muốn đổi → dùng ②③④ |
| 6.0 ② | Input Current Pass | Input Current Pass | TEXTBOX | Nhập lại password hiện tại. Sai → hiển thị message: `MG-25 現在のパスワードが間違っています` |
| 6.0 ③ | Input New Pass | Input New Pass | TEXTBOX | Nhập password mới (validate giống Tenant Admin) |
| 6.0 ④ | Change Pass | Change Pass | BUTTON | Sau khi đủ điều kiện → enable. Click → update password mới |
| 7.0 ① | None (2FA off) | None | RADIO | Default. 2FA = không bật |
| 7.0 ② | 2FA via Mail | 2FA via Mail | RADIO | Sau khi chọn & Save → login lần sau sẽ yêu cầu nhập authen code qua mail |
| 7.0 ③ | 2FA via OTP | 2FA via OTP | RADIO | Sau khi chọn & Save → login lần sau sẽ yêu cầu nhập OTP qua Authenticator App |

---

## (7) Logout

| Item (JP) | Item (EN) | Component Type | Behavior |
|---|---|---|---|
| ログアウト | Log Out | BUTTON | Click → thực hiện logout và quay về màn hình (1) Login |

---

## (8) Favicon & Tab Name

| No. | Item (EN) | Behavior |
|---|---|---|
| 1.0 | Favicon | Lấy theo design logo của Tenant tương ứng |
| 2.0 | Tab Name | Format: `"Tenant Name" + "アプリコンソール"`<br>① SOO: `SOOアプリコンソール`<br>② 39 Drug: `サンキュードラッグアプリコンソール`<br>③ FK Drug: `FKアプリコンソール` |

---

## Tenant-specific Notes

| Tenant | Console Name | Login Screen Logo |
|---|---|---|
| SOO | アプリコンソール（SOO） | Driver link |
| 39 Drug | アプリコンソール（サンキュードラッグ） | Figma |
| FK Drug | アプリコンソール（エフケイ） | Figma |
| Hashi Drug | アプリコンソール（ハシドラッグ） | Figma |

---

## Related Screens

- `SOO-LG01` — Login Screen (SOO Admin)
- `SOO-US01` — User Setting (SOO Admin)
- `TC-US01` — Tenant User (Tenant Console)
- **Forgot Password flow** → xem mục (2) trong tài liệu này
