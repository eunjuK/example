###### Front-End Develop SCHOOL

# DAY 20
## 1. 이벤트(EVENTS)

### 1.1. 오래된 이벤트 모델(The Old Way, Event Model)
##### 1.1.1. 스크립팅
 - 인라인 스크립팅 이벤트 추가
>  - **< element onclick = "fnName()">**
>  - 예전방식이나 현재도 쓰고 있긴 함
>  - 예시 )
  >
  >   - 인라인 스크립팅   
  >
    ```
      <button onclick="window.alert('clicked button element.'); lang="en-US" type="button">
         click me
      </button>
    ```
    >> \- __단점:__ 위 코드방식으로 쓰면 코드가 길어지기 때문에 보기 불편해짐.
    >>
    >> \- __결론:__ 아래처럼 스크립팅을 분리하여 사용.

    >   - 스크립팅 분리 1.
    >
    ```
      <button onclick="clickButton()" lang="en-US" type="button">
        click me
      </button>
    >
      <script>
        function clickButton() {
          window.alert('clicked button element.');
          if(this.firstChild.nodeValue === 'click me') {
            this.firstChild.nodeValue = 'this is button. clicked!';
          } else {
            this.firstChild.nodeValue = 'click me';
          }
        }
      </script>
    ```
    >> \- __문제점:__ clickButton()에 매개변수로 this를 넣어주지 않으면 script함수에서 this는 window를 가리킴
    >>
    >> \- __결론:__ 아래방식으로 매개변수로 this를 넣어주자.

  >   - 스크립팅 분리 2.
  >
    ```
    <button onclick="clickButton(this)" lang="en-US" type="button">
       click me
    </button>
    ```
    >> \- 매개변수로 onclick을 가리키는 this를 넣어 줌
  >
    ```javascript
    <script>
        function clickButton(button) {
          window.alert('clicked button element.');
          if(button.firstChild.nodeValue === 'click me') {
            button.firstChild.nodeValue = 'this is button. clicked!';
          } else {
            button.firstChild.nodeValue = 'click me';
          }
        }
    </script>
    ```
    >> \- button이라는 매개변수로 onclick을 가리키는 this 값을 받아 사용


 - 스크립팅 분리 이벤트 추가
>  - **el.onclick = fnNmae; or el.onclick = function(e) {...};**
>  - 전통 방식 (현재 사용 방식)
>  - 예시 )
>
  >   방법 0. ES3
  >
    ```
      <button type="button" class="look-at-button">
        Look
      </button>
    ```
  >
    ```javascript
      <script>
        function clickButton(button) {
          window.alert('clicked button element.');
          if(button.firstChild.nodeValue === 'click me') {
            button.firstChild.nodeValue = 'this is button. clicked!';
          } else {
            button.firstChild.nodeValue = 'click me';
          }
        }
  >
        (function(global) {
          'use strict';
  >
          var look_at_button = document.querySelector('.look-at-button');
          look_at_button.onclick = clickButton;
          }
        })(this);
      </script>
    ```
  >   방법 1. ES5
  >
    ```
      <button type="button" class="look-at-button">
        Look
      </button>
    ```
  >> \- window = this /  look_at_button = argument
  >
    ```javascript
    <script>
      function clickButton(button) {
        window.alert('clicked button element.');
        if(button.firstChild.nodeValue === 'click me') {
          button.firstChild.nodeValue = 'this is button. clicked!';
        } else {
          button.firstChild.nodeValue = 'click me';
        }
      }
  >
      (function(global) {
        'use strict';
  >
        var look_at_button = document.querySelector('.look-at-button');
        look_at_button.onclick = clickButton.bind(window, look_at_button);
        }
      })(this);
    ```
  >   방법 2.
  >
    ```
    <button type="button" class="look-at-button">
      Look
    </button>
    ```
  >
    ```javascript
      <script>
        function clickButton(button) {
          window.alert('clicked button element.');
          if(button.firstChild.nodeValue === 'click me') {
            button.firstChild.nodeValue = 'this is button. clicked!';
          } else {
            button.firstChild.nodeValue = 'click me';
          }
        }
  >
        (function(global) {
          'use strict';
  >
          var look_at_button = document.querySelector('.look-at-button');
          look_at_button.onclick = function() {
            window.clickButton(this);
          };
        })(this);
      </script>
    ```
  >> \- this = look_at_button
  >> \- 함수 지역 내에서 참조가 되지 않는 변수 or 함수는 암묵적으로 스코프 체이닝을 통해 상위 영역을 거슬러~ 거슬러~ 결국은! 전역까지 가서 전역 함수를 실행하게 됨
  >> \- window를 명시적으로 쓰지 않을 경우, 성능 이슈, 디버깅 이슈가 있으므로 명시적으로 써주도록하자.
  >
  >   방법 3.
  >
    ```
    <button type="button" class="look-at-button">
      Look
    </button>
    ```
  >
    ```javascript
      <script>
        function clickButton(button) {
          if(this.nodeName.toLowerCase() === 'button' && (typeof button === 'object') {
             button = this;
          }
  >
          window.alert('clicked button element.');
          if(button.firstChild.nodeValue === 'click me') {
            button.firstChild.nodeValue = 'this is button. clicked!';
          } else {
            button.firstChild.nodeValue = 'click me';
          }
        }
      </script>
    ```

 - 스크립팅 분리 이벤트 제거
