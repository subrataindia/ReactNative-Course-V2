
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

  
## Styling in React Native
  
All the react native core components accepts a prop named `style`. The value that is assigned to this prop is a JavaScript object of key value pairs. The key value pair is similar to property and value of CSS rule with one exception that CSS properties are written in camel case. For example, `background-color` is written as `backgroundColor`.

We can style in three different ways.
  
### Example of inline styling  

```js
import React from "react";
import { View, Text } from "react-native";

const App = () => {
  return (
    <View style={{backgroundColor:'#333300', padding:10}}>
      <Text style={{color:'#fff'}}>Hello World!</Text>
    </View>
  );
};

export default App;  
```  
  
### Example of internal styling    

```js
import React from "react";
import { View, Text, StyleSheet } from "react-native";

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Hello World!</Text>
    </View>
  );
};

export default App;

const styles = StyleSheet.create({
  container: {
    backgroundColor:'#333300', 
    padding:10
  },
  text : {
    color:'#fff'
  }
})
```  

### Example of external styling
  
We can also place the styles object in another JS file and import it to use in this file.

## Display Flex Property
  
By default the View of React Native is assigned `display:flex` and `flexDirection: 'column'`, But we can change the flexDirection to `row`, `row-reverse` or `column-reverse`
  
```js
import React from 'react'
import {View, Text, Platform} from 'react-native'

const App = () => {
  return(<View style={{flexDirection:'row', marginTop: Platform.select({android:24,ios:47})}}>
    <Text style={{flex:1, backgroundColor:'red'}}>Text 1</Text>
    <Text style={{flex:2, backgroundColor:'green'}}>Text 2</Text>
  </View>)
}

export default App;
```

In the above example we have used `flex` to set the width. The flex of text1 is set to 1 and of text2 is set to 2. So the available width will be divided in to 3 equal parts out of which 2 parts will be assigned to text2 and 1 part will be assigned to text1. Instead of this, we can use percentage also for width.


# Showing list of items

Let us write a program which will display a list of items from an array on the screen. If we click on add item. It will add an item to the array.

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, Button} from 'react-native'
import  Constants  from 'expo-constants'


const App = () => {
  const [data, setData] = useState([])

  const addItem = () => {
    setData(prev => [...prev, 'item '+Math.ceil(Math.random()*100)]);
  }
  return(<View style={styles.container}
  >
    <Button title="Add Item" onPress={addItem}/>
    <Text style={styles.heading}>List of Items</Text>
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container : {
    marginTop: Constants.statusBarHeight
  },
  item : {
    borderRadius:10,
    borderWidth:1,
    padding:5,
    margin:5,
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  }
})
```

Can you tell me what is the drawback of this program ? 

Ans: When the list of item is large, some items are going out of screen. 

The list should be scrollable. To make a large list scrollable, we can use ScrollView or FlatList.

## ScrollView

The ScrollView is a generic scrolling container that can contain multiple components and views. By default it is a vertical scrollable container but you can make it horizontal by specifying horizontal property.

Let us modify our previous program and wrap our list of items with in a scrollview.

### Example of vertical list of items

```js
    <ScrollView>
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
```

### Example of horizntal list of items

```js
    <ScrollView horizontal>
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
```

Note : The ScrollView works best to present a small number of things of a limited size. All the elements and views of a ScrollView are rendered, even if they are not currently shown on the screen. If you have a long list of items which cannot fit on the screen, you should use a FlatList instead.

Before moving to FlatList let us find out, How to attach a pull to refresh to our ScrollView.

## Pull to Refresh

`RefreshControl` component is used inside a ScrollView to provide pull to refresh functionality.

most used props
- onRefresh: Function to be executed when the ScrollView is pulled down.
- refreshing : when true displays spinner. So at the beginning of `onRefresh` method set it true then at the end set it false.
- colors: on Android sets the spinner color
- tintColor: on IOS sets the spinner color

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, RefreshControl, ScrollView} from 'react-native'
import  Constants  from 'expo-constants'

const App = () => {
  const [data, setData] = useState([])
  const [refreshing, setRefreshing] = React.useState(false);

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'item '+Math.ceil(Math.random()*100)]);
    setRefreshing(false);
  }
  return(
    <View style={styles.container}>
      <Text style={styles.heading}>List of Items</Text>
      <ScrollView
    refreshControl={<RefreshControl refreshing={refreshing} onRefresh={onRefresh} colors={['red']} tintColor='green' />}
  >
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container :{
    marginTop: Constants.statusBarHeight
  },
  item : {
    borderRadius:10,
    borderWidth:1,
    padding:5,
    margin:5,
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  }
})
```

