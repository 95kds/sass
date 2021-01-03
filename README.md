# sass

## TIP



### 번들링 모드
production : 제품 모드   
development : 개발 모드 (디버깅 가능)    


### 확장과 인자가 없는 믹스인의 사용
연관성있는 특성을 묶을때만 확장을 사용   
연관성없는 선택자(단순히 동일한 코드의 반복)를 확장 시킬 경우   
연관성없이 확장된 선택자 모음이 컴파일 되며 용량이 커지는 단점이 생긴다.   
(ex : header > dt, .item > p {font-weight:bold}... )   


연관없는 동일 코드의 반복을 사용하려 할 땐 인자가 없는 mixin을 사용하는 방법을 추천    
< 컴파일 결과물에서 반복은 나쁜 것이 아니다. 소스를 반복하는 것이 나쁜 것이다. >   


mixin의 경우 연관없는 확장 선택자 모음이 만들어 지지 않으며   
동일한 코드의  copy & paste 를 수동 반복 입력 하지 않을 수 있다   
확장으로 선택자 모음이 생성 되는 것 보단 동일 코드의 반복이 용량이 더 줄어든다.   


> @extend : 선택자간의 연관이 있는 확장클래스 모음을 만들때(%placeholder)   
> @mixin() : 선택자간의 연관이 없는 동일 코드의 직접 반복 양을 줄이고 싶을 때 사용(인자X)   


참고 URL : https://mytory.net/2016/12/23/when-to-use-extend-when-to-use-a-mixin.html   







## MY MODULE



### 가변 이미지 제작 mixin

#### math : sass module 
#### $paddingTop : padding-top value
#### $image-url : img url 
#### $image-name : image name 
#### $extension : defualt type png 
#### ?v=#{math.random(100000)} : image cach boosting 


#### example
```
@use sass:math; 
$image-url: "/imges/";

@function urls($image-url, $image-name, $extension: "png") {
    @return url("#{$image-url}#{$image-name}.#{$extension}?v=#{math.random(100000)}");
}
@mixin urls-responsive($paddingTop, $image-url, $image-name, $extension: "png") {
    padding-top: $paddingTop;
    font-size: 0;
    background-image: urls($image-url, $image-name, $extension);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: top center;
}

//@function 
background-image : urls($image-url,'img');

//mixin
@includ urls-responsive('20px', $image-url,'img');

```



### vw 계산기 function (multiple)


#### 작업 방식 : pixel -> vw
#### 계산식 : 컨텐츠 가로 또는 세로 * 100 / 부모의 넓이
#### 적용 : padding, margin, top, let, bottom.... (height 값 제외)


#### example
```
@function calcVw($num, $num1: null, $num2: null, $num3: null, $defualtWidth: 360) {
    $value: $num * 100 / $defualtWidth * 1vw;
    $result: $value;
    $numlist: $num1, $num2, $num3;
    @each $item in $numlist {
        @if ($item != null) {
            $item: $item * 100 / $defualtWidth * 1vw;
            $result: append($result, $item, space);
        }
    }
    @return $result;
}

//function (unit : pixel)
width: calcVw(20);
padding: calcVw(20, 30, 50, 40);
```