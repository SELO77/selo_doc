# jquery file upload and save it on django

### Front with jquery
```javascript
var formData = new FormData();
formData.append('image', $('#id_image')[0].files[0]);

$.ajax({
    url: URL_BOOK.FILEUPLOAD_API,
    type: "POST",
    processData: false,
    contentType: false,
    data: formData,
    success: function (data) {
        console.log("success file upload");
    }
})
```

**processData / boolean** <br/>
data 지정한 개체를 쿼리 문자열로 변환할지 여부를 설정합니다. 기본값은 true로, 자동으로 "application / x - www - form - urlencoded"형식으로 변환합니다. DOMDocument 자체 등의 다른 형식으로 데이터를 보내기 위하여 자동 변환하고 싶지 않은 경우는 false를 지정합니다.

**contentType / string** <br/>
서버에 데이터를 보낼 때 사용 content - type 헤더의 값입니다. 기본값은 "application / x - www - form - urlencoded"대부분의 경우이 설정으로 문제 없을 것입니다.

### Back with django rest framework
```python
class FileUploadView(APIView):
    authentication_classes = (SessionAuthentication,)
    permission_classes = (IsAuthenticated, IsAdminUser)
    # parser_classes = (FileUploadParser,)

    def handle_upload_file(self, f):
        import os
        print os.path.abspath(f._name)


        TEMPORARY_FILE_SAVE_PATH = ''
        with open(TEMPORARY_FILE_SAVE_PATH + f._name, 'wb+') as d:
            for chunk in f.chunks():
                d.write(chunk)

    def put(self, request):
        # file_obj = request.data['file']
        binary_file = request.data['image']
        self.handle_upload_file(binary_file)

        return Response(status.HTTP_204_NO_CONTENT)
```


[stackoverflow : how-to-send-formdata-objects-with-ajax-requests-in-jquery](http://stackoverflow.com/questions/6974684/how-to-send-formdata-objects-with-ajax-requests-in-jquery)

[Django Upload Handlers](https://docs.djangoproject.com/en/dev/topics/http/file-uploads/#upload-handlers)

[Django image upload | Cloudinary](http://cloudinary.com/documentation/django_image_upload#direct_uploading_from_the_browser)


[What Python file option 'wb' mean?](http://stackoverflow.com/questions/2665866/what-is-the-wb-mean-in-this-code-using-python)

