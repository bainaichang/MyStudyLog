处理get请求

```py
@app.route('url', methods=['GET'])
```

处理post请求

```python
@app.route('url', methods=['POST'])
```

get接受参数

```py
@app.route('/login/', methods=['GET'])
def login():
    name = request.values.get("name")
    age = request.values.get("age")
    return "hello!" + name + "," + age

```

post接受参数

```py
@app.route('/login/', methods=['POST'])
def login():
    name = request.values.get("name")
    age = request.values.get("age")
    #或者
    name = request.form.get("name")
    age = request.form.get("age")
    return "hello!" + name + "," + age
```

通过路径获取参数

```py
@app.route('/login/<name>/<int:age>', methods=['GET'])
def login(name="defoultName", age=0):
    return "hello!" + name + "," + age
```

第二种路径映射

```py
app.add_url_rule(rule='url', view_func=func)
```

重定向

```py
def red():
    return redirect("url")
```

如果form提交文本

```
request.stream.read().decode('utf-8')
```

通过Header获取数据

```py
request.headers.get("key")
```

如果form提交附件和文本

上传

html示例

```html
<form id="form1" method="post" action="/upload" enctype="multipart/form-data">
    <div>
        <input type="text" name="filename">
        <input id="File1" type="file" name="myfile"/>
        <input type="submit">submit</input>
    </div>
</form>
```



```py
@app.route('/upload', methods=['POST'],strict_slashes=False)
def api_upload():
    file_dir=os.path.join(basedir,app.config['IPLOAD_FOLDER'])
    if not os.path.existe(file_dir):
        os.makerdirs(file_dir)
    f = request.files['myfile'] #f是文件, key为表单中的name
    if f and allowed_file(f.filename):
        fname=secure_filename(f.filename)
        print(fname)
        ext=fname.resplit('.',1)[1]#获取文件后缀
        f.save(os.path.join(file_fir,new_filename))
        #拿text还是通过form.get("key")
```

下载

```py
@app.route('/download/<filename>')
def download(filename):
    if request.method=="GET":
        #upload是文件夹名,as...为false时一般是图片, send_file....
        return send_from_directory('upload',filename,as_attachment=True)
```

Response

```py
resp = make_response('delete cookie')
剩下的看源码
```

设置加密秘钥

```
app.secret_key="sadafvsvaer"
```

添加cookie

```py
resp = make_response('xxx')
outdate=datetime.datetime.today()+datetime.timedelta(days=30)
resp.set_cookie('key','value',expires=过期时间)
```

拿cookie

```python
value=request.cookies.get('key')
```

代理

```py
@app.after_request
@app.before_request
```

