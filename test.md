# Tổng quan về Linux
## - Xem dung lượng Disk
1. Xem bằng ứng dụng mặc định trong ubuntu là “Disk Usage Analyzer” hoặc “Disks”
![Disk Usage Analyzer](images/disk.png)
1. Xem bằng GUI Tools của bên thứ 3
1. Dùng lệnh df (có thể kèm tham số -h cho các phần có thể đọc được)
1. Hiển thị bao gồm các thông tin chi tiết về dung lượng disk còn trống, dung lượng đang sử dụng, thông tin về disk đang chạy, nhiệt độ,…
## - Xem phân vùng Disk
1. xem bằng GUI tools là Gparted
![Gparted](images/Gparted.png)
1. Dùng lệnh cfdisk hoặc fdisk (chạy ở quyền root)

Trong đó: 
* cfdisk cho phép list tạo xóa và resize phân vùng
* fidsk cho phép list ,tạo và xóa phân vùng (--help với các option khác nhau cho việc tương tác với các phân vùng)
```
$ sudo fdisk --help
```
![fdisk](images/fdisk.png)

## - Xem thông tin phần cứng
_**Toàn bộ thông tin được dùng bằng command line**_
1. lscpu: cung cấp thông tin về cpu và các đơn vị xử lí liên quan
    ![lscpu](images/lscpu.png)
1. lspci: cung cấp thông tin các bus PCI và các linh kiện bên ngoài kết nối với chúng

    ![lspci](images/lspci.png)
1. lshw: cung cấp thông tin chi tiết, ngắn gọn về mọi linh kiện trong máy từ motherboard, cpu, RAM,… (với lệnh `lshw -short` sẽ hiển thị đơn giản hơn)

    ![lshw -short](images/lshw%20-short.png)
1. lsscsi: liệt kê các ổ disk đang sử dụng
1. lsusb: liệt kê các usb controller, hiển thị chi tiết về thông tin chúng kết nối với máy, mặc định sẽ ngắn gọn, để có nhiều thông tin chi tiết hơn dùng `lsusb -v`
1. kiểm tra lượng ram trên máy dùng lệnh `free`, để dễ hiểu hơn sử dụng lệnh `free -m`
* `ls option`: tổng hợp các option phổ biến khi dùng `ls`
![](images/ls%20option.png)
## - Theo dõi chi tiết tiến trình
_**Cần thực hiện trên quyền root**_

* `$ sudo ps -a`: hiển thị mọi tiến trình có trên máy, kèm PID, thời gian thực thiện và lệnh thực hiện 
* `$ sudo ps -sa`: hiển thị ID tiến trình, ID unix của user, trạng trái thực thi, thời gian, lệnh thực hiện
* `$ sudo ps -u`: hiển thị user, ID tiến trình, % hệ thống hoạt động, trạng thái, thời gian và lện thực hiện
## - Liệt kê danh sách file/folder

Dùng lệnh `ls` để liệt kê các file hiện có tại vị trí trỏ hiện tại cùng các option có điều kiện:

`$ ls [options] [file|dir]` để tìm ở 1 file được chỉ định trước

![ls option](images/ls_option.png)

## - Tương tác với file/folder
* Sao chép: lệnh `cp` để sao chép file/ folder mới được chỉ định trước vào vị trí mới 

![copy file](images/copy.png)

syntax: `cp -R <source_folder> <destination_folder>`
* Với trường hợp sao chép vào phân vùng backup hay remotely host server dùng 2 lệnh sau:(yêu cầu thực hiện sudo apt-get install rsync )
```
rsync -ar <source folder/file> <destination_user>@<ipserver>:<destination folder/file/>
``` 
```
scp -r <source_folder> <destination_user>@<destination_host>:<ip server>
```
* Di chuyển: lệnh “mv” để di chuyển file/folder được chỉ định tới vị trí mới

![123](images/move.png)

```
mv [option] <source_file> </destination_folder>
```
Để đổi tên file dùng lệnh: `mv [option] <source> <destination>`

