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
  line.x = function(funcToGetX) {
    if (!arguments.length) return getX;
    getX = funcToGetX;
    return line;
  }
  /*
    자바스크크립트 인자의 자유도가 높은이유
    arguments는 호출한 함수의 인자를 담은 유사 배열(array-like) 객체로 함수 내부에서 사용된다.
    if 문은 이 유사 배열 길이를 체크해서 인자가 한도 없으면 길이는 0이고 getX를 그대로 반환

    다시 말해 인자 없이 line.x를 호출하면 x 좌표의 현재 접근자를 내어준다.
    인자가 있으면 x좌표 접근자를 주언진 인자로 바꾼뒤, line 함수 객체를 돌려준다,
    인자 i도 위와 같은 방법으로 함수 오버로딩을 할 수 있다.

    Tip: 자바스크립트에서 객체 지향적 함수 오버로딩 개념은 함수의 arguments를 보고 여기에 뭔가 맞추는 행위다.
    즉 길이를 보고 각각 다른 행동을 취하게 로직을 짤 수 있다.

    line 객체를 반환한 이유는 메서드 체이닝을 써서 좀더 사용자가 쓰기 편하게 하기 위함이다.
  */
  lineGenerator = rj3.svg.line()
    .x()
    .y()
  // 위코드가 메서드 체이닝
```
- z 좌표가 하나 더 추가 되었을떄의 처리 방식은 ?
```javascript
var objectData = [
  { x: 10, y: 130, z: 90 },
  { x: 100, y: 60, z: 202 },
  { x: 190, y: 160, z: 150 },
  { x: 200, y: 10, z: 175 }
],
```
- 딱히 다른 코드를 구현하지 않아도 제대로 동작한다.
- 이미 설계자가 동적으로 코드를 처리하께끔 구현 되어져 있단 뜻이다.
```javascript
// 생성자 함수로 만들어도 모양은 딴판이지만, 결과는 같다.
function XYPair(x, y) {
  this.x = x;
  this.y = y;
}

var objectData = [
  new XYPair(10, 130),
  new XYPair(100, 60),
  new XYPair(190, 160),
  new XYPair(200, 10)
],
```
- 이런 기법을 덕 타이핑(duck typing)이라 한다.
- 오리처럼 생겨서 오리처럼 걷고 오리처럼 꽥꽥 소리를 낸다면 그건 오리다.
- 정의를 어긋나지 않으면 같은걸로 취급한다.
```javascript
// 다음과 같이 판별 가능하다.
if (something instanceof XYPair)
// C# || 또는 자바 개발자는 이런식으로 딱딱하게 사고 하겠지만
// 자바스크립트 개발자라면
if ('x' in something) // something이 x라는 프로퍼티를 소유 또는 상속하는가?
// 그냥 x 속성을 지니고 있냐 이정도만 생각하면 됨
// another way
if (something.hasOwnProperty('x'))  
// something이 x를 상속이 아닌 자신만의 프로퍼티로 소유하는가?
```
- 덕 타이핑은 지저분하게 보이지만, 컴포넌트에 더 다가갈 수 있게 해준는 중요한 수단이다.
- Tip: 덕 타이핑을 잘 활용하라. 적은 코드로도 객체를 폭 넓게 다룰 수 있다.
- 다른 개발자는 제어권이 빠져 나갈때 스택에서 튀어나갈 것이라 예상한다.
- 클로져 라는 자바스크립트 만의 독특한 개념이 존재한다.
- rj3.svg.line()을 호출하면 내부 line 함수를 반환하는데, 실제로 반환하는것은 클로저이다.
- 클로저는 꼭 함수 같은 객체지만, 함수의 생성 당시 환경(getX, getY, interpolate 변수)를 내부에 고스란히 간직한다.
- 내부 line에 있는 함수들을 호출할 때, 이들은 line의 원래 환경을 일부 기억하는 셈이다.
- 이해하기 어려우면 특정 상황에서 외부 변수들을 잠깐 기억하고 있다.(단기 기억 같은걸로 살짝 이해하자)
- Tip: 클로저는 자바스크립트의 정말 강력한 설계 요소다. 모든 함수는 클로저다.
``` javascript
  // while 루프의 호출부를 다시 살펴 보자
  while(++i < n) {
    d = data[i];
    points.push([+getX.call(this, d, i), +getY.call(this, d, i)]);
  }
