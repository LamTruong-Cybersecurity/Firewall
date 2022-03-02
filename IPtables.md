# Tường lửa IPtables

![Iptables](https://user-images.githubusercontent.com/97789851/154507989-4efe18d2-80f3-45ac-a636-1064830a7c7a.jpg)

***Tài liệu tham khảo:***

* [https://help.ubuntu.com/community/IptablesHowTo](https://help.ubuntu.com/community/IptablesHowTo)
* [https://www.hostinger.com/tutorials/iptables-tutorial](https://www.hostinger.com/tutorials/iptables-tutorial)
* [https://wiki.matbao.net/iptables-la-gi-cach-cau-hinh-bao-mat-ubuntu-vps-linux-firewall/](https://wiki.matbao.net/iptables-la-gi-cach-cau-hinh-bao-mat-ubuntu-vps-linux-firewall/)

## I.Basic Iptables Options
1. -A : Nối quy tắc này vào 1 chuỗi quy tắc. Các chuỗi hợp lệ có dạng INPUT, FORWARD, OUTPUT. Nhưng chủ yếu xử lý INPUT, ảnh hưởng đến lưu lượng đến
2. -L : Bộ lọc liệt kê các quy tắc hiện tại
3. -m conntrack : Cho phép các quy tắc lọc khớp với nhau dựa trên trạng thái kết nối. Cho phép xử dụng tùy chọn --ctstate
4. -- ctstate : Xác định danh sách các trạng thái cho quy tắc phù hợp
    
	VD:

	NEW - Kết nối vẫn chưa được nhìn thấy
		
	RELATED - Kết nối mới nhưng liên quan đến 1 kết nối khác đã được cho phép
		
	ESTABLISHED - Kết nối đã được thiết lập
		
	INVALID - Không thể xác định được lưu lượng truy cập vì lý do nào đó
5. -m limit : Yêu cầu quy tắc chỉ khớp với 1 số lần giới hạn. Cho phép sử dụng tùy chọn --limit. Hữu ích khi giới hạn các quy tắc ghi nhật ký
6. -p : Giao thức kết nối được sử dụng
7. --dport : 1 hoặc nhiều cổng đích cần thiết cho quy tắc này. Một cổng duy nhất có thể được cung cấp hoặc 1 phạm vi có thể được cung cấp là start: end, sẽ khớp với tất cả các cổng từ đầu đến cuối, bao gồm cả
8. -j : Nhảy đến mục tiêu được chỉ định. Theo mặc định, iptables cho phép 4 mục tiêu sau:
		
	ACCEPT - Chấp nhận gói và dừng các quy tắc xử lý trong chuỗi này
		
	REJECT - Từ tối gói tin và thông báo cho người gửi rằng chúng tôi đã làm như vậy và ngừng xử lý các quy tắc trong chuỗi này
		
	DROP - Bỏ qua gói tin 1 cách im lặng và dừng các quy tắc xử lý trong chuỗi này
	
	LOG - Ghi lại gói và tiếp tục xử lý các quy tắc khác trong chuỗi này. Cho phép sử dụng các tùy chọn --log-prefix và --log-level
9. --log-prefix : Khi ghi nhật ký, hãy đặt văn bản này trước thông báo nhật ký. Xử dụng dấu ngoặc kép xung quanh văn bản để sử dụng
10. --log-level : Nhật ký sử dụng cấp, nhật ký hệ thống được chỉ định. 7 là một lựa chọn tốt trừ khi bạn đặc biệt cần thứ gì đó khác
11. -i : Chỉ khớp nếu gói đến trên giao diện được chỉ định
12. -I : Chèn 1 quy tắc. Có 2 tùy chọn, chuỗi để chèn quy tắc và số quy tắc phải có
	
	VD: 
	
	-I INPUT 5 - Sẽ chèn quy tắc vào chuỗi INPUT và biến nó thành quy tắc thứ 5 trong danh sách
13. -v : Hiển thị thêm thông tin trong đầu ra. Hữu ích nếu có các quy tắc trông tương tự mà không sử dụng -v
14. -s --source : địa chỉ [/mask] đặc điểm kỹ thuật nguồn
15. -d --destination : địa chỉ [/mask] thông số kỹ thuật đích
16. -o --out-interface : tên đầu ra [+] tên giao diện mạng ([+] cho ký tự đại diện)

## II. Basic rules are often used
		sudo iptables -L
-> Liệt kê các quy tắc hiện tại của iptables

**1. Kích hoạt lưu lượng truy cập trên Localhost**

		sudo iptables -A INPUT -i lo -j ACCEPT
**Giúp đảm bảo các thao tác trên máy chủ hoạt động ổn định**. Lệnh này có nghĩa là:

-A INPUT: khai báo kiểu kết nối sẽ được áp dụng (A nghĩa là Append).

-i lo: Khai báo thiết bị mạng được áp dụng (i nghĩa là Interface).
	
-j ACCEPT: khai báo hành động sẽ được áp dụng cho quy tắc này (j nghĩa là Jump).

**2. Cho phép lưu lại các kết nối hiện tại tránh hiện tượng tự block ra khỏi máy chủ**

		sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
**3. Bật kết nối trên cổng SSH, HTTP và HTTPS**

		sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
		sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
		sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
-p tcp : Giao thức được áp dụng (tcp, udp, all)

–dport 22, 80, 443: Cổng cho phép áp dụng lần lượt là SSH, HTTP, HTTPS

**4. Chặn tất cả lưu lượng truy cập khác**

		sudo iptables -A INPUT -j DROP

Bây giờ, tất cả kết nối bên ngoài cổng được chỉ định sẽ bị ngắt.

**5. Xóa rule**

		sudo iptables -F
Lệnh này xóa tất cả các quy tắc hiện tại. Tuy nhiên, để xóa một quy tắc cụ thể, bạn phải sử dụng tùy chọn -D. Trước tiên, bạn cần xem tất cả các quy tắc có sẵn bằng cách nhập lệnh sau:
		
		sudo iptables -L --line-numbers
Bạn sẽ nhận được một danh sách các quy tắc tương ứng với các số:

		Chain INPUT (policy ACCEPT)
		num  target     prot opt source               destination
		...
		2    ACCEPT     tcp -- anywhere             anywhere tcp dpt:https
		3    ACCEPT     tcp -- anywhere             anywhere tcp dpt:http
		4    ACCEPT     tcp -- anywhere             anywhere tcp dpt:ssh
		...
Để xóa quy tắc bất kì, hãy chèn chuỗi và số tương ứng từ danh sách. Giả sử trong bảng iptables này, bạn muốn loại bỏ quy tắc số 3 của chuỗi INPUT. Lệnh sẽ là:
		
		sudo iptables -D INPUT 3
**6. Chèn một rule mới vào vị trí xác định:**

		sudo iptables -I INPUT 2 -p tcp --dport 8080 -j ACCEPT
Thay tham số Append -A  bằng tham số INSERT -I. Ở đây luật mới sẽ ở vị trí số 2

**7.  Lọc gói tin dựa trên nguồn**

Iptables cho phép bạn lọc các gói tin dựa trên địa chỉ IP hoặc một dải địa chỉ IP. Bạn cần chỉ định nó sau tùy chọn -s . Ví dụ, để chấp nhận các gói từ 192.168.1.3 , lệnh sẽ là:
		
		sudo iptables -A INPUT -s 192.168.1.3 -j ACCEPT

Cấu hình chỉ cho phép kết nối SSH từ một địa chỉ IP duy nhất được xác định. Ví dụ IP 192.168.1.3

		sudo iptables -I INPUT -p tcp -s 192.168.1.3 -m tcp --dport 22 -j ACCEPT
Bạn cũng có thể từ chối các gói từ một địa chỉ IP cụ thể bằng cách thay thế đích ACCEPT bằng DROP

		sudo iptables -A INPUT -s 192.168.1.3 -j DROP
Chặn 1 IP truy cập vào 1 port cụ thể, ví dụ chặn IP 192.168.1.3 truy cập vào port 22 (SSH)

		sudo iptables -A INPUT -p tcp -s 192.168.1.3 –dport 22 -j DROP
Nếu bạn muốn loại bỏ các gói tin từ một loạt các địa chỉ IP, bạn phải sử dụng tùy chọn **-m** và mô-đun **iprange**. Sau đó, chỉ định dải địa chỉ IP với **–src-range**. Hãy nhớ rằng, một dấu gạch nối sẽ phân tách phạm vi địa chỉ ip mà không có dấu cách, như sau:
		
		sudo iptables -A INPUT -m iprange --src-range 192.168.1.100-192.168.1.200 -j DROP
**8. Mở port Mail**

Cho phép user dùng SMTP Server qua port mặc định là 25 và 465:

		sudo iptables -I INPUT -p tcp -m tcp --dport 25 -j ACCEPT
		sudo iptables -I INPUT -p tcp -m tcp --dport 465 -j ACCEPT
-> IPtables sẽ hiển thị SMTP và URD theo mặc định

		ACCEPT   tcp  -- anywhere      anywhere           tcp dpt:smtp
		ACCEPT   tcp  -- anywhere      anywhere           tcp dpt:urd
Mở port POP3 (port mặc định 110 và 995) cho phép user đọc mail trên server:

		sudo iptables -A INPUT -p tcp -m tcp --dport 110 -j ACCEPT
		sudo iptables -A INPUT -p tcp -m tcp --dport 995 -j ACCEPT
-> IPtables sẽ hiển thị POP3 và POP3S theo mặc định

		ACCEPT 	tcp  --  anywhere         	anywhere        	tcp dpt:pop3
		ACCEPT 	tcp  --  anywhere         	anywhere        	tcp dpt:pop3s
Cho phép sử dụng giao thức IMAP mail protocol (port mặc định là 143 và 993):

		sudo iptables -A INPUT -p tcp -m tcp --dport 143 -j ACCEPT
		sudo iptables -A INPUT -p tcp -m tcp --dport 993 -j ACCEPT
-> IPtables sẽ hiển thị IMAP và IMAPS theo mặc định

		ACCEPT 	tcp  --  anywhere         	anywhere        	tcp dpt:imap
		ACCEPT 	tcp  --  anywhere         	anywhere        	tcp dpt:imaps
**9. Kết thúc thiết lập IPtables**

Sau khi đã thiết lập tất cả bao gồm mở các port cần thiết hoặc hạn chế kết nối cần block toàn bộ các kết nối còn lại và cho phép toàn bộ các kết nối ra ngoài.

		sudo iptables -P OUTPUT ACCEPT
		sudo iptables -P INPUT DROP
**10. Lưu cấu hình IPtables**

Lúc này những IPtables rules được tạo ra đều được lưu trong bộ nhớ, khi khởi động lại máy chủ cần phải tạo lại các rules này. Để lưu các rule ra một file backup ta dùng lệnh:

		sudo iptables-save > /usr/src/iptables-rule
Trong trường hợp xấu không mong muốn có thể bạn xóa một rule nào đó dẫn đến mất kết nối dịch vụ. Để khôi phục lại những rule đã tạo từ file backup ta dùng lệnh sau:

		sudo iptables-restore < /usr/src/iptables-rule