Có thể di chuyển từng file, nhiều file hoặc tất cả file có cùng định dạng 

    -i :để xác nhận ghi đè hay không
    -f :để tắt thông báo xác nhận ghi đè (trường hợp thực hiện nhiều file liên tục)
    -n :để chặn ghi đè file đã tồn tại
    -b :để sao lưu file đã có trước đó
    -v :để hiện thông báo tiến trình di chuyển từng file

* Tìm: dùng lệnh “find” kèm với đường dẫn vào file cần tìm để ra kết quả chính xác nhất bằng đệ quy

```
find /path/ -type f -name file-to-search
```
    /path: nơi bắt đầu tìm
    -type [option]: định dạng file cần tìm 
    (f-regular file, d-directory, b-block, l-symbolic link, c-character device)
    -name: tên file cần tìm 
Có thể kết hợp lệnh `mv` và `rm`(remove) để lọc file tốt hơn

Ngoài ra lệnh `locate` cũng được dùng tương đương lệnh `find`, nhưng thời gian nhanh hơn do dùng cơ sở dữ liệu xây dựng từ trước, trong khi `find` quét hệ thống theo thời gian thực 

![find](images/find.png)
## - Phân quyền cơ bản và phân quyền nâng cao

```
$ sudo ls -l <file>
```
cấu trúc của 1 phân quyền file bao gồm :

![](images/file-permission.jpg)

1. file type: trong đó
    1. "-" regular file
    1. "d" directory
    1. "i" link
1. user: người sở hữu
1. group: nhóm user
1. others: bất kì đối tượng không thuộc nhóm và không phải user sỡ hữu file
- 'r' : read giá trị = 4
- 'w' : write giá trị = 2
- 'x' : execute giá trị = 1
- '-' : deny giá trị = 0

![](images/example.png)

Giá trị quyền lúc này của file task 1.odt
|  file type |   user   |    group  | others |
|------------|----------|-----------|--------|
|     -      |   rw-    |    rw-    |    r-- |
|regular file|  6       |   6       |   4    |

Để thay đổi giá trị phân quyền tiêu chuẩn có 2 cách:
- Chế độ symbolic
- chế độ kí hiệu bát phân (octal nonation)

ví dụ: để thay đổi quyền của file task 1.odt thành tất cả chỉ đọc và chỉ người sở hữu có thể thực thi, sử dụng lệnh (octal nonation)
```
chmod 744 task 1.odt 
```
hoặc `chmod u=rwx,g=r,o=r task 1.odt` (symbolic mode)

![chmod](images/chmod.png)

Để thây đổi phân quyền nâng cao , sử dụng lệnh `chown`, có thể thay được group và owner ban đầu (yêu cầu quyền root)

```
$ sudo chown [new-user] [file]
$ sudo chgrp [new-group] [file]
```
![](images/permission.png)

## - Mount và Umount
Để kiểm tra phân vùng ổ cứng, dùng lệnh `lsblk` để hiển thị toàn bộ phân vùng đang có trên máy

Có 2 cách mount ổ cứng bằng device file hoặc UUID của ổ cứng đó
- Cách 1: device file
```
$ sudo mount [/dev/disk-name] [/mount-file-name]
```
Là hình thức mount tạm thời, sẽ bị mất sau khi reset hoặc shutdown máy
- Cách 2: mount bằng UUID

Để ổ có thể mount tự động sau khi mở máy, sử dụng UUID của ổ đó để mount
sử dụng `blkid` hoặc list toàn bộ UUID phân vùng ổ cứng như sau:
```
lsblk -o NAME,UUID,SIZE
```
Sau khi lấy được UUID ổ cần tìm, cần chỉnh sửa file "/etc/fstab"
thêm lệnh mount vào cuối file hoặc dùng `echo` thay thế
```
UUID =[uuid của ổ cần mount] [/file-to-mount] ext4 default 0 0
```
Sau đó lưu lại chỉnh sửa file và dùng lệnh ` $ sudo mount -a` để hoàn tất
 ## - Các trình text editor 
 Có rất nhiều trình text editor khác nhau trên cả version Ubuntu chính thức và các bản mở rộng, tùy biến khác
 Trong đó có 5 trình text editor phổ biến được sử dụng nhiều nhất:
 1. Vim/Vi
 1. Nano
 1. Gedit
 1. Emacs
 1. Sublime Text