```
- getX.call(this, d, i)가 하는 일은 뭘까?
- getX가 this 객체의 멤버인 양 인자 d, i를 넘겨 호출한다.
- this는 특별한 변수로서 대충 설명하면 '영어로는 주어 .앞의 객체를 가리킨다.'
- 자바스크립트에서 this를 잘 매칭하는것이 설계의 포인트이다.
- 동적으로 처리할때 가장 중요한 요소라고 봐도 무방하다.
- 자바스크립트에서 'this'는 설계 관점에서 절호의 기회가 될 수 있다. 잘 써먹도록 하자!
```javascript
// 외부 객체에서 값을 얻게끔 라인 생성기를 확장
// 1장/rj3/pathFromFunction.js
rj3.svg.smaples = {};

rj3.svg.samples.functionBaseLine = function functionBaseLine() {
  var firstXCoord = 10,
  xDistanceBetWeenPoints = 50,
  lineGenerator,
  svgHeight = 200;

  lineGenerator = rj3.svg.line()
    .x(function(d, i){ return firstXcoord + i * xDistanceBetweenPoints })
    .y(function(d){ return svgHeight - this.getValue(d); });
  
  return lineGenerator;
};

(function(){
  var yearlyPriceGrapher = {
    lineGenerator: rj3.svg.samples.functionBasedLine(),
    getValue: function getValue(year) {
      // 마치 웹 서비스처럼 호출합니다.
      return 10 * Math.pow(1.8, year-2010);
    }
  },
  years = [2010, 2011, 2012, 2013, 2014, 2015],
  path = yearlyPriceGrapher.lineGenerator(years);

  document.getElementById('pathFromFunction').setAttribute('d', path);
}());
```
- getValue는 어디 있을까?
- yearlyPriceGrapher 객체가 인스턴스화할 때 lineGenerator를 연도마다 특정 값을 반환하는 getValue 함수와 함꼐 묶는다.
- path = yearlyPriceGrapher.lineGenerator(years) 호출에서
- yearlyPriceGenerator의 '점 앞의 객체'다.
- 즉 yearlyPriceGrapher가 y 좌푯값 접근자의 this이므로 getValue 함수는 문제 없이 실행된다.
- 그림 1-4 실행결과 참고(p042)
- this는 자신이 모습을 드러낸 지점의 함수 또는 그 함수를 감싸는 객체를 가리킨다고 착각하기 쉽다.
- 절대 아니다. this는 함수를 호출한 객체를 참조한다.
  - 실행 문맥에 따라 this를 판별해야 한다.

#### 자바스크립트는 싱글 스레드로 움직인다.
- 자바스크립트는 싱글 스레드로 움직인다.
- 블로킹 모델을 채용했다는 의미가 아니다.
- 자바스크립트는 다른 식으로 비동기 프로그래밍을 해야 한다는 뜻이다.
- 멀티 스레드 언어세서는 어떤 작업을 해당 코드와 병렬 실행시킬수 있다.
- 자바스크립트는 어떤 이벤트가 끝나자마자 실행함 함수를 큐에 넣는 게 고작이다.(직렬이네)
- 여기서 이벤트란?
  - 일정 시간 경과(setTimeout)
  - 다른 웹 사이트에서 데이터 조회(XMLHttpRequest.sned)
  - 마우스 클릭 등
- 자바스크립트 이벤트 엔진은 이벤트 루프에서 한 번에 하나씩 함수를 꺼내 실행한다.
- 설계 관점에서 보면 이런 방식이 오히려 진정한 멀티 스레드 환경보다 생각하기 편하다.
- 제어권을 가진 상태에서 인터럽트를 당하거나 다른 객체가 변수에 마음대로 접근하는 식의 문제는 안생긴다.
- 의외로 중간에 치고 들어 올수 없다는 점이 장점이다.
- 또한, 프로세서를 독차지할 염려도 없다.

### 대규모 시스템에서 자바스크립트 함정을 피하라
- 클래스(자바스크립트는 객체)가 5개인 시스템보다 50개인 시스템에서 개발 및 유지 보수 난이도가 10배 이상 높다.
- 객체 5개가 제각기 다른 객체들에 의존한다고 보면 최대 20개의 통신 채널이 유발된다.
  - 이들 객체가 저마다 다른 객체 4개를 호출한다는 전제하에 A가 B를 호출하고 B가 A를 호출하는것도 따로 센다.
  - 개수가 50개면, 2,450(50 * 49)개 채널이 만들어지니 100배 이상 증가한다.

- 클라이언트/ 서버 양쪽에서 규모가 큰 시스템의 부하를 감당할 수 있게 해주는 단일 페이지 애플리케이션 노드JS 같은 자바스크립트 기술이 등장하면서 개발자는 통신 채널을 최소화하는 문제를 진지하게 고민하게 됨
- 본연의 임무를 수행하기 위해 객체 간 인터페이스가 불가피하다면 어떤 상황에서도 모든 객체가 올바르게 작동하게끔 지속해서 연결을 관리해야 한다.

#### 스크립트는 모듈이 아니다.
- 자바스크립트는 데이터와 함수를 적절히 캡슐화한 모듈을 만드는 조리법이 매우 다양하다.
- 스크립트 파일은 그런 조리법이 아니다.
- ES6 모듈화 가능하다.

#### 스코프는 중첨 함수로 다스린다.
- C#/자바는 클래스 안에 다른 클래스를 둘 수 있다.
- 물론 자주 쓰는 기법은 아니다.
- 심지어 MS사는 "중첩된 타입은 감춰야 한다"는 경고까지 했다.
- 즉 "중첩된 타입은 멤버 접근성 이란 의미가 있어서 개발자가 분명히 인식하기 어렵다."
- 자바스크립트는 클래스가 없지만, 함수를 중첩하여 코드를 계층화 할 수 있다.
- 덕분에 개발자가 원하는 것을 찾는 데 도움이 될 뿐만 아니라 프로그램에서 변수/함수의 스코프를 최소화 가능하다.
- 바로 이런 특성이 대규모 시스템을 효과적으로 유지하는 핵심이자 자바스크립트 코드의 탁월한 근본 요소이다.
```javascript
  rj3.svg.line = function() {
    var getX = function(point) {
      return point[0];
    },
    // var 선언문 줄임

    function line(data) {
      var segments = [],
      // 다른 변수 줄임
      
      function segment() {
        segemnts.push("M", interpolate(points));
      }

      while (++i < n) {
        d = data[i];
        points.push([+getX.call(this, d, i), +getY.call(this, d, i)]);
      }

      if (points.length) {
        segment();
      }

      return segments.length ? segments.join(""): null;
    }

    line.x = function(funcToGetX) {
      if (!arguments.length) return getX;
      getX = funcToGetX;
      return line;
    }
  }
