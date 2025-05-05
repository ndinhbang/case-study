### Tìm tới thư mục chứa vhd

https://learn.microsoft.com/en-us/windows/wsl/disk-space#how-to-locate-the-vhdx-file-and-disk-path-for-your-linux-distribution

thường có dạng: 

`%LOCALAPPDATA%\Packages\<PackageFamilyName>\LocalState\<disk>.vhdx`

### Gỡ bỏ nén và mã hóa trong Windows Explorer
Nhấn chuột phải vào file `.vhd` hoặc `.vhdx` → chọn `Properties` (Thuộc tính)

Nhấn nút `Advanced` (Nâng cao)

Bỏ chọn cả hai mục sau:

`Compress contents to save disk space` (Nén nội dung để tiết kiệm dung lượng)

`Encrypt contents to secure data` (Mã hóa nội dung để bảo mật dữ liệu)

### Tắt sparseVhd 

https://learn.microsoft.com/windows/wsl/wsl-config

C:\Users\[user]\.wslconfig

```
[experimental]
sparseVhd=false
```

### Kiểm tra thuộc tính file
```
fsutil sparse queryflag "D:\VMs\disk.vhdx"
```
### Gỡ bỏ thuộc tính sparse (nếu có)
```
fsutil sparse setflag "D:\VMs\disk.vhdx" 0
```

### Gỡ bỏ thuộc tính nén (nếu có)
```
compact /u "D:\VMs\disk.vhdx"
```

### Sau đó làm theo hướng dẫn

https://learn.microsoft.com/en-us/windows/wsl/disk-space#manual-expansion

### optimize VHD

https://github.com/microsoft/WSL/issues/4699#issuecomment-627133168
