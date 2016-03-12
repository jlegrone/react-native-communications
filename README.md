# react-native-communications

[Release Notes](https://github.com/anarchicknight/react-native-communications/releases)

Open a web address, easily call, email, text, iMessage (iOS only) someone in React Native

## Installation

If you are on **React Native >= 0.20** just

```bash
npm install react-native-communications
```

Versions 0.15 through to and including 0.19 of React Native are also supported by running the following installation command

```bash
npm install react-native-communications@0.2.3
```

## Methods

```js
phonecall(phoneNumber, prompt)

phoneNumber - String
prompt - Boolean
```

Both arguments are required and need to be of the correct type.

###### iOS

If `prompt` is true it uses the undocumented `telprompt:` url scheme. This triggers an alert asking user to confirm if they want to dial the number. There are conflicting reports around the internet about whether Apple will allow apps using this scheme to be submitted to the App store (some have had success and others have had rejections).

If you face any problems having apps approved because of this raise an issue in this repo and I will look at removing it.

###### Android

`telprompt:` is not supported on Android. The `prompt` argument is ignored when run on Android devices.

---

```js
email(to, cc, bcc, subject, body)

to - String Array
cc - String Array
bcc - String Array
subject - String
body - String
```

You must supply either 0 arguments or the full set (5).

If 0 arguments are supplied the new email view will be launched with no prefilled details.

If 5 are supplied they will be checked against their expected type and ignored if the type is incorrect.

If there are any arguments you don't want to provide a value for set them as `null` or `undefined`.

e.g.

`email(['emailAddress'], null, null, null, 'my body text')` would open the new email view with a recipient and body text prefilled.

`email(null, ['emailAddress1', 'emailAddress2'], null, 'my subject', null)` would open the new email view with two recipients in the cc field and a subject prefilled.

`email([123, 'emailAddress'], null, null, 789, ['my body text'])` would open the new email view with one recipient prefilled and no other values.

---

```js
text(phoneNumber)

phoneNumber - String
```

`phoneNumber` is an optional argument. If it is not provided then the new message view will be launched with no recipient specified.

If it is provided but is the wrong type then again the new message view will be launched with no recipient specified.

---
```js
web(address)

address - String
```

Current requirements for `address` are that it is provided and is a String. There is no validation on whether it is a valid address.

## Usage

**Note:**

This will only work fully when run on actual devices. The iOS simulator only supports the `web` method and does not have the required device components installed to run any of the other methods. The Android emulator can run all the methods apart from `email`.

Assuming you have `npm install -g react-native-cli`, first generate an app:

```bash
react-native init RNCommunications
cd RNCommunications
npm install react-native-communications --save
```

Then paste the following into `RNCommunications/index.ios.js` and/or `RNCommunications/index.android.js`:

```js
'use strict';

var React = require('react-native');
var {
  AppRegistry,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
} = React;

var Communications = require('react-native-communications');

var RNCommunications = React.createClass({

  render: function() {
    return (
      <View style={styles.container}>
      <TouchableOpacity onPress={() => Communications.phonecall('0123456789', true)}>
        <View style={styles.holder}>
          <Text style={styles.text}>Make phonecall</Text>
        </View>
      </TouchableOpacity>
      <TouchableOpacity onPress={() => Communications.email(['emailAddress1', 'emailAddress2'],null,null,'My Subject','My body text')}>
        <View style={styles.holder}>
          <Text style={styles.text}>Send an email</Text>
        </View>
      </TouchableOpacity>
      <TouchableOpacity onPress={() => Communications.text('0123456789')}>
        <View style={styles.holder}>
          <Text style={styles.text}>Send a text/iMessage</Text>
        </View>
      </TouchableOpacity>
      <TouchableOpacity onPress={() => Communications.web('https://github.com/facebook/react-native')}>
        <View style={styles.holder}>
          <Text style={styles.text}>Open react-native repo on Github</Text>
        </View>
      </TouchableOpacity>
      </View>
    );
  }
});

var styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    backgroundColor: 'rgb(253,253,253)',
  },
  holder: {
    flex: 0.25,
    justifyContent: 'center',
  },
  text: {
    fontSize: 32,
  },
});

AppRegistry.registerComponent('RNCommunications', () => RNCommunications);
```

## TODO

- [ ] Refactor how arguments are passed in to the methods (change from multiple parameters to an args object)