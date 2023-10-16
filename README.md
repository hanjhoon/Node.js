# Node.js
+ Node.js는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임
+ JavaScript -> 웹, 앱, 서버 소스 코드 실행 가능

# 구조
![image](https://github.com/hanjhoon/Node.js/assets/121271030/de64780b-5cae-4c18-9ab1-4460a5e111ba)

+ libuv란 비동기 I/O에 집중하는 멀티 플랫폼 라이브러리이다. C언어로 개발되었으며 사실상 Node.js를 위해 개발된 것이다. 이 라이브러리는 다양한 I/O 폴링 메커니즘에 대한 단순한 추상화 이상의 기능을 제공한다. 어느 운영체제에서도 가능한 file I/O와 thread 기능 또한 제공된다. 
libuv에는 thread pool이 존재하는데 이 thread pool에 있는 thread가 동기적인 입출력 작업을 이벤트 루프 대신 처리를 해준다. 또한 이 libuv를 통해 이벤트 루프를 제어한다.
  + libuv 홈페이지: http://libuv.org/

+ Node.js는 binding API(C++ Addon)를 활용해 위에서 설명한 libuv과 연결되며 I/O 작업을 동기식으로 libuv가 처리한 뒤 I/O 작업이 끝나면 콜백함수가 처리되는 것이다.
I/O 작업은 네트워크, 파일 시스템, 프로세스 등이 있다.
따라서 I/O 작업이 많은 곳에서 Node.js가 활용이 좋다. (여러 스레드를 활용하기 때문)
Node.js 코드를 수행하는 스레드와 이벤트 루프의 스레드는 동일하다. 

# 특성
+ 이벤트 기반
  
![image](https://github.com/hanjhoon/Node.js/assets/121271030/a945de67-396f-43f5-9d3e-eff2a0b370fe)

+ 싱글 vs 멀티 스레드

![image](https://github.com/hanjhoon/Node.js/assets/121271030/89b726c7-d042-4023-837d-30b57b8a99f0)

14ver : 싱글 스레드 -> 조건 만족 -> 멀티 스레드
싱글 스레드 1개를 어떻게 잘 관리하나?


### 스택과 큐
+ 스택은 출입구가 하나인 데이터 구조로 순서대로 a, b, c 데이터를 넣었다면 꺼낼때는 반대로 c, b, a순서로 꺼내게 된다. (FILO, First In Last Out)
+ 큐는 종류에 따라 양쪽 모두 입/출력이 가능한 큐도 있으나 일반적으로 큐에 순서대로 a, b, c 데이터를 넣었다면 꺼낼때는 a, b, c 순서대로 꺼내게 된다. (FIFO, First In First Out)

![image](https://github.com/hanjhoon/Node.js/assets/121271030/f9b0abd3-b024-4ec4-b526-497d4da9efb0)



### 콜 스택(Call Stack)이란?
+ 콜 스택이란, 자바스크립트 코드가 실행되며 생성되는 실행 컨텍스트(Execution Context)를 저장하는 자료구조라 정의 할 수 있다.

1. 함수를 호출하면 실행 컨텍스트가 생성되고, 이를 콜 스택에 추가한 다음 함수를 수행하기 시작
2. 함수에 의해 호출되는 모든 함수(내부 함수들)는 콜 스택에 추가되고 해당 위치에서 실행
3. 함수의 실행이 종료되면 해당 실행 컨텍스트를 콜 스택에서 제거한 후 중단 된 시점부터 다시 시작
4. 만약 스택이 할당 된 공간보다 많은 공간을 차지하면 'stack overflow'에러가 발생


### 브라우저 환경에서의 자바스크립트
![image](https://github.com/hanjhoon/Node.js/assets/121271030/050623ce-f0c5-4eff-9017-4111d1d936d8)

+ Heap
힙은 구조화되지 않은 넓은 메모리 영역을 지칭합니다.
이전에 알아본 자바스크립트의 Reference Type 즉, 객체는 모두 Heap안에 할당됩니다.

+ Web API
브라우저에서 제공하는 별도의 API 입니다.
프론트엔드 개발을 하며 주로 사용하는 DOM, SVG, Fetch, Canvas, setTimeOut등은 모두 자바스크립트가 아닌 브라우저에서 제공하는 API입니다.

+ Callback Queue(Messege Queue)
비동기 함수가 실행 된 후 콜백 함수가 대기하는 자료구조 입니다.

+ Microtask Queue(Job Queue)
ES6에서 도입 된 새로운 컨셉으로, Callback Queue와 동일 계층에 존재하며 Promise를 통한 비동기 요청 시의 콜백 함수는 Microtask Queue에 대기하게 됩니다.

+ Animation Frames
requestAnimationFrame에 의해 등록되는 자료구조로 requestAnimationFrame의 콜백 함수가 대기하는 자료구조 입니다.

+ Event loop(이벤트 루프)
이벤트 루프는 Call Stack과 각 Queue를 감시하고 있다가 Call Stack이 비었을 경우 정해진 우선순위에 따라 queue에서 하나씩 꺼내 Call Stack에 추가해주는 역할을 합니다.

1. 호출 스택의 작업을 모두 처리합니다.
2. 호출스택이 비었을 경우, Microtask Queue를 확인하고 처리해야 할 작업이 있다면 Call Stack에 넣고 처리합니다.
3. 만약 MicroTask Queue가 비었을 경우에는 Animation Frames를 확인하고 마찬가지로 처리해야 할 작업이 있다면 Call Stack에 넣고 처리합니다.
4. 1~3과정을 거치고 난 후 마지막으로 Callback Queue를 확인하고 마찬가지로 Call Stack에 넣고 처리합니다.

## Sequelize란?
+ ORM(Object-Relational Mapping)은 객체지향 패러다임을 활용하여 관계형 데이터베이스(RDB)의 데이터를 조작하게 하는 기술이다. 이를 활용하면 쿼리를 작성하지 않고도 객체의 메서드를 활용하는 것처럼 쿼리 로직을 작성할 수 있다.

+ Sequelize는 MySQL, PostgreSQL, MariaDB 등 많은 RDBMS를 지원하고 Promise 기반으로 구현되었기 때문에 비동기 로직을 편리하게 작성할 수 있다.

### Sequelize사용법
먼저 Sequelize를 사용하기 위해서 아래 3가지 패키지들을 터미널에서 명령어를 이용해서 설치해준다.
```
npm install sequelize // 시퀄라이즈 설치
npm install mysql2 // mysql2 설치
npm install -g sequelize-cli // sequelize-cli를 전역으로 설치한다.
```

위에 3가지 설치가 완료되면, 아래와 같이 터미널에 sequelize init명령어를 사용해서 초기화를 시켜준다.
```
sequelize init
```
초기화를 시켜주면 config, models, migrations, seeders 와 같은 폴더들이 생긴다.

+ config : 데이터베이스 설정 파일, 사용자 이름, DB 이름, 비밀번호 등의 정보 들어있다.
+ migrations : git과 비슷하게, 데이터베이스 변화하는 과정들을 추적해나가는 정보로, 실제 데이터베이스에 반영할 수도 있고 변화를 취소할 수도 있다.
+ models : 데이터베이스 각 테이블의 정보 및 필드타입을 정의하고 하나의 객체로 모은다.
+ seeders : 테이블에 기본 데이터를 넣고 싶은 경우에 사용한다.
+ 데이터베이스 관련 설정 파일인 config.json 이 있다. 여기선 로컬 MySQL을 사용하기로 하고 Student 란 이름의 데이터베이스를 생성해보자. 나의 경우 아이디/비밀번호 모두 root 이다.
```
mysql -uroot -p
create database student;
use student;
```
데이터베이스를 생성하고 선택하자. 이제 이 과정들에 대한 정보들을 config.json 에 적어주면 되는데 development , test , production 이 있기 때문에 모두 수정해준다.












