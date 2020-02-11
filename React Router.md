### React Router

- SPA에서의 라우팅 문제를 해결하기 위해 사용되는 네비게이션 라이브러리
- 브라우저 내장 객체 사용 가능
  - location
  - history
- React Router
  - Web
  - Native
  - react-route-dom 라이브러리 필요 (React Router V4부터 필요)

#### 종류

Native   

- AoS - Java 코틀린
- IoS - Objective-c, swift

Mobile- Web

- HTML5, Javascript css

Hybrid-App

- Mobile-Web + PhoneGap, React, Anguler 필요 ex) 모바일 뱅킹 사이트

### Router 실습

- npx create-react-app reat-router1

- src 밑에 폴더 생성

  - components
    - header.js
    - routers.js
  - routes
    - page1.js
    - page2.js
    - page3.js

- plugin

  편리한 구조 호출 기능

  - reactjs code snippets 설치
  - rcc 입력후 엔터시
    - class 컴포넌트
  - rsc 입력후 엔터시
    - function 컴포넌트

- npm install --save react-router-dom 설치
  
- router를 사용하기 위함?
  
- Router 밑에는 Route 여러개가 와야함

  - Link도 Router 밖에 있으면 안됨

  ```javascript
          <Router>
              <Header />
              <Route path="/my_note1" component={Page1} />
              <Route path="/my_note2" component={Page2} />
              <Route path="/my_note3" component={Page3} />
          </Router>
  ```

  > 이처럼 <Header /> 도 Router 안에 들어와야 한다.

- Link

  - <a> 태그와 유사한 기능의 컴포넌트

#### 실습 예제

App.js

```javascript
import React from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
// 설치한 react-router-dom으로 부터 각각을 받아서 Browser는 Router라는 이름으로
// 바꿔서 받아서 사용하겠다는 뜻
import Page1 from './routes/page1';
import Page2 from './routes/page2';
import Page3 from './routes/page3';
import Header from './components/header';

const App = () => {
  return (
    <Router>
    <Header />
    <Route path="/my_note1" component={Page1} />
    <Route path="/my_note2" component={Page2} />
    <Route path="/my_note3" component={Page3} />
    </Router>
);
};

export default App;
```

page1

```javascript
import React from 'react';

const Page1 = () => {
    return (
        <h1>Page1 입니다.</h1>
    );
};

export default Page1;
```

page2

```javascript
import React from 'react';

const Page2 = () => {
    return (
        <h1>Page2 입니다.</h1>
    );
};

export default Page2;
```

page3

```javascript
import React from 'react';

const Page3 = () => {
    return (
        <h1>Page3 입니다.</h1>
    );
};

export default Page3;
```

header.js

```javascript
import React from 'react';
import { Link } from 'react-router-dom';

const Header = () => {
    return (
        <div>
            <ul>
                <li><Link to="/my_note1">MY Page1</Link></li>
                <li><Link to="/my_note2">MY Page2</Link></li>
                <li><Link to="/my_note3">MY Page3</Link></li>
            </ul>
        </div>
    );
};

export default Header;
```



- react developers tools
  - 크롬 웹스토어에 검색해서 확장프로그램 추가
  - 요소 검사에 용이