```
- 내부 line 함수에 멤버 함수 line.x가 있다.
- x는 line의 멤버임에도 segments 같은 line의 지역 변수는 볼수 없지만
- 에워싼 함수 안에서 line과 line.x는 getX 변수를 바라볼 수 있다.
- 이런 식으로 클로저를 교묘하게 잘 섞어 쓰면 대규모 자바스크립트 시스템에 꼭 필요한, 강력한 도구가 된다.
- 자바스크립트 캡슐화를 함수의 중첩과 스코프를 활용하여 특성을 잘 활용하란 뜻.
- 자바스크립트의 특성을 잘 파악해서 자유자재로 쓰면 의도한대로 코드가 잘 돌아간단 말

#### 규약을 지켜 코딩한다
- 대규모 시스템을 잘 꾸려 나가려면 될 수 있는 한 작게 만들어야 효과적이다.
- 자바스크립트는 간이 고루 잘 밴 덕 타이핑의 기상천외한 유연성 덕분에 적은 코드로도 많은 일을 할 수 있다.
- 반면에, 자신이 짠 프로그램에 누가 무엇을 던져 넣을지 도통 알수가 없다.
  - 사용자가 어떤 인자를 전달하지 파악이 불가능하다.
- 함수 인자에 특정한 조건이 있다면 그 값을 꼭 검증해야 한다.
  - 좋은 호스트 코드란 사용자가 어떤 값을 넣어도 예외처리를 잘해주어야 한다.
  - 2장의 AOP(Aspect-Oritented-Programming)을 참고하자.

### 소프트웨어 공학 원칙을 적용하라
- 음악 거장의 콘서트를 구경한적 있는가?
- 거장은 자신의 악기가 쉬우므로 악기를 쉽게 다룰 수 있다.
- 손가락을 효율적으로 움직이고 몸이 편안한 상태에서 호흡할 수 있고
- 또 잡념에 사로잡히지 않고 오로지 음악에만 집중할 수 있게 오랜시간을 자신을 단련해 왔다.
- 그 역시 처음에는 아주, 아주 천천히 연주하면서 곡을 익혔고
- 곡을 완전히 섭력한 다음에는 메트로놈 눈금에 맞는 속도로 연주할 수 있게 되었을 것이다.
- 그러니 실수하기란 생각하기 어렵다.
- 실수없이 연주하기에 가장 좋은 방법은 느리게 연주하는 방법이 베스트이다.
- 이 절에서 말한 원칙들은 개발 함에 있어서 반드시 지켜야할 규약이라고 생각하는 것이 좋다.

#### SOLID의 원칙
- 마이클 페더스 시초
- 로버트 마틴이 확립
- 다섯 가지 객체 지향 설계 원칙이다.
  - Single Responsibility(단일 책임 원칙)
  - Open/Closed Principle(개방/폐쇄의 원칙: 확장에는 열려있고 수정에는 닫혀 있어야 한다.)
  - List Substitution(리스코프 치환 원칙)
  - Interface Segregation Principle(인터페이스 분리 원칙)
  - Dependency Inversion Principle(의존성 역전 원칙)

#### 단일 책임 원칙
- 다소 과장해서 말하면 모든 클래스(자바스크립트 함수)는 반드시 한 가지 변경 사유가 있어야 한다.
- 정말 무리한 요건이다. 코드 한 줄 한 줄이 앞으로 변경될 무언가를 나타내는데, 그럼 함수를 모조리 코드 한 줄만으로 작성하란 말인가?
- 함수는 단서가 될 만한 정보는 사실상 외부에서 다 받고 결과는 내부에서 처리해야한다.
- 정의 함수 자체는 아는게 거의 없어야 한다.
- 그저 데이터를 받아와서 내부 함수를 반환할 뿐이다.
- 관심사의 분리가 핵심이다.

#### 개방/폐쇄 원칙
- "모든 소프트웨어 개체는 확장 가능성은 열어 두되 수정 가능성은 닫아야 한다."
- 즉, 어떤 경우라도 실행 코드를 변경하지 말고, 어떻게든 재사용하고 확장하라는 뜻이다.
- 역시 만만찮은 요건이다.
- 로버튼 마티조차도 
  - "일반적으로 모듈이 꽉 '닫혀있다.' 하더라도 닫혀있지 않은 어딘가에서 어떤 식으로든 변경될 가능성은 있습니다." 
  - "완벽한 닫힘은 있을수 없으니 모종의 전략이 필요 합니다."
  - "모종의 전략이 필요로 합니다."
  - 즉 설계자는 불가피한 변경 유형을 미리 선택할 수 밖에 없다.
  - 오랜 경험에서 필요한 통찰력이 필요한 부분이다.
- 예제 코드의 경우 d3.svg.line 함수와 점을 연결하는(보간하는) 방법에서 변경 사항이 있으리라 내다보고 아러한 특성을 추상화하여 함수밖으로 빼내는 기지를 발휘함.
- 그가 변경되지 않을거라고 본건 SVG경로의 규격이다.
- 반드시"M"으로 시작해서 그 뒤에 점 데이터가 이어지는 문자열 형태로 과감히 경로 문자열을 하드 코딩한 것이다.
- 설계자는 변화할것과 변화하지 않을것을 판단해서 코딩해야 한다.

#### 리스코프 치환 원칙
- 바바라 리스코프가 '자료 추상화와 계층화'에서 천명한 리스코프 치환 원칙이다.
- 어떤 타입에서 파생된 타입의 객체가 있다면 이 타입을 사용하는 코드는 변경하지 말아야 한다.
- 다시 말해 한 객체를 다른 객체에서 파생하더라도 그 기본 로직이 변경 되어서는 안된다.
- 작정 중인 함수가 기반 클래스로 하는 일과 서브 클래스로 하는 일이 다르다면 이를 어긴 셈이다.
- 서로 파생 관계가 없는 타입 간에는 적용되지 않는다.
- 예를 들어 인자에 따라 분기 처리 해두는 것이 좋은 습관이다.(문자열, 숫자, undefined)
- 앞에서 보았듯이 자바스크립트는 함수 오버로딩이라는 객제 지향 개념으로 이를 실천한다.
- 쉽게 간략이 설명하면 파생된 타입에 변동을 가하지 않는다.
- 덕 타이핑 역시 파생과는 조금 다르지만, 이 원착과 잘어 맞는다.

#### 인터페이스 분리 원칙
- 이 원칙은 C++, 자바 같은 인터페이스 기반 언어 환경에서 비롯되었다.
- 이들 언어에서 인터페이스란 클래스에서 어떤 기능을 '구현'하지 않고(명칭, 파라미터, 반환 타입을)'서술'만 한 코드 조각이다.
- 기능이 많은 인터페이스는 더 작게 응축시킨 조각으로 나누어야 한다는 발상이다.
- 인터페이스 사용부는 '뚱뚱한' 전체가 아니라 아주 작은 인터페이스 하나만 바라보면 된다.
- 물론 모듈 간 연결 폭을 최소화하는 방향으로 가야 한다.
- 전에도 일렀듯이 통신 채널을 줄이는건 덩치 큰 자바스크립트 시스템의 중요한 관리 포인트다.
- 자바스크립트에는 클래스도, 인터페이스도 없다.(지금은 클래스는 존재한다.(ES6 기준))
- 자바스크립트에서 이 원칙을 실현 할 수 있다.
  - 함수가 기대하는 인자가 무엇인지 명확히 한다.
  - 그 기대치를 최소화해야 한다.
  - 이럴때도 덕 타이핑이 도움이 된다.
  - 특정 타입의 인자를 바라기 보다는 이 타입에서 실제로 필요한 프로퍼티가 더러 있을 거라 기대하는 것이다.
  - 나중에는 기대치를 구체화하고 강제화하는 정규 방안을 제시한다.

#### 의존성 역전 원칙
- 이 원칙도 인터페이스와 관련 있다.
- 로버트 마틴은 "상위 수준 모듈은 하위 수준 모듈에 의존 해서는 안 되며 이 둘은 추상화에 의존해야 한다."
- 인터페이스 기반 언어에서는 대개 의존성 주입 이라는 연관된 개념으로 표현한다.
- 클래스 A가 클래스 B의 서비스가 필요할 때 A는 B를 생성하지 않는다.
- 대신 A 생성자에 건넨 파라미터 하나가 B를 서술하는 인터페이스 역활을 한다.
- 이제 A는 B에 의존하지 않고 자신의 인터페이스만 바라본다.
- A가 생성되면 구체화한 B를 넘겨받는다.
- B 역시 인터페이스에 의존한다.
- 리스코프 치환 원칙 덕분에 인터페이스를 만족하는, B의 파생형 버전을 제공할 수 있는 이점이 있다.
- 또한 인터페이스는(개방/폐쇄 원칙에도 불구하고)B를 고쳐야 할 경우 하위 버전 호환성을 유지하려면 어떤 로직을 계속 갖고 있어야 하는지 일목요연하게 서술한다.
- A와 B의 인터페이스는 분리 되어져 있어야 한다.
```javascript
  d3.svg.line = function() {
    return d3_svg_l;ine(d3_identity)
  };

  function d3_svg_line(projection) {
    // vars 선언문 줄임
    function line(data) {
      // vars 선언문 줄임

      function segment() {
        segments.push("M", interpoate(projection(points), tension));
      }
    }
  }
  /*
  d3_svg_line의 projection은 점 데이터를 다른 좌표 공간에 투사 할떄를
  대비한 파라미터다. projection 기본값은 d3_identity로 점 데이터를 변경하지는 않지만,
  다른 방법으로 투사할 수는 있다.
  예컨대 d3_svg_lineRadial 투사를 주입하면 d3.svg.lineradial로 극 좌표계를 사용할 수 있다.
  */
 d3.svg.line.radial = function() {
   var line = d3_svg_line(d3_svg_lineRadial);
   // 코드 줄임
   return line;
 };

 function d3_svg_lineRadial(points) {
   // 점 데이터를 극 좌표계로 변환
   return points;
 }
 // 좌표 공간에 의존성을 주입한 D3의 라인 생성기는 더없이 유연한 모습이다.
