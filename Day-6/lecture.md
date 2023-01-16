# Async Storage

- unencrypted, Asynchronous, Persistent, key-value storage system
- Global Local Storage
- Offline Local Storage
- can only store string data
- To store object use `JSON.stringify()` while storing and `JSON.parse()` after reading.
- Don't use it to store secrets
- AsyncStorage is not shared between multiple apps. Every app runs it it's own sandbox environment and has no access to other apps
- core component is deprecated, use npm package: @react-native-async-storage/async-storage

```js
// Counter APP using AsyncStorage
import React, { useEffect, useState } from "react";
import { View, Text, TouchableOpacity } from "react-native";
import AsyncStorage from "@react-native-async-storage/async-storage";

const storeData = async (key, value) => {
  try {
    await AsyncStorage.setItem(key, value);
    console.log("Successfully stored data to async storage", value);
  } catch (e) {
    // saving error
    console.log("Error saving data to async storage.");
  }
};

const App = () => {
  const [counter, setCounter] = useState(null);

  const inc = () => {
    setCounter((prev) => prev + 1);
  };

  const dec = () => {
    setCounter((prev) => prev - 1);
  };

  useEffect(() => {
    if(counter !== null)
      storeData("counter", counter);
  }, [counter, setCounter]);

  useEffect(() => {
    AsyncStorage.getItem("counter")
      .then((data) => {
        console.log(typeof data, data);
        setCounter(parseInt(data));
      })
      .catch((e) => console.log("Error receiving data"));
  }, []);

  return (
    <View
      style={{
        flexDirection: "row",
        alignItem: "center",
        justifyContent: "center"
      }}
    >
      <TouchableOpacity onPress={dec}>
        <Text>-</Text>
      </TouchableOpacity>
      <Text style={{ marginHorizontal: 20 }}>{counter}</Text>
      <TouchableOpacity onPress={inc}>
        <Text>+</Text>
      </TouchableOpacity>
      {/* <TouchableOpacity onPress={()=>{
        AsyncStorage.getItem('counter').then(d => {
          console.log("Counter data:",d)
          setCounter(d);
        })
      }}>
        <Text>Get Data</Text>
      </TouchableOpacity> */}
      <TouchableOpacity onPress={()=>{
        storeData('counter',0);
        setCounter(0);
      }}>
        <Text>Reset Data</Text>
      </TouchableOpacity>
    </View>
  );
};

export default App;
```

## Modal

The Modal component is a basic way to present content above an enclosing view.

### Props

- inherits `View` Props
- visible: `true or false` : determines the visibility of modal
- onRequestClose: `function` : It is called when the user taps hardware back button on android or the menu button on IOS. 
- animationType: `none, slide, fade` : controls modal animation, default is none
- onShow: `function` : The passed function will be called once the modal is shown
- transparent: `true or false` : If true renders the modal over a transparent background and fill the entire view.

```js
import React, { useState } from "react";
import { Alert, Modal, StyleSheet, Text, Pressable, View, TouchableOpacity } from "react-native";

const App = () => {
  const [modalVisible, setModalVisible] = useState(false);
  return (
    <View style={styles.centeredView}>
      <Modal
        animationType="slide"
        visible={modalVisible}
        transparent={true}
        onRequestClose={() => {
          Alert.alert("Modal has been closed.");
          setModalVisible(!modalVisible);
        }}
      >
      <TouchableOpacity 
        style={{backgroundColor:'rgba(153, 153, 102,0.5)', flex:1}} 
        onPress={() => setModalVisible(false)}>
        <View style={styles.centeredView}>
          <View style={styles.modalView}>
            <Text style={styles.modalText}>Hello World!</Text>
            <Pressable
              style={[styles.button, styles.buttonClose]}
              onPress={() => setModalVisible(!modalVisible)}
            >
              <Text style={styles.textStyle}>Hide Modal</Text>
            </Pressable>
          </View>
        </View>
        </TouchableOpacity>
      </Modal>
      <Text style={{marginBottom:180, fontSize:22}}>Modal Example</Text>
      <Pressable
        style={[styles.button, styles.buttonOpen]}
        onPress={() => setModalVisible(true)}
      >
        <Text style={styles.textStyle}>Show Modal</Text>
      </Pressable>
    </View>
  );
};

const styles = StyleSheet.create({
  centeredView: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    marginTop: 22
  },
  modalView: {
    margin: 20,
    backgroundColor: "white",
    borderRadius: 20,
    padding: 35,
    alignItems: "center",
    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 2
    },
    shadowOpacity: 0.25,
    shadowRadius: 4,
    elevation: 5
  },
  button: {
    borderRadius: 20,
    padding: 10,
    elevation: 2
  },
  buttonOpen: {
    backgroundColor: "#F194FF",
  },
  buttonClose: {
    backgroundColor: "#2196F3",
  },
  textStyle: {
    color: "white",
    fontWeight: "bold",
    textAlign: "center"
  },
  modalText: {
    marginBottom: 15,
    textAlign: "center"
  }
});

export default App;
```

