# Quản lý tường lửa UFW trên hệ điều hành Linux

![ufw](https://user-images.githubusercontent.com/97789851/156337826-01ccf2f6-c7b3-448a-b9f6-7ef066039c10.png)

***Tài liệu tham khảo:***

* [https://galaxyz.net/cach-thiet-lap-firewall-voi-ufw-tren-ubuntu-2004.1536.anews#step-3-%E2%80%94-allowing-ssh-connections](https://galaxyz.net/cach-thiet-lap-firewall-voi-ufw-tren-ubuntu-2004.1536.anews#step-3-%E2%80%94-allowing-ssh-connections)
* [https://help.ubuntu.com/community/UFW](https://help.ubuntu.com/community/UFW)
* [https://thuanbui.me/ufw-linux-firewall/](https://thuanbui.me/ufw-linux-firewall/)

## I. Giới thiệu về tường lửa UFW
Hầu hết các bản phiên bản **Linux** đều được cài đặt sẵn tiện tích tường lửa **iptables** (riêng **CentOS** sử dụng **firewalld**) để quản lý gói tin dựa theo các quy tắc được thiết lập sẵn nhằm nâng cao tính bảo mật cho hệ thống. Tuy nhiên việc cấu hình **[iptables](https://github.com/LamTruong-Cybersecurity/Firewall/blob/main/IPtables.md)** khá phức tạp bởi cấu trúc lệnh dài dòng và khó nhớ.

Đối với **Ubuntu**, công cụ cấu hình tường lửa mặc định là **UFW** - hay Tường lửa không phức tạp. Là một giao diện quản lý firewall được đơn giản hóa nhằm giảm thiểu sự phức tạp hơn so với các firewall như **[iptables](https://github.com/LamTruong-Cybersecurity/Firewall/blob/main/IPtables.md)**.

**-> Như vậy, nếu mới bắt đầu sử dụng tường lửa thì UFW cung cấp một cách thân thiện với người dùng để tạo tường lửa trên hệ điều hành Linux**.

***Lam Trường*** sẽ hướng dẫn các bạn cách cấu hình và sử dụng tường lửa **UFW**.

**Cùng theo dõi nhé!! Chúc các bạn thành công :>**

## Thiết lập ban đầu
**Người dùng root là người dùng quản trị trong môi trường Linux có các đặc quyền rất rộng. Do các đặc quyền cao của tài khoản root, bạn không nên sử dụng nó một cách thường xuyên. Tài khoản root có thể thực hiện các thay đổi rất nguy hiểm, thậm chí là do tình cờ**.

**-> Bạn cần truy cập vào máy chủ Ubuntu với tư cách là người dùng thông thường, không phải root**. Bạn có thể cài đặt theo **[Hướng dẫn thiết lập ban đầu](https://github.com/LamTruong-Cybersecurity/Initial-Server-Setup)**.

## II. Cài đặt UFW (Bỏ phần này nếu bạn sử dụng máy chủ Ubuntu)
**=> LƯU Ý**: Bạn chỉ nên cài đặt **UFW** trên hệ thống **Linux** mới, tắt tất cả các loại tường lửa trên máy chủ của bạn nếu nó đang hoạt động. Chưa cài đặt bất cứ control panel, script quản lý nào như cPanel, aaPanel, CyberPanel, Centminmod,… Vì các loại control panel này luôn cài đặt sẵn các tiện ích tường lửa đi kèm. Cài đặt thêm **UFW** sẽ gây xung đột hệ thống.

Nếu dùng **CentOS** thì các bạn thay thế **apt** bằng **yum**

Cập nhật hệ thống trước khi cài đặt:

        sudo apt update
        sudo apt upgrade -y
Kiểm tra xem **UFW** đã được cài sẵn trên máy chưa:

        which ufw
Nếu không nhận được kết quả nào, nghĩa là ufw chưa được cài đặt trong máy. Bạn cài đặt **UFW** như các package khác bằng lệnh:

        sudo apt install ufw
**-> Riêng đối với CentOS**. Theo mặc định, UFW không có sẵn trong kho lưu trữ **CentOS**. Vì vậy, bạn sẽ cần cài đặt kho **EPEL** vào hệ thống của mình. Bạn có thể thực hiện việc này bằng cách chạy lệnh sau:

        sudo yum install epel-release -y
Sau khi kho **EPEL** được cài đặt, bạn có thể cài đặt **UFW** bằng cách chạy lệnh sau:

        sudo yum install --enablerepo="epel" ufw -y

Mặc định sau khi cài đặt, **UFW** sẽ không được kích hoạt. Bạn cần giữ nguyên như thế. Chỉ kích hoạt **UFW** sau khi đã thực hiện những bước cấu hình căn bản.

        sudo ufw status
Kết quả trả về: **inactivate** (nghĩa là chưa kích hoạt)

        Status: inactive

## III. Cấu hình căn bản UFW
### Thiết lập chế độ mặc định
Đầu tiên, bạn cần thiết lập chế độ hoạt động mặc định của UFW:
* Chặn tất cả các kết nối từ ngoài truy cập vào máy chủ.
* Chỉ cho phép kết nối từ máy chủ ra bên ngoài.