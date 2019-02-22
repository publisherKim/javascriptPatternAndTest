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
