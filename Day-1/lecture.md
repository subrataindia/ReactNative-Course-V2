
# Prerequisites

I assume that you have a sound knowledge of JavaScript, ES6 and React. No prior knowledge of android or IOS development is required.

# Introduction

Almost any business, especially B2C could benefit from creating App for their business. When we are thinking about app development we have take into consideration two major mobile operating systems Android and IOS. Android app can be developed using Java or Kotlin and IOS app can be developed using swift which is mostly written in C++. This type of development is called **Native Development**. There is an alternative to this which is called **Hybrid Development**. In **Hybrid Development** you have to develop using only one language or framework and it will run both on Android and IOS.

In Hybrid Development there are two famous frameworks. React Native developed by facebook and Flutter developed by Google. 

# Why choose React Native ?

- React Native uses JavaScript while Flutter uses Dart. All of us know that JavaScript is a popular language and uses for webdevelopment as well as backend development. So learning JavaScript is suggestible. As you already know JavaScript and React, It will be easier for you.
- React Native released in 2015 and Flutter released in 2017. React native is mature and it has a large community support which makes tasks easier.
- React Native can call Native functions and vice versa.

# Create First React Native App

# A Simple Counter App in React Native

```js
import React, {useState, useEffect} from 'react'
import {View, Text, Button, Alert} from 'react-native'

const App = () => {
  const [count, setCount] = useState(0);

  const increase = () => {
    setCount(prev => prev + 1);
  }

  const decrease = () => {
    setCount(prev => prev - 1)
  }

  return(<View style={{flexDirection:'row', justifyContent:'center', marginTop:47}}>
    <Button 
      title="-"
      onPress={decrease}
    />
    <Text style={{margin:10}}>{count}</Text>
    <Button 
      title="+" 
      onPress={increase}
    />
  </View>)
}

export default App;
```

Now see this counter app in android, IOS and Web. Do you observe any difference ? Yes, the difference is the height of the counter from top. We have hardcoded it to 47. But it looks different for different platforms. 

It means to look this perfect, we have to write platform specific code. (Button is also looking different in IOS. It don't have background color. But we will discuss about that on next session.)

## Platform Specific Code

#### Let us write platform specific code for marginTop using `platform` module.

```js
marginTop: Platform.OS === 'ios' ? 47 : Platform.OS === "android" ? 24 : 0
```

or

```js
marginTop: Platform.select({ios: 47, android:24})
```

Here we have not mentioned any size for `web` so automatically `zero` will be assigned to it.

#### Let us write platform specific code for marginTop using platform specific file extension

In this case we have a little change so using `platform` module is suggested. But there may be cases where you have to make a lot of changes if platform changes. In that case writing platform specific files are suggested.

write three variations of `App.js` file as `App.android.js` , `App.ios.js` and `App.web.js`.

```js
// App.android.js 
...
marginTop : 24
...
```

```js
// App.android.js 
...
marginTop : 47
...
```

```js
// App.js
...
marginTop: 0
...
```

In the above example `App.js` will work for all platforms other then `android` and `ios`.


## Use of Alert

If counter is zero and user is pressing `-` button, then display a message "The counter can't be lessthen zero" .

```js
import React, {useState, useEffect} from 'react'
import {View, Text, Button, Alert, Platform} from 'react-native'

const App = () => {
  const [count, setCount] = useState(0);

  const increase = () => {
    setCount(prev => prev + 1);
  }

  const decrease = () => {
    setCount(prev => {
      if(prev <= 0){
        if(Platform.OS === "web"){
          alert("The Counter Can't be Lessthen Zero!")
        }else{
          Alert.alert("Counter Alert","The counter can't be lessthen zero!");
        }
        return 0;
      }else{
        return prev - 1;
      }
    })
  }

  return(<View style={{flexDirection:'row', justifyContent:'center', marginTop: Platform.select({ios: 47, android:24}) }}>
    <Button 
      title="-"
      onPress={decrease}
    />
    <Text style={{margin:10}}>{count}</Text>
    <Button 
      title="+" 
      onPress={increase}
    />
  </View>)
}

export default App;
```


