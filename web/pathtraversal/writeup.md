# B4 pathtraversal

# 날짜
2026-09-21

# 코드 분석
```python
@app.route('/get_info', methods=['GET', 'POST'])
def get_info():
    if request.method == 'GET':
        return render_template('get_info.html')
    elif request.method == 'POST':
        userid = request.form.get('userid', '')
        info = requests.get(f'{API_HOST}/api/user/{userid}').text
        return render_template('get_info.html', info=info)
```
```get_info```함수는 사용자로부터 userid를 입력 받고, ```/api/user/{userid}``` 경로에서 ```info```에 저장된 해당 userid에 대한 정보를 출력한다. 

# 취약점
## 실패
flag는 ```/api/flag```에 있으므로 ```/get_info``` 엔드포인트에서 ../flag 를 입력했으나 실패함. 

```python
def get_flag(uid):
    try:
        info = users[uid]
    except:
        info = {}
    return json.dumps(info)
```
파라미터로 전달하면 안되고, ```users[uid]```값을 바꿔야함

## 성공
```/get_info```엔드포인트에서 F12로 개발자 도구를 연다. console에서 
```users``` 를 입력하면 ```{guest: 0, admin: 1}```이라는 userid:uid 형태로 유저 정보가 출력된다. 

```python
users[guest]='../flag'
```
로 userid값을 변조한다.

그러면 화면에 user정보 대신 flag가 출력된다.

# flag
DH{8a33bb6fe0a37522bdc8adb65116b2d4}