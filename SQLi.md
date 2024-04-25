# SQL Injection


### Kiến thức

Hệ thống quản lý cơ sở dữ liệu (DBMS) là hệ thống phần mềm được sử dụng để lưu trữ, truy xuất và chạy các truy vấn trên dữ liệu. DBMS đóng vai trò là giao diện giữa người dùng cuối và cơ sở dữ liệu, cho phép người dùng tạo, đọc, cập nhật và xóa dữ liệu trong cơ sở dữ liệu.
**Các loại SQL:** MySQL, SQL Server, MSSQL, PostpreSQL, Oracle, ...

*SQL tĩnh và SQL động*
**SQL tĩnh:**  truyền thống, các câu truy vấn SQL được cố định trong mã nguồn ứng dụng, không thay đổi dựa trên đầu vào của người dùng.

**SQL động:** linh hoạt, các câu truy vấn SQL được tạo ra hoặc điều chỉnh dựa trên dữ liệu đầu vào của người dùng.

*Khái niệm:* SQLi là lỗ hổng bảo mật web cho phép kẻ tấn công có thể xem trái phép cơ sở dữ liệu của trang web. Có nhiều trường hợp tệ hơn, kẻ tấn công có thể xoá và sửa dữ liệu quan trọng làm thiệt hại lớn cho người dùng và nhà phát triển.

*Nguyên nhân:* việc xảy ra sqli là do nhà phát triển. Thường khi code họ không filter đi dữ liệu đầu vào của người dùng (tức là các nơi nhập liệu) hoặc kiểm tra không kỹ lưỡng. Kẻ tấn công lợi dụng điều đó để truy cập trái phép, xâm phạm tài nguyên, cơ sở dữ liệu của trang web.

*SQLi gồm các dạng:*
- In-band SQLi: Dạng tấn công phổ biến nhất và cũng dễ để khai thác. Lỗ hổng  xảy ra khi kẻ tấn công xâm phạm và thu nhập kết quả trực tiếp trên cùng một nơi.
	- Error-based SQLi: Dạng tấn công phụ thuộc vào kết quả lỗi trả về. Nhận biết lỗ hổng này qua lỗi khi thực hiện 1 số câu truy vấn.
	- Union-based SQLi: Dạng tấn công nối các câu truy vấn lại với nhau để có thể truy xuất ra dữ liệu của trang web.
- Inferential SQLi (Blind SQLi): Dạng tấn công này dự đoán dựa trên kết quả phản hồi của web và hành vi của cơ sở dữ liệu.
	- Blind-boolean-based SQLi: Dạng tấn công này dựa theo kết quả trả về của phản hồi. Ta sẽ kiểm tra ra bằng cách sử dụng câu truy vấn đúng sai và dựa theo phản hồi đồng thời phân tích và đánh giá.
	- Time-based-blind SQLi: Dạng tấn công này dựa theo thời gian trả về của phản hồi. Ta sẽ kiểm tra ra bằng cách sử dụng câu truy vấn thời gian và dựa theo phản hồi đồng thời phân tích và đánh giá.
- Out-of-band SQLi: Dạng tấn công này phụ thuộc vào tính năng được sử dụng trên dữ liệu máy chủ và web. Kẻ tấn công rất khó để xâm phạm và thu nhập được dữ liệu vì nó không cùng trên một nơi, đặc biệt phản hồi ở đây không được ổn định.

*Khắc phục:* 
- Kiểm tra và lọc dữ liệu đầu vào, nơi nhập liệu của người dùng. Cấm truy cập khi phát hiện các câu truy vấn nguy hiểm, có khả năng nguy hiểm tồn tại ở các nơi nhập liệu.
- Sử dụng SQL động để giảm thiểu nguy cơ bị SQLi.

### Kinh nghiệm

Xác định trang web sử dụng loại SQL, database.

Nên thêm null khi SELECT và có dấu -- (comment) đường sau mỗi câu truy vấn.

