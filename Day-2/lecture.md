## Expo and React Native

Expo is a platform that helps you develop, build, deploy, and quickly iterate on React Native mobile apps. It provides a range of tooling to make development with React Native even easier. Although Expo isn’t required to create React Native apps, it helps developers by removing the need for basic native knowledge.

#### **Expo Snack** is a web-based playground where you can write React Native snippets and run them in the browser.

#### **Expo Go** is an app you can download on your phone to “view” your app in development.

#### **Expo CLI** is a tool to create, manage, and develop your apps.

#### **Expo SDK** is a modular set of packages that provide access to native APIs, like Camera or Notifications.

#### **Expo Application Services (EAS)** deeply integrated cloud services for Expo and React Native apps, from the team behind Expo.

  - EAS Build: Compile and sign Android/IOS apps with custom native code in the cloud.
  - EAS Submit: Upload your app to the Apple App store or google play store from the cloud with one CLI command.
  - EAS Update: Address small bugs and push quick fixes directly to end-users.

Now you may ask, Do companies use Expo ? 2 Years back, companies are prefering bare react native project, because expo don't have sufficient facilities to access native components. Previously Expo also aware of that, so there was an eject option, using which any time you can eject from expo to bare react native project. 

Expo depricated eject from SDK 46 August 2022. The concept of ejecting was replaced by popular expo prebuild feature.

Unlike the expo eject library, authors can configure their libraries to work with Expo Prebuild by creating a config plugin. This means you can use any library with Expo Prebuild. You can also use any custom native code with Expo Prebuild by creating a development build.

For these reasons, now a days most of the start-ups prefer expo.

If we summarise, the reasons may be :
  - Easy to develop application.
  - Using windows system we can develop both android and ios apps.
  - Without installing the actual app, then can verify by scanning the QR code.
  - Easy to create build.

## TextInput in ReactNative

A foundational component for inputting text into the app via a keyboard. Props provide configurability for several features, such as auto-correction, auto-capitalization, placeholder text, and different keyboard types, such as a numeric keypad. 

Porps:

- onSubmitEditing: onSubmitEditing is triggered when you click the text input submit button (keyboard button). `event.nativeEvent.text` will give the value of input.
- onChangeText: onChangeText is triggered when you type anything in the text input. Its typical use to update the state of Component with TextInput value like Reactjs onChange event.
- placeHolder
- placeHolderTextColor
- onPressIn: Callback that is called when a touch is engaged.
- onPressOut: Callback that is called when a touch is released.
- onFocus: Callback that is called when the text input is focused.

```js
import React, {useState} from "react";
import { SafeAreaView, StyleSheet, TextInput, Platform,TouchableOpacity, Text } from "react-native";

const App = () => {
  const [text, onChangeText] = useState("");
  const [number, onChangeNumber] = useState("");


  return (
    <SafeAreaView style={styles.container}>
      <TextInput
        style={styles.input}
        onChangeText={onChangeText}
        value={text}
        placeholder={"Enter Name"}
      />
      <TextInput
        style={styles.input}
        onSubmitEditing={(event) => onChangeNumber(event.nativeEvent.text)}
        value={number}
        onChangeText={onChangeNumber}
        placeholder="Enter Age"
        keyboardType="numeric"
      />
      <TouchableOpacity onPress={() => alert(`${text} - ${number} `)} style={styles.button}>
        <Text style={styles.btnText}>SUBMIT</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  container : {
    marginTop: Platform.OS === "android" ? 24 : 0
  },
  input: {
    height: 40,
    margin: 12,
    borderWidth: 1,
    borderRadius: 5,
    padding: 10,
  },
  button:{
    backgroundColor:'red',
    marginHorizontal:10,
    borderRadius:5,
    padding:5,
    alignItems:'center'
  },
  btnText:{
    color:'#FFF'
  }
});

export default App;
```

## Vector Icons

`@expo/vector-icons` or `react-native-vector-icons` can be used to include popular icon sets in our app. `@expo/vector-icons`  is built on top of `react-native-vector-icons` and uses similar API.

`https://oblador.github.io/react-native-vector-icons/` can be used to search for any icon. The icon sets are `AntDesign`, `Entypo`,
`EvilIcons`,`Feather`, `FontAwesome`, `FontAwesome5`, `FontAwesome5Brands`, `Fontisto`, `Foundation`, `Ionicons`, `MaterialCommunityIcons`, `MaterialIcons`, `Octicons`, `SimpleLineIcons`, `Zocial`.

