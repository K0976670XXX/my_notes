###### tags: `python_函式庫`

# flask函式庫:

> [name=Luoshiyong]
> [time=Fri, Jul 8, 2022 3:51 PM]
## flask簡介:
&emsp;&emsp;**Flask**是一個使用編寫的輕量級。基於Werkzeug [WSGI(Web伺服器閘道器介面)](https://zh.wikipedia.org/wiki/WSGI "WSGI")工具箱 和 [Jinja2](https://zh.wikipedia.org/wiki/Jinja2 "Jinja2")[模板引擎](https://zh.wikipedia.org/wiki/%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E "模板引擎")。Flask使用BSD授權。

Flask被稱為「微框架」，因為它使用簡單的核心，用擴充增加其他功能。Flask沒有預設使用的資料庫、表單驗證工具。然而，Flask保留了擴增的彈性，可以用Flask-extension[\[2\]](https://zh.wikipedia.org/zh-tw/Flask#cite_note-2)加入這些功能：[ORM](https://zh.wikipedia.org/wiki/%E5%B0%8D%E8%B1%A1%E9%97%9C%E4%BF%82%E6%98%A0%E5%B0%84 "物件關係對映")、表單驗證工具、檔案上傳、各種開放式身分驗證技術。

因此只需要簡單的幾行代碼就能簡單的做到網路服務的功能。

flask開放原始碼庫:<https://github.com/pallets/flask>
函式庫的安裝:**pip install Flask**
## 函式的使用:

### flask基本用法:
&emsp;&emsp;以下程式碼為一個flask程式**最少所需要的內容**

```python=
from flask import Flask   # 載入 Flask

#初始化宣告:__name__表示當前執行執行的程式
#該段表示，將客戶端的所有請求都交由這個程式去執行
app = Flask(__name__)

#定義路由(網路資料傳遞的路徑):依據客戶端發出的請求，判斷要將資料送往哪個路由，並用@app.route去建立路由，故可以定義多個路由
@app.route("/r1")           #建立路由，可針對主網域 / 發出請求
def f1():                   #發出請求後會執行 f1() 的函式
    return "hello world"    #執行函式後會回傳特定的內容

@app.route("/r2")           #建立第2個路由
def f2():                   #函式名字不能相同
    return "goodbye world"  #回傳

@app.route("/<string:msg>")        #建立第3個路由路由為使用者寫得
def f3(msg):                   #函式名字不能相同
    return '該路經'+msg+'錯誤'  #回傳
app.run()                   # 執行
```
&emsp;&emsp;**執行結果:**
![](https://i.imgur.com/q5nSnCt.png)

**看到之後不要關閉**，將http://127.0.0.1:5000複製到瀏覽器上，會變成這樣是正常的

![](https://i.imgur.com/GW1S6m9.png)

**再來在網址後面加上程式中定義的路由**http://127.0.0.1:5000/r1
![](https://i.imgur.com/pv9yYHw.png)
**另一個路由**http://127.0.0.1:5000/r2
![](https://i.imgur.com/SXGriaM.png)
**自訂義的路徑:
![](https://i.imgur.com/nfmqmcL.png)

#### &emsp;&emsp;**補充資料:**
&emsp;&emsp;上面的最後一條說到可以自訂義變數，flask提供了5種基本的變數轉換，稍微改一下就好，p.s:string為預設的可以選擇不要打。
| 類型 | 說明 |
| --- | --- |
| string | 預設值，表示不包含斜線的文字。 |
| int | 正整數。 |
| float | 正浮點數。 |
| path | 路徑，類似 string，但可以包含斜線。 |
| uuid | UUID 字串。 |



### GET和POST:
&emsp;&emsp;GET和POST的差別在於:
&emsp;&emsp;&emsp;&emsp;使用**GET**傳遞訊息時資料以**網址**做傳送，所以資料可以在網址上顯示，其中route在沒有做設定時預設為GET模式，如上面程式的效果
&emsp;&emsp;&emsp;&emsp;使用**POST**傳遞訊息時資料以**message-body**做傳送，所以資料不會在網頁上呈現。
&emsp;&emsp;使用:
```python=
from flask import Flask
app = Flask(__name__)
@app.route("/r1")
def f1():                   
    return "hello world"
@app.route("/r2",methods=['POST'])
def f2():
    return "goodbye world"
app.run()
```
直接貼上網址:顯示錯誤
![](https://i.imgur.com/aSMK6Ln.png)
使用requests函式庫的POST方法:**上面的程式要開著**
```python=
import requests#使用requests函式庫
web = requests.post('http://127.0.0.1:5000/r2')  # 使用 post 方法
print(web.text)    # 讀取並印出 text 屬性
```
輸出:成功取得資料
![](https://i.imgur.com/176yew5.png)

### 定義port和host:
&emsp;&emsp;使用:因為前面的方法僅能用於當前電腦中使用，還不能對外使用該程式，故加上host並將其定義為0.0.0.0便能供**區域網路**內的所有設備使用，還不能連入外網，除非你有[申請專屬的IP位置](https://ipapply.twnic.tw/)。
```python=
from flask import Flask
app = Flask(__name__)
@app.route("/")
def home():
    return "hello world"
#host設為0.0.0.0表示以本機的IP位址為地址，post為溝資料溝通的專屬通道
app.run(host="0.0.0.0", port=5555)
```
&emsp;&emsp;**輸出:**
![](https://i.imgur.com/bKJxVBy.png)
&emsp;&emsp;**此為手機連入區域網路的畫面:**
![](https://i.imgur.com/7BC98v3.png)

### 讀取參數:
&emsp;&emsp;當route使用GET時，request.args 會將網址的參數讀取為 tuple 格式，tuple 第一個項目為參數，第二個項目為值，如下
```python=
from flask import Flask, request   # 載入了 request
app = Flask(__name__)
@app.route("/")
def home():
    print(request.args)            # 使用 request.args
    return "<h1><center>hello world</h1>"
    #<h1></h1>為html語法的標題，<center>為置中對齊
app.run()
```
&emsp;&emsp;在網址路由後面加上?age=20&name=luoshiyong，程式就會接收到對應tuple形式的資料。
![](https://i.imgur.com/hYTOi86.png)

如上同理，若使用者以POST發送訊息也可以:
接收端:
```python=
from flask import Flask, request
app = Flask(__name__)
@app.route("/",methods=['POST'])
def home():
    print(request.form)            # 使用 request.form
    return "hello world"
app.run()
```
發送端:
```python=
import requests
data = {'age': '20','name': 'luoshiyong'}
web = requests.post('http://127.0.0.1:5000/', data=data)   # 發送 POST 請求
print(web.text)
```

### 使用網頁模板(html檔)
&emsp;&emsp;利用render_template方法便可做到依據網頁模板，快速更改網頁資料，讓網頁的html不是寫死的。[可用樣板語法](https://jinja.palletsprojects.com/en/3.0.x/templates/)
html:一般html不能這樣寫，**由於flask的預設檔案路徑為Templates**故此檔須放在Templates的資料夾中。
```html=
<!DOCTYPE html>
<html>
    <head>
        <title>test</title>
    </head>
    <body>
        {% if name %}
            <h1>Hello {{ name }}!</h1>
        {% else %}
            <h1>Hello, World!</h1>
        {% endif %}
    </body>
</html>
```
python檔寫法:
```python
from flask import Flask,request,render_template
app=Flask(__name__)
@app.route('/r')
def f0():
    name = request.args.get('name')
    return render_template('01.html',name=name)
app.run()
```
輸出:
![](https://i.imgur.com/8WNuFyY.png)

python檔改造寫法:因為Flask檔案預設路徑為Templates，稍微有點麻煩，故為了改變該路徑程式可以改寫成這樣:
```python=
from flask import Flask,request,render_template
#將flask的預設檔案路徑設為根目錄(當前檔案目錄)
app=Flask(__name__,template_folder='./')
@app.route('/r')
def f0():
    name = request.args.get('name')
    return render_template('01.html',name=name)
app.run()
```

## flask連入外網方法ngrok:
&emsp;&emsp;通常情況下不會有專屬IP位址，這種情況下想要對外網公開，做到將電腦做為伺服器可以利用ngrok(<https://ngrok.com/>)，可以利用他將電腦連入外網。
用法:
&emsp;&emsp;1.登入後先依據電腦下載檔案
![](https://i.imgur.com/BqI4UBE.png)
&emsp;&emsp;2.將檔案解壓縮，會看到一個ngrok.exe，點他並執行，執行後回到網頁找token。
![](https://i.imgur.com/3klv37X.png)
&emsp;&emsp;3.直接複製他貼到CMD然後按enter，畫面會變成這樣
![](https://i.imgur.com/0WXp2OS.png)
&emsp;&emsp;4.輸入**ngrok http 80**，80是port，因為port80通常用於傳輸網頁的，詳細情況看這[**TCP/UDP端口列表**](https://zh.wikipedia.org/wiki/TCP/UDP端口列表)，若想要自訂port需先確認該port是否已被占用，cmd指令:**netstat -ano|find "80"**或直接用**netstat -na**顯示全部正在使用的。
輸入後會出現這個畫面:
![](https://i.imgur.com/z0Di6ww.png)
&emsp;&emsp;5.這樣伺服器就啟用了，哪個Forwarding後面的網址即為外網連入的網址，該網址每次重開皆不同，若要固定的網址需要付費。

### 使用ngrok架設簡易網頁服務:
python程式瑪:直接沿用前面的檔案
```python=
from flask import Flask,request,render_template
app=Flask(__name__,template_folder='./')
@app.route('/r')
def f0():
    name = request.args.get('name')
    return render_template('01.html',name=name)
#由於前面ngrok設定的port為80，故這裡需要將flask程式的port設成80
#若前面設定為5000，則可以不用設定port
#debug=True，用於偵錯，當設為True時可以不用一直重開.py檔，保持cmd介面直接去更改.py檔的內容
app.run(host='0.0.0.0', port=80,debug=True)
```
此為手機用行動網路時的畫面，表示已成功連上外網
![](https://i.imgur.com/WvvY3TO.jpg)
可以利用**127.0.0.1:4040**去檢查當前伺服器狀態。
![](https://i.imgur.com/SU3TWt8.png)

## 應用:---待補