>  - **el.onclick = null;**
>  - 예시 )
>
  >
    ```
      <button type="button" class="look-at-button">
        Look
      </button>
    ```
  >
    ```javascript
      <script>
        (function(global) {
          'use strict';
  >
          var look_at_button = document.querySelector('.look-at-button');
  >
          // 버튼을 몇 회 이상 클릭한 후에는 버튼을 사용자가 클릭할 수 없게 만들고자 한다.
          // 버튼을 클릭한 횟수를 기억할 변수
          var click_count = 0;
  >
          // [이벤트 연결] 이벤트 속성에 함수 값 연결
          look_at_button.onclick = function() {
            console.log('clicked:', this.onclick);
            if( ++click_count === 2 ) {
              // 클릭한 횟수가 2회가 되면 버튼을 사용자가 클릭할 수 없게 만든다.
              // this.setAttribute('disabled', 'disabled');  // this = look_at_button
  >
              // [이벤트 제거] 이벤트 속성에 null 대입함으로 연결괸 함수를 끊음
              this.onclick = null;  // 참조한 함수를 끊고 null 대입
              console.log('finished:', this.onclick);
            }
          }
        })(this);
      </script>
    ```


##### 1.1.2. 인터페이스(Interface) 이벤트
 - 로드(Load)
>	- **window.onload**
>    - DOM이 완성된 이후, 이벤트 감지
>    - 늦게 실행 됨(이미지를 전부 불러와야 실행됨 )
>    - 예시 )
>    
>   \*\* **init()** : 애플리케이션 초기화 (initialization()) 
>   \- 애플리케이션 초기화는 문서의 모든 것이 준비된 다음에 실행.
>
    ```
        <button type="button" class="look-at-button">
            Look
        </button>
    ```
>
	```javascript
    	<script>
            (function() {
              'use strict';
>
              /** @function init(): 애플리케이션 초기화 */
              function init() { ... }
>
              // 애플리케이션 초기화는 문서의 모든 것이 준비된 다음에 실행하라.
              // 주의점!!
              // load 이벤트는 무척이나 느림!!! (특히! 이미지 개수가 많고, 용량이 큰 페이지)
              // 스크립트코드는 head에 load 이벤트를 쓰지말고 body 맨 마지막에 쓰는 것이 좋음.
              window.onload = init;
            })(this);
    	</script>
	```

 - 언로드(Un Load)
> - **windo.onunload**
> - 창이 닫히는 순간, 이벤트 감지

 - 에러(Error)
