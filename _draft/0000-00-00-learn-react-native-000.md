---
layout: single
title: react native 기초 배우기 001 - intro
categories: 
  - learn react native
tags:
  - react native
---

## Intro

- 터미널 실행

  ~~~bash
  # 새 프로젝트 생성
  $ react-native init --version 0.61.5 react_native_01
  # 프로젝트로 이동
  $ cd react_native_01
  ~~~

- 시뮬레이터 실행
  ~~~bash
  $ react-native run-ios
  ~~~

- App.js의 내용이 화면에 출력됨. 어떻게?
- 앱의 입구인 index.js와 app.json 을 살펴보자

  ~~~javascript
  // index.js
  // react-native 으로부터 AppRegistry 을 사용
  import {AppRegistry} from'react-native';
  // App 으로부터 App 을 사용
  import App from './App';
  // app.json 으로부터 name 을 appName으로 사용
  import {name as appName} from './app.json';
  // appName 와 App을 AppRegistry에 등록
  AppRegistry.registerComponent(appName, () => App);
  ~~~

  ~~~javascript
  // app.json
  {
    "name": "react_native_01",
    "displayName": "react_native_01"
  }
  ~~~

- App.js를 알맞게 수정

  ~~~javascript
  import React, { Component } from 'react';
  import { View, Text } from 'react-native';
  class App extends Component {
    render() {
      return (
        <View>
          <Text>hello world</Text>
        </View>
      )
    }
  }

  export default App;
  ~~~

## View, Text

  ~~~javascript
  // react 모듈로부터 Component 클래스를 import
  import React, { Component } from 'react';
  // react-native 모듈로부터 View, Text 클래스를 improt  
  import { View, Text } from 'react-native';
  // Component 을 상속하는 App 클래스
  class App extends Component {
    render() {
      return (
        <View>
          <Text>hello world</Text>
        </View>
      )
    }
  }

  export default App;
  ~~~ 

- react-native를 통해 필요에 따라 다양한 클래스를 import 할 수 있음 (ex, Button, Image)

### View

- `<View>`는 다른 컴포넌트를 감싸는 컴포넌트
- UI를 만드는 기본적인 컴포넌트임
- `<View>`안에 또 `<Veiw>`가 있어도 됨
  
  ~~~javascript
  ...
  return (
    <View>
      <View>
        <Text>hello world</Text>
      </View>
      <View>
        <Text>hello world</Text>
      </View>
      <View>
        <Text>hello world</Text>
      </View>
    </View>
  )
  ...
  ~~~

### Text

- `<Text>`는 텍스트를 표시하는 컴포넌트

## Style

- inline 와 StyleSheet 을 사용하는 방법이 있음

### inline 방식

  ~~~javascript
  ...
  return (
    <View style=({
      backgroundColor: 'green',
      height: '100%',
      paddingTop: 50
    })>
      <View>
        <Text>hello world</Text>
      </View>
      <View>
        <Text>hello world</Text>
      </View>
      <View>
        <Text>hello world</Text>
      </View>
    </View>
  )
  ...
  ~~~