```

#### DRY 원칙
- "남에게 대접받고 싶은 대로 남을 대접하라"
- 황금률을 따르면 만사가 평온하리라.
- DRY 원칙, 즉 "반복하지마라(Don't Repeat Yourself)"라는 말 역시 좋으 소프트웨어 개발 습관의 근원이라 할 만하다.
- "모든 지식 조각은 딱 한 번만 나와야 한다."
- 가장 중요한 SOLID 원칙중 일부가 DRY 원픽의 필연적 산물이다.
- 단일 책임 원칙이 그렇다.
- 이 원칙을 어긴 코드는 결과적으로 재사용이 불가능하다.
- DRY한 코드로 만드는 과정에 의존성 주입과 단일 책임 문제가 개입된다.
- d3.svg.line은 아주 DRY한 함수다.
```javascript
  // DRY 한 코드
  var d;

  while(++i < n) {
    d = data[i];
    points.push([+getX.call(this, d, i), +getY.call(this, d, i)]);

  }
  // DRY 하지 못한 코드
  while (++i < n) {
    points.push([
      +getX.call(this, data[i], i),
      +getY.call(this, data[i], i)
    ]);
  }

  /*
    코드는 후자가 더 짧지만, data[i]를 처리하는 '지식조각'이 반복되어 DRY 하지 않다. getX 호출 부에서 한 번, getY 호출부에서 한 번 더 반복된다.

    DRY함은 여타 언어와 비교했을 때 자바스크립트에서 특별히 중요하다.
    코딩 실수는 늘 하기 마련인데, 자바스크립트는 잘못 코딩한 특정 클래스를 컴파일러가 미리 알려주지 않으니 자칫 그대로 운영 환경에 노출될 가능성이 크다.
    참사를 예방하려면 될 수 있는대로 빨리 실수를 인지하는게 상책이다.
    '지식 조각'을 한 번에 한 개씩 입력하면 에러는 사방에서 손을 들 것이다.
  */
