# apidoc-tutorial

### install apidoc
```bash
$ npm install apidoc -g
```

### make doc file
js, python, ruby 등 다양한 형태의 언어 set 지원.

**python sample**

```python
"""
@api {get} /user/:id Request User information
@apiName GetUser
@apiGroup User

@apiParam {Number} id Users unique ID.

@apiSuccess {String} firstname Firstname of the User.
@apiSuccess {String} lastname  Lastname of the User.
"""
```

### make apidoc html
```bash
$ apidoc -i <input_apidoc_dirname> -o <output_dirname>
```

## Reference
* [apidocjs](http://apidocjs.com/)