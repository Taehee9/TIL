# 메모리 영역

프로세스, 쓰레드를 공부하면서 봤던 code, data, heap, stack 영역인 메모리 영역에 대해 정리

추가로 BSS(Block Stated Symbol) 이 있음

## Code 영역

- text 부분
- 작성한 코드 ( 기계어 포함 )
- read only and fixed
- 프로세스 종료될 때까지 유지
- 객체 파일의 부분이나 프로그램의 실행 가능한 명령어를 저장하는 주소공간을 담당하는 부분

## Data 영역

- 전역변수(global), 정적변수(static), 배열(array), 구조체(structure) 등 저장 → 초기값 있고 수정 가능한 것들
- 프로그램 실행 될 때 생성, 종료 시 반환
- 함수 내 static 변수는 프로그램 실행시 공간만 할당, 함수 실행될 때 초기화
- 처음에는 code 영역에 저장되고 프로그램 시작동안 data 영역에 복사된다.

ex ) int val = 3;

## BSS 영역

- 초기화 되지 않은 데이터 영역
- 0으로 초기화되거나 코드에서 명시적으로 초기화되지 않은 모든 전역변수와정적변수

ex ) static int i;

## Stack 영역

- 지역변수, 매개변수, 리턴값 저장
- 임시 메모리 형태
- 함수 호출 시 생성, 종료 시 반환
- 스택 포인터와 힙 포인터가 만났다면 메모리가 없다는 것

## Heap 영역

- 동적으로 사용
- malloc, free, new, delete로 할당 및 반환
- Java의 경우엔 자동으로 반환  
Java의 메모리 구조는 다시 정리
- 모든 쓰레드와 공유하고(같은 프로세스 안) 동적으로 로드된 모듈들과 라이브러리들을 공유한다.

참고  
[Data segment](https://en.wikipedia.org/wiki/Data_segment)  
[메모리 영역(Code, Data, Heap, Stack)](https://box0830.tistory.com/150)