```

## 바르게 유지되는 코드 작성하기
- 건반 악기 연주가 쉬운 것처럼 프로그래밍도 쉽다 했었다.
- "그저 제떄 건반을 짚으면 됩니다."
- 무대에서 연주자가 완벽히 연주하더라도 청중은 그 연주를 가져 갈 도리가 없다.
- 아무리 업무 요건을 정확하게 프로그래밍해 유지보수 해도 5년이 지나면 개발팀에서 수근대기 시작할 것이다.
- 이렇게 계속 가느니 차라리 새로 짜는 게 났겠어요. 곪아 터진 부위를 도려내야 하지 않을까요?
- 안타까운 상황을 방지하고 여러분이 짠 코드가 천세를 누릴 방법을 알아보자.

### 단위 테스트는 미래에 대비한 투자다.
- 단위테스트는 시간이 흐르면서 아무리 큰 변화의 소용돌이 속에 빠져도 완벽한 프로그램을 만들 수 있게 한다.
- 여기서 단위란 특정 조건에서 어떻게 작동해야 할지 정의한 것이다.
- 언제나 그런 것은 아니지만, 대개 함수로 표현한다.
- 첫째, 테스트 준비다. 단위를 실행할 조건을 확실히 정하고, 의존성 및 함수 입력 데이터를 설정한다.
- 둘째, 단위를 실행하여 테스트 한다. 예를 들어 단위가 함수면 준비 단계에서 미리 설정한 입력값을 함수에 넘겨 실행한다.
- 마지막은 테스트 단언이다 미리 정한 조건에 따라 예상대로 단위가 작동하는지 확인한다. 단위가 함수인 테스트라면 예상한 값을 반환하는지 조사하면 된다.
- 전체 단위 테스트 꾸러미를 작성하는 데 든 시간과 노력은 앞으로 프로그램이 직면할 갖가지 변화에 대처하기 위한 보험이자. 안정적인 애플리케이션 유지에 꼭 필요한 최선의 투자라고 할 수 있다.
- 단위 테스트가 실패한다는 것은 어떤 변화로 인해 기존 프로그램의 기능이 바뀌었으니 실패한 유발한 이 변화를 자세히 들여다보아야 한다는 사실을 일깨워주는 적색 신호등이다.

### 테스트 주도 개발을 실천하라
- 테스트 주도 개발(Test-Driven Developer)은 처음부터 프로그램을 제대로 작성했는지 확실히 보장한다.
- TDD에서는 애플리케이션 코드를 짜기 전에 이 코드가 통과해야 할 단위 테스트를 먼저 작성한다.
- 마치 애플리케이션을 개발하듯 전체 단위 테스트 꾸러미를 만들어가는 TDD방식을 따르면 단위 정의와 인터페이스 설계에 도움이 많이 된다.
- TDD를 실천하는 개발자는 애플리케이션에 어떤 변화가 생길 때마다 다음 단계를 밟는다.
- 여기서 변화란 완전히 새로운 기능을 추가하거나 기존 시스템을 고치고 버그를 잡는 일이다.
  - 완벽히 변경하면 성공하나 그렇게 되기 전까지는 반드시 실패하는 단위 테스트를 작성한다.
  - 테스트가 성공할 수 있을 만큼 '최소한으로' 코딩한다.
  - 애플리케이션 코드를 리팩토링하며 중복을 제거한다.
- 이 세단계를 흔히 적색 녹색 리팩터라고 줄여 말한다.
- 적색과 녹색은 새 단위 테스트의 실패와 성공 상태를 가리킨다.
- TDD의 핵심은 테스트 코드를 먼저 작성한다.
- TDD를 시작할때 가장 문제가 되는 사고 방식이다.
- 특히 변경할 내용이 사소할수록 어색하게 느끼기 쉽다.
- TDD 습관이 몸에 배지 않은 개발자는 빨리 애플리케이션 코드를 뜯어고쳐 다음 개발 업무를 진행하고 싶어함
- 애플리케이션 코드를 작성하고 나서 테스트를 작성하면 되지않나 싶지만 습관을 들인다는건 우선순위를 정한다는 이야기
- TDD는 자바스크립트 코드가 올바르게 동작하기 위해 매우 중요한 요소이다.

### 테스트하기 쉬운 코드로 다음어라
- 테스트하기 쉬운 코드를 작성하려면 가장 중요한 단계는 관심사를 적절히 분리하는 일이다.
- validteAndRegisterUser 함수는 관심사가 다양하다.
```javascript
  var Users = Users || {};
  Users.registration = function() {
    return {
      validateAndRegisterUser: function validateAndDisplayUser(user) {
        if(!user || 
          user.name === "" || 
          user.password === "" || 
          user.password.length < 6
        ) {
          throw new Error('사용자 인증이 실패했습니다.');
        }

        $.post('http://yourapplicaion.com/user', user);

        $('#user-message').text('가입해주셔서 감사합니다.' + user.name + '남');
      };
    }
  };