Luôn chuyển đổi 2 cách là -- và # (%23).

Dấu # được hiểu là đánh dấu hoặc chú thích.

Sử dụng  OR, AND, ;, || hoặc các ký tự được cho phép.

Khi thực hiện các lệnh trên url thì nên url encode. Có những trang web họ đã lọc đi ký tự của nó, do đó ta nên url encode để có thể bypass thành công.

Blind SQLi xảy ra khi ta ghi sai hoặc có câu truy vấn nhưng trang web vẫn chấp nhận rằng nó đúng.

Đọc các cheat sheet, lọc các cheet sheet phù hợp để dùng, Có thể brute force để tìm ra payload phù hợp (để ý rate limit).

Khi khai thác, tấn công 1 trang web bất kỳ thì cần hiểu rõ trang web đó, chức năng đó để có thể khai thác đúng cách, tránh mất thời gian. 

Không tin tưởng bất kỳ nơi nào cả, không có thứ gì là bảo mật 100%. 

Luôn kiểm tra những nơi nghi ngờ.



### Trải nghiệm

`PortSwigger`

Mỗi khi làm bài lab, họ đã đưa ra các gợi ý để giúp chúng ta hiểu rõ hơn về lỗ hổng và nơi xảy ra lỗ hổng. Điều cần làm ở đây là khai thác và đưa ra kết quả trả về phù hợp với yêu cầu bài lab.
Khi nhìn vào bài lab thì cần chú ý là những nơi có thể nhập liệu. Và tiếp theo là đặt ra những câu hỏi: 
- Tại sao nơi đó xảy ra lỗ hổng ?
- Liệu còn cách nào khác để khai thác không ?
#### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm. Và câu truy vấn mỗi khi phân loại sản phẩm là: 
**SELECT * FROM products WHERE category = '?'**
Khi nhìn vào bài lab thì điều đầu tiên cần chú ý là nơi có thể nhập liệu. Thì ở đây, nơi nhập liệu là phân loại sản phẩm trên url. Theo suy nghĩ thì đây có thể là nơi xảy ra lỗ hổng.
Vì nơi đây cho phép sử dụng **'** nên tôi có thể thêm một số lệnh phổ biến đường sau **'** để kiểm tra lỗ hổng này.
Câu lệnh khai thác là: **' OR '1'='1** (Câu lệnh tuỳ theo mỗi trường hợp, có những trang web họ chặn thì mình cần tìm câu lệnh khai thác khác để phù hợp) 
Ở đây sử dụng **'** để bypass (việc dùng ký tự khác để bypass phụ thuộc vào trang web), **OR** là hoặc, **1=1** thì luôn đúng cho nên nó sẽ thực hiện câu lệnh **1=1**
*Payload:*  **/filter?category=?' OR '1'='1**
Sau khi thực hiện các câu truy vấn thì lỗ hổng đã xảy ra, liệt kê thành công và bài lab hoàn thành.
***Success***
#### Lab: SQL injection vulnerability allowing login bypass
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở phần đăng nhập tài khoản. Thì ở đây khi đăng nhập thì bài yêu cầu đăng nhập tài khoản **administrator**. Nhưng ở đây không hề có mật khẩu.
Theo suy nghĩ thì ta cần phải bypass được tài khoản **administrator** hoặc có cách nào mà không cần mật khẩu mà có thể vượt qua được cơ chế đăng nhập không.
Sau khi suy nghĩ và thử nghiệm thì tôi có thể đăng nhập thành công tài khoản administrator bằng cách thêm **' OR '1'='1** (Câu lệnh tuỳ theo mỗi trường hợp, có những trang web họ chặn thì mình cần tìm câu lệnh khai thác khác để phù hợp) vào sau phần username.
Ở đây sử dụng **'** để bypass (việc dùng ký tự khác để bypass phụ thuộc vào trang web), **OR** là hoặc, **1=1** thì luôn đúng cho nên nó sẽ thực hiện câu lệnh **1=1**
Sau khi thực hiện các câu truy vấn thì lỗ hổng đã xảy ra, đăng nhập thành công và bài lab hoàn thành.
*Payload:* **administrator' OR '1'='1**
***Success***
#### Lab: SQL injection attack, querying the database type and version on Oracle
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION để nối các chuỗi truy vấn đồng thời hiển thị phiên bản cơ sở dữ liệu.
Trước khi nói tới phần khai thác lỗ hổng của bài lab này thì chúng ta cần phải hiểu được: Oracle là gì ?
Các câu truy vấn của Oracle ?
Thì Oracle là một hệ thống cơ sở dữ liệu quan hệ hỗ trợ tất cả các tính năng cốt lõi của SQL. Các câu truy vấn của Oracle chỉ khác những mục chọn, các bảng và cũng tuỳ theo người phát triển xây dựng database.
Đề bài yêu cầu hiển thị ra phiên bản cơ sở dữ liệu nên tôi sẽ tìm kiếm các câu truy vấn liên quan đến phiên bản cơ sở dữ liệu của Oracle.
Trên PortSwigger có các cheat sheet thì tôi nhận thấy câu truy vấn hiển thị ra phiên bản cơ sở dữ liệu: **'UNION SELECT banner FROM v$version**
Nhưng khi tôi thực hiện thì trang web không hiển thị. Tiếp theo thì chúng ta cũng chưa chắc là trong bảng đó chỉ có mỗi cái **banner** nên chúng ta sẽ thêm **null** và câu lệnh là: **'UNION SELECT banner,null FROM v$version**. Đến đây vẫn còn thiếu, chúng ta nên thêm **--** là để trang web hiểu là đây là comment và có thể bypass thành công.
*Payload:* **/filter?category=?'UNION SELECT banner,null FROM v$version--**
Sau khi thực hiện các câu truy vấn thì lỗ hổng đã xảy ra, hiển thị thành công và bài lab hoàn thành.
***Success***

#### Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION để nối các chuỗi truy vấn đồng thời hiển thị phiên bản cơ sở dữ liệu.
Trước khi nói tới phần khai thác lỗ hổng của bài lab này thì chúng ta cần phải hiểu được:
MySQL, Microsoft là gì ?
Các câu truy vấn của MySQL, Microsoft ?
Nói chung quy thì hai cơ sở dữ liệu này gần như giống nhau. Và câu truy vấn để hiển thị phiên bản cơ sở dữ liệu là: **SELECT @@version**. Sau đó tôi sẽ thêm **null**, **--** đường sau câu **SELECT**, và câu truy vấn đầy đủ là: **' UNION SELECT @@version,null--**. Nhưng khi tôi sử dụng thì trang web không hiển thị, không chấp nhận câu truy vấn này.
Sau đó tôi đọc lại cheat sheet và kiểm tra thì thấy **%23** là phù hợp và có thể bypass thành công, **%23** đại diện cho ký tự **#** khi url encode.
Khi làm một bài bất kỳ trên thanh url thì chúng ta nên chuyển đổi sang url encode để có tỷ lệ bypass thành công cao hơn, nhiều khi cần phải chuyển đổi thì mới thành công bypass được.
*Payload:* **/filter?category=?' UNION SELECT @@version,null%23**
Sau khi thực hiện các câu truy vấn thì lỗ hổng đã xảy ra, hiển thị thành công và bài lab hoàn thành.
***Success***

