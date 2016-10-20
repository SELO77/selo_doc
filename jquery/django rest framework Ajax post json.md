# django rest framework Ajax post json


### Convert javascript object to string json
```javascript
$.ajax({
                type: "POST",
                url: URL_BOOK.EVENT_API,
                dataType: 'json',
                data: {
                    param: JSON.stringify({
                        event: event,
                        eventLineup: eventLineup,
                        ticketTypes: ticketTypes,
                        promotion: {
                            id: $('#id_promotion').val(),
                            type: selected_promotion_type
                        }
                    })
                },
                success: function (data) {

                },
                error: function (e, status) {
                    console.log(e)
                }
            });
```

### string to dict by json.loads()

```python
data = request.DATA.copy()
param = json.loads(data['param'])
```