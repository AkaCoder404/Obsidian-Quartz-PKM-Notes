---
creation_date: 2024年03月12日
banner: "![[daily-note-banner.gif]]"
banner_icon: "🌞"
tags: "#笔记"
banner_y: 0.4705
---

# React Native
# 01 Background
React Native in 100 Seconds https://www.youtube.com/watch?v=gvkqT_Uoahw
*Learn once, use anywhere*
![[Pasted image 20240312080725.png]]

Using react native components, with using JSX components

```javascript
<View> // uses flexbox by default
<TouchableOpacity> // Button
<ActivityIndicator>  // Loading Indicator
<Flatlist> // For larger list
// map function for smaller
<ScrollView> // navigate through list of image
```


# 02 Core Features

- Localization
- Theme
- In-memory storage
- State management
- Storage/Database

# 03 DevOps
### Code Coverage
Here is an example setting up code coverage in a React Native project. First let's configure `jest` for React Native, which should be installed by default. However, we have to configure `jest.config.js` with the following code.
```js
"jest": { 
	"preset": "react-native", 
	"collectCoverage": true 
}
```
Then in order to ensure tests and coverage works, run `npm test -- --coverage`. This should give something like this.


Here, we show how to use ![Codecov]
![[Pasted image 20240314120825.png]]
![[Pasted image 20240314120904.png]]

Then you should see this in installed apps
![[Pasted image 20240314121338.png]]

**3. Integrate the code coverage service with your github repository**
For GitHub Actions, you can add a step in your `.github/workflows/ci.yml`

```yaml
-   name: Upload coverage to Codecov 
	uses: codecov/codecov-action@v2 
	with: token: ${{ secrets.CODECOV_TOKEN }} # Not required for public repos 
	file: ./coverage/lcov.info 
	flags: unittests name: codecov-umbrella
```

Generate the token here
![[Pasted image 20240314121713.png]]

10bdc969-8185-48e3-a083-94887a24b4bb
Place secrets here, create new organization secret called `CODECOV_TOKEN`
![[Pasted image 20240314121804.png]]
## Deploying
Here is an example of setting up React Native ios automatic deploying...





Continuous integration for react native applications

https://hemanthkollanur.medium.com/invariant-violation-requirenativecomponent-rnsscreenstackheaderconfig-was-not-found-in-the-1eeebfb72235

https://blog.csdn.net/qq_36833171/article/details/121183079

https://stackoverflow.com/questions/42610070/what-is-the-meaning-of-no-bundle-url-present-in-react-native


Zustand can be used for state managemnt 
MMKV is used for cache storage https://github.com/mrousavy/react-native-mmkv, synchronous, no need for wait or loading, auth tokens, 
Async Storage
realm for database

Bitrise for CI/CD
CircleCI

Color pallete generator https://coolors.co
Charts https://gifted-charts.web.app

What for code coverage?

Jest for writing test code?

Axios for requests...

Writing tests when there is navagation...
https://stackoverflow.com/questions/66514035/test-suite-failed-to-run-with-react-navigation-stack
https://reactnavigation.org/docs/testing/
https://github.com/software-mansion/react-native-reanimated/issues/4071

use react-native for icon development

Errors

Answer from https://github.com/expo/expo-cli/issues/4200 
`watchman watch-del-all`
![[Pasted image 20240313094125.png]]

Errors
Hangs after npm start...
```
➜ Implicit dependency on target 'React-nativeconfig' in project 'Pods' via options '-lReact-nativeconfig' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'React-perflogger' in project 'Pods' via options '-lReact-perflogger' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'React-rendererdebug' in project 'Pods' via options '-lReact-rendererdebug' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'React-runtimescheduler' in project 'Pods' via options '-lReact-runtimescheduler' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'React-utils' in project 'Pods' via options '-lReact-utils' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'ReactCommon' in project 'Pods' via options '-lReactCommon' in build setting 'OTHER_LDFLAGS'
 ➜ Implicit dependency on target 'RealmJS' in project 'Pods' via options '-lRealmJS' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'SocketRocket' in project 'Pods' via options '-lSocketRocket' in build setting 'OTHER_LDFLAGS'
➜ Implicit dependency on target 'Yoga' i
```
![[Pasted image 20240313093847.png]]
Fix?
This occurred after trying to set up react-native-vector-icons library.... especially the.plist https://github.com/oblador/react-native-vector-icons

Make sure u copy and paste the array in the right place...



Error 
When building using xcode...
```
unable to read property list from file: /Users/georgeli/Documents/Projects/Kalless-mobile/app/ios/app/Info.plist: The operation couldn’t be completed. (XCBUtil.PropertyListConversionError error 2.)
```


Error
![[Pasted image 20240313095943.png | 300]]
https://stackoverflow.com/questions/67130651/reanimated-2-failed-to-create-a-worklet-maybe-you-forgot-to-add-reanimateds-b
```
npx react-native start --reset-cache
```

Error?
![[Pasted image 20240314105201.png]]