import React, { Component } from 'react';
import {

  StyleSheet,
  Text,
  View,
  ListView,
  TouchableHighlight
} from 'react-native';
import firebase from './firebase';

 



export default class itemlister extends Component {
  constructor(){
    super();
    let ds = new ListView.DataSource({rowHasChanged:(r1, r2) => r1 !== r2});
    this.state = {
      itemDataSource: ds
    }

    this.itemsRef = this.getRef().child('items');

    this.renderRow = this.renderRow.bind(this);
    this.pressRow = this.pressRow.bind(this);
  }

  getRef(){
    return firebase.database().ref();
  }

  componentWillMount(){
     this.getItems(this.itemsRef);
  }



  getItems(itemsRef){
   
    itemsRef.on('value',(snap) => {
      let items = [];
      snap.forEach((child) => {
        items.push({
          title: child.val().title,
          _key: child.key
        });
      });
      this.setState({
        itemDataSource: this.state.itemDataSource.cloneWithRows(items)
      });
    });
    console.log(items.title);
    
  }

  pressRow(item){
    console.log(item);
  }

  renderRow(item){
    return (
      <TouchableHighlight onPress={() => {
        this.pressRow(item);
      }}>
        <View >
          <Text >{item.title}</Text>
        </View>
      </TouchableHighlight>
    );
  }

  render() {
    return (
      <View >
       
        <ListView
          dataSource={this.state.itemDataSource}
          renderRow={this.renderRow}
        />
      </View>
    );
  }
}


