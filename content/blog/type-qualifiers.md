+++
title = "Type Qualifiers of C99"
description = "역할을 제한하기 위한 용도로 사용"
date = 2022-03-02T07:00:00+09:00
updated = 2022-03-02T00:00:00+09:00
template = "blog/page.html"
draft = false

[taxonomies]
authors = ["Seongsu Park"]

[extra]
lead = '역할을 제한하기 위한 용도로 사용'
images = []
tags = ["qualifier", "c", "42Seoul"]
toc = true
+++

오늘 42Seoul 본과정의 가장 첫 프로젝트인 `libft`를 등록했다.

만들어야 할 함수들의 프로토타입을 확인하다가, 참고사항에

> 특정 함수의 프로토타입에는 `restrict` 한정자를 사용하세요.

라고 의외로 친절한(?) 팁이 적혀있었는데, 저 키워드가 어떤 역할을 하는지 전혀 알지 못했다.

그래서 찾아 보았다.

## <cite>한정자(Type qualifiers)[^qualifier]</cite>

영어로는 자격을 주는 것 같은데, 한글로는 뭔가를 제한하는 느낌이다.

왜 이런 이름이 지어졌을까? 한정자의 역할 때문이다.

먼저, 한정자는 뭔가를 **제한**하기 위해서 사용한다.

뭘? 익숙한 `const` 한정자부터 보면 


1. `const`: 데이터의 변경권을 제한한다.
2. `volatile`: 컴파일러의 최적화를 제한한다.
3. `restrict`: 데이터에 접근 할 수있는 변수를 제한한다.

## <cite>[`restrict`](https://dojang.io/mod/page/view.php?id=760)[^restrict]</cite>

`restrict` 키워드가 있다면 `a`, `b`, `x`가 모두 다르다는 것을 명시하는 것이다. (하지만 세 변수가 같은 곳을 가리킨다고 컴파일 오류가 나지 않기 때문에 사람이 일일이 확인해줘야 한다.) 포인터가 가리키는 곳이 모두 다르다면 아래와 같은 함수에서 각 명령의 순서는 바뀌어도 상관없다. 과연 빌드했을 때 프로그램은 우리가 의도한 대로 동작할까?

```c
void increase(int *restrict a, int *restrict b, int *restrict x)
{
    *a += *x;
    *b *= *x;
}
```
```c
int v = 5;
increase(&v, &v, &v);
```
```shell
$ gcc -std=c99 -O3 -c increase.c
$ ./increase
50
```

놀랍다. `5 + 5 = 10` 하고 난 뒤 `10 * 10 = 100` 이어야 하는데 결과는 `50` 이다. 아무래도 컴파일러 최적화로 인해서 `5 * 5 = 25`, `25 + 25 = 50` 로 순서가 바뀐 것 같다.

그렇다면 `restrict` 키워드를 제거하고 빌드 하면 어떻게 될까?

```c
void increase(int * a, int * b, int * x)
{
    *a += *x;
    *b *= *x;
}
```
```c
int v = 5;
increase(&v, &v, &v);
```
```shell
$ gcc -std=c99 -O3 -c increase.c
$ ./increase
100
```

우리가 의도한 대로 결과가 나왔다.

`restrict` 키워드를 사용할 때 정말 해당 키워드의 데이터 접근 권한이 제한되었는지 확인할 필요가 있을 것 같다. 이를 하지 않는다면 위와 같은 오류가 생길 수도 있으니 사용에 주의하자.

## 참고문헌

[^qualifier]: [12.18 자료형 한정자들(Type Qualifiers)-const, volatile, restrict | cuore J](https://m.blog.naver.com/cuorej/221688482474)

[^restrict]: [85.16 restrict 포인터 | 코딩도장](https://dojang.io/mod/page/view.php?id=760)
