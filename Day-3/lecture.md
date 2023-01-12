## FlatList

`ScrollView` renders all its child components at once, but this has a performance downside.

Imagine you have a very long list of items you want to display, maybe several screens worth of content. Creating JS components and native views for everything all at once, much of which may not even be shown, will contribute to slow rendering and increased memory usage.

This is where `FlatList` comes into play. `FlatList` renders items lazily, when they are about to appear, and removes items that scroll way off screen to save memory and processing time.

`FlatList` is also handy if you want to render separators between your items, multiple columns, infinite scroll loading, or any number of other features it supports out of the box.


In React Native, you can use the FlatList component to render a long list of data. It renders only the items shown on the screen in a scrolling list and not all the data at once. To render a scrollable list of items using FlatList , you need to pass the required data prop to the component. The other props are self explanatory.

- data: array of items
- renderItem: a function that returns a JSX for a single item.
- keyExtractor: extracts key from data. But remember that it should be a string.
- numColumns: number of columns
- horizontal: show horizontal list
- itemSeparatorComponent:
- listHeaderComponent:
- listFooterComponent:
- listEmptyComponent: 

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, RefreshControl, FlatList} from 'react-native'
import  Constants  from 'expo-constants'

const App = () => {
  const [data, setData] = useState([])
  const [refreshing, setRefreshing] = React.useState(false);

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'Task no: '+Math.ceil(Math.random()*100)]);
    setRefreshing(false);
  }
  return(
    <View style={styles.container}>
      <Text style={styles.heading}>List of Tasks</Text>
      <FlatList
      refreshControl={<RefreshControl 
          refreshing={refreshing} 
          onRefresh={onRefresh} 
          colors={['red']} 
          tintColor='green' />}
      data={data}
      renderItem={({item}) => <Text style={styles.item}>{item}</Text>}
      keyExtractor={(_, index) => index.toString() }
      numColumns={2}
      ItemSeparatorComponent={() => <View style={styles.separator}></View>}
      ListHeaderComponent={() => <Text style={{textAlign:'center'}}>This is header of FlatList</Text>}
      ListFooterComponent={() => <Text  style={{textAlign:'center'}}>This is footer of FlatList</Text>}
      ListEmptyComponent={() => <Text style={{textAlign:'center'}}>No Items in list</Text>}

      />
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container :{
    marginTop: Constants.statusBarHeight
  },
  item : {
    flex:1,
    borderRadius:10,
    borderWidth:1,
    borderColor:'#006666',
    padding:5,
    margin:5,
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  },
  separator:{
    flex:1,
    borderColor:'#006666',
    borderBottomWidth:2,
    margin:10
  }
})
```

## Redux Flow

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/project_files%2Freduxdataflowdiagram.gif?alt=media&token=2c7e8868-dd02-4246-9590-660229af7ed6)

## Redux Async

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/project_files%2Fredux-async.gif?alt=media&token=43510a42-bfcd-4329-bc69-a319c5392054)

## Redux Toolkit

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/project_files%2Fredux-toolkit.avif?alt=media&token=05ae5353-2072-4f98-b19d-ad0faaf8ab79)

## Networking

In mobile applications we may need to load resources from a backend server and may need to save something to backend server.

For this purpose we can use inbuilt `fetch` or third party networking library `axios`.

## GET Request

```js
    axios.get('https://jsonplaceholder.typicode.com/postss')
      .then(response => {
        console.log(response.data);
      }).catch(error => {
        console.log("Unable to access data");
      });
    },[]);

// fetch()
fetch('https://jsonplaceholder.typicode.com/posts')
  .then(response => response.json())    // one extra step
  .then(data => {
    console.log(data) 
  })
  .catch(error => console.error(error));
```

## POST Request with Custom Header

```js
// axios

