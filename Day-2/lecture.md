


`ScrollView` renders all its child components at once, but this has a performance downside.

Imagine you have a very long list of items you want to display, maybe several screens worth of content. Creating JS components and native views for everything all at once, much of which may not even be shown, will contribute to slow rendering and increased memory usage.

This is where `FlatList` comes into play. `FlatList` renders items lazily, when they are about to appear, and removes items that scroll way off screen to save memory and processing time.

`FlatList` is also handy if you want to render separators between your items, multiple columns, infinite scroll loading, or any number of other features it supports out of the box.


```js
npx create-expo-app reduxflatlistexample
npm install --save axios react-redux @reduxjs/toolkit
```

## Redux-Toolkit Slice

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

