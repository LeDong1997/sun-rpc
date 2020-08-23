# SUN ONC/RPC Framework
Xây dựng chương trình cung cấp lời gọi thủ tục từ xa RPC dựa trên Sun ONC/RPC Framework
#
# 1. Thiết lập môi trường
Xây dựng hệ phân tán trong môi trường ảo hóa với VMware Workstation 15 Pro
## 1.1. Thông số cấu hình phần cứng
Cài đặt 2 máy ảo gồm máy trạm RPC Client chạy **Ubuntu Desktop 18.04 LTS** và máy chủ RPC Server chạy **Ubuntu Server 18.04 LTS**
| Máy  trạm RPC Client ||
|--------------|-------|
| Hệ điều hành| Ubuntu Desktop 18.04 LTS|
| RAM | 2GB. Thiết lâp thêm vùng nhớ swap|
|Processor| 1 core|
|Ổ cứng| 60GB|
|Cấu hình mang|Một card eth0 nhận cấu hình NAT có địa chỉ IP là 192.168.10.20/24 trên VMware Workstation|
| **Máy  chủ RPC Server** ||
| Hệ điều hành| Ubuntu Server 18.04 LTS|
| RAM | 4GB. Thiết lâp thêm vùng nhớ swap|
|Processor| 4 core|
|Ổ cứng| 60GB|
|Cấu hình mang|Một card eth0 nhận cấu hình NAT có địa chỉ IP là 192.168.10.100/24 trên VMware Workstation|

**Chú ý:** Nếu máy triển khai ảo hóa cấu hình yếu, có thể chỉ cần cài đặt một máy ảo Ubuntu Server 18.04 đóng vai trò client/server
## 1.2. Tài khoản truy nhập
Cấu hình dịch vụ cho phép truy nhập từ xa SSH. Cần sử dụng quyền quản trị root để cài đặt một số gói phần mềm bổ trợ

Chi  tiết cài đặt hệ điều hành, cấu hình card mạng cho máy ảo, cấu hình dịch vụ SSH không được trình bày ở mục này.
# 2. Cài đặt gói phần mềm
Cập nhật hệ điều hành lên bản mới nhất

`bkcs@server:~$ sudo apt-get update && sudo apt-get upgrade -y`

Cài đặt chương trình dịch **g++, g++** và **make** để biên dịch mã nguồn C/C++

`bkcs@server:~/rpc$ sudo apt install build-essential -y`

Cài đặt bộ biên dịch mã nguồn **rpcgen**

`bkcs@server:~$ sudo apt-get install libc-dev-bin -y`

Cài đặt dịch vụ **portmapper** trên Linux

`bkcs@server:~$ sudo apt-get install rpcbind -y`

Cài đặt **git** trên Linux

`bkcs@server:~$ sudo apt-get install git -y`

# 3. Thử nghiệm chương trình thực thi lời gọi thủ tục từ xa RPC

## Chương trình tính tổng 2 số nguyên nhập vào thông qua việc thực thi lời  gọi thủ tục  từ xa RPC
Tải mã nguồn chương trình về máy chủ và máy khách

`bkcs@server:~$ git clione https://github.com/LeDong1997/sun-rpc && cd sun-rpc`

`bkcs@client:~$ git clione https://github.com/LeDong1997/sun-rpc && cd sun-rpc`

## Biên dịch chương trình

Chạy lệnh **`make`** để biên dịch lại mã nguồn chương  trình  trên cả máy chủ và máy khách

`bkcs@server:~/sun-rpc$ make`

`bkcs@client:~/sun-rpc$ make`

## Tính tổng 2 số nhập vào  từ dòng lệnh
**Chú ý** phải khởi chạy chương trình phía máy chủ  RPC trước để lắng nghe và chờ các kết nối tới

`bkcs@server:~/sun-rpc$ ./add_server`

Trên máy khách RPC chạy chương trình thực thi lời gọi thủ tục từ xa treen máy chủ RPC

`bkcs@client:~/sun-rpc$ ./add_client 192.168.10.100 3 4`
- Trong đó `192.168.10.100` là địa chỉ IP của máy chủ RPC. Nếu chạy cục bộ thì thay bằng **localhost**
- Giá trị `3` và `4` là 2 số nguyên dùng để tính tổng

Kết quả phía máy chủ sẽ hiển thị thông báo thủ tục đã được gọi từ xa
```
bkcs@server:~/sun-rpc$ ./add_server
Ham "add()" duoc  goi.
Tham so: a = 3, b = 4.
Ket qua: 7.
```

Kết quả phía máy khách sễ hiển thị kết quả của phép tính  tổng

```
bkcs@client:~/sun-rpc$ ./add_client 192.168.10.100 3 4
Ket qua: 7.
```

Dưới đây là chương trình đơn  giản tính tổng 2 số nguyên nhập từ dòng lệnh thông qua việc  thực thi lời gọi thủ tục từ xa RPC./

# TÀI LIỆU THAM KHẢO
- A. Birrell and B.J.Nelson, "Implementing Remote Procedure Calls," *ACM Trans on Computer Systems*, vol. 2, no. 1, pp. 39-59, 1984.
- B. H. Tay and A. L. Ananda, "A survey of remote procedure calls," *ACM SIGOPS Operating Systems Review*, pp. 68-79, 1990.
- O. D. Guide, "Chapter 03, rpcgen Programming Guide," 2010.
- H. Q. Thụy, "Giáo Trình Hệ Điều Hành Phân Tán," 2009.
- T. H. Anh, "Bài giảng Hệ Phân Tán," Đại Học Bách Khoa Hà Nội, 2014.
- M. v. Steen and A. S. Tanenbaum, Distributed Systems, Pearson Education, Inc, 2017.