const url = 'https://jsonplaceholder.typicode.com/posts'
const data = {
  title: 'This is title',
  body: 'This is body',
  userId: 1,
};
axios
  .post(url, data, {
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
    },
  })
  .then(({data}) => {
    console.log(data);
});

// fetch()

    const url = "https://jsonplaceholder.typicode.com/posts";
    const options = {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json;charset=UTF-8",
      },
      body: JSON.stringify({
        title: 'This is title',
        body: 'This is body',
        userId: 1,
      }),
    };
    fetch(url, options)
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
      });
```

Notice that:

- To send data, fetch() uses the body property for a post request to send data to the endpoint, while Axios uses the data property
- The data in fetch() is transformed to a string using the JSON.stringify method
- Axios automatically transforms the data returned from the server, but with fetch() you have to call the response.json method to parse the data to a JavaScript object. More info on what the response.json method does can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/Response/json).
- With Axios, the data response provided by the server can be accessed with in the [data object](https://github.com/axios/axios#response-schema), while for the fetch() method, the final data can be named any variable.

[Json Placeholder Typicode Guide](https://jsonplaceholder.typicode.com/guide/)

## Project Using ReduxToolkit and FlatList

Example Path: D:\SubratSir\masai2\reduxflatlist (For Instructor's Reference)

```js
npx create-expo-app reduxflatlistexample
npm install --save axios react-redux @reduxjs/toolkit
```

### Redux-Toolkit Slice

```js
// src/redux/slices/productSlice.js
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";
import axios from "axios";

export const fetchProducts = createAsyncThunk(
  "products/fetchProducts",
  async () => {
    const response = await axios.get("https://dummyjson.com/products");
    console.log(response);
    return response.data;
  }
);

const initialState = {
  products: [],
  status: "idle", //'idle' | 'loading' | 'succeeded' | 'failed'
  error: null
};

export const productSlice = createSlice({
  name: "product",
  initialState,
  reducers: {},
  extraReducers(builder) {
    builder
      .addCase(fetchProducts.pending, (state, action) => {
        state.status = "loading";
      })
      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.products = action.payload;
        state.status = "fulfilled";
      });
  }
});

export default productSlice.reducer;
```

### Store

```js
// src/redux/store.js
import { configureStore } from "@reduxjs/toolkit";
import productsReducer from "./slices/productSlice";

export const store = configureStore({
  reducer: {
    productsReducer: productsReducer
  }
});

```

### Provider

```js
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, SafeAreaView, View, Platform } from 'react-native';
import { Provider } from 'react-redux';
import Home from './src/components/Home';
import {store} from './src/redux/store'

export default function App() {
  return (
    <Provider store={store}>
      <SafeAreaView style={styles.container}>
        <Home />
        <StatusBar style="auto" />
      </SafeAreaView>
    </Provider>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    marginTop: Platform.select({android:24, ios:47})
  },
});

```

### Home Component

```js
import React, {useEffect} from 'react';
import {View, FlatList, Text, StyleSheet} from 'react-native';
import {useSelector, useDispatch} from 'react-redux';
import {fetchProducts} from '../redux/slices/productSlice';

const Home = () => {
  const dispatch = useDispatch();
  const products = useSelector(state => state.productsReducer.products);

  useEffect(() => {
    dispatch(fetchProducts());
  }, []);

  const renderItem = ({item}) => {
    return (
      <View style={styles.itemContainer}>
        <Text>{item.title}</Text>
        <Text>{item.description}</Text>
      </View>
    );
  };

  return (
    <View style={{flex: 1}}>
      <Text style={styles.heading}>Products</Text>
      <FlatList data={products.products} renderItem={renderItem} />
    </View>
  );
};

export default Home;

const styles = StyleSheet.create({
  heading: {
    fontSize: 20,
    textAlign: 'center',
  },
  itemContainer: {
    borderWidth: 1,
    borderColor: 'rgb(20,20,20)',
    borderRadius: 5,
    margin: 10,
    padding: 10,
  },
});

```

