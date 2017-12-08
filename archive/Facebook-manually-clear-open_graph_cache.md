# Facebook-manually-clear-open_graph_cache

### using Curl Post (Get 으로는 캐쉬 클린 안됩니다.)
```bash
$ curl -X POST -F "id=https://www.mymusictaste.com/campaign/%EC%83%A4%EC%9D%B4%EB%8B%88-SHINee-x-Jakarta-Indonesia,1344/" -F "scrape=true" https://graph.facebook.com
```

Start your cURL command with curl -X POST and then add -F for every field=value you want to add to the POST:

### using python built-in package urllib example
```python
import urllib
# import urllib2

# url = curl -X POST -F "id=https://www.mymusictaste.com/campaign/%EC%83%A4%EC%9D%B4%EB%8B%88-SHINee-x-Jakarta-Indonesia,1344/" -F "scrape=true" https://graph.facebook.com
url = "https://graph.facebook.com"
param_id = "https://www.mymusictaste.com/campaign/%EC%83%A4%EC%9D%B4%EB%8B%88-SHINee-x-Jakarta-Indonesia,1344/"
is_scrape = "true"
values = {
    'id': param_id,
    'scrape': is_scrape,
}

res = urllib.urlopen(url, urllib.urlencode(values))
print res.read()

# req = urllib2.Request(url, data=urllib.urlencode(values))
# res = urllib2.urlopen(req, timeout = 10)
```


### References
* http://stackoverflow.com/questions/12100574/is-there-an-api-to-force-facebook-to-scrape-a-page-again
* https://developers.facebook.com/docs/sharing/opengraph/using-objects
* https://docs.python.org/2/library/urllib.html#example