```js
import * as React from 'react';
import { View, StyleSheet } from 'react-native';
import Ionicons from '@expo/vector-icons/Ionicons';

export default function App() {
  return (
    <View style={styles.container}>
      <Ionicons name="md-checkmark-circle" size={32} color="green" />
      <Ionicons name="eye-off" size={32} color="green" />
      <Ionicons name="fast-food" size={32} color="green" />
      <Ionicons name="finger-print" size={32} color="green" />                  
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
});
```

## Custom Buttons using TouchableOpacity or Pressable

In our previous session we have seen that the Button component's UI customisation is limited to the color prop only and buttons look different on different platforms. Therefore, React Native offers touchable and pressable components to create customise and cross-platform elements.

Showing feedback effects for a user gesture is definitely a good UI/UX practice. For example, if we change a particular menu item’s color for the pressed state, users know that the app accepted the most recent user gesture. Therefore, every app typically shows feedback animations for interactive UI elements. Let us discuss Touchable and Pressable in detail to know which we should use and why ?

### TouchableOpacity

It is a core component that reduces the opacity level to 0.2 as the touch event feedback. It internally uses the Animated.View component to implement the opacity transition. We will read about Animated.View in later sessions. We can also use style prop for styling. It also supports child components, which allow us to create touchable images, buttoms or complex list items.

- activeOpacity: Pressed opacity level can be adjusted with this prop.
- disabled: Disable the opacity animation and touch event callback with the disabled prop.
- hitSlop: Defines the extra area of user's touch action. This prop helps us to create an UI element smaller than a fingertip on screen. We can define a number or Rect Object (object containing 4 different values for bottom, left, right, top).
- pressRetentionOffset: Defines how far the user needs to move their finger from the region to deactivate a pressed button. This offset value includes the hitSlop value, too.
- 

```js
import React, {useState} from 'react'
import {Button, Text, View, Pressable, SafeAreaView, TouchableOpacity} from 'react-native'

const App = () => {
  const [touchPressed, setTouchPressed] = useState(false);

  return(<SafeAreaView style={{marginTop:25, alignItems:'center'}}>
    <Button title="click me" color="green" onPress={()=>alert("Clicked")}/>
    <TouchableOpacity style={{backgroundColor: touchPressed ?'green' :'red', borderRadius:5, padding:5 }}
      onPressIn={()=>setTouchPressed(true)}
      onPressOut={()=>setTouchPressed(false)}
      activeOpacity={1}
      onLongPress={()=>console.log('Long Pressed')}
      hitSlop={{top: 100, left: 50, right: 50, bottom: 100}}
      pressRetentionOffset={200}
    >
      <Text style={{color:'#fff'}}>Touchable Opacity Button</Text>
    </TouchableOpacity>
  </SafeAreaView>)
}

export default App;
```

- callbacks : We can attach functions to various events in a particular touch event flow with the onPress, onPressIn, onPressOut, and onLongPress callbacks.

React Native offers four different touchable components to handle basic user gestures. In below table I have given the differences among them. No need to go in details because all these we are going to create using advanced pressable component. It is only for reference.

Touchable Component Name | Touchable Opacity | TouchableHighlight | TouchableWithoutFeedback | TouchableNativeFeedback
-------------------------|-------------------|--------------------|--------------------------|------------------------
touch event feedback | reduces opacity | changes background color |  no feedback | Plays the android-specific ripple effect
Platform Compatibility | Both Android & IOS |  Both Android & IOS |  Both Android & IOS | Only Android
Child Element | Able to wrap multiple child elements | Able to wrap only one child element. If multiple elements are to be wrapped then wrap them first in a View | Able to wrap only one child element. If multiple elements are to be wrapped then wrap them first in a View | Able to wrap only one child element. If multiple elements are to be wrapped then wrap them first in a View 

## Pressable Component

Pressable components are introduced in React Native with the Pressability API in v0.63. If you use Expo, then use Expo SDK40 or later. 

As discussed earlier in this session, React Native offers four different touchable components to handle basic user gestures. But with Pressable we can create all those predefined touchable components with new added effects like zoom, rotation etc. 

Instead of offering multiple inbuilt, animated components, as touchable components do, The React Native Pressability API offers us one core component called Pressable. 

