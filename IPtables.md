# IPtables
![Iptables](https://user-images.githubusercontent.com/97789851/154507989-4efe18d2-80f3-45ac-a636-1064830a7c7a.jpg)

## I. Basic Commands

	$ sudo iptables -L

-> Liệt kê các quy tắc hiện tại của iptables

## II.Basic Iptables Options
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