> - **window.onerror**
> - 오류가 발생되었을 때, 이벤트 감지
> - Image Error
> 	- 이미지 호출 시 에러 발생시, 이를 체크

 - 리사이즈(Resize)
 >	- **window.onresize**
> 	- 창의 크기가 변경될 때, 이벤트 감지
> 	- load할 때도 실행시켜 줘야함
> 	- 지원하는 브라우저
> 		- window.innerWidth
>    - 지원하지 못하는 브라우저
>    	- document.documentElement.clientWidth()
>    - 비표준 문서(DOCtype 없는)
>    	- document.body.clientWidth()
>    - 예시 )
>
>   ```
    	<button type="button" class="look-at-button">
            Look
        </button>
    ```
>
	```javascript
          <script>
            (function() {
              'use strict';
    >
              /** @function init(): 애플리케이션 초기화 */
              function init() {
                function clickButton(button) {
                  window.alert('clicked button element.');
                  if(button.firstChild.nodeValue === 'click me') {
                    button.firstChild.nodeValue = 'this is button. clicked!';
                  } else {
                    button.firstChild.nodeValue = 'click me';
                  }
                }
    >
                // window {} 객체의 resize 이벤스 속성에 실행할 함수를 연결
                window.onresize = checkWindowResize;
              }
    >
              /** @functioncheckWindowResize */
              function checkWindowResize() {
                console.log('창(window)의 너비:', this.innerWidth);
              }
    >
              // 애플리케이션 초기화는 문서의 모든 것이 준비된 다음에 실행하라.   
              window.onload = init;
            })(this);
          </script>
    ```
>
    
 - 스크롤(Scroll)
> - **window.onscroll**
>  - 스크롤 되는 순간, 이벤트 감지
>  - *scrollBy(xnum, ynum)*
>  - *scrollTo(xpos, ypos)*
>  - *scrollX*
>  - *scrollY*
>  
> - **parallax scroll native**
	```css
        <style media="screen">
            html {
              overflow-x: hidden;
             height: 300vh;
            }
            [class*="circle"] {
              opacity: 0;
              background: hsla(0, 0%, 0%, 0.6);
              border-radius: 50%;
            }
            .circle-50 {
              width: 50px;
              height: 50px;s
              background: hsla(0, 100%, 50%, 0.6);
            }
            .circle-100 {
              width: 100px;
              height: 100px;
            }
            .circle-1000 {
              width: 1000px;
              height: 1000px;
              background: hsla(0, 0%, 95%, 0.6)
            }
        </style>
    ```
>
	```javascript
        <script>
            (function(){
              'use strict';
    >
              /** @function getRandomNumber 임의의 숫자(정수)를 반환하는 함수 */
              function getRandomNumber(number) {
                return Math.floor(Math.random() * number);
              }
    >
             // circle 객체 위치 임의 설정 함수
             function randomCirclePosition() {
                 // 초기화 과정에서는 문서에 존재하는 [class*="circle-"] 문서 객체를 수집
                 var circles = document.querySelectorAll('[class*="circle-"]');
                 // console.log('circles:', circles);
    >
                for( var i=0, l=circles.length; i<l; i++ ) {
                    var circle = circles[i];
                    // 수집된 circle 객체에 공통적으로 absolute 포지션을 설정 후,
                    // 랜덤하게 화면의 곳곳에 배치(x, y)
                    for ( var i=0, l=circles.length; i<l; i++ ) {
                         var circle = circles[i];
                         circle.style.position = 'absolute';
                         circle.style.top = getRandomNumber( window.innerHeight ) + 'px';
                         circle.style.left = getRandomNumber( window.innerWidth ) + 'px';
                         circle.style.opacity = 1;
                    }
                }
    >
                  // 패럴럭스 이벤트 제어 함수
                  function circleParallaxScroll() {
                      var scroll_y = this.scrollY || this.pageYOfsset || this.scrollTop;
                      // circle 원을 각각 제어
                      var circles = document.querySelectorAll('[class*="circle-"]');
                      for ( var i=0, l=circles.length; i<l; i++ ) {
                           var circle = circles[i];
                           var top = parseInt(circle.style.top, 10);
                           var x = 0.5 * i;
                           if ( i === 1 ) { x = -1 * x * (i+1); }
                           circle.style.top = top + (scroll_y/300 * x) + 'px';
                      }
                }
    >
                 // 애플리케이션 초기화
                 function init() {
                      // circle 객체의 위치를 임의로 설정함수실행
                      randomCirclePosition();
                      // 스크롤 이벤트가 발생하면 각 객체의 위치를 조정
                      window.onscroll = circleParallaxScroll;
                 }
    >
                 // window {} 객체의 load 이벤트가 발동(감지)되면, init() 함수 실행
                 window.onload = init;
             })(this);
        </script>
	```
