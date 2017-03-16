# MMT SCRAPY TUTORIAL DAY

* [virtualenv](https://virtualenv.pypa.io/en/stable/): 가상환경 세팅 툴
* [pyenv](http://kwonnam.pe.kr/wiki/python/pyenv): system에 다양한 python 버전으로 손쉽게 스위칭 가능 


### install scrapy
```bash
$ pip install Scrapy
```

### create scrapy project
```bash
$ scrapy startproject tutorial
$ ls
tutorial
$ cd tutorial
$ ls
scrapy.cfg tutorial
$ tree
.
├── scrapy.cfg
└── tutorial
    ├── __init__.py
    ├── items.py
    ├── pipelines.py
    ├── settings.py
    └── spiders
        └── __init__.py

```



### References
* [Scrapy Official Tutorial](https://doc.scrapy.org/en/latest/intro/tutorial.html#creating-a-project)