#### Lab: SQL injection attack, listing the database contents on non-Oracle databases
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION để nối các chuỗi truy vấn đồng thời hiển thị các bảng dữ liệu, tiến hành trích xuất thông tin dữ liệu tài khoản **administrator** và đăng nhập.
Trước khi nói tới phần khai thác lỗ hổng của bài lab này thì chúng ta cần phải hiểu được: Non-Oracle là gì ?
Các câu truy vấn của Non-Oracle ?
Non-Oracle là một hệ thống cơ sở dữ liệu không thuộc Oracle.
Câu truy vấn của Non-Oracle khác phần bảng so với Oracle nhưng các loại SQL khác thì có phần giống nhau.
Khi làm một bài lab bất kỳ thì tôi sẽ tìm hiểu về cách nó hoạt động và các payload, cheat sheet trên mạng để tiến hành bypass nhanh chóng, đúng cách.
Ở đây sau khi tìm hiểu và đọc các cheat sheet thì tôi phát hiện và liệt kê thành công tất cả các bảng trong cơ sở dữ liệu này
*Payload 1:* **' UNION SELECT table_name,null FROM information_schema.tables--**
Câu truy vấn này sẽ liệt kê tất cả các bảng trong cơ sở dữ liệu. Chúng ta sẽ xem rằng cần lấy cái bảng nào để có thể xem được tài khoản **administrator**. Thì thường sẽ bảng có tên **users** sẽ là phần lưu tài khoản mật khẩu của một số tài khoản.
*Payload 2:* **' UNION SELECT column_name,null FROM information_schema.columns WHERE table_name='users_?'--**
Câu truy vấn này sẽ liệt kê ra các cột trong bảng, chúng ta sẽ thấy rằng tồn tại tài khoản **administrator**, và đồng thời là các nơi lưu trữ tài khoản mật khẩu.
*Payload 3:* **' UNION SELECT password_?,username_? FROM users_?--**
Câu truy vấn này sẽ liệt kê ra username, password của **administrator** sau khi mà tôi đã liệt kê các bảng, cột và gọi ra đúng với tài khoản **administrator**.
Sau khi thực hiện các câu truy vấn trên thì đăng nhập thành công **administrator** và bài lab hoàn thành.
***Success***

#### Lab: SQL injection attack, listing the database contents on Oracle
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION để nối các chuỗi truy vấn đồng thời hiển thị các bảng dữ liệu, tiến hành trích xuất thông tin dữ liệu tài khoản **administrator** và đăng nhập.
Trước khi nói tới phần khai thác lỗ hổng của bài lab này thì chúng ta cần phải hiểu được: Oracle là gì ?
Các câu truy vấn của Oracle ?
Oracle là một hệ quản trị cơ sở dữ liệu lớn được phát triển bởi công ty Oracle.
Câu truy vấn của Oracle với các loại SQL khác thì có phần giống nhau.
Khi làm một bài lab bất kỳ thì tôi sẽ tìm hiểu về cách nó hoạt động và các payload, cheat sheet trên mạng để tiến hành bypass nhanh chóng, đúng cách.
Ở đây sau khi tìm hiểu và đọc các cheat sheet thì tôi phát hiện và liệt kê thành công tất cả các bảng trong cơ sở dữ liệu này
*Payload 1:* **' UNION SELECT table_name,null FROM all_tables--**
Câu truy vấn này sẽ liệt kê tất cả các bảng trong cơ sở dữ liệu. Chúng ta sẽ xem rằng cần lấy cái bảng nào để có thể xem được tài khoản **administrator**. Thì thường sẽ bảng có tên **users** sẽ là phần lưu tài khoản mật khẩu của một số tài khoản.
*Payload 2:* **' UNION SELECT column_name, null FROM all_tab_columns WHERE table_name='USERS_?'--**
Câu truy vấn này sẽ liệt kê ra các cột trong bảng, chúng ta sẽ thấy rằng tồn tại tài khoản **administrator**, và đồng thời là các nơi lưu trữ tài khoản mật khẩu.
*Payload 3:* **' UNION SELECT USERNAME_?,PASSWORD_? FROM+USERS_?--**
Câu truy vấn này sẽ liệt kê ra username, password của **administrator** sau khi mà tôi đã liệt kê các bảng, cột và gọi ra đúng với tài khoản **administrator**.
Sau khi thực hiện các câu truy vấn trên thì đăng nhập thành công **administrator** và bài lab hoàn thành.
***Success***

