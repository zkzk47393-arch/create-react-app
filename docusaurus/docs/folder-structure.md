NatureApp/
├── App.js                  # Main app entry
├── package.json            # Project dependencies
├── firebase.js             # Firebase config (replace YOUR_API_KEY etc)
├── navigation/
│   └── AuthStack.js        # Login/Register navigation
├── screens/
│   ├── LoginScreen.js
│   ├── RegisterScreen.js
│   ├── HomeScreen.js
│   ├── ChatScreen.js
│   ├── ProfileScreen.js
│   ├── LiveScreen.js
│   └── UpgradeScreen.js
└── assets/                 # Images / icons / fonts
{
  "name": "NatureApp",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios"
  },
  "dependencies": {
    "expo": "~48.0.0",
    "react": "18.2.0",
    "react-native": "0.71.8",
    "firebase": "^10.0.0",
    "react-navigation": "^4.4.4",
    "react-native-gesture-handler": "^2.9.0",
    "react-native-safe-area-context": "^4.5.0",
    "react-native-screens": "^3.20.0",
    "react-native-vector-icons": "^9.2.0",
    "i18next": "^23.0.1",
    "react-i18next": "^13.0.1"
  }
}import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import AuthStack from './navigation/AuthStack';

export default function App() {
  return (
    <NavigationContainer>
      <AuthStack />
    </NavigationContainer>
  );
}
import React from 'react';
import { createStackNavigator } from '@react-navigation/stack';
import LoginScreen from '../screens/LoginScreen';
import RegisterScreen from '../screens/RegisterScreen';
import HomeScreen from '../screens/HomeScreen';

const Stack = createStackNavigator();

export default function AuthStack() {
  return (
    <Stack.Navigator initialRouteName="Login" screenOptions={{ headerShown: false }}>
      <Stack.Screen name="Login" component={LoginScreen} />
      <Stack.Screen name="Register" component={RegisterScreen} />
      <Stack.Screen name="Home" component={HomeScreen} />
    </Stack.Navigator>
  );
}
import React, { useState } from 'react';
import { View, TextInput, Button, Text, StyleSheet } from 'react-native';
import { signInWithEmailAndPassword } from 'firebase/auth';
import { auth } from '../firebase';

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      navigation.navigate('Home');
    } catch (e) {
      setError(e.message);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>🌿 Nature Login</Text>
      <TextInput placeholder="Email" value={email} onChangeText={setEmail} style={styles.input} />
      <TextInput placeholder="Password" value={password} onChangeText={setPassword} secureTextEntry style={styles.input} />
      {error ? <Text style={styles.error}>{error}</Text> : null}
      <Button title="Login" onPress={handleLogin} />
      <Text onPress={() => navigation.navigate('Register')} style={styles.link}>Create Account</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent:'center', alignItems:'center', backgroundColor:'#fff' },
  title: { fontSize: 28, fontWeight:'bold', marginBottom: 20 },
  input: { width:'80%', borderWidth:1, padding:10, marginBottom:10, borderRadius:8 },
  error: { color:'red' },
  link: { color:'#007bff', marginTop:10 }
});
