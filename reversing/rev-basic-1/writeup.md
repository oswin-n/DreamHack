# B4 rev-basic-1
# 날짜
2026-09-18

# 취약점
Ida로 프로그램을 디컴파일하면 ```main```함수 내에 입력값을 받아 문자열을 아스키코드로 변환하는 함수를 찾을 수 있다. 

그 다음에 오는 ```sub_7FF7E2361000```함수는 아스키코드가 담긴 배열을 for루프를 돌며 한 칸씩 비교해서 value 하나라도 원하는 값과 일치하지 않으면 ```false```를 반환한다.

```C
_BOOL8 __fastcall sub_7FF7E2361000(_BYTE *a1)
{
  if ( *a1 != 67 )
    return false;
  if ( a1[1] != 111 )
    return false;
  if ( a1[2] != 109 )
    return false;
  if ( a1[3] != 112 )
    return false;
  if ( a1[4] != 97 )
    return false;
  if ( a1[5] != 114 )
    return false;
  if ( a1[6] != 51 )
    return false;
  if ( a1[7] != 95 )
    return false;
  if ( a1[8] != 116 )
    return false;
  if ( a1[9] != 104 )
    return false;
  if ( a1[10] != 101 )
    return false;
  if ( a1[11] != 95 )
    return false;
  if ( a1[12] != 99 )
    return false;
  if ( a1[13] != 104 )
    return false;
  if ( a1[14] != 52 )
    return false;
  if ( a1[15] != 114 )
    return false;
  if ( a1[16] != 97 )
    return false;
  if ( a1[17] != 99 )
    return false;
  if ( a1[18] != 116 )
    return false;
  if ( a1[19] != 51 )
    return false;
  if ( a1[20] == 114 )
    return a1[21] == 0;
  return false;
}
```
아스키코드를 문자로 변환하면 ```Compar3_the_ch4ract3r```가 된다.

# Flag
DH{Compar3_the_ch4ract3r}