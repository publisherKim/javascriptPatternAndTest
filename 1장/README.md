# 좋은 소프트웨어 만들기
## 바르게 시작하는 코드 작성하기
## 바르게 유지되는 코드 작성하기
## 정리하기

### This chapter Topics
- 바르게 시작하는 코드를 작성한다.
- 바르게 유지되는 코드를 작성한다.

### 예제 파일 내려 받기
- https://github.com/gilbutITbook/006844

### HeadTopic
- '성숙한' 시스템을 경험한 사람들을 잘 알고 있다.
- 설계가 생명이다.
- 유지보수의 난이도를 결정한다.
- 취약한 코드를 리팩토링하는 일은 시간적 낭비를 향후 유발한다.
- 똑똑하다고 믿는 개발자는 최악이다.
- 올바른 지침서 확립의 중요성
- SOLID 최소한의 자격기준으로 삼자
- DRY 최소한의 자격기준으로 삼자
- 위의 두단어를 아예 모르거나 비슷하게 중요도에 설명할수 없는 개발자는 협어대상에서 피하자.
- 테스트 주도 개발을 꿰어찬 개발자는 더욱 드물다.

## 바르게 시작하는 코드 작성하기
- 신기할것이 없다. 그저 제때 제 건반을 짚으면 악기가 자신을 연주할 뿐이다.(요한 제바스티안 바흐가 건반악기에 남긴 기록)
- 명확한 규칙과 지침을 지키는 회사라면 프로그램에도 잘 들어 맞는다.

### 자바스크립트 특성을 완벽히 섭
렵하라
- 스쿼시 코트의 예시
- 초보자의 라켓의 휘두름에 상대방은 머리가 날아갈수 있다.
- 마스터라면 그런 일격에 자신의 얼굴을 보호할 안목 및 통찰을 가지고 있어야 한다.
- 자바스크립트 언어의 유연성 특징상 C# C등의 언어를 생각하고 들어오면 큰코 다치기 쉽다.
- 이전에 다른 언어로 프로그래밍을 해본 적이 있는 사람이 대규모 자바스크립트 개발 업무를 맡게 되었다면 기법의 차이점을 스스로 익혀야 한다.
- 이 차이점은 작은 단위의 구문부터 대규모 아키텍처/엔지니어링까지 고루 존재한다.
- 첫술에 배부를순 없다.
- 한땀 한땀 코딩하면 배우자.

#### 사례연구: D3.js
- D3(Data-Driven Documents)의 약자로, 데이터에서 화려한 SVG 그래픽을 표현해준다.
- http://bl.ocks.org/mbostock 눈으로 한번 보는 것이 좋다.
- 잘 정돈된 수십줄의 자반스크립트 만으로 복잡한 방사형 다이어 그램을 브라우저에 표현 가능하다.
- D3홈페이지 주소: http://d3js.org 소스코드: https://github.com/mbostock/d3

```javascript
    // SVG 라인을 생성하는 함수
    // 다른 전역 변수와 충돌을 피하기 위해 이름 공간을 생성한다.
    var rj3 = {};

    //svg라는 하위 이름 공간을 만든다.
    rj3.svg = {};

    // rj3.svg 이름 공간에 line 함수를 넣는다.
    rj3.svg.line = function() {
        var getX = function(point) {
            return point[0];
        },
        getY = function(point) {
            return point[1];
        },
        interpolate = function(points) {
            return points.join("L");
        };

        function line(data) {
            var segments = [],
                points = [],
                i = -1,
                n = data.length,
                d;

            function segment() {
                segments.push("M", interpolate(points));
            }

            while (++i < n) {
                d = data[i];
                points.push([+getX.call(this, d, i), +getY.call(this, d, i)]);
            }
        }
    };
```
- 배열 데이터를 SVG 경로(path)로 변환하는 함수다.
- 양의 기울기 직선, 음의 기울기 직선, 양의 기울기 직선을 그린다고 가정하자
  - cf: p33 picture 그림 1-3
  - <path d="M10, 130L, 100, 60L190, 160L280, 10"></path>
  - 우리가 특정 선을 어떻게 그릴까에 관한 이야기
  - (x, y)좌표가 (10, 130)인 지점으로 이동한뒤("M")한 뒤 (100, 60)까지 선(L)의 반복
  - 명령 체계가 어떻게 이루어지는 지 확인하자
  - 10, 130 출발 100, 60 좌표까지 이어지는 직선을 그린다.
  - 모눈 종이에 직선 그려 본 사람은 이해하기 좀더 쉬울듯
```javascript
  // 예제 1-2 rj3.svg.line() 호출부 예시
  // 소스파일 1장/rj3/path/pathFromArray.js
  var arrayData = [
    [10, 130],
    [100, 60],
    [190, 160],
    [280, 10]
  ],
  lineGenerator = rj3.svg.line(),
  path = lineGenerator(arrayData);

  document.getElementById('pathFormArrays').setAttribute('d', path);
```
- 위 코드가 최동단의 사용하는 코드이다.
- data는 api로 연동하면 단 두줄이면 직선그리기가 끝난다는 걸 알수 있다.
- 물온 정의 코드를 만드는건 조금 다른 문제이긴 하지만 수학에 관심이 있어야 한다.
  - Tip: 자바스크립트 함수는 다른 함수 내부에 중첩될 수 있으며, 이는 스코프를 다스리는 중요한 수단
