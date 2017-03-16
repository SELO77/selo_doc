# telnet 으로 이해하는 HTTP 기본

```bash
$ telnet www.joes-hardware.com 80
Trying 128.121.66.211...
Connected to joes-hardware.com.
Escape character is '^]'.
GET /tools.html HTTP/1.1
Host: www.joes-hardware.com

HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 09:00:09 GMT
Server: Apache/2.2.22 (Unix) DAV/2 FrontPage/5.0.2.2635 mod_ssl/2.2.22 OpenSSL/1.0.1h
Last-Modified: Fri, 12 Jul 2002 07:50:17 GMT
ETag: "146deb7-1b1-3a58f649c4040"
Accept-Ranges: bytes
Content-Length: 433
Content-Type: text/html

<HTML>

<HEAD><TITLE>Joe's Tools</TITLE></HEAD>

<BODY>

<H1>Tools Page</H1>

<H2>Hammers</H2>

<P>Joe's Hardware Online has the largest selection of
<A HREF="./hammers.html">hammers</A> on the earth.</P>

<H2><A NAME=drills></A>Drills</H2>

<P>Joe's Hardware has a complete line of cordless and corded drills,
as well as the latest in plutonium-powered atomic drills, for those
big around the house jobs.</P> ...

</BODY>

</HTML>
```