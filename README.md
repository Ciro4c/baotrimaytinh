# rescue-usb

Web chọn ISO -> tải bộ cài tự động -> chạy -> USB boot có menu: Cứu hộ / Cài Windows (bản quyền) / Cài Linux (miễn phí).

## Cách hoạt động
- Bootloader: [Ventoy](https://www.ventoy.net) (UEFI + BIOS). Menu dạng cây, 3 thư mục: `01-Cuu-ho`, `02-Cai-Windows`, `03-Cai-Linux`.
- `site/index.html` sinh ra `rescue-setup.ps1` (Windows) hoặc `rescue-usb.sh` (Linux) đã nhúng danh sách ISO đã chọn.
- Windows ISO tải chính thức qua [Fido](https://github.com/pbatard/Fido) (Microsoft), cần key bản quyền để kích hoạt.
- ISO riêng: thả vào `isos/<thu-muc>/` (script Linux) hoặc chép tay vào USB.
- **Windows: tải về là chạy, không cần PowerShell.** Trang web đóng gói script PowerShell thành 1 file `rescue-setup.bat` duy nhất — người dùng chỉ cần bấm đúp, Windows tự hỏi quyền Admin (UAC) rồi chạy thẳng, không phải mở PowerShell hay gõ lệnh gì. File `.bat` tự chứa toàn bộ nội dung script (mã hóa base64, bọc định dạng PEM ở cuối file), tự tách và giải mã bằng `certutil -decode` (có sẵn trên mọi Windows từ bản 7 trở lên) ra một file `.ps1` tạm rồi chạy, xong thì tự xóa file tạm.

## Tính năng web
- **Gói đề xuất (1 click):** Chỉ cứu hộ / Cứu hộ + Windows / Cứu hộ + Linux / Đầy đủ. Bấm là tự chọn mục và tải ngay bộ cài khớp hệ điều hành máy bạn đang dùng.
- **Tìm kiếm + Bỏ chọn tất cả:** lọc theo tên/công dụng/ứng dụng khi danh mục dài, và nút xóa lựa chọn để chọn lại từ đầu.
- **Hướng dẫn boot:** phần "Vì sao cắm USB vào máy cứ vào thẳng Windows" giải thích thứ tự boot BIOS/UEFI và bảng phím boot-menu theo từng hãng máy.
- **Công cụ cứu hộ sâu hơn:** Kaspersky Rescue Disk (diệt virus khi Windows không vào được), Super Grub2 Disk (sửa mất bootloader dual-boot), ShredOS (xóa trắng ổ đĩa), Rescuezilla (sao lưu/phục hồi dễ dùng hơn Clonezilla), reset mật khẩu Windows local (chntpw — chỉ dùng trên máy được phép truy cập).
- **Driver sau khi cài/cứu xong:** Snappy Driver Installer Origin (nhóm "Tiện ích") — chạy trong Windows sau khi cài mới hoặc sau cứu hộ để tự quét và cài driver mạng, âm thanh, đồ họa còn thiếu. Không nằm trong menu boot của Ventoy (không phải ISO), chỉ được chép vào thư mục `04-Tien-ich` để mở bằng File Explorer sau khi vào được Windows.
- **Dành dung lượng làm ổ lưu trữ:** chọn số GB cuối USB để tạo một phân vùng exFAT riêng (nhãn `LUUTRU`), dùng như USB thường, không bị Ventoy ghi đè khi cập nhật ISO sau này. Trên Linux dùng `Ventoy2Disk.sh -r` rồi tự tạo và định dạng phân vùng; trên Windows cần nhập đúng số MB vào ô "Reserve Space" trong cửa sổ Ventoy trước khi bấm Install, sau đó script dùng `diskpart` tạo phần còn lại.

## Dùng trang web
`site/index.html` là file độc lập, đã nhúng sẵn nội dung hai bộ cài — **mở trực tiếp bằng cách bấm đúp (file://) cũng chạy được**, không bắt buộc phải có server hay internet (ngoại trừ lúc bộ cài tự tải ISO thì cần mạng).
Muốn chia sẻ qua link web thì đưa `site/` lên GitHub Pages, nginx, hay Caddy đều được — không có yêu cầu gì đặc biệt.
Thư mục `site/tools/` giữ bản gốc của hai script để tiện sửa; sau khi sửa, chạy lại bước nhúng (xem `scripts/embed.py` nếu có, hoặc nhúng tay bằng JSON.stringify) rồi mới đưa `index.html` lên.
Sửa `CATALOG` trong `index.html`: phiên bản, link, SHA256.