```
- 이 함수가 하는 일은 3가지 이다.
  - user 객체가 올바르게 채워졌는지 검증한다.
  - 검증을 마친 user 객체를 서버로 전송한다.
  - UI에 메세지를 표시한다.

- 관심사는 세가지로 요약된다.
  - 사용자 검증
  - 서버와 직접 통신
  - UI 직접 다루기

- validateAndRegisterUser 함수가 작동하는지 테스트할 조건을 모두 나열해보자.
  - user가 null 이면 에러를 낸다.
  - null인 user는 서버로 전송하지 않는다.
  - user가 null이면 UI를 업데이트하지 않는다.
  - user가 undefined이면 에러를 낸다.
  - undefined인 user는 서버로 전송하지 않는다.
  - user가 undefined이면 UI를 업데이트하지 않는다.
  - user의 name 프로퍼티가 빈 상태면 에러를 낸다.
  - name 프로퍼티가 빈 user는 서버로 전송하지 않는다.
  - user의 name 프로퍼티가 비어 있으면 UI를 업데이트하지 않는다.

- 이 밖에도 오류 조건은 더 있다.
- 사실 아주 많다.
- UI가 제대로 업데이트 되는지
- 유효조건이 정확한지 등
- 겉보기엔 상관없는 테스트가 빠진 상태에서 내일 다른 동료가 user를 검증하는 코드를 마지막 부분에 넣으면 어찌될까?
```javascript
  var Users = Users || {};
  Users.registration = function() {
    return {
      validateAndRegisterUser: function validateAndDisplayUser(user) {
        $.post('http://yourapplication.com/user', user);

        $('#user-message').text('가입해주셔서 감사합니다.' + user.name + '님');

        if (!user ||
          user.name === "" ||
          user.password === "" ||
          user.password.length < 6
        ) {
          throw new Error('The user is not valid');
        }
      }
    }
  };
