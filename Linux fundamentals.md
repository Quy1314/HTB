# Linux fundamentals
## Introdution
### **Linux structure**
#### Philosophy


| Principle                                                   | Description                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------------- |
| Everything is a file                                        | Tất cả các thao tác trong linux đều là các thao tác với tệp.                            |
| Small, single-purpose programs                              | Linux cung cấp các công cụ với để giải quyết các vấn đề cần thiết một cách hiệu quả và linh hoạt. |
| Ability to chain programs together to perform complex tasks | Các nhiệm vụ phức tạp có thể giải quyết bằng nhiều các bước nhỏ được kết nối với nhau bằng pipelines và các cơ chế chuyển hướng.                                                                  |
| Avoid captive user interfaces                               | Linux làm việc trực tiếp với terminal qua đó cung cấp khả năng làm việc một cách chi tiết và hiệu quả hơn.                                                                    |
| Configuration data stored in a text file                                                            | Các thông tin cấu hình trong linux được lưu trữ dưới dạng văn bản, dễ đọc, dễ chỉnh sửa.                                                                    |

![image](https://hackmd.io/_uploads/S1xsErxD1l.png)

## The shell
### **Exercise**
#### [System Information](https://academy.hackthebox.com/module/18/section/70)
**Q1.Find out the machine hardware name and submit it as the answer.**
- Có thể tìm kiếm "the machine hardware name" bằng cách sử dụng **uname** .
```linux=
us-academy-5]─[10.10.14.171]─[htb-ac-1673302@htb-03ogqexwop]─[~]
└──╼ [★]$ uname -m
x86_64
```
```Answer: x86_64```
**Q2.What is the path to htb-student's home directory?.**
Đia chỉ dẫn đến **htb-student's home directory**
Yêu cầu đường dẫn đến một user nên nó có thể bắt đầu từ **/home**.
```Answer: /home/htb-student```
**Q3.What is the path to the htb-student's mail?.**
Thông tin về email trong linux thường được lưu trữ trong tệp **/var**
```Answer:/var/mail/htb-student```
**Q4.Which shell is specified for the htb-student user?.**
Có thể tìm kiếm shell dành cho **htb-student** bằng lệnh **grep** hoặc bằng cách gọi biến **$SHELL**.
```linux=
htb-studentÉnixfund:ü$ echo $SHELL
/bin/bash
```
```linux=
htb-studentÉnixfund:ü$ cat /etc/passwd | grep htb-student
htb-student:x:1002:1002::/home/htb-student:/bin/bash
```
```Answer: /bin/bash```
**Q5. Which kernel version is installed on the system? (Format: 1.22.3).**
Có thể sử dụng lệnh **uname** để kiểm tra phiên bản của kernel.
```linux=
htb-studentÉnixfund:ü$ uname -r
4.15.0-123-generic
```
```Answer: 4.15.0```
**Q6. What is the name of the network interface that MTU is set to 1500?.**
Để có thể kiểm tra thông tin về MTU(Maximum Transmission Unit) ta có thể sử dụng lệnh **ifconfig**.
```linux=
htb-studentÉnixfund:ü$ ifconfig
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.179.210  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 dead:beef::250:56ff:feb0:9b1c  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::250:56ff:feb0:9b1c  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b0:9b:1c  txqueuelen 1000  (Ethernet)
        RX packets 29807  bytes 2303042 (2.3 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3164  bytes 1385090 (1.3 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 5002  bytes 393433 (393.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5002  bytes 393433 (393.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
Từ kết quả trả về ta thấy giao diện mang với MTU=1500 tên là **ens192**.
```Answer: ens192```
## Workflow
### [Navigation](https://academy.hackthebox.com/module/18/section/75)
**Q1. What is the name of the hidden "history" file in the htb-user's home directory?.**
Vì đây là một file ẩn nên cần thêm một điều kiện **-a** khi sử dụng **ls**.
```linux=
htb-studentÉnixfund:ü$ ls -a
.  ..  .bash_history  .bash_logout  .bashrc  .cache  .gnupg  .profile
```
có thể thấy tên của file history bị ẩn đi là **.bash_history**.
```Answer: .bash_history```
**Q2. What is the index number of the "sudoers" file in the "/etc" directory?.**
Sau khi nghiên cứu về các điều kiện kèm theo của **ls** bằng **ls --help**, ta biết có thể tìm index number của file "sudoers" bằng lệnh **```ls -i```**.
```linux=
htb-studentÉnixfund:ü$ ls -i /etc/sudoers
147627 /etc/sudoers
```
Index number ta tìm được à **147627**.
```Answer: 147627```
### [Working with Files and Directories](https://academy.hackthebox.com/module/18/section/78)
**Q1. What is the name of the last modified file in the "/var/backups" directory?.**
Để tìm file được chỉnh sửa gần nhất, ta có thể tìm kiếm với điều kiện "-t" trong đó file được chỉnh sửa gần nhất sẽ hiển thị đầu tiên. 
```linux=
htb-studentÉnixfund:ü$ ls -lt /var/backups/
total 2160
-rw-r--r-- 1 root root    41872 Nov 12  2020 apt.extended_states.0
-rw-r--r-- 1 root root     4437 Nov 12  2020 apt.extended_states.1.gz
-rw-r--r-- 1 root root   742750 Nov 11  2020 dpkg.status.0
-rw-r--r-- 1 root root   206270 Nov 11  2020 dpkg.status.1.gz
...
```
Ta thấy file đầu tiên là "apt.extended_states.0"
```Answer: apt.extended_states.0```
**Q2. What is the inode number of the "shadow.bak" file in the "/var/backups" directory?**
Ta có thể sử dụng **```ls -i```** như trước đó.
```linux=
htb-student@nixfund:~$ ls -i /var/backups/shadow.bak
265293 /var/backups/shadow.bak
```
Có thế thấy index number là ```265293```
```Answer: 265293```
### [Editing Files](https://academy.hackthebox.com/module/18/section/93)
### [Find Files and Directories](https://academy.hackthebox.com/module/18/section/81)
**Q1.  What is the name of the config file that has been created after 2020-03-03 and is smaller than 28k but larger than 25k?.**
Có thể sử dụng các **optional** của lệnh **find** để thực hiện tìm kiếm
```linux=
htb-student@nixfund:~$ find / -type f -name *.conf -size +25k -size -28k -newermt 2020-03-03  -exec ls -la {} \; 2>/dev/
null
-rw-r--r-- 1 root root 27422 Jun 12  2020 /usr/share/drirc.d/00-mesa-defaults.conf
```

| Command             | Meaning                                                                |
| ------------------- | ---------------------------------------------------------------------- |
| /                   | Tìm kiếm từ đường dẫn hiện tại                                         |
| -type f             | Chỉ trả về file                                                        |
| -name *.conf        | Trả về file có đuôi là .conf                                           |
| -size +25k          | Trả về file có kích thước lớn hơn 25k                                  |
| -28k                | Trả về file có kích thước nhỏ hơn 28k                                  |
| -newermt 2020-03-03 | Trả về file được tạo ra hoặc chỉnh sửa sau ngày 03-03-2020             |
| -exec ls -la {} \   | Thực thi lệnh thể hiện thông tin của các tệp thõa mãn sau khi tìm kiếm |

```Answer: 00-mesa-defaults.conf```
**Q2.How many files exist on the system that have the ".bak" extension?.**
```linux=
htb-student@nixfund:~$ find / -type f -name *.bak 2>/dev/null
/var/backups/shadow.bak
/var/backups/gshadow.bak
/var/backups/group.bak
/var/backups/passwd.bak
```
Dựa vào kết quả trả về ta thấy có 4 file với đuôi **.bak**
```Answer: 4```
**Q3. Submit the full path of the "xxd" binary.**
Tìm kiếm đường dẫn đầy đủ đến "xxd" bằng lệnh **which**
```linux=
htb-student@nixfund:~$ which xxd
/usr/bin/xxd
```
```Answer: /usr/bin/xxd```
### [File Descriptors and Redirections](https://academy.hackthebox.com/module/18/section/79)
**Q1.How many files exist on the system that have the ".log" file extension?.**
Có thể sử dụng công cụ **wc** để dếm số dòng xuất hiện sau khi tìm kiếm.
```linux=
htb-student@nixfund:~$ find / -type f -name *.log 2>/dev/null
/var/log/installer/curtin-install.log
/var/log/installer/block/discover.log
/var/log/installer/subiquity-debug.log
/var/log/mail.log
/var/log/proftpd/controls.log
/var/log/proftpd/proftpd.log
/var/log/vmware-vmsvc.log
/var/log/fontconfig.log
/var/log/bootstrap.log
/var/log/kern.log
/var/log/dpkg.log
/var/log/vmware-vmsvc-root.3.log
/var/log/vmware-vmsvc-root.log
/var/log/alternatives.log
/var/log/vmware-network.log
/var/log/vmware-network.1.log
/var/log/vmware-vmsvc-root.2.log
/var/log/vmware-vmtoolsd-root.log
/var/log/apt/history.log
/var/log/apt/term.log
/var/log/cloud-init.log
/var/log/landscape/sysinfo.log
/var/log/cloud-init-output.log
/var/log/auth.log
/var/log/vmware-vmsvc-root.1.log
/var/log/vmware-network.2.log
/run/cloud-init/ds-identify.log
/run/cloud-init/cloud-init-generator.log
/run/initramfs/overlayroot.log
/usr/share/grc/conf.log
/snap/powershell/137/opt/powershell/Modules/PowerShellGet/GovCompDisc_Log_20200422202439.log
/snap/powershell/137/opt/powershell/Modules/PowerShellGet/GovCompDisc_Log_20200422202549.log
```
Ta có thể thấy nếu không sử dụng **wc** thì sẽ in ra từng đường dẫn có chứa file có đuôi là **.log** còn khi sử dụng **wc** thì chỉ in ra số dòng xuất hiện. 
```linux=
htb-student@nixfund:~$ find / -type f -name *.log 2>/dev/null | wc -l
32
```
```Answer: 32```
**Q2.How many total packages are installed on the target system?.**
Sau khi tìm hiểu thì có một công cụ quản lí các gói đã tải xuống tên là: **dpkg(Debian Package)**. 
```linux=
htb-student@nixfund:~$ dpkg -l | grep -c "^ii"
737
```


| Command       | Meaning                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------- |
| dpkg -l       | Hiển thị các gói đã được cài đặt.                                                                   |
| grep -c "^ii" | Chỉ hiện thị các gói có kí hiệu là "ii" ở đầu dòng trong đó ii thể hiện trạng thái đã được cài đặt. Sau đó đếm số dòng xuất hiện bằng optional "-c" |

```Answer: 737```
### [Filter Contents](https://academy.hackthebox.com/module/18/section/80)
**Q1. How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)**
Theo yêu cầu của đề bài là hãy chỉ ra số lượng các thiết bị đang trong trạng thái **LISTEN**, chỉ thuộc **IPv4** và không phải **localhost**, từ đó ta có thể sử dụng netstat (một công cụ quản lí các giao diện mạng) kết hợp với các **Optional** để tìm kiếm.
```linux=
htb-student@nixfund:~$ netstat -ln4 | grep -v 127 | grep LISTEN | wc -l
7
```


| Command      | Meaning                                                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------------------------------------- |
| netstat -ln4 | Chỉ hiện thị các giao thức mạng IPv4, các địa chỉ ip và cổng được hiện thị dưới dạng số và đang trong trạng thái "listen". |
| grep -v 127  | "-v"(inverted match) là một optional trong grep trong đó "-v 127": chỉ hiển thị các địa chỉ khác **localhost**.           |
| grep LISTEN             | Chỉ hiện thị các dòng có các kí tự trùng khớp với yêu cầu, trong trường hợp này là "LISTEN".                                                                                                                          |

```Answer: 7```
**Q2.Determine what user the ProFTPd server is running under. Submit the username as the answer.**
Đề yêu cầu xác định tên người dùng đang kết nối vào **ProFTPd server** , sau khi tìm hiểu thì ProFTPd là một công cụ mã nguồn mở cung cấp dịch vụ giao thức FTP(File Transfer Protocol). Trong linux, có thể sử dụng công cụ **ps**(Process status) để kiểm tra các tiến trình đang chạy.
```linux=
proftpd   1901  0.0  0.1 126444  3612 ?        Ss   18:37   0:00 proftpd: (accepting connections)
htb-stu+  6401  0.0  0.0  13144  1060 pts/0    S+   20:01   0:00 grep --color=auto proftpd
```
Ta có thể thấy tên của user đang kết nối vào **ProFTPd sever** là **proftpd**
```Answer: proftpd```
### [Regular Expressions](https://academy.hackthebox.com/module/18/section/2092)
### [Permission Management](https://academy.hackthebox.com/module/18/section/83)













