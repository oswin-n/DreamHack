# Return Address Overwrite

## 2026-09-17

## 취약점
스택 버퍼오버플로우 공격

rao.c는 입력값을 ```scanf```로 받기 떄문에 입력값 길이 체크를 안한다. buf 크기가 0x28이고, SFP 크기가 0x08이므로 0x30크기 이상의 값을 입력하면 버퍼 오버플로우를 일으킬 수 있다.

```get_shell```의 메모리 주소는 ```0x4006aa```임을 gdb로 확인할 수 있다. 

익스플로잇 코드로 쉘을 탈취할 수 있다.
```shell 
cat flag.txt
```
로 플래그를 출력할 수 있다.

## flag
DH{5f47cd0e441bdc6ce8bf6b8a3a0608dc}