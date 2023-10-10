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


