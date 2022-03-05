+++
title = "Will type casting impact on the execution performance?"
description = "Assembly says No"
date = 2022-03-05T07:00:00+09:00
updated = 2022-03-05T00:00:00+09:00
template = "blog/page.html"
draft = false

[taxonomies]
authors = ["Seongsu Park"]

[extra]
lead = "Assembly says No"
images = []
tags = ["type casting", "performance", "c", "assembly", "42Seoul"]
toc = true
+++

때론 반복적으로 타입을 바꿔서 데이터를 저장해야 하는 경우가 있다.

```c
while (n-- != 0)
	*(string++) = (unsigned char)c; // -> performance loss?
```

반복된 형변환이 프로그램 성능에 영향을 줄 것 같아서 변형을 미리하고 그걸 대입하는 경우도 있다.

```c
unsigned char value = (unsigned char)c;
while (n-- != 0)
	*(string++) = value;
```

하지만 과연 형변환이 프로그램 성능에 영향을 미칠까?

어셈블리 코드를 보면 생각이 바뀔 것이다.

## 어셈블리 코드로 예상하기

```asm
; type casting in while loop (there is no casting)
0000000000000000 <ft_memset>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   48 83 ec 10             sub    $0x10,%rsp
   8:   48 89 4d 10             mov    %rcx,0x10(%rbp)
   c:   89 55 18                mov    %edx,0x18(%rbp)
   f:   4c 89 45 20             mov    %r8,0x20(%rbp)
  13:   48 8b 45 10             mov    0x10(%rbp),%rax
  17:   48 89 45 f8             mov    %rax,-0x8(%rbp)

  1b:   eb 11                   jmp    2e <ft_memset+0x2e>
  1d:   48 8b 45 f8             mov    -0x8(%rbp),%rax
  21:   48 8d 50 01             lea    0x1(%rax),%rdx
  25:   48 89 55 f8             mov    %rdx,-0x8(%rbp)

  29:   8b 55 18                mov    0x18(%rbp),%edx
  2c:   88 10                   mov    %dl,(%rax)

  2e:   48 8b 45 20             mov    0x20(%rbp),%rax
  32:   48 8d 50 ff             lea    -0x1(%rax),%rdx
  36:   48 89 55 20             mov    %rdx,0x20(%rbp)
  3a:   48 85 c0                test   %rax,%rax

  3d:   75 de                   jne    1d <ft_memset+0x1d>
  3f:   48 8b 45 10             mov    0x10(%rbp),%rax
  43:   48 83 c4 10             add    $0x10,%rsp
  47:   5d                      pop    %rbp
  48:   c3                      ret
```

```asm
; type casting before the loop (it has some codes)
0000000000000000 <ft_memset>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   48 83 ec 10             sub    $0x10,%rsp
   8:   48 89 4d 10             mov    %rcx,0x10(%rbp)
   c:   89 55 18                mov    %edx,0x18(%rbp)
   f:   4c 89 45 20             mov    %r8,0x20(%rbp)
  13:   48 8b 45 10             mov    0x10(%rbp),%rax
  17:   48 89 45 f8             mov    %rax,-0x8(%rbp)

  1b:   8b 45 18                mov    0x18(%rbp),%eax
  1e:   88 45 f7                mov    %al,-0x9(%rbp)

  21:   eb 12                   jmp    35 <ft_memset+0x35>
  23:   48 8b 45 f8             mov    -0x8(%rbp),%rax
  27:   48 8d 50 01             lea    0x1(%rax),%rdx
  2b:   48 89 55 f8             mov    %rdx,-0x8(%rbp)

  ; unsigned char value = (unsigned char)c; ______________
  2f:   0f b6 55 f7             movzbl -0x9(%rbp),%edx
  33:   88 10                   mov    %dl,(%rax)
  ;_______________________________________________________

  35:   48 8b 45 20             mov    0x20(%rbp),%rax
  39:   48 8d 50 ff             lea    -0x1(%rax),%rdx
  3d:   48 89 55 20             mov    %rdx,0x20(%rbp)
  41:   48 85 c0                test   %rax,%rax

  44:   75 dd                   jne    23 <ft_memset+0x23>
  46:   48 8b 45 10             mov    0x10(%rbp),%rax
  4a:   48 83 c4 10             add    $0x10,%rsp
  4e:   5d                      pop    %rbp
  4f:   c3                      ret
```

위 두 어셈블리 코드를 보면 알 수 있듯이

첫 번째, 우리가 우려했던 코드(type casting in while loop)는 예상과는 달리, 형변환이 없이 바로 값을 넣어버린다. 반면, 형변환을 미리했던 코드(type casting before the loop)는 명시적으로 새로운 변수에 값이 대입되고 그 값을 사용한다.

이를 보아 두 코드 사이에 명확한 속도차이는 없을 것으로 예상된다. (우려와 다르다는게 매우 놀랍다!)

**사실 자료형이란 건 컴파일러한테만 필요한 것 아니었을까?**

_TODO: 그렇다면 클래스와 같이 크기가 큰 자료형도 형변환으로 인한 성능 변화가 없을 것이라는 가설은 유효한가? 이것은 독자가 검증해보록 하자._