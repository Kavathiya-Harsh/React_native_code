# React Native Local Authentication

## Study Code

```jsx
import React from 'react';
import { View, Text, Button, TextInput, Alert } from 'react-native';
import { useState } from 'react';
import * as SecureStore from 'expo-secure-store';
import  {router}  from 'expo-router';
import * as LocalAuth from 'expo-local-authentication';


export default async function LocalAuthentication() {
const [username, setUsername] = useState("");
const [password, setPassword] = useState("");


const handlebiomatric = async () => {
  const token  = await SecureStore.getItemAsync('token');
  const biometric = await SecureStore.getItemAsync('biometric');


    if(!token || biometric !== "true") {
      Alert.alert("Login Required", "Please login first to enable biometric authentication");
      return;
    };

  const hasHardware = await LocalAuth.hasHardwareAsync();
  if(!hasHardware) {
    Alert.alert("Error", "Biometric authentication is not supported on this device");
    return;
  }

  const isEnrolled = await LocalAuth.isEnrolledAsync();
  if(!isEnrolled) {
    Alert.alert("Error", "No biometric records found. Please set up biometrics on your device");
    return;
  }

  const res = await LocalAuth.authenticateAsync({
    promptMessage: "Authenticate with biometrics",
  });

  if(res.success) {
    router.replace("/");
  };
}

const handleLogin = async () => {
  if(username === "admin" && password === "123456") {
    await SecureStore.setItemAsync('token', 'abc123');
    await SecureStore.setItemAsync('biometric', "true");
    Alert.alert("Success", "Login successful");
  }else{
    Alert.alert("Error", "Invalid username or password");
  }

}
const FaceLock = async () => {
const types = await LocalAuth.getEnrolledLevelAsync();
console.log("Supported authentication types:", types);
}

return(
  <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center', backgroundColor: '#fff' }}>
    <Text>Local Authentication</Text>

    <TextInput
    placeholder="Username"
    value={username}
    onChangeText={setUsername}
    style={{ width: 200, height: 40, borderColor: 'gray', borderWidth: 1, marginBottom: 10 }}
    />

    <TextInput
    placeholder="Password"
    value={password}
    onChangeText={setPassword}
    style={{ width: 200, height: 40, borderColor: 'gray', borderWidth: 1, marginBottom: 10 }}
    />

    <Button title="Login" onPress={handleLogin} />
    <Button title="biometric" onPress={handlebiomatric}/>
    <Button title="FaceLock" onPress={FaceLock}/>

  </View>
)
}
```
