## Ubuntu16.04.1-LTS-에서 nginx 셋팅


```bash
/etc/nginx$
/etc/nginx$ vim nginx.conf
```


### nignx virtual hosting setting example

기존 nginx 설정에 아래와 같이 호스팅할 서버를 추가 설정합니다.
기존의 설정에 아래 내용을 추가하시면 됩니다. 가상호스팅 설정 부분의 소스는 다음과 같습니다.

```
server {
        listen 8888; # 저는 기존에 8080 포트를 사용하였으므로, 새로운 호스팅은 8888포트와 연결합니다.

        server_name <host_name>;

        root /home/selo/works/mmt_mk2-apidoc/html;
        index index.html;

        location / {
                try_files $uri $uri/ =404;
        }
}
```


### nginx log 확인

```bash
$ sudo tail -f /var/log/nginx/error.log
2016/11/01 16:03:08 [notice] 931#931: signal process started
$ sudo tail -f /var/log/nginx/access.log
```

## References
* [nginx virtual host settings](http://webdir.tistory.com/241)

