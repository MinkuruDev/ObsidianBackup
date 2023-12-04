# Triển khai hệ thống file

## Bố cục hệ thống file

Hệ thống file được lưu trữ trên đĩa. Mỗi đĩa có thể được chia làm một hoặc nhiều phân vùng đĩa, với mỗi hệ thống file độc lập trên mỗi phân vùng. Phân vùng 0 của đĩa gọi là **Bản ghi khởi động chính** (MBR - Master Boot Record) được dùng để khởi động máy tính. Cuối MBR là bảng phân vùng. Bảng này cho biết địa chỉ bắt đầu và kết thúc của mỗi phân vùng đĩa. Một trong các phân vùng trong bảng được đánh dấu là đang hoạt động.

Khi máy tính khởi động, BIOS sẽ đọc và thực thi MBR. Điều đầu tiên mà chương trình MBR thực hiện là xác định vị trí phân vùng đang hoạt động, đọc khối đầu tiên của nó, được gọi là khối khởi động và thực thi nó. Chương trình trong khối khởi động sẽ tải hệ điều hành có trong phân vùng đó. Để thống nhất, mọi phân vùng đều bắt đầu bằng một khối khởi động, ngay cả khi nó không chứa hệ điều hành có khả năng khởi động. Ngoài ra, nó có thể chứa một cái trong tương lai.

Ngoài việc bắt đầu với một khối khởi động, một phân vùng đĩa còn có các thành phần khác

## Cấp phát không gian cho file (Thực hiện file)

### Cấp phát các khối liên tiếp

### Sử dụng danh sách liên kết (link list)

### Sử dụng danh sách liên kết trên bảng chỉ số

### Sử dụng khối chỉ số

## Thực hiện thư mục

## Tệp tin chia sẻ

## Hệ thống file có cấu trúc nhật ký

**_3.6 Ghi nhật ký hệ thống file_**

**_3.7 Hệ thống tệp ảo_**

**5. Một số hệ thống file**

**_5.1 Hệ thống file MS-DOS (FAT)_**

**_5.2 Hệ thống file UNIX V7_**