>
	```html
        <body>
          <div class="circle-50"></div>
          <div class="circle-100"></div>
          <div class="circle-1000"></div>
        </body>
	```

 - 포커스(Focus)
> - **window.onfocus**
>  - 요소가 포커스 되었을 때, 이벤트 감지
>  - 예전에는 악용되어 사용되기도 함

 - 블러(Blur)
> - **window.onblur**
> 	- 요소가 블러 되었을 때, 이벤트 감지



##### 1.1.3. 키(Key) 이벤트
 - 키 다운(Key down)
> - **element.onkeydown**
> - 사용자가 키를 누른 순간 이벤트 감지
> - 1회
>  - 게임에서 한발자국 오른쪽으로 감
> - 예시 )
>
	```javascript
    document.onkeydown = function(e) {
        console.log('e.keyCode:', e.keyCode);
        console.log('e.shiftKey:', e.shiftKey );
        console.log('e.ctrlKey:', e.ctrlKey );
}
    ```

- 키 프레스(Key press)
> - **element.onkeypress**
> - 사용자가 키를 누른 순간 이벤트 감지
> - 여러번 누르고 있는 상태
>  - 게임에서 오른쪽으로 달려감: (오른쪽 키를 누르고 있는 상태)
> - ctrl, art, shift 는 동작이 안됨
 - 키 업(Key up)
> - **window.onkeyup**
> - 사용자가 누른 키를 땐 순간 이벤트 감지


##### 1.1.4. 마우스 이벤트
 \- 접근성에서는 좋지 않다.

 - 클릭(Click)
>  - **element.onclick**
>  - 사용자가 요소를 누르고 떠난 순간, 이벤트 감지

 - 마우스 오버(Mouse Over)
>  - **element.onmouseover**
  - 요소에 마우스 커서가 올라간 순간, 이벤트 감지
   - MouseEnter
   - IE 전용 이벤트로 :hover를 완벽히 구현

 - 마우스 아웃(Mouse Over)
>  - **element.onmouseout**
   - 요소에 마우스 커서가 나간 순간, 이벤트 감지
   - MouseLeave
 	- IE 전용 이벤트로 마우스 아웃에 대응

 - 더블 클릭(Double Click)
>  - **element.ondblclick**
  - 사용자가 요소를 연속하여 2번 누른 순간, 이벤트 감지

 - 마우스 다운(Mouse Down)
>  - **element.onmousedown**
  - 요소를 마우스 커서로 누른 순간, 이벤트 감지

 - 마우스 무브(Mouse Move)
>  - **element.onmousemove**
  - 요소를 마우스 커서로 누르고 움직이는 순간, 이벤트 감지

 - 마우스 업(Mouse Up)
>  - **element.onmouseup**
  - 요소에 마우스 커서가 떠난 순간, 이벤트 감지


##### 1.1.5. 폼 이벤트

- 서브밋(Submit)
>  - element.onsubmit
>  - 폼 전송 시, 이벤트 감지

- 리셋(Reset)
>  - element.onreset
>  - 리셋 버튼을 누를 때, 이벤트 감지

- 체인지(Change)
>  - window.onchange
>  - 내용 변경 시, 이벤트 감지


----

