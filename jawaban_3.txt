import React, { Component, createRef, useRef } from "react";

import { Client } from "@stomp/stompjs";

import RNShare from './RNShare';

 import { 
  Alert,
  TouchableHighlight,
  View,
  ImageBackground,
  Spinner,
  StyleSheet,
  Text,
  Image,
  Button,
  TouchableOpacity,
  Thumbnail,
  AsyncStorage,
  Content,
  Modal,
  Dimensions,
  SafeAreaView,
  ScrollView,
  NativeModules,
  ActivityIndicator,
  FlatList,
  DatePickerIOS,
  // PushNotificationIOS
  Vibration,
  TextInput,
  BackHandler,
  AppState,
  PixelRatio,
  Easing,
  Animated
 } from "react-native";


export default class App extends Component {

  constructor() {
    super();
    this.state = {
      value_for_conv: "0",
      value_bitcoin: "0",
      data_bitcoin: {},
      value_bitcoin: 0,

      btc: 0,
      idr: 0,

      AUS: {},
      EUR: {},
      POND: {},
      YEN: {},
      USD: {}
    };

    this.start()

  }


  async start()
  {
    await fetch('https://blockchain.info/ticker', {
    method: 'GET',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
    },
    timeout: 1500,
    }).then((response) => response.json())
    .then((responseJson) => {
      this.setState({
        AUS: responseJson["ARS"],
        EUR: responseJson["EUR"],
        POND: responseJson["NZD"],
        YEN: responseJson["JPY"],
        USD: responseJson["USD"],
      })
    }).catch((error) => {
      console.error(error);
      Alert.alert("Gagal Terhubung ke server portal spanduk",JSON.stringify(error))
    })
  }

  async konversikan_btc(text)
  {
    this.setState({btc: text})
    if(text != "")
    {
      var idr = parseInt(text) * 268384270 /////// <- nothing doc for looking rate 1 BTC to IDR , so i search on google and hardcode on this
      this.setState({idr: idr})
    }
  }

  async konversikan(text)
  {
    this.setState({value_for_conv: text})
    if(text != "")
    {
      await fetch('https://blockchain.info/tobtc?currency=USD&value='+text, {
      method: 'GET',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json',
      },
      timeout: 1500,
      }).then((response) => response.json())
      .then((responseJson) => {
        this.setState({value_bitcoin: responseJson})
      }).catch((error) => {
        console.error(error);
        this.konversikan(text)
      })
    }
  }

  
  render(){
    return(
      <View style={{width: "100%", height: "100%"}}>
        <SafeAreaView style={{width: "100%", height: "100%", backgroundColor: "white", marginTop: 28, alignSelf: "center", justifyContent: "center"}}>
            <ScrollView contentInsetAdjustmentBehavior="never" overScrollMode="auto" scrollEnabled={true} >

            
              <View style={{width: "80%", backgroundColor: "#F0FFF0", marginTop: 18, flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      fontWeight: "bold"
                  }}>
                    Mata Uang
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      fontWeight: "bold"
                  }}>
                    Harga Beli Bitcoin
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      fontWeight: "bold"
                  }}>
                    Harga Jual Bitcoin
                  </Text>
                </View>
              </View>

              <View style={{width: "80%", backgroundColor: "#F0FFF0", flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    Dollar Australia
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.AUS.buy}
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.AUS.sell}
                  </Text>
                </View>
              </View>

              <View style={{width: "80%", backgroundColor: "#F0FFF0", flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    Euro Eropa
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.EUR.buy}
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.EUR.sell}
                  </Text>
                </View>
              </View>

              <View style={{width: "80%", backgroundColor: "#F0FFF0", flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    Poundsterling Inggris
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.POND.buy}
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.POND.sell}
                  </Text>
                </View>
              </View>

              <View style={{width: "80%", backgroundColor: "#F0FFF0", flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    Yen Jepang
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.YEN.buy}
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.YEN.sell}
                  </Text>
                </View>
              </View>

              <View style={{width: "80%", backgroundColor: "#F0FFF0", flexDirection: "row", alignSelf: "center"}}>  
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    Dollar Amerika
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.USD.buy}
                  </Text>
                </View>
                <View style={{width: "30%", height: 30, justifyContent: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 8, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center"
                  }}>
                    {this.state.USD.sell}
                  </Text>
                </View>
              </View>














                  
              <View style={{width: "80%", backgroundColor: "#F0FFF0", marginTop: 18, alignSelf: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 32, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      marginTop: 18
                  }}>
                    Konversi Rupiah ke Bitcoin
                  </Text>

                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 18, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      marginTop: 18
                  }}>
                    Kurs 1 USD = 14.000 IDR
                  </Text>

                  <TextInput
                    style={{
                      flex: 0,
                      height: 38,
                      width: "80%",
                      borderRadius: 6,
                      borderWidth: 1,
                      borderColor: '#666',
                      color: '#AAA',
                      backgroundColor: 'white',
                      alignSelf: "center",
                      paddingLeft: 10,                  
                      marginBottom: 10
                    }}
                    maxLength={5}
                    value={this.state.value_for_conv}
                    keyboardType = 'numeric'
                    placeholderTextColor="#AAA"
                    onChangeText={(text) => {
                      
                      this.konversikan(text)
                    }}
                  />

                  <View style={{flexDirection: "row", alignSelf: "center", width: "100%", justifyContent: "center"}}>
                    <View>
                      <Text style={{
                        fontFamily: "Helvetica", 
                        fontSize: 18, 
                        color: "black",
                        fontWeight: "bold",
                        alignSelf: "center",
                        marginTop: 18
                      }}>
                        RP.{this.state.value_for_conv} = BTC {this.state.value_bitcoin}
                      </Text>
                    </View>
                  </View>

                  <View style={{width: "100%", height: 28}}></View>

            </View>












            <View style={{width: "80%", backgroundColor: "#F0FFF0", marginTop: 18, alignSelf: "center"}}>
                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 32, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      marginTop: 18
                  }}>
                    Konversi Bitcoin ke Rupiah
                  </Text>

                  <Text style={{
                      fontFamily: "Helvetica", 
                      fontSize: 18, 
                      color: "black",
                      fontWeight: "bold",
                      alignSelf: "center",
                      marginTop: 18
                  }}>
                    Kurs 1 USD = 14.000 IDR
                  </Text>

                  <TextInput
                    style={{
                      flex: 0,
                      height: 38,
                      width: "80%",
                      borderRadius: 6,
                      borderWidth: 1,
                      borderColor: '#666',
                      color: '#AAA',
                      backgroundColor: 'white',
                      alignSelf: "center",
                      paddingLeft: 10,                  
                      marginBottom: 10
                    }}
                    value={this.state.btc}
                    maxLength={5}
                    keyboardType = 'numeric'
                    placeholderTextColor="#AAA"
                    onChangeText={(text) => {
                      
                      this.konversikan_btc(text)
                    }}
                  />

                  <View style={{flexDirection: "row", alignSelf: "center", width: "100%", justifyContent: "center"}}>
                    <View>
                      <Text style={{
                        fontFamily: "Helvetica", 
                        fontSize: 18, 
                        color: "black",
                        fontWeight: "bold",
                        alignSelf: "center",
                        marginTop: 18
                      }}>
                        BTC {this.state.btc.toString()} = Rp {this.state.idr}
                      </Text>
                    </View>
                  </View>

                  <View style={{width: "100%", height: 28}}></View>

            </View>


          </ScrollView>
        </SafeAreaView>
      </View>
    )
  }






  
}


