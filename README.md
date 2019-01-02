Styled Components
=========================
이 프로젝트는 [Nomad Coders](https://academy.nomadcoders.co/)의 니콜라스님 강의를 토대로 진행했습니다.

## `시작하기`
### create-react-app
`npm install -g create-react-app`
- create-react-app를 설치한다.<br>

`create-react-app [Project Name]`
- create-react-app으로 프로젝트를 생성한다.

`cd [Project Name]`<br>
`npm start`
- 프로젝트 폴더 경로로 이동하여 실행한다.

## `1일차`
### Setup and a trip to the past
- 버튼을 두개 만들어 보자.
```javascript
class App extends Component {
    render() {
        return (
        <Fragment>
            <button className='button button--success'>Hello</button>
            <button className='button button--danger'>Hello</button>
        </Fragment>
        );
    }
}
```
- 위와 같이 2개의 클래스를 사용하는 것은 나쁜 것은 아니지만 최선도 아니다.

### Hello World with Styled Components
`npm install styled-components`
- styled-components를 설치한다.
- styled-components는 css가 내부에 있는 컴포넌트를 준다.
```javascript
import styled from 'styled-components';

class App extends Component {
    render() {
        return (
        <Container>
            <Button>Hello</Button>
            <Button danger>Hello</Button>
        </Container>
        );
    }
}

const Container = styled.div`
    height: 100vh;
    width: 100%;
    background-color: pink;
`

const Button = styled.button`
    border-radius: 50px;
    padding: 5px;
    min-width: 120px;
    color: white;
    font-weight: 600;
    -webkit-appearance: none;
    cursor: pointer;
    &:active,
    &:focus {
        outline: none;
    }
    background-color: ${props => props.danger ? "#e67e22" : "#2ecc71"};
```
- styled-components를 사용하기 이전에는 2개의 컴포넌트와 하나 이상의 파일과 2개의 클래스명이 필요했다.
- 하지만 styled-components를 통해 위와 같이 깔끔한 코드 작성이 가능하다.

### createGlobalStyle and Extend
```javascript
import styled, { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
    body{
        padding: 0;
        margin: 0;
    }
    `

    class App extends Component {
    render() {
        return (
        <Container>
            <GlobalStyle />
            <Button>Hello</Button>
            <Button danger>Hello</Button>
        </Container>
        );
    }
}
```
- createGlobalStyle은 전역 스타일을 처리하는 특별한 styled-component를 생성하는 함수이다.
- css 재설정 또는 기본 스타일 같은 항목을 적용할 수 있다.
```javascript
const Anchor = styled(Button.withComponent('a'))`
text-decoration: none;
`;
```
- 또한 위와 같이 styled 함수 내에 .withComponent를 이용하여 기존에 만들었던 버튼에 추가로 원하는 스타일을 추가할 수 있다.

### Animations