As this task ends quickly so you may not see the spinner properly. While getting data from server you may see it properly. To test it now letus create a function which intentionaly delay the process, so that we can see the spinner.

```js
  const wait = (timeout) => {
    return new Promise(resolve => setTimeout(resolve, timeout));
  }

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'item '+Math.ceil(Math.random()*100)]);
    wait(2000).then(() => setRefreshing(false));
  }
```

## Image

# Image in React Native

A React component for displaying different types of images, including network images, static resources, temporary local images, and images from local disk, such as the camera roll.

```js
import React from 'react'
import { View, Image, StyleSheet } from 'react-native'
import snackIcon from './assets/snack-icon.png'

const App = () => {
  return(<View>
    <Image source={{uri:"https://cdn.pixabay.com/photo/2021/11/22/05/02/dalmatian-6815838_960_720.jpg"}} style={styles.roundImage}/>
    <Image source={require('./assets/snack-icon.png')} style={styles.squareImage}/>
    <Image source={snackIcon} style={styles.squareImage}/>
  </View>)
}

export default App;

const styles = StyleSheet.create({
  squareImage : {
    width:100,
    height:100,
    borderRadius:10  
  },
  roundImage: {
    width:100,
    height:100,
    borderRadius:'50%'  
  }
})

```

### Image Props

- source: specifies source of the image
- resizeMode: Determines how to resize the image when the frame doesn't match the raw image dimensions.
    Value of this prop can be any one of the following. The default is `cover`
  - cover: Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or larger than the corresponding dimension of the view (minus padding). at least one dimension of the scaled image will be equal to the corresponding dimension of the view (minus padding). 
  - contain: Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or less than the corresponding dimension of the view (minus padding).
  - stretch: Fit the width and height of image to mentioned width and height, This may change the aspect ratio.
  - repeat: Repeat the image to cover the frame of the view. The image will keep its size and aspect ratio, unless it is larger than the view, in which case it will be scaled down uniformly so that it is contained in the view.
  - center: Center the image in the view along both dimensions. If the image is larger than the view, scale it down uniformly so that it is contained in the view.


## Simple Gallery

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, Image, Pressable} from 'react-native'
import  Constants  from 'expo-constants'

const data = ["https://cdn.pixabay.com/photo/2022/11/25/16/30/brown-hairstreak-7616422_960_720.jpg", "https://cdn.pixabay.com/photo/2012/03/01/00/55/flowers-19830_960_720.jpg","https://cdn.pixabay.com/photo/2014/02/27/16/10/flowers-276014_960_720.jpg","https://cdn.pixabay.com/photo/2014/04/14/20/11/pink-324175_960_720.jpg","https://cdn.pixabay.com/photo/2015/07/05/10/18/tree-832079_960_720.jpg"]

const App = () => {
  const [images, setImages] = useState(data);

  const deleteImage = (i) => {
    setImages(prev => prev.filter((_, index) => index !== i))
  }

  return(<View style={styles.container}>
      <Text style={styles.heading}>Simple Gallery</Text>
      <View>
        {images.map((img, index) => 
          <Pressable onLongPress={() => deleteImage(index)} key={index}> 
            <Image source={{uri:img}} style={{width:"100%", height:120, resizeMode: 'cover'}}/>
          </Pressable>)
        }
        {images.length === 0 && <Text style={{textAlign:'center', marginTop:40}}>No Images found in gallery</Text>}
      </View>
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container :{
    marginTop: Constants.statusBarHeight
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  }
})
```
