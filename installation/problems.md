1. Could not resolve host: mirrorlist.centos.org

Mô tả: 
- Khi yum update hoặc install  trả về thông báo lỗi 'Could not resolve host: mirrorlist.centos.org'
- Kiểm tra nội dung trong file /etc/resolv.conf trống
- Kiểm tra trạng thái kết nối của ethernet bằng lệnh "nmcli d", thấy thông báo disconnected

Giải quyết:
1. Thêm vào file /etc/resolv.conf dòng sau vaf lưu lại

nameserver 8.8.8.8
nameserver 8.8.4.4

2. Sửa file /etc/sysconfig/network-scripts/ifcfg-eth0

ONBOOT=yes

3. restart network service

/etc/init.d/network restart

4. Kiểm tra lại với lệnh "nmcli d"