### StyleSheet 방식

  ~~~javascript
  ...
  import { View, Text, StyleSheet } from 'react-native';
  ...
  return (
    <View style={styles.mainView}></View>
  ...
  const styles = StyleSheet.create({
    mainView: {
      flex: 1,
      backgroundColor: 'green',
      paddingTop: 50,
      alignItems: 'center',
      justifyContent: 'center',
    }
  })
  ...
  ~~~

### style 꾸며보기

  ~~~javascript
  ...
  return (
    <View style={styles.mainView}>
      <View style={styles.subView}>
        <Text style={styles.mainText}>hello world</Text>
      </View>
      <View style={styles.subView}>
        <Text>hello world</Text>
      </View>
      <View style={styles.anotherSubView}>
        <Text style={styles.mainText}>hello world</Text>
      </View>
    </View>
  )
  ...
  const styles = StyleSheet.create({
    mainView: {
      flex: 1,
      backgroundColor: 'green',
      paddingTop: 50,
      alignItems: 'center',
      justifyContent: 'center',
    },
    subView: {
      flex: 1,
      backgroundColor: 'yellow',
      marginBottom: 10,
      width: '50%'
    },
    anotherSubView: {
      flex: 2,
      backgroundColor: 'yellow',
      marginBottom: 10,
      width: '100%',
      alignItems: 'center',
      justifyContent: 'center'
    },
    mainText: {
      fontSize: 50,
      fontWeight: 'bold', 
      color: 'red',
      padding: 20
    }
  })
  ...
  ~~~

## TouchEvent

- TouchEvent 는 컴포넌트를 선택하면 발생하는 이벤트

  ~~~javascript
  ...
  return (
    <View style={styles.mainView}>
      <View style={styles.subView}>
        <Text style={styles.mainText}>hello world</Text>
      </View>
    </View>
  )
  ...
  const styles = StyleSheet.create({
    mainView: {
      flex: 1,
      backgroundColor: 'white', // green -> white
      paddingTop: 50,
      alignItems: 'center',
      justifyContent: 'center',
    },
    subView: {
      // flex: 1,
      backgroundColor: 'yellow',
      marginBottom: 10,
      // width: '50%'
    },
    anotherSubView: {
      flex: 2,
      backgroundColor: 'yellow',
      marginBottom: 10,
      width: '100%',
      alignItems: 'center',
      justifyContent: 'center'
    },
    mainText: {
      fontSize: 20, // 50 -> 20
      fontWeight: 'normal', // 'bold' -> 'normal'
      color: 'red',
      padding: 20
    }
  })
  ...
  ~~~

- header.js 파일생성

  ~~~bash
  # pwd = react_native_01
  $ mkdir src/header.js
  ~~~

  ~~~javascript
  // header.js
  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  const Header = () => (
    <View style={styles.header}>
      <Text>This is header</Text>
    </View>
  )

  const styles = StyleSheet.create({
    header: {
      backgroundColor: 'pink',
      alignItems: 'center',
      padding: 5,
      width: '100%'
    }
  })

  export default Header;
  ~~~

- App.js 수정

  ~~~javascript
  ...
  import { View, Text, StyleSheet } from 'react-native';
  import Header from './src/header';
  ...
      return (
        <View>
          <!-- header.js 의 내용이 표시 -->
          <Header/>
        </View>
      )
  ...
  ~~~

---
**NOTE**
- {} 와 () 의 차이
 
   ~~~javascript
   eFunc = () => {
     // return 되는 JSX 컴포넌트가 없음
   }
 
   eFunc = () => (
     // JSX 컴포넌트를 return 할 수 있음
   )
   ~~~
 
- JSX(javascript xml)
 
   ~~~javascript
   const element = <h1>Hello, world!</h1>;
   ~~~

- 참고
  - [https://ko.reactjs.org/docs/introducing-jsx.html](https://ko.reactjs.org/docs/introducing-jsx.html)
---

- state를 통해 header의 메시지 변경

  ~~~javascript
  // App.js
  ...
  class App extends Component {
    state = {
      appName: 'My First App'
    }
  ...
      <View style={styles.mainView}>
        <Header name={this.state.appName}/>
      </View>
  ...
  }
  ~~~

  ~~~javascript
  // header.js
  ...
  const Header = (props) => (
    <View style={styles.header}>
      <Text>{props.name}</Text>
    </View>
  )
  ...
  ~~~

- touchableOpacity 을 통해 touchEvent 생성

  ~~~javascript
  // header.js
  ...
  import { View, Text, StyleSheet, TouchableOpacity } from 'react-native';
  ...
  const Header = (props) => (
    <TouchableOpacity
      style={styles.header}
      // onPress 로 alert 띄우기
      onPress={()=alert('hello world')}
      // onLongPress 로 alert 띄우기
      onLongPress={()=alert('hello world')}
      // onPressIn 로 alert 띄우기
      onPressIn={()=alert('hello world')}
      // onPressOut 로 alert 띄우기
      onPressOut={()=alert('hello world')}
    >
      <View>
        <Text>{props.name}</Text>
      </View>
    </TouchableOpacity>
  )
  ...
  ~~~

## Button

- generator.js 파일생성

  ~~~bash
  # pwd = react_native_01
  $ mkdir src/generator.js
  ~~~

  ~~~javascript
  // generator.js

  import React from 'react';
  import { View, Text, StyleSheet, Button } from 'react-native';

  const Generator = () => {
    return (
      <View>
        <Button
          title="Add Number" // title : 필수 property
        />
      </View>
    )
  }

  const styles = StyleSheet.create({
    
  })

  export default Generator;
  ~~~

- App.js 에서 Generator 추가

  ~~~javascript
  ...
  import Header from './src/header';
  import Generator from './src/generator';
  ...
      return (
        <View>
          <Header name={this.state.appName}/>
          <View>
            <Text
              style={styles.mainText}
              onPress={()=>alert('text touch event')}
            >Hello World</Text>
          </View>
          <Generator/>
        </View>
      )
  ...
  ~~~

- ios, aos은 기본 버튼 모양이 다르므로, 스타일 정의가 필요

- generator.js 스타일 정의

  ~~~javascript
  // generator.js

  ...
    return (
      <View style={style.generator}>
        <Button
          title="Add Number"
          onPress={()=>alert('Button Pressed!!!')}
        />
  ...
  const styles = StyleSheet.create({
    generator: {
      backgroundColor: '#D197CF',
      alignItme: 'center',
      padding: 5,
      width: '100%'
    }
  })
  ...
  ~~~

- App.js 에 변수 추가

  ~~~javascript
  // App.js

  ...
    state = {
      appName: 'My First App',
      random: [36, 999]
    }

    onAddRandomNum = () => {
      alert('add random number!!!')
    }
  ...
        </View>
        <Generator add={this.onAddRandomNum}/>
      </View>
  ...
  ~~~

- generator에 props, props.add() 추가

  ~~~javascript
  // generator.js

  ...
  const Generator = (props) => {
    return (
  ...
          title="Add Number"
          onPress={()=>props.add()}
        />
  ...
  ~~~

- numlist.js 파일생성

  ~~~bash
  # pwd = react_native_01
  $ mkdir src/numlist.js
  ~~~

  ~~~javascript
  // numlist.js

  import React from 'react';
  import { View, Text, StyleSheet } from 'react-native';

  const NumList = (props) => {
    return (
      <View>
        <Text>Hello Again~~</Text>
      </View>
    )
  }

  const styles = StyleSheet.create({
    numList: {
      backgroundColor: '#cecece',
      alignItems: 'center',
      padding: 5,
      width: '100%',
      marginTop: 5
    }
  })

  export default NumList;
  ~~~

- App.js 에서 NumList 추가

  ~~~javascript
  ...
  import Generator from './src/generator';
  import NumList from './src/numlist';
  ...
      return (
        <View>
          <Header name={this.state.appName}/>
  ...
          <Generator add={this.onAddRandomNum}/>
          <NumList num={this.state.random}/>
        </View>
      )
  ...
  ~~~

- numlist에 props, props.add() 추가

  ~~~javascript
  // numlist.js

  ...
  const NumLisst = (props) => {
    return (
      props.num.map((item, idx)=>(
        <View style={styles.numList} key={idx}>
          <Text>{item}</Text>
        </View>
      ))
  ...
  ~~~

## TouchEvent 심화

- 'Add Number' 버튼을 누르면 랜덤숫자가 추가되고, 숫자를 누르면 해당 숫자가 제거되는 기능을 만들어 보겠음

- app.js 에서 style 수정

  ~~~javascript
  ...
    const style = StyleSheet.create({
      mainView: {
        ...
        // justifyContent: 'top' 삭제
      }
    })
  ~~~

- App.js 에 onAddRandomNum, onNumDelete 추가

  ~~~javascript
  ...
    onAddRandomNum = () => {
      const randomNum = Math.floor(Math.random()*100)+1; // 1~100 사이의 숫자 생성
      this.setState(prevState => {
        return random: [...prevState.random, randomNum]
      })
    }

    onNumDelete = (position) => {
      const newArray = this.state.random.filter((num, index)=>{
        return position != index;
      })
      this.setState({
        random: newArray
      })
    }
  ...
    <NumList
      num={this.state.random}
      delete={this.onNumDelete}
    />
  ...
  ~~~

- numlist.js 에 prop 추가

  ~~~javascript
  ...
  import { View, Text, StyleSheet, TouchableOpacity } from 'react-native';
  ...
    return (
      props.num.map((item, idx)=>(
        <TouchableOpacity
          style={styles.numList}
          key={idx}
          onPress={()=>props.delete(idx)}
        >
          <Text>{item}</Text>
        </TouchableOpacity>
      ))
    )
  ...
  ~~~

## ScrollView

- 화면을 스크롤할 수 있는 컴포넌트
- scrollView

  ~~~javascript
  ...
  import React, { Component } from 'react';
  import { View, Text, StyleSheet, ScrollView } from 'react-native';
  import Header form './src/header';
  ...
  <Generator add={this.onAddRandomNum}/>
  <ScrollView
    style={{width: '100%'}}
  >
    <NumList
      num={this.state.random}
      delete={this.onNumDelete}
    />
  </ScrollView>
  ...
  ~~~

### ScrollView의 Properties

#### onMomentumScrollBegin

- 스크롤이 움직이기 시작했을 때 트리거되는 함수

  ~~~javascript
  ...
  <ScrollView
    style={{width: '100%'}}
    onMomentumScrollBegin={()=>alert('begin')}
  >
  ...
  ~~~

#### onMomentumScrollEnd

- 스크롤의 움직임이 끝나면 트리거되는 함수

  ~~~javascript
  ...
  <ScrollView
    style={{width: '100%'}}
    onMomentumScrollEnd={()=>alert('end')}
  >
  ...
  ~~~

#### onScroll

- 스크롤이 움직이면 트리거되는 함수

  ~~~javascript
  ...
  <ScrollView
    style={{width: '100%'}}
    onScroll={()=>alert('Scrolling')}
  >
  ...
  ~~~

#### onContentSizeChange

- View의 크리가 바뀌면 트리거되는 함수

  ~~~javascript
  ...
  <ScrollView
    style={{width: '100%'}}
    onContentSizeChange={(width, height)=>alert(height)}
  >
  ...
  ~~~

#### bounces

- 스크롤이 바운스 되는 것을 허용/방지

  ~~~javascript
  ...
  <ScrollView
    style={{width: '100%'}}
    bounces={true}
  >
  ...
  ~~~

## TextInput

- 텍스트를 입력할 수 있는 컴포넌트
- src/input.js 생성

  ~~~javascript
  // input.js

  import React, { Component } from 'react';
  import { View, Text, StyleSheet, TextInput } from 'react-native';

  class Input extends Component {

    state = {
      myTextInput: 'text input'
    }

    onChangeInput = (event) => {
      this.setState({
        myTextInput: event
      })
    }

    render() {
      return (
        <View style={styles.mainView}>
          <TextInput
            value={this.state.myTextInput}
            style={styles.input}
            onChangeText={this.onChangeInput}
          />
        </View>
      )
    }
  }

  const styles = StyleSheet.create({
    mainView: {
      width: '100%'
    },
    input: {
      width: '100%',
      backgroundColor: '#cecece',
      marginTop: 20,
      fontSize: 25,
      padding: 10
    }
  })

  export default Input;
  ~~~

- App.js 에 추가

  ~~~javascript
  import React, { Component } from 'react';
  import { View, Text, }
  ~~~

### TextInput의 Properties

#### multiline

- 개행을 허용/방지

  ~~~javascript
  <TextInput
    multiline={true}
  >
  ~~~

#### maxLength

- 입력값의 길이를 제한

  ~~~javascript
  <TextInput
    maxLength={100}
  >
  ~~~

#### autoCapitalize

- 대문자 자동수정 허용/방지

  ~~~javascript
  <TextInput
    autoCapitalize={'none'}
  >
  ~~~

#### editable

- 입력을 허용/방지

  ~~~javascript
  <TextInput
    editable={true}
  >
  ~~~

## Button, ScrollView, TextInput 심화

- 버튼을 누르면 텍스트 입력창으로부터 값을 받아 하단의 목록으로 보여주는 App.js

  ~~~javascript
  import React, { Component } from 'react';
  import { TextInput, Button, View, StyleSheet, ScrollView } from 'react-native';

  class App extends Component {
    state = {
      maTextInput: '',
      alphabet: ['a','b','c','d']
    }

    onChangeInput = (event) => {
      this.setState({
        myTextInput: event
      })
    }

    onAddTextInput = () => {
      this.setState(prevState => {
        return {
          myTextInput: '',
          alphabet: [...prevState.alphabet, prevState.myTextInput]
        }
      }
    }

    render() {
      return (
        <View style={styles.mainView}>
        <TextInput
          value={this.state.myTextInput}
          style={styles.input}
          onChangeText={this.onChangeInput}
          multiline={true}
          maxLength={100}
          autoCapitalize={'none'}
          editable={true}
        >
        <Button
          title="Add Text Input"
          onPress={this.onAddTextInput}
        />

        <ScrollView style={{width: '100%'}}>
          {
            this.state.alphabet.map((item, idx) => (
              <Text
                styles={styles.main}
                key={idx}
              >
                {item}
              </Text>
            ))
          }
      )
    }
  }

  const style = StyleSheet.create({
    mainView: {
      flex: 1,
      backgroundColor:'white',
      paddingTop: 50,
      alignItems: 'center',
    },
    subView: {
      backgroundColor: 'yellow',
      marginBottom: 10,
    }
    anotherSubView: {
      flex: 2,
      backgroundColor: 'yellow',
      marginBottom: 10,
      width: '100%',
      alignItems: 'center',
      justifyContent: 'center'
    },
    mainText: {
      fontSize: 20,
      fontWeight: 'normal',
      color: 'red',
      padding: 20,
      margin: 20,
      backgroundColor: 'pink'
    }
    input: {
      width: '100%',
      backgroundColor: '#cecece',
      marginTop: 20,
      fontSize: 25,
      padding: 10
    } 
  })
  ~~~

## Picker

- 콤보박스 같은 여러가지 옵션이 있고 그 중에 선택할 수 있는 컴포넌트

### picker 설치

#### picker 다운로드

~~~bash
# install picker
$ npm install @react-native-community/picker --save  
~~~

#### pod install

~~~bash
$ cd ios
# pod install
$ npx pod-install
$ cd ..
~~~

#### picker 파일 생성

~~~bash
# pwd = react_native_01
$ mkdir src/picker.js
~~~

~~~javascript
// picker.js

import React, { Component } from 'react';
import { View, Text, StyleSheet } from 'react-native';
import { Picker } from '@react-native-community/picker';

class PickerComponent extends Component {
  state = {
    country: 'korea'
  }
  render() {
    return (
      <View style={styles.container}>
        <Picker
          style={{height:50, width: 250)}}
          selectedValue={this.country}
          onValueChange={(val, idx) => 
            this.setState({country: val})
          }
        >
          <Picker.Item label="Korea" value="korea">
          <Picker.Item label="Canada" value="canada">
          <Picker.Item label="China" value="china">
        </Picker>
      </View>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: 20,
    marginBottom: 200,
    alignItems: 'center'
  }
})

export default PickerComponent
~~~

#### App.js 에 추가

  ~~~javascript
  ...
  import { TextInput, Button, View, Text, StyleSheet, ScrollView} from 'react-native';
  import Picker from './src/picker';
  ...
  render() {
    return (
      <View style={styles.mainView}>
      <Picker/>
      ...
    )
  }
  ~~~

## Slider

- 가로선이 있고 좌우로 값을 바꿀 수 있는 컴포넌트

### Slider 설치

#### Slider 다운로드

~~~bash
# install slider
$ npm install @react-native-community/slider --save  
~~~

#### pod install

~~~bash
$ cd ios
# pod install
$ npx pod-install
$ cd ..
~~~

#### picker 파일 수정

~~~javascript
...
import { Picker } from '@react-native-community/picker';
import Slider from '@react-native-community/slider';
...
  state = {
    country: 'korea',
    value: 50
  }
  sliderValueChange=(value) => {
    this.setState({
      value: value
    })
  }
  ...
  render() {
    return (
      <View style={styles.container}>
        <Slider
          style={{height:40, width:300}}
          value={this.state.value}
          mininumValue={0}
          maximunValue={100}
          onValueChange={this.sliderValueChange}
          maximumTrackTintColor='red'
          minimumTrackTintColor='blue'
          step={10}
        />
        <Text
          style={styles.input}
        >
        {this.staet.value}
        </Text>
        ...

const style = StyleSheet.create({
  ...
  input:{
    width:'100%'
  }
})
~~~

## ActivityIndicator

- 화면전환 시 표시되는 화면

- picker 파일 수정

  ~~~javascript
  import React, { Component } from 'react';
  import { View, Text, StyleSheet, ActivityIndicator } from 'react-native';
  import { Picker } from '@react-native-community/picker';
  ...
    render() {
      return (
        <View style={styles.container}>
          ...
          <Text
            style={styles.input}
          >{this.state.value}</Text>
          <ActivityIndicator
            style={{paddingTop: 200}}
            size="large"
            color="green"
            animating={true} // false 로 변경
          />
          <Picker>
          ...
      )
    }
  ~~~

## Image

- 폴더 생성

  ~~~bash
  # pwd = react_native_01
  $ mkdir assets/images
  ~~~

### 로컬에서 파일 가져오기

- 폴더에 이미지 추가(파일명 steak.jpg)

- App.js 수정

  ~~~javascript
  import React, { Component } from 'react';
  import { TextInput, Button, View, Text, StyleSheet, ScrollView, Image } from 'react-native';
  ...
  import Steak from './assets/images/steak.jpg';
  ...
    render() {
      return(
        <View style={styles.mainView}>
          <Image
            style={styles.image}
            resizeMode='contain'
            source={Steak}
          />
      )
    }
  ...
  const style = StyleSheet.create({
    ...
    image: {
      width: '100%',
      height: 700
    }
  ~~~

### 원격에서 파일 가져오기

- picsum.photos 접속하여 이미지주소 가져오기

- App.js 수정

  ~~~javascript
  ...
    render() {
      return(
        <View style={styles.mainView}>
          <Image
            style={styles.image}
            source={{uri: `이미지 주소`}}
            resizeMode="contain"
            onLoadEnd={()=>alert('Image Loaded!')}
          >
          ...
      )
    }
  ...
  ~~~

## Modal

- 화면 가장 위에 표시되는 레이어로 사용자의 다른 작업을 막는 컴포넌트

### modal 파일 생성

~~~bash
# pwd = react_native_01
$ mkdir src/modal.js
~~~

~~~javascript
import React, { Component } from 'react';
import { View, Text, Button, Modal } from 'react-native';

class ModalComponent extends Component {
  state = {
    modal: false
  }

  handleModal = () => {
    this.setState({
      modal: this.stae.modal ? false : true
    })
  }

  render() {
    return (
      <View style={{width: '100%'}}>
        <Button
          title="Open Modal"
          onPress={this.handleModal}
        />
        <Modal
          visible={this.state.modal}
          animationType={'slide'} // none, slide, fade
          onShow={()=>alert('Warning!')}
        >
          <View style={{
            marginTop:60,
            backgroundColor: 'red'
          }}>
            <Text>This is modal content</Text>
          </View>
          <Button
            title="Go Back"
            onPress={this.handleModal}
          />
        </Modal>
      </View>
    )
  }
}
export default ModalComponent;
~~~

### App.js 에 추가

~~~javascript
...
import Steak from './asset/images/steak.jpg'
import Modal from './src/modal'
...
  render() {
    return(
      <View>
        <Modal/>
      </Veiw>
    )
  }
~~~



## 참고
- [https://www.inflearn.com/course/리액트-네이티브-기초](https://www.inflearn.com/course/리액트-네이티브-기초)
- [https://reactnative.dev/docs/components-and-apis](https://reactnative.dev/docs/components-and-apis)
- [https://ko.reactjs.org/docs/getting-started.html](https://ko.reactjs.org/docs/getting-started.html)
- [https://reactnative.dev/docs/touchableopacity](https://reactnative.dev/docs/touchableopacity)