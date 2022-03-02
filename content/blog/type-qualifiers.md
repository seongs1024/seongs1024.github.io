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

## 참고문헌

[^qualifier]: [12.18 자료형 한정자들(Type Qualifiers)-const, volatile, restrict | cuore J](https://m.blog.naver.com/cuorej/221688482474)