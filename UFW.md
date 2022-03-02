# Tường lửa UFW

![ufw](https://user-images.githubusercontent.com/97789851/156337826-01ccf2f6-c7b3-448a-b9f6-7ef066039c10.png)

***Tài liệu tham khảo:***

* [https://galaxyz.net/cach-thiet-lap-firewall-voi-ufw-tren-ubuntu-2004.1536.anews#step-3-%E2%80%94-allowing-ssh-connections](https://galaxyz.net/cach-thiet-lap-firewall-voi-ufw-tren-ubuntu-2004.1536.anews#step-3-%E2%80%94-allowing-ssh-connections)
* [https://help.ubuntu.com/community/UFW](https://help.ubuntu.com/community/UFW)

## I. Giới thiệu về tường lửa UFW
Công cụ cấu hình tường lửa mặc định cho **Ubuntu** là **UFW** - hay Tường lửa không phức tạp. Là một giao diện quản lý firewall được đơn giản hóa nhằm giảm thiểu sự phức tạp hơn so với các firewall như **[IPtables](https://github.com/LamTruong-Cybersecurity/Firewall/blob/main/IPtables.md)**. Nếu mới bắt đầu sử dụng tường lửa thì **UFW** cung cấp một cách thân thiện với dùng để tạo tường lửa trên máy chủ **Ubuntu**.

***Lam Trường*** sẽ hướng dẫn các bạn cách cấu hình và sử dụng tường lửa **UFW** trên máy chủ Ubuntu.

**Cùng theo dõi nhé!! Chúc các bạn thành công :>**

## Thiết lập ban đầu
Người dùng **root** là người dùng quản trị trong môi trường Linux có các đặc quyền rất rộng. Do các đặc quyền cao của tài khoản root, bạn không nên sử dụng nó một cách thường xuyên. Tài khoản **root** có thể thực hiện các thay đổi rất nguy hiểm, thậm chí là do tình cờ.

**->** Bạn cần truy cập vào máy chủ Ubuntu 20.04 với tư cách là người dùng thông thường, không phải **root** và tường lửa **UFW** được bật trên máy chủ của bạn. Bạn có thể cài đặt theo **[Hướng dẫn thiết lập ban đầu](https://github.com/LamTruong-Cybersecurity/LEMP-Stack/blob/master/LEMP_Ubuntu20.04.md)**.
