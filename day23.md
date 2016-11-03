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
 - 스크립팅 분리 이벤트 제거
 -
 -

##### 1.1.2. 인터페이스(Interface) 이벤트
 -
>
> - 예시 )

##### 1.1.3. 키(Key) 이벤트
##### 1.1.4. 마우스 이벤트
##### 1.1.5. 폼 이벤트

----

### 1.2. 진보된 이벤트 모델(The New Way, Event Model)
##### 2.1. 자바스크립트의 이벤트


----

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
