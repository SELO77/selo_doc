# Jquery pagination usage

### Sample code

**페이지 최초 요청시 서버에서 totalPages 값을  요청 받아서 세팅하면 페이지네비게이션 자동 생성.**


```javascript
function setPagenation() {
            console.log(totalPages);
            $('.sync-pagination').twbsPagination({
                totalPages: totalPages,
                onPageClick: function (evt, page) {
                    console.log("pagination evt", evt);
                    console.log("pagination page", page);
                    getArtistYoutube("", page);
                }
            });
        }
```

### CDN
```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/twbs-pagination/1.4.1/jquery.twbsPagination.min.js"></script>
```


### References
* [jquery pagination library api](https://esimakin.github.io/twbs-pagination/)