### 1.2. 진보된 이벤트 모델(The New Way, Event Model)
##### 2.1. 자바스크립트의 이벤트

- 자바스크립트의 이벤트 시스템은 스레드를 전혀 사용하지 않는 **완벽한 비동기 방식**으로 작동.
> - if (스레드 방식 언어)
   - 페이지가 로딩되기를 수시로 확인하다가 페이지가 로딩되면 일을 시작하는 방식
   - But.!
   - 자바스크립트 루프는 블로킹 방식이라 루프가 끝날 때까지 다른 일을 할 수 없다.
 - **동기(Synchronous)**
  - 함수가 호출된 후 끝날 때까지 다음 구문을 실행하지 않고 대기.
  - 요청과 그 결과가 동시에 일어남
   - 시간이 얼마가 걸리든 요청한 그 자리에서 결과를 주겠다!
  - **비동기(Asynchronous)**
   - 함수가 호출된 후 끝날 때까지 기다리지 않고 바로 다음 구문 실행.
     - 동작하고 있을 때 다른 동작도 가능
    - 요청과 그 결과가 동시에 일어나지 않음
     - 이따가 결과 줄게.
  - **비동기 콜백 방식**
   - 사용자가 원하는 시점에 실행될 코드(함수)를 수시로 **1회 호출**

- 자바스크립트의 이벤트는 두 단계에 걸쳐 실행!

![bubble-capture](bubble-capture.png)

  - **캡쳐(Capture)**
>   - 자식에서 부모로 이벤트 전달
>   - 클릭가능 유형이 겹칠때 이벤트가 전파됨

![capture](http://take.ms/uYqzK)

- **버블(Bubble)**
> - 자식에서 부모로 이벤트 전달
 - 구형은 Bubble (IE 6-8)만 지원
 - MS는 비표준 (IE 9+ 지원)
 - 우리나라는 IE 를 사용하고있기 때문에 대부분 버블을 사용
 - this
   - event.currentTarget과 비슷
 - event.target
   - 클릭한 타켓 값 반환
 - event.currentTarget
   - 현재 이벤트가 머물러 있는 값 반환
   - 이벤트가 흐르는 와중에 현재 누구를 가르키고있는지 알려줌
  - 헷갈리면 this를 사용

![bubble](bubble.png)

- W3C vs MS
 - W3C
   - 이벤트 모델 - 추가
     - .addEventListner(event_type, event_handler, capture');
     - capture: false(기본값)
     - bubble: true

----

##### Math
 - Math.random()
 	- 랜덤하게 0~1 사이의 실수를 난수로 반환.
 	- Math.random(10) 형식으로 사용하면 안됨.
 	- Math.random() * 10 형식으로 사용해야 함.
 - Math.floor()
 	- 내림
 - Math.round()
 	- 반올림
 - Math.ceil()
 	- 올림
 - Math.floor( Math.random() * 10 ) : 10까지 랜덤으로 숫자를 반환하게 해줄 수 있음


##### Width 차이점
 - **screenWidth()**: 모니터 기준
 - **screenAvailWidth()**: 작업표시줄을 제외한 모니터 기준(가용가능범위)
 - **innerWidth()**: 브라우저 창 기준

##### Function.prototype 객체의 능력(Methods)
 \- 함수 객체의 메소드
 \- 함수를 실행할 때 this가 참조하는 객체랑 전달인자를 임의로 설정할 때 유용

 \* context: this  / look_at_button: 전달인자
 - __ES 3__
 - Function.prototype.__call__(context, look_at_button, ...)
   - 함수의 this와 객체를 연결하여 함수의 return 값을 반환
   - 함수 실행함과 동시에 바로 실행
 - Function.prototype.__apply__

 - __ES 5__
  - Function.prototype.__bind__(context, look_at_button, ...)
    - 함수의 this와 객체를 연결하여 다시 그 함수를 반환
    - 나중에 호출했을 때 실행


#### 금요일 예고편@_@
 - constructor
 - prototype