- CheckBox: https://github.com/react-native-checkbox/react-native-checkbox
- DropDown: https://www.npmjs.com/package/react-native-element-dropdown
- Switch: https://reactnative.dev/docs/switch

# UI Library: REACT-NATIVE-PAPER

`React Native Paper` is a collection of customizable and production-ready components for React Native, following Google’s Material Design guidelines. Go through [getting started](https://callstack.github.io/react-native-paper/getting-started.html) documentation.

```js
// ./screens/Products.js
import React, {useState, useEffect} from 'react'
import {SafeAreaView, FlatList} from 'react-native'
import { Avatar, Button, Card, Text } from 'react-native-paper';
import { useNavigation } from '@react-navigation/native';

const Products = () => {
  const [data, setData] = useState([]);
  const navigation = useNavigation();

  useEffect(()=>{
    fetch('https://fakestoreapi.com/products')
            .then(res=>res.json())
            .then(json=> {
              setData(json);
            })
  },[])

  const renderItem = ({item}) => {


    return(
      <Card style={{margin:10}} mode="outlined">
        <Card.Title title={item.title} subtitle={item.category} />
        <Card.Cover source={{ uri: item.image }} resizeMode= 'center' style={{backgroundColor:"#FFF"}} />
        <Card.Content style={{marginTop:15}}>
          <Text variant="bodyMedium" style={{textAlign:'justify'}}>{item.description}</Text>
        </Card.Content>
        <Card.Actions>
          <Button onPress={()=> navigation.navigate("singleitem",{id:item.id})} mode={"outlined"}>View Details</Button>
        </Card.Actions>
      </Card>      
    )
  }

  return(<SafeAreaView>
    <FlatList data={data} renderItem={renderItem} />
  </SafeAreaView>)
}

export default Products;
```

```js
// ./screens/SingleItem.js
import React, {useState, useEffect} from 'react'
import {SafeAreaView,View, FlatList} from 'react-native'
import { Avatar, Button, Card, Text } from 'react-native-paper';
import { Rating, AirbnbRating } from 'react-native-ratings';

const SingleItem = ({route, navigation}) => {
  const [item, setItem] = useState(null);

  navigation.setOptions({ title:item?.category?.toUpperCase()})

  useEffect(()=>{
    fetch(`https://fakestoreapi.com/products/${route?.params?.id}`)
        .then(res=>res.json())
        .then(json=>setItem(json))    
  },[])

  return( item !== null ? 
    <Card style={{margin:10}} mode="outlined">
      <Card.Title title={item.title} subtitle={item.category} />
      <Card.Cover source={{ uri: item.image }} resizeMode= 'center' style={{backgroundColor:"#FFF"}} />
      <Card.Content style={{marginTop:15}}>
        <Text variant="bodyMedium" style={{textAlign:'justify'}}>{item.description}</Text>
        <View style={{flexDirection:'row', justifyContent:'space-between', alignItems:'center'}}>
          <Text style={{fontSize:24}}>₹{item.price}</Text>
          <Rating
            type='star'
            startingValue={item.rating?.rate}
            fractions={1}
            ratingCount={5}
            imageSize={24}
            ratingColor={"#000"}
            ratingTextColor={"#000"}
            showRating
          />
        </View>
      </Card.Content>
      <Card.Actions>
        <Button mode={"outlined"}>Add To Cart</Button>
      </Card.Actions>
    </Card>
   
  : <></>)
}

export default SingleItem;
```

```js
// ./navigator/MyStack.js
import Products from '../screens/Products'
import SingleItem from '../screens/SingleItem'

import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function MyStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="home" component={Products} options={{title:"My Store"}}/>
      <Stack.Screen name="singleitem" component={SingleItem} />
    </Stack.Navigator>
  );
}

export default MyStack;
```

```js
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import MyStack from './navigator/MyStack'
import { Provider as PaperProvider } from 'react-native-paper';

export default function App() {
  return (
    <PaperProvider>
      <NavigationContainer>
        <MyStack />
      </NavigationContainer>
    </PaperProvider>
  );
}
```

[Working Demo](https://snack.expo.dev/@subrat1977/react-native-paper-demo)
