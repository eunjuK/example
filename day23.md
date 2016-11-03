###### Front-End Develop SCHOOL

# DAY 20
## 1. 이벤트(EVENTS)

### 1.1. 오래된 이벤트 모델(The Old Way, Event Model)
##### 1.1.1. 스크립팅
 - 인라인 스크립팅 이벤트 추가
>  - < element onclick = "fnName()">
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
    ```
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
>  - el.onclick = fnNmae; or el.onclick = function(e) {...};
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
    ```
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
    ```
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
    ```
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
    ```
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
>  - el.onclick = null;
>  - 예시 )
>
  >
    ```
      <button type="button" class="look-at-button">
        Look
      </button>
    ```
  >
    ```
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
>	- window.onload
>    - DOM이 완성된 이후, 이벤트 감지
>    - 늦게 실행 됨(이미지를 전부 불러와야 실행됨 )
>    - 예시 )
>    
>   \*\* init() : 애플리케이션 초기화 (initialization()) 
>
	>
        <button type="button" class="look-at-button">
            Look
        </button>
		
	>
 - 언로드(Un Load)
 - 에러(Error)
 - 리사이즈(Resize)
 - 스크롤(Scroll)
 - 포커스(Focus)
 - 블러(Blur)
>
> - 예시 )

##### 1.1.3. 키(Key) 이벤트
##### 1.1.4. 마우스 이벤트
##### 1.1.5. 폼 이벤트

----

### 1.2. 진보된 이벤트 모델(The New Way, Event Model)
##### 2.1. 자바스크립트의 이벤트


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
