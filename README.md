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

## `2일차`
### Animations
```javascript
import styled, { createGlobalStyle, css, keyframes } from 'styled-components';

const Button = styled.button`
    // skip

    &:active,
    &:focus {
        outline: none;
    }
    background-color: ${props => props.danger ? "#e67e22" : "#2ecc71"};
    ${props => {
        if(props.danger){
        return css`animation: ${rotation} 2s linear infinite`;
        }
    }}
`

const rotation = keyframes`
    from {
        transform: rotate(0deg);
    }
    to {
        transform: rotate(360deg);
    }
`
```
- keyframes를 import하고 rotation이라는 애니메이션 컴포넌트를 생성한다.
- 이 애니메이션을 danger prop를 가지고 있는 Button에서 동작하도록 세팅한다.
- styled-components를 사용하여 클래스네임 없이 함수, 컴포넌트만으로 구성할 수 있고, 모든 건 props를 가질 수 있다.
```javascript
class App extends Component {
    render() {
        return (
        <Container>
            <GlobalStyle />
            <Button>Hello</Button>
            <Button danger rotationTime={5}>Hello</Button>
        </Container>
        );
    }
}

const Button = styled.button`
    // skip

    &:active,
    &:focus {
        outline: none;
    }
    background-color: ${props => props.danger ? "#e67e22" : "#2ecc71"};
    ${props => {
        if(props.danger){
        return css`animation: ${rotation} ${props.rotationTime}s linear infinite`;
        }
    }}
`
```
- 위와 같이 props를 추가해서 props로 스타일을 변경할 수 있다.

### Extra Attributes and Mixins
```javascript
const Input = styled.input.attrs({
    required: true
    })`
    border-radius: 5px;
`

class App extends Component {
    render() {
        return (
        <Container>
            <GlobalStyle />
            <Input placeholder="hello"/>
        </Container>
        );
    }
}
```
- attributes를 변경하고 싶을 경우, attrs를 이용하여 변경할 수 있다.
- Mixin은 css 그룹이며, 사용 방법은 아래와 같다.
```javascript
import styled, { createGlobalStyle, css } from 'styled-components';

const awesomeCard = css`
    box-shadow: 0 4px 6px rgba(50, 50, 93, 0.11), 0 1px 3px rgba(0, 0, 0, 0.08);
    background-color: white;
    border-radius: 10px;
    padding: 20px;
`
const Input = styled.input.attrs({
    required: true
    })`
    border: none;
    ${awesomeCard}   // mixin 사용
`
```
- css를 import하며, css는 css 규칙을 그룹화하게 도와준다.
- awesomeCard가 바로 mixin 이다.
- 자주 사용하는 스타일을 mixin으로 만들어서 사용할 수 있다.
- 헤더, 푸터, 카드 콤비네이션 등을 만들고 싶을 때 유용하게 활용할 수 있다.
- 또한 팀 프로젝트에서 다른 사람들도 동일한 스타일을 손쉽게 사용할 수 있다.

### Theming
- 팀 프로젝트 진행 시 모든 선은 '특정' 파란색이어야 할 때, '특정' 파란색을 전체 프로덕트에 사용하기 위해 어떻게 기억할까?
- 그리고 '특정' 파란색보다 '특정' 빨간색이 더 트렌디하다며 변경해달라고 하면 어떻게 모두 변경할 것인가?
- 위 상황은 단순 가정이지만, 이러한 상황들을 위해 theme을 만든다.
```javascript
//theme.js
const theme = {
    mainColor: "#3498db",
    dangerColor: "#e74c3c",
    successColor: "#2ecc71"
}

export default theme;
```
- 위와 같이 theme.js 파일을 생성한다.

```javascript
// App.js
import styled, { createGlobalStyle, ThemeProvider } from 'styled-components';  // import ThemeProvider
import theme from './theme';

const Card = styled.div`
    background-color: red;
`

const Button = styled.button`
    border-radius: 30px;
    padding: 25px 15px;
    background-color: ${props => props.theme.successColor};     // background-color를 theme successColor 적용
`

class App extends Component {
    render() {
        return (
        <ThemeProvider theme = {theme}>     // ThemeProvider에 theme props 추가
            <Container>
                <GlobalStyle />
                <Form />
            </Container>
        </ThemeProvider>
        );
    }
}

const Form = () => (
    <Card>
        <Button>Hello</Button>
    </Card>
)
```
- ThemeProvider를 import하고 ThemeProvider를 return하는 동시에 theme props를 추가한다.
- 그리고 Button에 theme successColor를 적용한다.
- Button은 5 depth 밑에 있음에도 theme에서 successColor를 가져와서 적용한다.
- theme을 통해 원할 때 언제든 테마를 바꿀 수 있다.
- 예를 들어, 앱의 나이트 모드 / 정상 모드를 변경한다거나 색맹인 사람을 위한 기능도 가능하다.

### Nesting
- Nesting은 SASS에 있는 기능이며, styled-components에서도 사용 가능하다.
```javascript
const Container = styled.div`
    height: 100vh;
    width: 100%;
    background-color: pink;
    ${Card}{
        background-color: blue;
    }
`
```
- 위와 같이 적용하면 Container 안에 있는 모든 Card를 선택해서 background-color를 변경할 수 있다.
- 즉, SASS가 하는 것처럼 실제 존재하는 컴포넌트를 참조할 수 있다.
- 이것을 'Nesting' 이라고 한다.

### CSS on React Native
`expo init`
- expo로 react native 프로젝트를 생성한다.
`yarn add styled-components`
- react native 프로젝트에 styled-components를 설치한다.
```javascript
import { StyleSheet, Text, View } from 'react-native';

export default class App extends React.Component {
    render() {
        return (
        <View style={styles.container}>
            <Text>Open up App.js to start working on your app!</Text>
        </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center',
    },
});
```
- 위와 같이 작업했던 것을 styled-components로 작업할 수 있다.
```javascript
import styled from "styled-components";

const Container = styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
    padding: 10px 100px;
    `

const Title = styled.Text`
    font-weight: 600;
    font-size: 32px;
    `

export default class App extends React.Component {
    render() {
        return (
        <Container>
            <Title>Open up App.js to start working on your app!</Title>
        </Container>
        );
    }
}
```
- View, Text를 Container와 Title로 대체하고 css 문법 그대로 스타일을 표한할 수 있다.
- 기존에는 backgroundColor와 같이 캐멀케이스 방식으로 사용해야 했다.
- 또한 축약 표현을 사용하지 못해서 paddingTop, paddingLeft 등과 같이 사용했지만 styled-components를 이용하면 축약 표현도 가능하다.
- 그렇다면 View는 어떻게 div로 변경될까?
- 그러한 작업을 해주는 것이 바로 [React-Native-Web](https://github.com/necolas/react-native-web)이다.
  > styled-components 예시 참조 : [styled-components package](https://github.com/serranoarevalo/styled-awesomeness/blob/master/07-extras/extras.md)
