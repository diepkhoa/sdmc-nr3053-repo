# ImmortalWrt cho SDMC NR3053 (MediaTek MT7981 / Filogic 820)
**Hệ điều hành:** ImmortalWrt 25.12.1 Stable | Nhân Linux Kernel 6.12.94  
**Kiến trúc bộ nhớ:** Chuẩn Full-UBI FIT Image  
**Thiết bị hỗ trợ:** SDMC NR3053 (Router Wi-Fi 6 Mesh Viettel / OEM)

---

## 1. Giới thiệu tổng quan về Bản Firmware

Đây là bản firmware ImmortalWrt tùy biến dành riêng cho router SDMC NR3053 sử dụng SoC MediaTek Filogic 820 (MT7981B). 

Bản build được đóng gói theo kiến trúc **Full-UBI FIT Image** hiện đại nhất hiện nay trên OpenWrt. Toàn bộ nhân Linux Kernel và cây thiết bị (Device Tree) được gộp chung vào volume `fit`, giúp quản lý bộ nhớ Flash NAND động, tự động xử lý Bad Block và tương thích tuyệt đối với trình nạp U-Boot Web Recovery (192.168.1.1).

Bản build được tùy chỉnh driver đảm bảo tính năng Hardware Flow Offloading (tăng tốc luồng phần cứng với PPE), driver `mt76` được nạp sẵn và hoạt động ổn định. Người dùng có thể thoải mái cài đặt thêm các gói kmod package mà không gặp trở ngại.

---

## 2. Thông số phần cứng và Các tính năng nổi bật

* **Bộ vi xử lý (CPU):** MediaTek MT7981B Dual-Core ARM Cortex-A53 xung nhịp 1.3GHz, xử lý đa nhiệm mạnh mẽ.
* **Bộ nhớ RAM:** Hỗ trợ đầy đủ 512MB RAM DDR3 tốc độ cao.
* **Bộ nhớ Flash:** 256MB SPI-NAND được phân vùng theo chuẩn Full-UBI sạch sẽ.
* **Mạng LAN/WAN:** 
  * 1 cổng WAN 1Gbps + 3 cổng LAN 1Gbps (sử dụng chip Switch MT7531 hỗ trợ SerDes 2.5Gbps).
  * Đã loại bỏ hoàn toàn cổng mạng ảo (internal PHY) để giao diện LuCI chỉ hiển thị đúng 3 cổng LAN vật lý.
* **Mạng Không dây (Wi-Fi):**
  * Wi-Fi 6 AX3000 (Băng tần 2.4GHz 574Mbps + Băng tần 5GHz 2402Mbps hỗ trợ độ rộng kênh 160MHz).
  * Tự động nạp dữ liệu cân chỉnh sóng vô tuyến gốc (RF EEPROM Calibration) và địa chỉ MAC in trên tem từ phân vùng `Factory`.
* **Phím bấm & Đèn báo:**
  * Nút Reset (GPIO 1) và nút Mesh/WPS (GPIO 0) hoạt động chuẩn xác.
  * Đèn hệ thống chuyển trạng thái thông minh: Đỏ khi khởi động/nâng cấp, Xanh khi hệ thống sẵn sàng.
* **Tối ưu hệ thống:**
  * Tối ưu hóa giao diện Web LuCI giúp phản hồi nhanh, giảm độ trễ.
  * Đồng bộ thời gian tự động qua Cloudflare và Google NTP Server.
  * Tích hợp sẵn WireGuard, Tailscale, SQM QoS chống nghẽn mạng.

---

## 3. Cơ chế giải quyết triệt để lỗi Kernel Hash (Vermagic)

### Vấn đề thường gặp trên OpenWrt:
Mỗi lần biên dịch bản firmware mới, mã băm nhân Linux Kernel (vermagic hash) sẽ thay đổi. Nếu người dùng cài thêm các gói mở rộng có chứa module nhân (kmod) từ kho mặc định trên mạng, hệ thống sẽ báo lỗi `kernel vermagic mismatch` và từ chối cài đặt.

### Bản phát hành (Release) của tôi khắc phục hoàn toàn vấn đề này:
Hệ thống CI/CD tự động xuất bản toàn bộ kho package tương thích theo từng bản build. Bạn có thể thoải mái cài đặt mọi gói package và kmod trên bản firmware này mà không bao giờ gặp lỗi xung đột nhân.

---

## 4. Hướng dẫn Nạp Firmware và Cứu hộ

### Cách 1: Nạp qua giao diện U-Boot Web Recovery (Khuyên dùng nhất)
Phương pháp này thực hiện nạp độc lập từ bộ nhớ RAM, giúp định dạng lại phân vùng Flash sạch sẽ 100%:

1. Rút dây nguồn khỏi router.
2. Dùng que tăm nhấn giữ nút **Reset**, đồng thời cắm lại dây nguồn.
3. Giữ nút Reset trong khoảng **5 đến 8 giây** cho đến khi đèn báo trạng thái nhấp nháy thì thả tay.
4. Đặt IP tĩnh trên cổng mạng LAN của máy tính:
   * IP Address: `192.168.1.2`
   * Subnet Mask: `255.255.255.0`
   * Gateway: `192.168.1.1`
5. Mở trình duyệt web truy cập địa chỉ: `http://192.168.1.1` để vào giao diện Recovery Mode WEBUI.
6. Tại mục **Firmware update**, chọn file `*factory.bin` (hoặc `*sysupgrade.bin`) và bấm **Upload**.
7. Chờ router tự động nạp Flash và khởi động lại trong 1-2 phút.

---

### Cách 2: Nâng cấp trực tiếp qua giao diện Web LuCI
Nếu router của bạn đã đang chạy bản ImmortalWrt của repo này và muốn cập nhật bản build mới:

1. Đăng nhập vào giao diện LuCI: `http://192.168.1.1`.
2. Vào mục: **System** -> **Backup / Flash Firmware**.
3. Tại mục **Flash new firmware image**, chọn file `*sysupgrade.bin` và bấm **Upload**.
4. Xác nhận tiếp tục và chờ router khởi động lại.

---

## 5. Cảnh báo an toàn
* **Tuyệt đối không ghi đè phân vùng `Factory` (`mtd2`):** Đây là phân vùng duy nhất chứa địa chỉ MAC vật lý và thông số cân chỉnh công suất sóng Wi-Fi độc quyền của từng chiếc router.
* Luôn lưu trữ một bản sao lưu (backup) của phân vùng `Factory` trên máy tính trước khi thực hiện bất kỳ thao tác can thiệp bootloader nào.
