# Firmware ImmortalWrt 25.12.1 cho SDMC NR3053 (Bản 256MB ROM)

Đây là kho lưu trữ Firmware và OPKG Repository cá nhân dành riêng cho bộ định tuyến **SDMC NR3053** (Sử dụng chip MediaTek MT7981, RAM 512MB, ROM 256MB NAND). 

Bản ROM này đã được tinh chỉnh phần cứng, fix lỗi treo phân vùng `fit0`, nhận diện đủ 256MB ROM và đi kèm với kho ứng dụng (Kmods) khớp mã Hash 100%.

---

## ⚠️ CẢNH BÁO MIỄN TRỪ TRÁCH NHIỆM (DISCLAIMER) ⚠️
- Hướng dẫn dưới đây **CHỈ DÀNH CHO CÁC ROUTER ĐÃ ĐƯỢC UNLOCK VÀ ĐANG CHẠY OPENWRT / CÓ SẴN U-BOOT**.
- Thao tác ghi đè phân vùng U-Boot (BL2 và FIP) là thao tác **cực kỳ nguy hiểm**. Nếu làm sai (ghi nhầm phân vùng, mất điện giữa chừng), router của bạn sẽ biến thành cục gạch (Hard Brick) và phải dùng mỏ hàn + kẹp nạp CH341A để cứu.
- **Tôi không chịu bất kỳ trách nhiệm nào** đối với các hỏng hóc, mất mát dữ liệu hoặc router bị brick trong quá trình bạn sử dụng các file từ kho lưu trữ này. Hãy tự chịu rủi ro khi thao tác!

---

## 🛠 HƯỚNG DẪN CÀI ĐẶT

### BƯỚC 1: Nạp U-Boot chuẩn của NR3053 (Bắt buộc)
Để bản ROM hoạt động hoàn hảo và tránh lỗi phân vùng, bạn cần nạp đè bộ U-Boot chuẩn của NR3053 (đã được trích xuất trong kho này) đè lên U-Boot hiện tại của bạn.

1. Tải 2 file `mt7981-sdmc-nr3053-bl2.bin` và `mt7981-sdmc-nr3053-fip.bin` từ kho này về máy tính.
2. Dùng phần mềm WinSCP (hoặc MobaXterm), đẩy 2 file vừa tải vào thư mục `/tmp/` trên router.
3. Dùng SSH (PuTTY/MobaXterm) truy cập vào router.
4. Kiểm tra kỹ tên phân vùng của bạn bằng lệnh:
   ```bash
   cat /proc/mtd
   ```
   *(Hãy chắc chắn rằng `bl2` và `fip` có tồn tại trong danh sách).*
5. Thực hiện nạp đè bằng 2 lệnh sau (TUYỆT ĐỐI KHÔNG GÕ NHẦM):
   ```bash
   mtd write /tmp/mt7981-sdmc-nr3053-bl2.bin bl2
   mtd write /tmp/mt7981-sdmc-nr3053-fip.bin fip
   ```

### BƯỚC 2: Nạp Firmware ImmortalWrt 25.12.1
Sau khi nạp xong U-Boot, bạn có 2 cách để nạp Firmware:

**Cách 1: Nạp trực tiếp qua SSH (Khuyên dùng)**
Vẫn ở màn hình SSH trên, tải file Sysupgrade (từ mục Releases) vào `/tmp` và chạy lệnh:
```bash
sysupgrade -n /tmp/immortalwrt-mediatek-filogic-sdmc_nr3053-squashfs-sysupgrade.bin
```

**Cách 2: Nạp qua giao diện cứu hộ (TFTP / UART)**
Nếu router không khởi động, hãy dùng cáp USB-UART truy cập vào menu U-Boot:
1. Dùng TFTP nạp file `immortalwrt-mediatek-filogic-sdmc_nr3053-initramfs-recovery.itb` để boot router lên RAM.
2. Đăng nhập vào trình duyệt web (192.168.1.1).
3. Vào mục **System -> Backup / Flash Firmware** -> Nạp file `immortalwrt-mediatek-filogic-sdmc_nr3053-squashfs-sysupgrade.bin`.
4. **Bỏ tích chọn "Keep settings"** và tiến hành Flash.

---

## 📦 KHO ỨNG DỤNG TÍCH HỢP (PACKAGE REPO)
Bản ROM trong mục Releases đã được nhúng sẵn cấu hình tự động trỏ về kho GitHub Pages này. Khi bạn truy cập vào LuCI và bấm `Update lists`, router sẽ tự động tải các gói `kmod` tương thích 100% với Kernel của ROM mà không gặp lỗi bảo mật.

Chúc các bạn thành công!
