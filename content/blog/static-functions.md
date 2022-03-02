+++
title = "Static Functions in C"
description = "해당 C 파일 안에서만 사용되도록 제한"
date = 2022-03-02T07:00:00+09:00
updated = 2022-03-02T00:00:00+09:00
template = "blog/page.html"
draft = false

[taxonomies]
authors = ["Seongsu Park"]

[extra]
lead = '해당 C 파일 안에서만 사용되도록 제한'
images = []
tags = ["static", "c", "42Seoul"]
toc = true
+++

규모가 큰 함수는 여러 하위 함수로 기능을 나눌 수가 있는데, 하위 함수가 불필요하게 라이브러리에 배포되는 것을 막기 위한 방법으로 `static` 키워드를 사용할 수 있다.

## 정적함수(Static functions)

정적함수는 선언된 파일 안에서만 사용할 수 있도록 접근 권한이 제한된다.[^스태틱참고]

따라서 아래처럼 정적함수로 정의된 함수를 다른 c 파일에서 사용할 수 없다.

```c
/* Inside file1.c */ 
static void fun1(void)
{
  puts("fun1 called");
}
```

```c
/* Inside file2.c  */ 
int main(void)
{
  fun1(); // -> ERROR: undefined reference to 'fun1'
  getchar();
  return 0;  
}
```


## 참고문헌

[^스태틱참고] [Static functions in C | GeeksforGeeks](https://www.geeksforgeeks.org/what-are-static-functions-in-c/)