# CryptBot Asset Application
โปรแกรมสำหรับตรวจนับของระบบพัสดุโดยสแกน QR Code พัฒนาโดยใช้ React Native ของ Expo เป็นหลัก

## สิ่งที่ต้องเตรียม
1. ลง [nodejs](https://nodejs.org/en/) ติดตั้งและตรวจสอบก่อนด้วยว่าติดตั้งสำเร็จ  
1. ลง [git](https://git-scm.com/download/win) แบบ 32 bit  
1. ลง [yarn](https://classic.yarnpkg.com/en/docs/install/#windows-stable)  
1. ลง [React Netive CLI]() โดยคำสั่ง **npm i -g react-native-cli** ผ่าน nodejs ที่ cmd หรือ powershell  
1. ลง [Expo CLI](https://docs.expo.io/versions/latest/workflow/expo-cli/#installation) โดยคำสั่ง **npm i -g expo-cli** ผ่าน nodejs ที่ cmd หรือ powershell  
1. ลง [Firebase CLI](https://firebase.google.com/docs/cli?authuser=0) โดยคำสั่ง **npm i -g firebase-tools** ผ่าน nodejs ที่ cmd หรือ powershell

## สิ่งที่ควรรรับรู้ก่อน
1. โปรเจคนี้ทั้งหมด component จะเขียนโดยใช้ การ return ออกมาเป็น function ทั้งหมด  
ไม่ได้ใช้หลักการเป็นแบบ class เหมือนที่ส่วนใหญ่ใน internet มี เพราะว่ามันเป็นวิธีเขียนแบบเมื่อก่อน  
1. ใช้ library axion ในการติดต่อกับ API ไม่ได้ใช้ function fetch
1. component จะถูกแบ่งออกเป็น 2 ส่วนหลักๆคือ screen กับ component ที่ใช้ทำงาน

## เริ่มต้น
1. เผื่อไม่ทราบหลังจาก clone project นี้ลงไปยังเครื่องแล้วให้ทำการ run คำสั่ง yarn install ที่ที่อยู่ของ project นี้
1. ควรอ่าน React / React Native ก่อน
1. ควรเข้าใจเรื่องการ Navigation ของ React Native ในระดับหนึ่ง


## Flow ของ Application
![Flow Application](CryptBot_Asset_Flow.png)

## โครงสร้างภายในโปรเจค
### 1. โฟลเดอร์
- **api** โฟลเดอร์นี้จะเก็บไฟลที่ใช้เก็บ root ของ api เอาไว้คือไฟล์ api.js
- **asset** เก็บไฟลต่างๆที่ไม่ใช่ .js เช่น font image
- **components** เก็บ component หรือส่วนย่อยๆที่ใช้ประกอบกันให้กับหน้า screen
- **screens** เก็บหน้าหลักเพื่อเอาไว้ใช้ navigate
- **styles** เก็บไฟล์ style ของหน้าต่างๆ
- **function** เก็บฟังก์ชันช่วยงานต่างๆ
- **firebase** เก็บ Bundle ไฟล์ที่ build จากการ export ออกมาจาก Expo เพื่อใช้รัน app โดยไม่พึ่งพา Expo ซึ่งจะต้อง deploy ขึ้นไปที่ firebase hosting ที่ https://cryptbotasset.web.app/$APP_VERSION"

### 2. ไฟล์เริ่มต้น
apllication เริ่มต้นการทำงานที่ไฟล์แรกคือไฟล์ App.js แล้วมีการเรียก component **StackScreen.js** ที่อยู่ใน /screens หรืออาจจะเรียกว่าได้ว่า StackScreen.js คือไฟล์แรกที่ apllication ทำงานก็ได้  
โดยไฟล์นี้จะเป็น Stack ของ Navigation

### 3. การสร้างหน้าใหม่  
หากต้องการสร้างหน้าใหม่ ขั้นแรก  
ให้สร้างไฟล์หน้า screen [ชื่อไฟล์].js ภายใต้โฟล์เดอร์ screens  
และเพิ่ม component ของ screen ที่สร้างใหม่ไปยัง /screen/StackScreen.js ด้วย
ตัวอย่าง..  
###### *ไฟล์ TestDrive.js
```javascript
import React from 'react'; // เพิ่ม react เข้ามาใช้งาน
import { View, StyleSheet } from 'react-native'; // เพิ่ม component พื้นฐานของ react native มีอีกมากมายลองหาอ่านใน document ของ  react native

import TestDrive from '../components/TestDrive' // เพิ่ม component อื่นๆเข้ามาที่จะใช้ประกอบเป็นหน้านี้
// ** โครงสร้างภายในก็เหมือนกับไฟล์นี้ แต่แค่สร้างออกมาเพื่อใช้งานกันคนละแบบ

// navigation ใช้สำหรับการเปลี่ยน screen
// route มีข้อมูลมากมายภายในส่วนใหญ่ใช้ในการดึงค่าที่ส่งผ่านระหว่าง screen 
const TestDriveScreen = ({ navigation, route }) =>
{
    // ส่วนนี้ทั้งหมดใช้ set ค่าต่างๆของ screen นั้นๆ
    navigation.setOptions(
    { 
        title: 'TestDriveScreen', // ชื่อที่จะแสดงบนหัว
        headerShown: false // เปิด / ปิด การแสดงชื่อ
    });

    return ( // ภายใต้ return จะมี Component หลักได้เพียง 1 Component เท่านั้น Component อื่นๆต้องอยู่ภายใต้ Component หลัก
        <View style={styles.container}>
            <TestDrive navigation={navigation} route={route} /> {/* เรียก Component TestDrive 
            ออกมา โดยส่ง navigation กับ route ไปให้ด้วยเพราะ Component ที่ไม่ใช่ screen 
            มันจะไม่มีมาด้วย จึงต้องส่งค่าไปให้ Component นั้นๆ 
            (เป็นเพราะโครงสร้างที่ใช้แบบนี้มันจะม่รู้ตัวว่ามันคือ หน้าอีกหน้าหนึ่ง จึงทำให้ไม่มี 2 ตัวแปลดังกล่าวมาด้วย 
            **ไม่ใช่บัคของ navigator นะ)*/}
        </View>
    )
}

export default TestDriveScreen; // เหมือน node js ทั่วๆไป ต้องส่งออก fn/class ไปด้วย ซึ่ง export ออกไปได้หลาย fn/class แล้วแต่เราเลือกใช้งาน

const styles = StyleSheet.create(
{
    container:
    {
        flex: 1,
        flexWrap: 'wrap',
        flexDirection: 'column',
        justifyContent: 'center',
        alignContent: 'center',
        backgroundColor: '#236192',
        paddingTop: (Platform.OS.toLowerCase() === 'ios') ? 25 : 0
    }
});
```
  
###### *ไฟล์ StackScreen.js
```javascript
import 'react-native-gesture-handler';

import React from 'react';

import { NavigationContainer } from '@react-navigation/native'; // เพิ่ม lib จัดการการเปลี่ยนหน้า
import { createStackNavigator } from '@react-navigation/stack'; // เพิ่ม lib จัดการการเปลี่ยนหน้า

import LoginScreen from '../screens/LoginScreen'
import ItemCardScreen from '../screens/ItemCardScreen'

import TestDrive from '../components/TestDrive'

const Stack = createStackNavigator();

const StackScreen = () =>
{
    return ( // ตรงนี้คือ screen ต่างๆ ที่จะมีใน application ที่เราสร้าง 
    // โดยที่อันบนสุดคือหน้าแรกที่จะแสดงออกมา ส่วนหน้าอื่นๆแล้วแต่เราจะบอกว่าจะให้ไปที่หน้าไหน
        <NavigationContainer>
            <Stack.Navigator>
                <Stack.Screen name="LoginScreen" component={LoginScreen}/>
                <Stack.Screen name="ItemCardScreen" component={ItemCardScreen}/>

                <Stack.Screen name="TestDrive" component={TestDrive}/>
            </Stack.Navigator>
        </NavigationContainer>
    )
}

export default StackScreen;
```

## การ Debug ด้วย Visual Studio Code
ปกติแล้วโปรแกรมที่เขียนด้วย node.js จะมี script สำหรับสั่งงานอยู่ในไฟล์ package.json อยู่ในส่วน scripts อยู่แล้ว ซึ่งจะสามารถสั่งด้วย yarn ดังนี้
- **start** สั่ง start Expo Developer Tools ใน Development Mode
- **start-no-dev** สั่ง start Expo Developer Tools ใน Production Mode
- **android** สั่ง start Expo Developer Tools พร้อมทั้งรัน Android Device หรือ Emulator
- **ios** สั่ง start Expo Developer Tools พร้อมทั้งรัน iOS Simulator
- **web** สั่ง start Expo Developer Tools พร้อมทั้งรันบน Web
- **eject** สั่งถอด Expo ออก

เช่น
```
yarn start
```
### ติดตั้ง Expo Go เพื่อการ Debug
1. **Android**
สำหรับมือถือ Android สามารถเข้า Google Play เพื่อติดตั้ง Expo Go ได้ตามปกติ หากเป็น emulator ควรเลือก image ที่รองรับ Google Play ซึ่งจะสามารถติดตั้ง Expo Go ได้เหมือนเครื่องปกติทุกประการ
1. **iOS**
สำหรับมือถือ iOS สามารถเข้า App Store เพื่อติดตั้ง Expo Go ได้ตามปกติ หากเป็น simulator สามารถติดตั้ง Expo Go ได้ด้วยคำสั่ง
```
expo client:install:ios
```

### VSCode Launch Script
การรันด้วย yarn จะ Debug ได้ยาก จึงได้สร้าง configuration สำหรับรัน Debug ชื่อ **Debug in Exponent** ไว้ใน .vscode/launch.json ทำให้สามารถกดเมนู Run/Start Debugging หรือเลือก run Debug in Exponent ที่ Run and Debug Panel ได้
1. Run Debug in Exponent แล้ว VSCode จะ start **React Native Packager** และ **Expo Server** เพื่อรอรับการติดต่อจากโปรแกรม Expo ในเครื่องผู้ใช้ ไม่ว่าเครื่องนั้นจะเป็นเครื่องจริง หรือ emulator ไม่ว่าจะเป็น ios หรือ android ก็ตาม และจะแสดง QR Code หรือ URL ที่ใช้เชื่อมต่อ (**เครื่องควรจะอยู่ภายในเครือข่ายที่เชื่อมถึงกันได้**)
1. Run Expo Go App ที่เครื่องที่ต้องการ Debug จากนั้นใช้เมนู Scan QR Code หรือป้อน URL ที่ใช้เชื่อมต่อ
1. หากเชื่อมต่อได้เรียบร้อย ถ้าเป็นครั้งแรก จะมีเมนูให้เลือกกำหนดค่า **Debug Remote JS** แต่หากไม่มีเมนูแสดงขึ้นมา สามารถเรียกเมนูตั้งค่าขึ้นมาได้ด้วยการ**เขย่าเครื่อง**
1. เมื่อเชื่อมต่อแบบ Remote Debugging ได้เรียบร้อยแล้ว VS Code จะแสดง Status Bar เปลี่ยนเป็นสีส้ม และมี Tool สำหรับ Debug ให้ใช้งาน

## การ Build โปรแกรมที่เครื่องตัวเอง
การใช้บริการของ Expo ทำให้สะดวกในแง่ที่ไม่ต้องใช้ทรัพยากรในเครื่องของเรามากนัก ทั้ง RAM และเนื้อที่ฮาร์ดิสต์ แต่บริการของ Expo นั้น เราก็ต้องเข้าคิวรอ ซึ่งค่อนข้างใช้เวลานาน หากมีเครื่องพร้อม เราก็สามารถ build โปรแกรมที่เครื่องของเราเองได้โดยใช้ Turtle-Cli โดยจะต้องสร้าง manifest ไฟล์ที่เป็น config ของโปรเจ็ค Expo ก่อน (ปกติจะมีอยู่ที่ Expo Server)
### 1. ติดตั้ง Turtle-Cli
ติดตั้ง Turtle-Cli ด้วยคำสั่ง
```
npm install -g turtle-cli
```
### 2. เตรียม SDK ในเครื่องก่อน
```
turtle setup:android
```
โปรแกรม Turtle-Cli จะดาวโหลด SDK ต่างๆที่จำเป็นมาไว้ในเครื่องทั้ง Expo SDK และ Android SDK ที่ใช้งานร่วมกับ Expo SDK
โดยเก็บไว้ภายใต้โฟลเดอร์ของตัวเอง (<USER_HOME>/.turtle) โดยสามารถระบุ version ของ Expo SDK ได้ด้วยคำสั่ง
```
turtle setup:android --sdk-version <SDK-VERSION>
```
เช่น
```
turtle setup:android --sdk-version 41.0.0
```
### 3. สั่ง Publish โปรแกรม
**หมายเหตุ:** ได้สร้าง Task ของ VS Code ไว้แล้ว สามารถเลือกเมนู Terminal/Run Task แล้วเลือกเมนูชื่อ **Export Asset Bundle** เพื่อรันคำสั่งได้เลย

การ publish โปรแกรม จะเป็นการสร้าง bundle ของชิ้นส่วนต่างๆ รวมทั้ง manifest ไฟล์ของโปรแกรม ซึ่งจะต้องนำไปแขวนไว้ที่เว็ป เพื่อให้เข้าถึงได้ตอนที่ build โปรแกรม และตอนที่โปรแกรมทำงาน จะมีการมาเช็คเพื่ออัพเดทโปรแกรมอัตโนมัติ แต่คำสั่ง expo publish นั้น จะอัพโหลดไฟล์ต่างๆขึ้น Expo Server อัตโนมัติ ดังนั้นเราจึงต้องใช้คำสั่ง expo export แทน ดังนี้ 
```
expo export --dev --public-url <your-url-here> --output-dir <local-dist-dir>
```
เช่น ถ้าเราจะแขวนไฟล์ไว้ที่ https://cryptbotasset.web.app ก็สั่ง
```
expo export --dev --public-url https://cryptbotasset.web.app --output-dir firebase/public
```
### 4. Build Bundle
**หมายเหตุ:** ได้สร้าง Task ของ VS Code ไว้แล้ว สามารถเลือกเมนู Terminal/Run Task แล้วเลือกเมนูชื่อ **Build Android Bundle** เพื่อรันคำสั่งได้เลย

การ build จะต้องระบุชนิดของไฟล์เป็น apk หรือ app-bundle และระบุชื่อไฟล์ของ keystore และ alias ของกุญแจที่จะใช้ในการเซ็นโปรแกรม ตามคำสั่งดังนี้
```
turtle build:android -t app-bundle --keystore-path <keystore file> --keystore-alias <key alias> --public-url <manifest url>
```
เมื่อ build เสร็จเรียบร้อย ก็สามารถอัพโหลดขึ้น Google Play Store ได้เลย

### 5. Build Android
**หมายเหตุ:** ได้สร้าง Task ของ VS Code ไว้แล้ว สามารถเลือกเมนู Terminal/Run Task แล้วเลือกเมนูชื่อ **Build Android** เพื่อรันคำสั่งได้เลย

การ build ทั้งหมด จะมีขั้นตอนดังนี้
1. Export Asset Bundle
1. Upload to Firebase -> Upload ส่วนที่ Export ขึ้น firebase web
1. Build Android Bundle