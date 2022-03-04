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
Đầu tiên, bạn cần thiết lập chế độ hoạt động mặc định của **UFW**:
* Chặn tất cả các kết nối từ ngoài truy cập vào máy chủ.

        sudo ufw default deny incoming
* Chỉ cho phép kết nối từ máy chủ ra bên ngoài.

        sudo ufw default allow outgoing
Sau đó, chúng ta sẽ thiết lập thêm các quy tắc để cho phép các kết nối bên ngoài truy cập vào các dịch vụ qua các cổng được chỉ định tuỳ theo nhu cầu sử dụng.

### Cho phép kết nối SSH
Bạn cần mở cổng kết nối **SSH** trước khi kích hoạt **UFW**. Nếu không, bạn sẽ không thể truy cập vào máy chủ được nữa, do thiết lập mặc định đã chặn mọi kết nối từ bên ngoài vào.

Bạn có thể mở kết nối **SSH** bằng 3 cách sau:

**Cách 1:** Sử dụng tên ứng dụng **OpenSSH**

        sudo ufw allow OpenSSH
**Cách 2:** Sử dụng tên dịch vu **ssh**

        sudo ufw allow ssh
**Cách 3:** Sử dụng port **22**

        sudo ufw allow 22
Nếu bạn đã cấu hình truy cập SSH qua cổng khác, cần phải thay đổi port tương ứng khi cấu hình UFW. Ví dụ, nếu bạn cấu hình truy cập SSH Server qua cổng 2222, hãy đổi thành lệnh sau để mở cổng kết nối:

        sudo ufw allow 2222
**-> Tất cả các cách trên đều trả về kết quả hiển thị:**

        Rule added
        Rule added (v6)

## IV. Bật tường lửa UFW
Sau khi đã mở cổng kết nối **SSH**, bạn đã có thể kích hoạt tường lửa **UFW**.

Trước khi kích hoạt, kiểm tra lại các quy tắc đã được thiết lập trên **UFW** bằng lệnh sau:

        sudo ufw show added
Kết quả hiện thị:

        Added user rules (see 'ufw status' for running firewall):
        ufw allow 22
Sau khi đã chắc chắn đã mở cổng kết nối **SSH**, kích hoạt **UFW** bằng lệnh:

        sudo ufw enable
Hệ thống sẽ cảnh báo việc kích hoạt **UFW** có thể gây gián đoạn kết nối **SSH**. Do bạn đã cấu hình mở cổng **SSH** nên sẽ không gặp vấn đề nào cả. Chọn "y" và bấm "Enter" để xác nhận.

        Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
        Firewall is active and enabled on system startup
Tường lửa giờ đã được kích hoạt. Kiểm tra lại tình trạng hoạt động của **UFW** để xác nhận các dịch vụ được cho phép:

        Status: active
        Logging: on (low)
        Default: deny (incoming), allow (outgoing), disabled (routed)
        New profiles: skip

        To                         Action      From
        --                         ------      ----
        22                         ALLOW IN    Anywhere
        22 (v6)                    ALLOW IN    Anywhere (v6)

## V. Cấu hình tường lửa UFW nâng cao
**Phần còn lại của hướng dẫn này trình bày chi tiết hơn về cách sử dụng UFW, chẳng hạn như cho phép hoặc từ chối các loại kết nối khác nhau.**

**Đến đây, bạn nên cho phép tất cả các kết nối khác mà server của bạn cần phản hồi. Các kết nối mà bạn nên cho phép tùy thuộc vào nhu cầu cụ thể của bạn.**

### Mở kết nối cho web server Apache / Nginx:
* Dịch vụ web server **HTTP** sử dụng cổng **80**, mở kết nối bằng lệnh:

        sudo ufw allow http
  Hoặc sử dụng:

        sudo ufw allow 80
* Dịch vụ web server **HTTPS** sử dụng cổng **443**, mở kết nối bằng lệnh:

        sudo ufw allow https
  Hoặc sử dụng:

        sudo ufw allow 443
**-> Bạn cũng có thể mở kết nối HTTP và HTTPS theo tên của Web Server:**

