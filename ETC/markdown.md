# Markdown 문법
***

>본격적인 TIL 작성을 위해 마크다운에 대해 먼저 알아보자.

## 마크다운(makrdown)은 무엇인가?
***
- 텍스트 기반의 마크업 언어이며 쉽게 쓰고 읽을 수 있고 HTML로 변환이 가능하다.
- 파일 확장자는 .md로 이루어져 있다. ex) README.md
- 깃에서 흔히 볼 수 있는 readme파일 또한 마크다운 문법으로 작성 됐다.

## 장점
- 간결하다.
- 별도의 도구없이 작성 가능하다.
- 다양한 형태로 변환이 가능하다.
- 용량이 적어 보관이 용이하다.
- 지원 되는 프로그램, 플랫폼이 다양하다.

## 단점
- 표준이 없다.
- 표준이 없어 도구에 따라 동작하지 않거나 다르게 표현된다.
- 모든 HTML 마크업을 대신하지 못한다.

## 마크다운 문법
***

## 1.제목
***


큰제목
====
```
큰제목
=====
```
소제목
-----
```
소제목
-----
```

H1 부터 H6 까지 지원 된다.

```
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6
```
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6

## 2.강조
***
```
이텔릭체는 *별표*, _언더바_ 사용
두껍게 **별표**, __언더바__ 사용
이텔릭체와 두껍게는 혼용 가능
취소선은 ~~물결~~ 사용
<u>밀줄은</u> <u>태그 사용
```

- 이텔릭체는 *별표*, _언더바_ 사용  
- 두껍게 **별표**, __언더바__ 사용  
- 이텔릭체와 두껍게는 혼용 가능  
- 취소선은 ~~물결~~ 사용  
- <u> 밀줄은 </u> u 태그 사용

## 3.목록
***

```
1. 순서가 있는 목록
2. 순서가 있는 목록
- 순서가 없는 목록 (대쉬)
* 순서가 없는 목록 (별표)
+ 순서가 없는 목록 (플러스)
```
1. 순서가 있는 목록
2. 순서가 있는 목록
- 순서가 없는 목록 (대쉬)
* 순서가 없는 목록 (별표)
+ 순서가 없는 목록 (플러스)

## 4.링크
***

```
[Google] (https://google.com)
[Naver] (https://naver.com)
[내부 링크] (../this/is/example)
[참조 링크] (https://google.com) "구글로 이동"
[참조 링크2] [네이버로 가자]
[네이버로 가자]: https://naver.com "네이버로 이동"  
```
[Google](https://google.com)  
[Naver](https://naver.com)  
[내부 링크](../this/is/example)  
[참조 링크](https://google.com) "구글로 이동"  


## 5.코드 강조

### 인라인 코드 강조
```
코드를 `강조` 하려면 `를 사용하세요.
```
- 코드를 `강조` 하려면 `를 사용하세요.

### 블록 코드 강조
- `를 세 번 이상 사용하여 적는다. 

````
```html
<a href="https://www.google.co.kr/" target="_blank">GOOGLE</a>
```

```css
.list > li {
  position: absolute;
  top: 40px;
}
```

```javascript
function func() {
  var a = 'AAA';
  return a;
}
```

```bash
$ vim ./~zshrc
```

```python
s = "Python syntax highlighting"
print s
```
````
```html
<a href="https://www.google.co.kr/" target="_blank">GOOGLE</a>
```

```css
.list > li {
  position: absolute;
  top: 40px;
}
```

```javascript
function func() {
  var a = 'AAA';
  return a;
}
```

```bash
$ vim ./~zshrc
```

```python
s = "Python syntax highlighting"
print s
```

## 6.줄바꿈
```
줄바꾸기를 하려면  
띄어쓰기 두 번하고 엔터를 누르세요.
```
줄바꾸기를 하려면 띄어쓰기  
두 번 하고 엔터를 누르세요.
- br 태그로도 줄바꿈이 가능하다.

## 마무리
마크다운 문법을 처음 사용 해봐서 익숙하지 않아  
기능 하나하나를 모두 찾아보며 작성하는데 시간이 많이 소요됐다  
하지만 TIL 작성을 위한 기본적인 문법 숙지는 되었으므로 만족한다.  
사용하면서 필요하다고 생각되는 문법은 추후에 추가하도록 하겠다.

+ 참고  
https://heropy.blog/2017/09/30/markdown/  
https://gist.github.com/ihoneymon/652be052a0727ad59601