```
- 이렇게 해도 Error 객체가 생성되는지 확인하는 테스트는 통과한다.
- 하지만 이 함수는 깨진 코드가 되어 버린다.
- 누가 일부로 이렇게 바꿀일은 적겠지만 사고는 늘 우연히 터진다.
- 이 코드는 테스트할 수는 있지만 까다롭다.
- 테스트할 조건이 엄청 다양하게 조합될 수 있고 이들을 죄다 보완하는 건 불가능에 가깝다.
- 애플리케이션 코드를 전부 이렇게 작성하면 단위 테스트 영역이 적합하지 않다고 확실히 말할 수 있다.
- 세 관심사를 validateAndRegisterUser 함수에 다 맡기지 말고 별개의 객체로 관심사를 추출하여 책임을 부여한다면 어떨까?
- 이렇게 만든 독립적인 객체는 각자 완전한 테스트 꾸러미를 갖게 될 테고 validateAndUser 코드는 다음과 같은 모습을 갖추게 될것이다.
```javascript
  var Users = Users || {};
  Users.registration = function(userValidator, userRegister, userDisplay) {
    return {
      validateAndRegisterUser: function validateAndDisplayUser(user) {
        if (!userValidator.userIsValid(user)) {
          throw new Error('사용자가 인증이 실패 했습니다.');
        }
        userRegister.registerUser(user);
        userDisplay.showRegistrationTankYou(user);
      }
    }
  }
