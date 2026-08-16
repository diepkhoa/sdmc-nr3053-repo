

# ImmortalWrt cho SDMC NR3053 (MediaTek MT7981 / Filogic 820)
**OS:** ImmortalWrt 25.12.1 Stable | Kernel 6.12  
**Board:** SDMC NR3053 (Router Wi-Fi 6 Mesh Viettel / OEM)

---

## 1. Gioi thieu ve 2 chuan Firmware trong Repository

Repository nay cung cap 2 loai firmware khac nhau nham dam bao tinh tuong thich voi tung loai Bootloader (U-Boot) ma router cua ban dang su dung:

### A. Chuan Full-UBI / FIT Image (Khuyen dung - Hien dai nhat)
* **Dac diem:** Su dung U-Boot Mod (co giao dien cuu ho Web Recovery tai 192.168.1.1). Kernel va Device Tree duoc dong goi chung vao Volume `fit` trong phan vung UBI.
* **Uu diem:** Khong bi gioi han dung luong Kernel, ho tro quan ly Bad Block tren Flash NAND tot hon, co giao dien Web de cuu ho neu gap su co.
* **Dinh dang file:** `*sysupgrade.bin` hoac `*factory.bin` (danh cho Full-UBI).

### B. Chuan Legacy (Tach biet Kernel va RootFS rieng le)
* **Dac diem:** Danh cho cac may van dang giu U-Boot goc hoac U-Boot SDK cu khong co Web Recovery. He thong chia tach roi 2 phan vung/volume: `kernel` (khoang 4MB) va `rootfs` (khoang 16MB).
* **Dinh dang file:** Ban build danh rieng cho layout tach phan vung truyen thong.

---

## 2. Huong dan kiem tra chuan U-Boot tren Router qua SSH

Truoc khi nap bat ky ban firmware nao, ban BAT BUOC phai kiem tra xem router hien tai dang chay theo chuan nao de tranh tinh trang bi treo bootloader (Hard/Soft Brick).

Dang nhap vao router qua SSH va thuc hien cac lenh sau:

### Buoc 1: Kiem tra cau truc Volume UBI
Chay lenh:
```bash
ubinfo -a
```

**Doc ket qua:**
* **Truong hop 1 - Router dung chuan Full-UBI:**
  Trong danh sach hien thi, ban se thay Volume ID 0 (hoac mot Volume duy nhat cho he thong) co ten la **`fit`**, di kem voi `rootfs` va `rootfs_data`. Khong co Volume nao ten rieng la `kernel`.
  -> **Ket luan:** Chon tai va nap ban **Firmware Full-UBI**.

* **Truong hop 2 - Router dung chuan Tach biet (Legacy Split):**
  Trong danh sach hien thi, ban se thay 2 Volume rieng biet:
  * Volume co ten la **`kernel`** (Dung luong khoang 4.4 MiB).
  * Volume co ten la **`rootfs`** (Dung luong khoang 16.2 MiB).
  -> **Ket luan:** Chon tai va nap ban **Firmware Legacy**.

---

### Buoc 2: Kiem tra cau lenh Boot cua U-Boot (Tuy chon them)
Chay lenh quet chuoi bien moi truong:
```bash
strings /dev/mtd1 | grep -iE "mtkboardboot|bootmenu_0|bootcmd"
```

* **Neu ket qua tra ve:** `bootmenu_0=Startup system (Default)=mtkboardboot`
  -> Router cua ban da duoc cai U-Boot Mod ho tro Full-UBI va co Web Recovery.
* **Neu ket qua tra ve:** Cac lenh dang `ubi read ... kernel` hoac `nand read ... kernel`
  -> Router cua ban dang dung co che doc Kernel tach roi.

---

## 3. Huong dan lua chon va Nap Firmware

| Tinh trang U-Boot hien tai | Ban Firmware can tai | Cach nap an toan |
| :--- | :--- | :--- |
| U-Boot ho tro Full-UBI (Co Web Recovery) | `Firmware NR3053 Full-UBI (*.bin)` | Nap qua giao dien Web Recovery (192.168.1.1) hoac LuCI Sysupgrade |
| U-Boot Legacy (Doc phan vung `kernel` rieng) | `Firmware NR3053 Legacy (*.bin)` | Nap qua giao dien LuCI cua ban ROM cu hoac SSH `sysupgrade` |

[Luu y quan trong]: Neu ban nap nham ban Firmware Full-UBI vao U-Boot Legacy (hoac nguoc lai), router se khong the tim thay Kernel de khoi dong va se bi dung o man hinh boot.

---

## 4. Kho goi phan mem (Kmod / APK Repository)

Repository nay da tich hop he thong stream package tu dong qua GitHub Pages de phong tranh loi khong khop Kernel Hash (vermagic):

### A. Danh cho Router chay chuan Full-UBI moi:
He thong duoc cau hinh tu dong tro den kho luu tru rieng tai:
```text
https://diepkhoa.github.io/sdmc-nr3053-repo/ubi/targets/mediatek/filogic/packages/packages.adb
```

### B. Danh cho Router chay chuan Legacy cu:
Cac router chay ban firmware cu van tiep tuc su dung kho kmod truyen thong ma khong bi anh huong:
```text
https://diepkhoa.github.io/sdmc-nr3053-repo/targets/mediatek/filogic/packages/packages.adb
```

---

## 5. Huong dan truy cap U-Boot Web Recovery (Doi voi ban Full-UBI)

Khi can cuu ho hoac nang cap firmware moi:
1. Rut nguon dien khoi router.
2. Dung que tam nhan va giu nut Reset, dong thoi cam lai day nguon.
3. Giu nut Reset trong khoang tu 5 den 8 giay cho den khi den bao trang thai nhap nhay thi tha tay.
4. Dat IP tinh tren may tinh:
   * IP Address: `192.168.1.2`
   * Subnet Mask: `255.255.255.0`
   * Default Gateway: `192.168.1.1`
5. Mo trinh duyet web va truy cap dia chi: `http://192.168.1.1` de vao giao dien Recovery Mode WEBUI.

---

## 6. Canh bao an toan
* Khong bao gio ghi de hoac xoa phan vung `Factory` (`mtd2`). Day la noi luu tru dia chi MAC va thong so can chinh song Wi-Fi goc cua thiet bi.
* Luon sao luu toan bo cac phan vung MTD truoc khi thuc hien cac thao tac can thiep vao Bootloader.
