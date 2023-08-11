1. Download RN repo:
`git clone https://github.com/facebook/react-native.git`
1. Go to RN project root folder, run `yarn` to install dependencies.
1. Go to `packages/rn-tester`, Run `npx react-native start` or `npm start` to start metro.
1. Run `adb reverse tcp:8081 tcp:8081`
1. Open RN project in Android Studio. Run the `rn-tester` app.