```
- 내부의 관심사 3가지를 각각 함수로 분리하여 인자로 전달한다.
- 새로 고친 registration 모듈은 user 검증, 등록, 화면 표시를 담당하는 개별 객체 인스턴스를 의조성 주입으로 제공한다.
- validateAndRegisterUser는 다른 관심사에 직접 영향을 미쳤던 코드 대신 주입된 객체를 사용한다.
- validateAndRegisterUser는 본질적으로 어떤 일을 하는 함수에서 남이 한 일을 조정하는 함수로 탈바꿈했다.
- 덕분에 이 함수는 테스트하기 쉬워져서 다음 여섯 조건만 확인하면 완벽한 테스트를 할 수 있다.
  - user가 잘못 넘어오면 에러가 난다.
  - 잘못된 user는 등록하지 않는다.
  - 잘못된 user는 표시하지 않는다.
  - 올바른 user를 인자로 userRegister.registerUser 함수를 실행한다.
  - userRegister.registerUser에서 에러가 나면 userDisplay.showRegistrationThankYou 함수는 실행하지 않는다.
  - user가 성공적으로 등록되면 user를 인자로 userDisplay.showRegistrationThankYou 함수를 실행한다.

- 관심사 분리가 가져온 변화 각각의 스크로프로 인해 고민해야 할 가지수가 확준다.
- 전체 테스트는 여섯개로 줄었다.
- 조금전에는 3*3 아홉개 였고 그나마 일부에 불과했다.
- 서로 다른 관심사는 작고 간단한 모듈로 나누어 만들면 코드 작성과 테스트, 그리고 이해가 쉽다.
- 이런 식으로 만든 코드는 장기적으로 별 탈 없이 작동할 가능성이 크다.
- TDD가 개발 속도를 늦출 것으로 생각하는 사람도 있으리라.
- 관심사를 제대로 분리하지 않은 채 먼저 코딩하고 테스트를 작성하는 식으로 진행하면 정말 늦어지게 될 것이라 확신한다.
- 무수한 실수의 늪에 빠져 허우적거리기 전에 단위 테스트가 실수를 가능한 한 빨리 발견하는 길이라는 진리를 기억하자
- 적색-녹색-리팩터 과정을 반복하며 작은 코드를 개발하다 보면 점점 속도가 붙게 된다.
- 음악연주가 처음엔 느리고 조심스럽게 연습하다가 점점 연주곡을 더 빨리 마스터하게 되는 것과 같은 이치다.
- 첫째, 작은 코드는 대개 간단하고 실수할 가능성이 작아 디버깅 시간을 상당히 줄일 수 있다.
- 둘째, 테스트로 코드를 완전히 커버하니 리팩토링을 하더라도 무서울게 없다.
- 따라서 코드를 DRY하게 유지하여 코드베이스에 오류 발생 여지를 줄이고 규모를 작게 가져갈 수 있다.
- DRY는 결국 재사용을 의미하여 알다시피 재사용 가능한 코드는 시간을 절약한다.
## 정리하기
- 자바스크립트는 개발자에게 훌륭한 설계 기회다.
- D3 함수를 축약한 코드를 보면서 그런 기회를 여럿 엿보았다.
- rj3.svg.line은 아주 작은 함수이다.
  - 함수의 객체성(자바스크립트 함수는 객체이다.)
  - 중첩함수
  - 함수 오버로딩
  - 덕 타이핑
  - 클로저 
  - this
- 등의 강력함을 제대로 보여준다.
- SOLID
- DRY
- 단위테스트는 장기적 관점에서 안정된 애플리케이션을 만들기 위한 최선의 투자다.
- 단위테스트가 없으면 그저 애플리케이션이 잘 작동작하기를 막연히 기도할 수 밖에 없다.(검증 불가)
- 테스트 주도 개발의 이점
  - 장기적인 믿음성을 보장하는 단위 테스트 꾸러미를 구축한다.
  - 애플리케이션 객체에 적확한 인터페이스를 설계할 때 도움이 된다.
  - 단위 테스트를 통해 코드를 더 빨리 개발할 수 있다.
- 테스트성을 높이려면?
  - 관심사를 분리
  - 단일 책임 원칙
  - 의존성 주입
  - 위와 같은 소프트웨어 공학 원칙을 잘 적용하는게 중요하다.
- 이런 생각으로 무장해야만 비로소 소프트웨어 장인이 될 준비를 마칠수 있다.