In the below example, We will create several buttons with different feedback animations. You can try something else rather then these.

```js
import React, { useState } from 'react';
import {
  SafeAreaView,
  Pressable,
  StyleSheet,
  Text,
} from 'react-native';

const PressableOpacityButton = ({title, onTap}) => {
  return (
    <Pressable
      onPress={onTap}
      style={({ pressed }) => [
        {
          opacity: pressed
            ? 0.2
            : 1,
          backgroundColor: '#2277ee'
        },
        styles.button,
      ]}>
        <Text style={styles.buttonText}>{ title }</Text>
    </Pressable>
  );
}

const PressableHighlightButton = ({title, onTap}) => {
  return (
    <Pressable
      onPress={onTap}
      style={({ pressed }) => [
        {
          backgroundColor: pressed
            ? '#55aaff'
            : '#2277ee'
        },
        styles.button
      ]}>
        <Text style={styles.buttonText}>{ title }</Text>
    </Pressable>
  );
}

const ZoomButton = ({title, onTap}) => {
  return (
    <Pressable
      onPress={onTap}
      style={({ pressed }) => [
        {
          transform: [{
            scale: pressed ? 1.07 : 1
          }],
          backgroundColor: '#2277ee'
        },
        styles.button
      ]}>
        <Text style={styles.buttonText}>{ title }</Text>
    </Pressable>
  );
}

const RotatingButton = ({title, onTap}) => {
  return (
    <Pressable
      onPress={onTap}
      style={({ pressed }) => [
        {
          transform: [{
            rotate: pressed ? '-5deg' : '0deg'
          }],
          backgroundColor: '#2277ee'
        },
        styles.button
      ]}>
        <Text style={styles.buttonText}>{ title }</Text>
    </Pressable>
  );
}

const ComplexButton = ({title, onTap}) => {
  return (
    <Pressable
      onPress={onTap}
      android_ripple={{color: '#fff'}}
      style={({ pressed }) => [
        pressed && {
          transform: [{
            rotate: '-5deg'
          },{
            scale: 0.7
          }]
        },
        {
          backgroundColor: '#2277ee'
        },
        styles.button,
      ]}>
        {({pressed}) => (
          <Text style={[styles.buttonText, pressed && {color: '#FFF'}]}>{ title }</Text>
        )}
    </Pressable>
  );
}


const App = () => {
  const onTap = () => {
    console.log("Pressed");
  }
  return (
    <SafeAreaView style={styles.container}>
      <PressableOpacityButton title="TouchableOpacity" onTap={onTap}/>
      <PressableHighlightButton title="TouchableHighlight"  onTap={onTap}/>
      <ZoomButton title="Zoom effect"  onTap={onTap}/>
      <RotatingButton title="Rotation effect"  onTap={onTap}/>
      <ComplexButton title="Complex effect"  onTap={onTap}/>
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    marginHorizontal: 36,
  },
  button: {
    padding: 12,
    marginBottom: 20,
    borderRadius: 6,
  },
  buttonText: {
    fontSize: 20,
    textAlign: 'center'
  },
});

export default App;
```

## Difference between Touchable and Pressable

Touchable | Pressable
----------|----------
Offers four different Touchable components: TouchableOpacity, TouchableHighlight, TouchableWithoutFeedback, TouchableNativeFeedback | Offers only one pressable component: Pressable
hitSlop & pressRetentionOffset for customizing the accepted user gesture area | It also supports hitSlop & pressRetentionOffset
offers subscribing to gesture lifecycle events through opPress, onPressIn, onPressOut and onLongPress callbacks. | It also supports 
limited touch event inbuilt feedback animation. Suitable for primary developer requirements. | Doesn’t offer any inbuilt feedback animations. The developer has to implement feedback effects as per requirement. Very flexible and customisable, so able to fulfil advanced developer requirements.


## Which to be used, Touchable or Pressable ?

Touchable components come with pre-built feedback animations with limited customization options — so you can use them more quickly, but you will face issues with customization. Meanwhile, the Pressability API lets you build feedback animations as you wish, so you have full freedom to implement feedback effects even though it takes some effort to implement. 

But overusing feedback animations reduces your app’s quality, so don’t use too many animations or colorful effects for user gestures . A minimal feedback animation that’s in line with your application’s theme will keep your app’s look professional.

If you use a UI kit like NativeBase, you don’t need to use both component types directly — since UI kits typically come with a pre-defined gesture feedback system.