- 정의 코드의 함수가 함수를 반환하는 일은 프로그래밍 언어에서 대개 혼동을 일으키는 금기 사항이다.
- 그러나 자바스크립트에서는 아키텍처의 선택권을 넓혀주는 지극히 당연한 습관이다.
- 강력한 자바스크립트 코드를 원한다면 우선 일급 객체인 함수를 인자로 주고 반환값으로 돌려받는 형태에 길들어야 한다.(어린왕자가 여우에게 물들듯, 혹은 여우가 왕자에게 물들듯)
- 자바스크립트는 함수는 일급 시민으로서 자신만의 프로퍼티와 메서드를 가질수 있다.
- C#이나 C혹은 Java를 배웠다고 자신만의 지식과 논리로 무장해서 함부로 주장해서는 안된다.
  - Tip: 자바스크립트 함수는 일급 시민으로서 자신만의 프로퍼티와 메서드를 가질 수 있다.
```javascript
  // 예제 1-1을 보면 함수에 메서드를 붙이는 장면이 나온다.
  line.x = function(funcToGetX) {
    if(!argument.lenth) return getX;
    getX = funcToGetX;
    return line;
  };
  // x는 반환된 함수 line의 멤버다.
  // rj3.svg.line()을 호출하면 함수가 반환된다.
  // 이말은 함수 자체를 반환하기 때문에 사용성이 올라간다.
  // 다음줄에 arrayData를 인자로 주고 호출이 가능해진다.
  // 다음과 같은 코드에 연결되어진다.
  while (++i < n) {
    d = data[i];
    points.push(+getX.call(this, d, i), +getY.call(this, d, i));
  }
  // data 원소는 각각 변수 d로 받아 getX, getY 함수에 전달하고 여기서 x,y 좌푯값을 추출한다.
  // + 는 자바스크립트 특성상 문자열을 숫자로 변환하기 위하여 쓰임
  // 다음 실행 순서
  // 두 좌푯값은 기본적으로 arrayData 각 원소를 구성하는 원소 2개짜리 배열의 각각 첫번째 두번쨰 원소 값이다.
  // 다음 코드 조각이 그렇게 할당한다.
  var getX = function(point) {
    return point[0];
  },
  getY = function(point) {
    return point[1];
  }
  // 그 다음 segment 함수를 호출하는데, 이 함수는 또 다른 중첩 수준에 위치한 함수로 line 함수의 프라이빗 함수다.
  // 첫 번째 원소에는 SVG "M" 명령어를, 두 번째 원소에는 경로를 넣어 segments 변수를 채운다.
  function segment() {
    segments.push("M", interpolate(points))
  }
  // ...
  if (points.length) {
    segment();
  }
  // 경로를 만드는건 interpolate 함수의 몫으로, 기본적으로 각 점 사이에 "L"을 넣어(각각 암시적인 문자열 변환을 거쳐) 결합한다.
  var interpolate = function(points) {
    return points.join("L");
  };
  // 결국 input => output 으로 보자면
  // input
  var arrData = [
    [10, 130],
    [100, 60],
    [190, 160],
    [280, 10]
  ]
  // output
  "10,130L100,60L190,160L280,10"
  // 끝으로 segments의 두원소("M"과 문자열로 바뀐 점의 좌푯값)를 return 문에서 결합해 SVG 경로를 완성한다.
  "M10, 130L100,60L190L280,10" 
  // 기본 로직은 여기까지이다. 
```
- 배열이 아닌 객체 형태는 어떻게 처리할것인가?
```javascript
  // 예제 1-3 데이터를 곧바로 변환
  (function(){
    var objectData = [
      { x: 10, y: 130 },
      { x: 100, y: 60 },
      { x: 190, y: 160 },
      { x: 280, y: 10 }
    ],
    arrayData = objectData.map(function(d) {
      return [+d.x, +d.y];
    }),
    lineGenerator = rj3.svg.line(),
    path = lineGenerator(arrayData);

    document.getElementById('pathFromTransfromeObjects')
      .setAttribute('d', path);
  }());
  // 그러나 같은 데이터를 복사하는 건 낭비다.
  // 링크의 효율성에 길든 C# 개발자가 짤법한 코드다.(책도 C#개발자를 비평함)
  // 이 방식은 인터페이스에서도 걸림돌
  // 데이터가 변동시 라인도 동적으로 처리하고 싶을때 해결 불가

  // line.x와 line.y 이미 D3제작자가 설계해 놓은 사용법을 익히자
  // 예제 1-4 line.x와 line.y 사용법
  // 소스파일 1장/rj3/pathFromObject.js
  (function(){
    var objectData = [
      { x: 10, y: 130 },
      { x: 100, y: 60 },
      { x: 190, y: 160 },
      { x: 280, y: 10 }
    ],
    lineGenerator = rj3.svg.line()
      .x(function(d) { return d.x; })
      .y(function(d) { return d.y; }),
    path = lineGenerator(objectData);

    document.getElementById('pathFromObjects').setAttribute('d', path);
  }());
  // 호출 방식만 .x(function(d) { return d.x; })로 교체한다. 그리고 while 루프에서
  points.push([+getX.call(this,d,i), +getY.call(this,d,i)]);
  //getX.call은 교체한 함수를 호출하여 해당 객체의 x 프로퍼티, 즉 사본 아닌 원래 객체 값을 반환한다.
  /*
    이 call을 눈여겨볼 가치가 있다. x 좌푯값을 얻는 함수가 무엇이든 인자 2개를 넘겨 호출하도록 마련 했다. 두 번쨰 인자는 i는 배열 인덱스로, 여기서는 쓰지 않지만 다른 곳에서는 사용 가능하다.
    바로 이것이 함수 오버로딩 이라는 객제 지향 개념을 자바스크립트에 녹인 원리다.
    너무나 당연스럽게 자바스크립트 개발자는 사용하고 있다.
  */
```