* Nếu máy chủ đang cài web server **Apache**:

        sudo ufw allow 'Apache Full'
* Nếu máy chủ đang cài web server **Nginx**:

        sudo ufw allow 'Nginx Full'
**=> Bạn có thể kiểm tra hồ sơ ứng dụng, dịch vụ đã được cài đặt trên máy chủ bằng lệnh:**

        sudo ufw app list

### Mở kết nối theo dãy cổng cụ thể:
**Bạn có thể cấu hình mở cổng mạng cho UFW bằng lệnh:**
        
        sudo ufw allow <port>
Ví dụ mình muốn mở kết nối cho cổng 873 dịch vụ truyền tải file qua mạng **rsync**

        sudo ufw allow 873
Bạn có thể chỉ định phạm vi cổng với **UFW**. Một số ứng dụng sử dụng nhiều cổng, thay vì một cổng duy nhất. Ví dụ dưới đây là cách mở kết nối cho dịch vụ **X11**

        sudo ufw allow 6000:6007/tcp
        sudo ufw allow 6000:6007/udp

### Cho phép kết nối theo địa chỉ IP cụ thể:
**Khi làm việc với UFW, bạn cũng có thể chỉ định địa chỉ IP.** 

Ví dụ: nếu bạn muốn cho phép kết nối từ một địa chỉ IP cụ thể, chẳng hạn như địa chỉ IP cơ quan hoặc nhà riêng là **192.168.5.50**, bạn cần chỉ định **from** , sau đó là địa chỉ IP:

        sudo ufw allow from 192.168.5.50
**Bạn có thể quy định thêm cổng kết nối để giới hạn truy cập cho IP.**

Ví dụ mình muốn cho phép địa chỉ IP **192.168.5.50** kết nối vào cổng **22 (SSH)**, sử dụng lệnh sau:

        sudo ufw allow from 192.168.5.50 to any port 22

### Cho phép kết nối theo Subnet (Mạng con)
**Bạn có thể thay thế IP bằng Subnet để cho phép kết nối theo lớp mạng.** 

Ví dụ: mình muốn cho phép dãy IP từ **10.0.1.1** đến **10.0.1.254**, sử dụng lệnh sau:

        sudo ufw allow from 10.0.1.0/24
**Tương tự như vậy, bạn cũng có thể chỉ định cổng đích mà Subnet 10.0.1.0/24 được phép kết nối.**

Ta sẽ sử dụng cổng **22 (SSH)** làm ví dụ:

        sudo ufw allow from 10.0.1.0/24 to any port 22

### Từ chối kết nối
Sử dụng tham số **deny** để chặn kết nối vào máy chủ.
* Chặn kết nối **HTTP**:

        sudo ufw deny http
* Hoặc nếu bạn muốn từ chối tất cả các kết nối từ địa chỉ IP cụ thể **123.123.123.123** bạn có thể sử dụng lệnh này:

        sudo ufw deny from 123.123.123.123
* Chặn cổng **25** từ máy chủ ra bên ngoài (mục đích nhằm chặn dịch vụ **email SMTP**)

        sudo ufw deny out 25

### Xoá kết nối theo số thứ tự:
Để xoá quy tắc theo số thứ tự, bạn cần liệt kê các quy tắc theo thứ tự bằng lệnh:

        sudo ufw status numbered
Kết quả hiển thị:

        Status: active

                To                         Action      From
                --                         ------      ----
        [ 1] 22                         ALLOW IN    Anywhere
        [ 2] 80                         ALLOW IN    Anywhere
        [ 3] 22 (v6)                    ALLOW IN    Anywhere (v6)
        [ 4] 80 (v6)                    ALLOW IN    Anywhere (v6)
Nếu muốn xoá kết nối dịch vụ **HTTP**, số thứ tự **2**, sử dụng lệnh:

        sudo ufw delete 2
Điều này sẽ hiển thị dấu nhắc xác nhận sau đó xóa luật **2**. Lưu ý nếu bạn đã bật IPv6, bạn cũng muốn xóa luật IPv6 tương ứng. Kết quả hiển thị:

        Deleting:
         allow 80
        Proceed with operation (y|n)? y
        Rule deleted