Để hình dung rõ hơn giao diện làm việc của từng text editor, ví dụ có file "test1.txt" ở trong đường dẫn `/Desktop/123` như sau:

![](images/text-content.png)

### Vim/Vi
Sử dụng lệnh `vi [file]` (trong 1 số trường hợp, `vi` là text editor mặc định thay cho vim buộc phải cài đặt và ngược lại)

![Vim](images/text-content2.png)  --------- Giao diện của Vim editor ---------

Vim có 2 mode: 
- Command mode: nội dung trong file được xem là các lệnh cần thực thi, đây cũng chính là mode mặc định khi mở file text bất kì

![](images/bash-file.png)

- Insert mode: cho phép user chỉnh sửa nội dung trong file, để vào mode này nhấn nút __Esc__ để thoát command mode sau đó nhấn nút __"i"__ để chỉnh sửa nội dung

![](images/after-edit.png)

Và để thực thi các lệnh trong file, cần dùng lệnh:
```
$ sudo bash [file]
```
![](images/bash-content.png)

Kết quả khi áp dụng cho file __test1.txt__ thấy được phân quyền owner và group đã thay đổi từ __nam123__ thành __nam__

Ngoài ra còn có rất nhiều command khác phục vụ cho Insert mode
có thể tham khảo tại [Basic Vi Commands](https://www.cs.colostate.edu/helpdocs/vi.html)
### Nano
Nano là một trình chỉnh sửa trực tiếp. Nó được thiết kế cho cả người mới bắt đầu và người dùng nâng cao và có nhiều tính năng tùy biến.
```
nano [file]
```
![Nano](images/nano-file.png)

Mặc định khi dùng `nano` để mở file sẽ ở dạng _modified_
### Gedit
Đây là trình text editor mặc định cho mội trường GNOME desktop

Khi mở file text bất kì, trình editor Gedit sẽ là ưu tiên 

Gedit có đầy đủ các tính năng cơ bản của trình soạn thảo văn bản tiêu chuẩn với giao diện đơn giản hơn

Ngoài ra Gedit có thể  hỗ trợ 1 vài ngôn ngữ lập trình và hầu hết các dạng họ font chữ

Có 2 cách sử dụng Gedit: double click vào file cần mở hoặc dùng lệnh `gedit [file]` mở trực tiếp

![Gedit](images/Gedit.png)
### Sublime Text

Đây là dạng soạn thảo văn bản dựa trên IDE rất phổ biến

Sublime có hỗ trợ nhiều plug-in dành cho lập trình và mark-up

Có 1 số tính năng khác:
- Có sẵn hệ thống bản lệnh đa đạng
- Là API dựa trên Python
- Hỗ trợ edit code song song
- Có 1 số option riêng cho R&D dự án

Tương tự Gedit, sublime có mở từ kho ứng dụng hoặc command `subl` (yêu cầu cài đặt trước)

![Sublime](images/sublime.png)

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -  

sudo apt-get install apt-transport-https  

echo "deb https://download.sublimetext.com/ apt/stable/" |  
 sudo tee /etc/apt/sources.list.d/sublime-text.list  .

apt-get update  

sudo snap install sublime-text --classic
```
### GNU Emacs
 Được xem là trình text editor lâu đời nhất nhưng độ phổ biến vẫn rất cao nhờ tính ổn định cũng như sự tối ưu và đơn giản của nó

 Ngoài ra còn có 1 số tính năng khác:
 - Tích hợp option mail và tin tức
 - Tính năng mở rộng về giao diện debugger
 - Cung cấp và hỗ trợ gói văn bản mở rộng

 Do không được tích hợp sẵn vào OS nên cần cài đặt trước
 ```
 sudo apt update
 sudo apt-get install emacs
 ```
 ![Emacs](images/emacs.png)

 ## - Symbolic Link
Đơn giản đây là 1 đường dẫn local trỏ đến vị trí file/foler được chỉ định

Giúp user dễ dàng thao tác với terminal cũng như truy cập, quản lý file dễ dàng hơn, đặc biệt đây là file độc lập gốc nên dù bị xóa cũng không ảnh hưởng các file khác
```
ln -[option] [target file] [Symbolic filename]
```
|Command Switch         |	Mô tả                                               |
|-----------------------|-------------------------------------------------------|
|–backup[=CONTROL]      | 	backup từng file gốc                                |
|-d, -F, –directory     | 	superuser được cho phép hard link                   |
|-f, –force             | 	file đích bị xóa                                    |
|-I, –interactive       | 	thông báo trước khi xóa file đích                   |
|-L, –logical           | 	chọn file gốc là symbolic links                     |
|-n, –non-dereference   | 	symbolic links tới thư mục được xem như là files    |
|-P, –physical          | 	tạo hard links trực tiếp tới symbolic links         |
|-r, –relative          | 	tạo symbolic links tương đối tới link location      |
|-s, –symbol            |	tạo symbolic links thay vì hard links               |
|-S, –suffix=SUFFIX     |	ghi đè lên backup suffix thông thường               |
|-v, –verbose           |	in ra tên của từng linked file                      |

- Symbolic link cho file

![File](images/symbolic.png)

- Symbolic link cho folder

![Folder](images/symbolic-folder.png)

Giống như việc tạo 1 shortcut, user có thể truy cập ngay file của folder đó ngay bên ngoài mà ko cần phải tìm kiếm dù các file đó nằm gần hay xa gốc tree-folder

- Xóa symbolic link

có thể dùng `unklink` hoặc `rm` với hiệu quả tương đương
```
unlink [symbolic link]
rm [symbolic_link name]
```
## - Hard link

Bên cạnh symbolic link thì hard link(dạng liên kết cấp độ thấp) cũng có những tính năng tương tự và syntax không thay đổi
```
ln [origin-file] [hardlink-file]
```

Tuy nhiên cả 2 cũng có 1 vài nhược điểm và có thể bù trừ cho nhau
|Hard link                                                          | symbolic link     |
|-------------------------------------------------------------------|-------------------|
|Chỉ liên kết được tới file, không liên kết được tới thư mục        | 	Có thể liên kết được tới thư mục|
|Không tham chiếu được tới file trên ổ đĩa khác                     | 	Có thể tham chiếu tới file/thư mục khác ổ đĩa|
|Liên kết tới một file vẫn còn ngay cả khi file đó đã được di chuyển| 	Liên kết không còn tham chiếu được nữa nếu file được di chuyển|
|Được liên kết với inode tham chiếu vật lý trên ổ cứng nơi chứa file| 	Liên kết tham chiếu tên file/thư mục trừu tượng mà không phải địa chỉ vật lý. Chúng được cung cấp inode riêng của mình|
|Có thể làm việc với mọi ứng dụng                                   | 	Một số ứng dụng không cho phép symbolic link|
## - Nén-giải nén file/folder
Là ứng dụng mặc định có sẵn trên OS, cho phép nén nhiều file với nhau để tạo file mới có dịnh dạng phù hợp (chủ yếu cho truyền tải dữ liệu)
```
zip [zip-name] [file 1] [file...n]
unzip [zip-name]
```
![zip](images/zip-unzip.png)

Giải nén tất cả trừ 1 file:
```
unzip [compress-file] [file-to stay]
```

Giải nén tất cả váo folder chỉ định:
```
unzip [compress-file] -d [desination folder] [file-to-stay]
```
Ghi đè file khi giải nén:
```
unzip -o [compress-file]
```
Liệt kê mọi file có trong file nén:
```
unzip -l [compress-file]
```
Kiểm tra, xác minh file khi giải nén:
```
unzip -t [compress-file] [file-to-stay]
```
Ngoài ra vẫn còn các option khác đi kèm lệnh `unzip -h`

## - Đo lưu lượng mạng sử dụng
Có rất nhiều tools phổ biến sử dụng để kiểm tra, đo lường băng thông lưu lượng mang đang sử dụng theo thời gian thực 

Trong đó __netstat, nethogs__ và __nload__ được sử dụng phổ biến vì tính trực quan cũng như chi tiết thông số

### Nethogs
Điểm mạnh của __nethogs__ chính là do được lưu lượng theo PID riêng lẻ, giúp xác định, quản lý dễ dàng hơn từng task của từng user khác nhau đang sử dụng mạng

![Nethogs](images/nethogs1.png)

### Nload

![nload](images/nload.png)

Tính năng nổi bật của nó là hiển thị thông số trực quan
Đo được lưu lượng tối thiểu-tối đa-trung bình-hiện tại và TimeToLive của gói

Tuy nhiên __nload__ lại không thể đo chi tiết từng task hay user bằng PID như __nethogs__ do đó đã làm hạn chế khả năng của nó
### Netstat
Là 1 tool CLI phổ biến về đo lưu lượng mạng cùng với rất nhiều option khác nhau phục vụ cho hiển thị thông số, đo lường, kiểm tra khác nhau

Dùng lệnh `netstat --help` để nắm rõ được các option có sẵn

![netstat](images/netstat.png)

Đây là 1 số option tiêu biểu được sử dụng trong __netstat__
- __-a__: Hiển thị tất cả các sockets, kể cả listening và non-listening
- __-l__: Hiển thị các socket đang lắng nghe
- __-t__: Chỉ hiển thị các kết nối tcp
- __-u__: Chỉ hiển thị các kết nối udp
- __-n__: Xem địa chỉ số (không phân giải)
- __-p__: Hiển thị chương trình PID cho từng socket
- __-r__: Hiển thị bảng định tuyến
- __-s__: Pull và hiển thị thống kê mạng được sắp xếp theo giao thức
- __-i__: Hiển thị danh sách các giao diện mạng
### Iftop
Là 1 tool chỉ được sử dụng bởi quản trị hoặc nhưng người có đặc quyền giám sát, __iftop__ đo và lắng nghe lưu lượng băng thông mạng theo thời gian thực được gán tên chỉ định và hiển thị bảng thông số theo cặp ghép host

__iftop__ sẽ đo lường lưu lượng qua từng socket mạng riêng lẻ, tuy nhiên lại không trả được kết quả của từng tên hay ID cụ thể mà chỉ chung trên 1 cổng kết nối cụ thể nhưng iftop sẽ có lợi thế đặt các filter chỉ định trước qua đó lọc lưu lượng và trả kết quả băng thông đang sử dụng

![iftop](images/iftop.png)
## - Xem nội dung không cần text editor
Trong trường hợp các text editor không hoạt động như mong muốn nhưng user vẫn cần hiển thị nội dung file, có một vài lệnh giúp thực hiện việc đó
### Cat
Là 1 trong những lệnh cơ bản nhất khi mới làm quen Linux
`cat` được dùng để hiển thị mọi nội dung của mọi file có trong máy mà không cần bất kì text editor nào

![cat](images/cat.png)
### less và more
Bên cạnh `cat`, lệnh `less` và `more` là 2 lệnh được gợi ý nhiều nhất khi luyện tập thao tác trên Linux
DÙ tương tự __cat__ chỉ để hiển thị nội dung file nhưng __less__ và __more__ lại có cách làm khác
- `less` command:

Hiển thị 1 trang nội dung file cho 1 lần, để tiếp tục cần nhấn __enter__ hoặc __space__, để thoát nhấn _q_ rồi _enter_

- Có 2 option chính khi dùng với __less__
    1. `less -N [file]` giúp hiển thị số dòng 
    1. `less -X [file]` giúp hiển thị hết nội dung mà không biến mất khi quay về Terminal
- `more` command:

__more__ khá giống với __less__ khi chỉ hiển thị 1 phần nội dung, để tiếp tục cần nhấn __enter__, tuy nhiên vào cuối file sẽ tự mất và thoát khỏi trình hiển thị nội dung

### head và tail
2 lệnh này rất nhau và thường sử dụng để hiển thị số dòng nội dung đầu hoặc cuối file tương ứng

Kết hợp option `-n[number-of-line]` để chọn hiển thị số dòng nội dung tùy ý
```
head -n 15 ./Document/text/txt (hiển thị 15 dòng nôi dung đầu file)
tail -n 4 ./Desktop/memlist.txt (hiển thị 4 dòng nội dung cuối file)
```

