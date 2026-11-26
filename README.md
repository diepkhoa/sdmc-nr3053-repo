# ImmortalWrt cho SDMC NR3053 / Viettel NR3053

Firmware **ImmortalWrt** (nhánh ổn định `25.12.x`) dành cho router **SDMC NR3053 / Viettel NR3053**.

> ⭐ **Theo dõi repository để cập nhật bản build mới!**
> Hãy bấm **Star** và **Watch** (chọn *Custom → Releases*) ở góc trên để nhận thông báo tự động mỗi khi có bản phát hành firmware mới nhất.

---

## Điểm nổi bật

- **Gọn nhẹ, không cá nhân hóa**: Giữ nguyên bản trải nghiệm ImmortalWrt thuần khiết, không cài sẵn phần mềm bên thứ ba hay giao diện thừa — bạn toàn quyền tự cài những gì mình cần.
- **Cài gói thoải mái, không lo lỗi kernel**: Mỗi bản build đều đi kèm kho gói (`kmod` & packages) đồng bộ riêng theo đúng phiên bản firmware. Bạn có thể cài thêm mọi ứng dụng mở rộng (AdGuard Home, WireGuard, SQM, driver USB...) trực tiếp trên giao diện web LuCI hoặc qua lệnh `apk` mà không gặp lỗi xung đột.
- **Phần cứng hoạt động chuẩn xác**:
  - Nhận đúng **1 cổng WAN + 3 cổng LAN** vật lý trên giao diện quản lý.
  - Wi-Fi 6 băng tần kép (2.4GHz & 5GHz) ổn định, giữ nguyên địa chỉ MAC và thông số gốc của thiết bị.
- **Tự động cập nhật hàng tuần**: Hệ thống tự động kiểm tra và biên dịch khi ImmortalWrt phát hành bản vá mới trên nhánh `25.12.x`.

---

## Hướng dẫn cài đặt & nâng cấp

### Cách 1: Cài lần đầu qua U-Boot Web Recovery
1. Rút nguồn router, dùng que chọc giữ nút **Reset** rồi cắm lại nguồn trong khoảng **5–8 giây** cho đến khi đèn nhấp nháy thì thả tay.
2. Nối dây mạng từ máy tính vào cổng LAN của router và đặt IP tĩnh:
   - IP: `192.168.1.2` | Subnet Mask: `255.255.255.0` | Gateway: `192.168.1.1`
3. Mở trình duyệt truy cập `http://192.168.1.1`, chọn file `*factory.bin` tải từ mục **Releases** và nhấn **Upload**.
4. Chờ 1–2 phút để router nạp firmware và tự khởi động lại.

### Cách 2: Nâng cấp trực tiếp từ LuCI
1. Truy cập `http://192.168.1.1` → **System → Backup / Flash Firmware**.
2. Tại mục **Flash new firmware image**, chọn file `*sysupgrade.bin` mới nhất và tiến hành nâng cấp.

---

## Lưu ý an toàn
- Luôn sao lưu cấu hình và giữ nguyên phân vùng `Factory` gốc của router (chứa địa chỉ MAC và thông số sóng Wi-Fi riêng của từng máy).
- Nếu bạn gặp lỗi hoặc muốn góp ý, hãy mở **Issue** trên repo này nhé!

---

*Kiểm tra cập nhật tự động gần nhất: 2026-11-27 04:33:54 (UTC+7) — ImmortalWrt 25.12.2*