# Animated View

Animation can be defined as manipulating images or objects to appear as moving images or objects. React native provide you with an animated API, which mean animated is an object provided by react native which provide heavy lifting of animation and coordination with the native operating systems such as Android and IOS.

Before starting remember the requirement needed.

- Define the starting value or location (x,y coordinate of screen) of the animation.
- Define the end value or ending location of the animation.
- Define which element we are animating.
- Define the type of animation you want
- Define the duration in which animation should get completed

# Animated View Basic Example

```js
import React, {useRef, useState} from 'react'
import { Animated, Text,SafeAreaView, View, StyleSheet, TouchableOpacity, Platform } from 'react-native';

export default function App() {
  const br = useRef(new Animated.Value(0)).current;
  const rt = useRef(new Animated.Value(0)).current;
  const [isCircle, setIsCircle ] = useState(false);

  const makeCircle = () => {
    Animated.parallel([
      Animated.spring(rt, {toValue: 1, useNativeDriver: true}),
      Animated.spring(br, {toValue: 50, useNativeDriver: true})
    ]).start()
    
    setIsCircle(true);
  }

  const makeSquare = () => {
    Animated.parallel([
      Animated.spring(rt, {toValue: 0, useNativeDriver: true}),
      Animated.spring(br, {toValue: 0, useNativeDriver: true})
    ]).start()
    setIsCircle(false);
  }

  return (
    <SafeAreaView style={styles.container}>
      <Animated.View style={[styles.square,{borderRadius:br, 
                                transform: [
                                  {rotate: rt.interpolate({
                                    inputRange: [0,1],
                                    outputRange: ['0deg', '135deg'],
                                  })}
                                ]}]}></Animated.View>
      <TouchableOpacity style={styles.btn} onPress={ isCircle ? makeSquare : makeCircle}>
        <Text style={styles.btnText}> {isCircle ? "Square" : "Circle"}</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems:'center',
    paddingTop: Platform.OS =='android' ? 24 : 0,
    backgroundColor: '#ecf0f1',
    padding: 8,
  },
  square : {
    width:100,
    height:100,
    backgroundColor:'red',
  },
  btn : {
    marginTop:20,
    backgroundColor:'blue',
    borderRadius:5,
    paddingHorizontal:10,
    paddingVertical:5
  },
  btnText:{
    color:'#fff'
  }
});
```

## Using the native driver

The Animated API is designed to be serializable. By using the native driver, we send everything about the animation to native before starting the animation, allowing native code to perform the animation on the UI thread without having to go through the bridge on every frame. Once the animation has started, the JS thread can be blocked without affecting the animation.

Using the native driver for normal animations is straightforward. You can add useNativeDriver: true to the animation config when starting it.

## Configuring animations

`Animated` provides three types of animation types. Each animation type provides a particular animation curve that controls how your values animate from their initial value to the final value:

Animated.decay() starts with an initial velocity and gradually slows to a stop.
Animated.spring() provides a basic spring physics model.
Animated.timing() animates a value over time using easing functions.

```js
Animated.timing({}).start(({ finished }) => {
  /* completion callback */
});
```
## Animatable Components

- Animated.Image
- Animated.ScrollView
- Animated.Text
- Animated.View
- Animated.FlatList
- Animated.SectionList

## Composing Animations

- Animated.delay() starts an animation after a given delay.
- Animated.parallel() starts a number of animations at the same time.
- Animated.sequence() starts the animations in order, waiting for each to complete before starting the next.
- Animated.stagger() starts animations in order and in parallel, but with successive delays.

## Combining animated values
You can combine two animated values via addition, subtraction, multiplication, division, or modulo to make a new animated value:

- Animated.add()
- Animated.subtract()
- Animated.divide()
- Animated.modulo()
- Animated.multiply()

## Interpolation

Each property can be run through an interpolation first. An interpolation maps input ranges to output ranges.

```js
  style={{
    opacity: fadeAnim, // Binds directly
    transform: [{
      translateY: fadeAnim.interpolate({
        inputRange: [0, 1],
        outputRange: [150, 0]  // 0 : 150, 0.5 : 75, 1 : 0
      }),
    }],
  }}
```

interpolate() supports multiple range segments as well

```js
value.interpolate({
  inputRange: [-300, -100, 0, 100, 101],
  outputRange: [300, 0, 1, 0, 0]
});
```

