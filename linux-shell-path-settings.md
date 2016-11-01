```bash
$ sudo apt-get update
$ sudo apt-get install nodejs
$ sudo apt-get install npm
```

### apt-get command list
```bash
$ sudo apt-get install npm #install package
$ sudo apt-get remove <pacakge_name> #remove package
$ sudo apt-cache search <pacakge_name> #searching pacakge 
$ sudo apt-cache show <pacakge_name>  #package info
```



.bashrc

```shell
export APIDOC_HOME=~/home/lib/apidoc/bin
PATH=$PATH:$APIDOC_HOME
```

```bash
$ export #show list of environment variable
$ whereis node #show where <file_name> is
node: /usr/sbin/node /usr/share/man/man8/node.8.gz

```

### npm uninstall gloabal package
```
$ sudo npm uninstall -g apidoc

/usr/local/bin/apidoc -> /usr/local/lib/node_modules/apidoc/bin/apidoc
```

### npm install gloabal package
```
$ sudo npm install apidoc -g 
/usr/local/bin/apidoc -> /usr/local/lib/node_modules/apidoc/bin/apidoc
```


## References
* [Ubuntu apt-get command list](https://blog.outsider.ne.kr/346)
* [Ubuntu-1404-에-NodeJS-설치-및-nodejs-버전-변경용-n-설치-및-사용법](http://www.tutorialbook.co.kr/entry/Ubuntu-1404-%EC%97%90-NodeJS-%EC%84%A4%EC%B9%98-%EB%B0%8F-nodejs-%EB%B2%84%EC%A0%84-%EB%B3%80%EA%B2%BD%EC%9A%A9-n-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)