#### Lab: SQL injection UNION attack, determining the number of columns returned by the query
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION xác định số cột hiện có trong database.
Ở đây chúng ta không biết bảng này chứa cái gì, dữ liệu gì. Chúng ta cần xác định số lượng cụ thể để có thể khai thác toàn bộ cơ sở dữ liệu.
Tôi có tham khảo cheat sheet và thử nghiệm thì thấy rằng có câu truy vấn này có thể xác định mà không cần biết nó chứa dữ liệu gì. Sử dụng **null** có thể bypass được qua cơ chế xác thực của trang web (có nhiều trang web sẽ chặn).
Sử dụng và thêm **null** cho đến khi xác định được số lượng cột, nhớ thêm ký tự comment.
*Payload:* **' UNION SELECT null,null,null--**
***Success***

#### Lab: SQL injection UNION attack, finding a column containing text
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION xác định dữ liệu hiện đang ở cột nào trong database.
Bài lab này về kỹ thuật lúc đầu thì tương tự bài trên. Chúng ta cần xác định số cột hiện có trong database. Dữ liệu cần tra cứu bài lab đã đưa ra, việc cần làm bây giờ là thay thế nó vào **null** cho đến khi ra được kết quả đúng.
*Payload:* **' UNION SELECT null,'?',null--**
***Success***
#### Lab: SQL injection UNION attack, retrieving data from other tables
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION xác định username, password của tài khoản **administrator**.
Để có thể hiển thị ra username, password thì sử dụng câu truy vấn sau:
*Payload:* **' UNION SELECT username,password FROM users--**
Sau khi thực hiện các câu truy vấn trên thì hiển thị ra username, password. Tiến hành đăng nhập thành công **administrator** và bài lab hoàn thành.
***Success***

#### Lab: SQL injection UNION attack, retrieving multiple values in a single column
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở danh mục phân loại sản phẩm và có thể sử dụng UNION, sử dụng UNION xác định username, password của tài khoản **administrator**.
Bài này hơi khác so với bài trên do nó lấy nhiều bảng khác nhau. Chúng ta cần tìm hiểu cách lấy nhiều giá trị các bảng khác với nhau.
Đầu tiên cần phải xác định nơi để dữ liệu thì ta sẽ sử dụng **null**
*Payload 1:* **' UNION SELECT null,'?'--**
Tôi có tham khảo cheat sheet và thử nghiệm thì thấy rằng có câu truy vấn này có thể bypass được thành công và lấy ra username, password của nhiều bảng khác nhau.
*Payload 2:* **' UNION SELECT null,username||'~'||password FROM users--**
Ký tự **||'~'||** là ký tự ghép các giá trị với nhau, **||** để nối chuỗi, **~** để tách chuỗi trên Oracle.
Sau khi thực hiện các câu truy vấn trên thì hiển thị ra username, password. Tiến hành đăng nhập thành công **administrator** và bài lab hoàn thành.
***Success***

