# B4 rev-basic-0

# 날짜
2026-09-18

# 취약점 
Ida에서 ```F5```를 눌러 프로그램을 디컴파일하면 ```Input: ```의 입력값에 따라 ```Correct```, ```Wrong```을 출력하는 것을 볼 수 있다. 

```main```의 내용을 살펴보면 입력값을 받아 프로그램에 저장된 값과 비교하는 ```sub_140001000```함수를 찾을 수 있다.
```C
if ( (unsigned int)sub_140001000(a1: v4) != 0 )
``` 

```sub_140001000```함수 내부는 다음과 같다.
```C
_BOOL8 __fastcall sub_140001000(const char *a1)
{
  return strcmp(Str1: a1, Str2: "Compar3_the_str1ng") == 0;
}
```

따라서 입력값이 ```Compar3_the_str1ng```와 동일하면 ```Correct```가 출력됨을 알 수 있다.


## Flag
DH{Compar3_the_str1ng}