#### Lab: Blind SQL injection with conditional responses
Đề bài đã cho gợi ý về lỗ hổng sqli nằm ở phần cookie. Ở đây chúng ta sẽ lợi dụng phần cookie để liệt kê ra username,password của tài khoản **administrator** và tiến hành đăng nhập.
Ở đây nếu câu truy vấn là đúng thì trang web trả về **Welcome back!** còn nếu câu truy vấn bị sai thì sẽ không hiển thị **Welcome back!**. Bài lab phần lý thuyết có đưa ra một số câu truy vấn hợp lệ để kiểm tra.
Đầu tiên tôi sử dụng câu truy vấn sau:
*Payload 1:* **Cookie: TrackingId' AND '1'='1**
Câu truy vấn này thêm phần **AND** có nghĩa là và **1=1**, mà **1=1** thì luôn đúng. Nếu **cookie** và câu truy vấn sau **AND** mà đúng thì câu **Welcome back!** hiển thị.
Sau khi thành công kiểm tra thì dưới đây là câu truy vấn để kiểm tra liệu kí tự này có tồn tại trong password **administrator** hay không.
Tiếp theo chúng ta cần phải biết được password này bao nhiêu ký tự. Tôi cần tìm câu truy vấn kiểm tra có bao nhiêu ký tự.
*Payload 3:* **Cookie: TrackingId' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>19)='a'**
Sử dụng câu truy vấn này và xác định được độ dài password là 20 ký tự
*Payload 3:* **Cookie: TrackingId' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) = 'a**
Có thể kiểm tra bằng cách đổi dấu **=** thành dấu **>**, **<** để xem ký tự đấy nằm ở vị trí nào trong bảng chữ cái.
**Substring** trong sql được hiểu là chuỗi con trong chuỗi lớn. Việc sử dụng truy vấn trên để kiểm tra liệu ký tự a có tồn tại trong chuỗi password **administrator** hay không.
Nhưng mà nếu dò như vậy thì rất mất thời gian, cho nên tôi sẽ sử dụng Burp Intruder để có thể tự động hoá việc tìm password **administrator**.
*Payload 4:* **Cookie: TrackingId' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), §1§, 1) = '§a§**
**§1§** cái này tôi sẽ cho chạy đến 20 vì độ dài chỉ đến 20 ký tự.
**§a§** cái này tôi sẽ set các ký tự, số (không set ký tự đặc biệt).
Ước tính khoảng 720 requests để có thể brute force thành công và có được password **administrator**. Reponse nào trả về có **Welcome back!** thì đấy là ký tự password đúng.
Sau khi brute force xong thì tôi sẽ tìm được đúng password **administrator**, tiến hành đăng nhập và bài lab hoàn thành.
***Success***

#### Lab: Blind SQL injection with conditional errors
Input cookie: 
Cookie: TrackingId' 
***Fail***
#### Lab: Visible error-based SQL injection
Input cookie: 
Cookie: TrackingId' 
***Fail***
#### Lab: Blind SQL injection with time delays
Theo tên bài và yêu cầu bài thì chúng ta cần để trang web delay 10s. Nếu câu truy vấn trong cookie đúng thì trang web cho ra kết quả bình thường còn nếu sai thì trang web sẽ ra lỗi 500.
Nhận ra đây là lỗ hổng Time-based-blind SQLi. Là lỗ hổng dựa trên thời gian trả về của phản hồi.
Đầu tiên cần nối chuỗi các câu truy vấn bằng cách sử dụng UNION. Tiếp theo tôi dùng các cheat sheet có sẵn trên PortSwigger thì thấy có một câu truy vấn thành công.
*Payload:* **Cookie: TrackingId' || pg_sleep(10)--**
Ở đây **||** là nối chuỗi và câu truy vấn còn lại là sử dụng trong hệ quản trị cơ sở dữ liệu **PostpreSQL** . Là truy vấn thời gian trả về của phản hồi.
***Success***

#### Lab: Blind SQL injection with time delays and information retrieval
Input cookie: 
Cookie: TrackingId' 
***Fail***
#### Lab: Blind SQL injection with out-of-band interaction
Input cookie: 
Cookie: TrackingId' 
***Fail***
#### Lab: Blind SQL injection with out-of-band data exfiltration
Input cookie: 
Cookie: TrackingId' 
***Fail***
#### Lab: SQL injection with filter bypass via XML encoding
Input cookie: 
Cookie: TrackingId' 
***Fail***

### Tham khảo

https://portswigger.net/web-security/sql-injection/cheat-sheet

https://tuananalytic.com/tu-hoc-sql-sao-lam-loai-sql-vay-phan-1/

https://tuananalytic.com/tu-hoc-sql-thuat-ngu-database-hay-gap-phan-2/

https://viblo.asia/p/sql-injection-la-gi-co-bao-nhieu-kieu-tan-cong-sql-injection-m68Z0QnMlkG




