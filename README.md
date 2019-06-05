# React-native-sticky-parallax-header
[WORK IN PROGRESS]
React Native sticky parallax header with scrollable tabs

## Installation
```bash
$ yarn add react-native-sticky-parallax-header
```

In order to make tab bar work, we have to link react-native-nested-scroll-view package.

```bash
$ react-native link react-native-nested-scroll-view
```

Depending on the version of React Native you use, the package can still making issues for you, you have to install patch-package
```bash
$ yarn add patch-package postinstall-postinstall
```
Then you add this script to your scripts:
```bash
 "scripts": {
+  "postinstall": "patch-package"
 }
```

After all those steps, just copy a 'patches' folder from this repository and run `yarn` again to apply the patched package.
You're ready to use the package now.

## Demo

## Example
```jsx
import React from 'react'
import { Text, View, Animated, Image, TouchableOpacity, Dimensions, Platform, StyleSheet } from 'react-native'
import StickyParalaxHeader from 'react-native-sticky-parallax-header'

const styles = StyleSheet.create({
  content: {
    height: 1000,
    marginTop: 50
  },
  foreground: {
    flex: 1,
    justifyContent: 'flex-end'
  },
  messageContainer: {
    paddingTop: 24,
    paddingBottom: 7
  },
  message: {
    color: 'white',
    fontSize: 40
  },
  background: {
    width: '100%',
    justifyContent: 'flex-end',
    backgroundColor: 'green',
    height: '100%'
  },
  headerWrapper: {
    backgroundColor: 'green',
    width: '100%',
    paddingHorizontal: 24,
    paddingBottom: 25,
    flexDirection: 'row',
    alignItems: 'center'
  },
  headerMenu: {
    flexDirection: 'row',
    alignItems: 'center'
  },
  headerTitleContainer: {
    marginLeft: 24,
    flexDirection: 'row',
    alignItems: 'center'
  },
  headerTitle: {
    fontSize: 16,
    color: 'white',
    marginLeft: 12
  }
})

const { height } = Dimensions.get('window')
const IS_IPHONE_X = height === 812 || height === 896
const STATUS_BAR_HEIGHT = Platform.OS === 'ios' ? (IS_IPHONE_X ? 44 : 20) : 0
const PADDING_TOP = STATUS_BAR_HEIGHT + 25

class UserScreen extends React.Component {
  state = {
    scroll: new Animated.Value(0)
  }

  componentDidMount() {
    const { scroll } = this.state
    scroll.addListener(({ value }) => (this._value = value))
  }

  renderContent = () => (
    <View style={styles.content}>
      <Text>Content</Text>
    </View>
  )

  renderForeground = () => {
    const { scroll } = this.state
    const titleOpacity = scroll.interpolate({
      inputRange: [0, 126, 174],
      outputRange: [1, 1, 0],
      extrapolate: 'clamp'
    })

    return (
      <View style={styles.foreground}>
        <Animated.View style={[styles.messageContainer, { opacity: titleOpacity }]}>
          <Text style={styles.message}>STICKY HEADER</Text>
        </Animated.View>
      </View>
    )
  }

  renderBackground = () => (
    <View
      style={styles.background}
    />
  )

  renderHeader = () => {
    const { scroll } = this.state
    const opacity = scroll.interpolate({
      inputRange: [0, 180, 250],
      outputRange: [0, 0, 1],
      extrapolate: 'clamp'
    })

    return (
      <View style={[styles.headerWrapper, { paddingTop: PADDING_TOP }]}>
        <View style={styles.headerMenu}>
          <TouchableOpacity>
            <Image source={require('../../assets/icons/Icon-Arrow.png')} />
          </TouchableOpacity>
          <Animated.View style={[styles.headerTitleContainer, { opacity }]}>
            <Text style={styles.headerTitle}>Sticky Header</Text>
          </Animated.View>
        </View>
      </View>
    )
  }

  render() {
    const { scroll } = this.state

    return (
      <StickyParalaxHeader
        foreground={this.renderForeground()}
        header={this.renderHeader()}
        background={this.renderBackground()}
        parallaxHeight={300}
        headerHeight={70}
        scrollEvent={Animated.event([{ nativeEvent: { contentOffset: { y: scroll } } }])}
      >
        {this.renderContent()}
      </StickyParalaxHeader>
    )
  }
}
```

## API Usage
| Property                       | Type              | Required | Description                                                   | Default |
| ------------------------------ | ----------------- | -------- | ------------------------------------------------------------- | ------- |
| `background`                   | `node`            | No       | This renders background component                             | -       |
| `backgroundImage`              | `number`          | No       | This renders background image instead of background component | -       |
| `children`                     | `node`            | No       | This renders all the children inside the component            | -       |
| `foreground`                   | `node`            | Yes      | This renders foreground component                             | -       |
| `header`                       | `node`            | Yes      | This renders header component                                 | -       |
| `headerHeight`                 | `number`          | No       | sets height of folded header                                  | -       |
| `headerSize`                   | `func`            | No       | returns size of hedaer for current device                     | -       |
| `initialPage`                  | `number`          | No       | set initial page of tab bar                                   | -       |
| `onChangeTab`                  | `func`            | No       | Tab change event                                              | -       |
| `onEndReached`                 | `func`            | No       | Tab change event                                              | -       |
| `parallaxHeight`               | `number`          | No       | sets height of opened header                                  | -       |
| `snapToEdge`                   | `bool`            | No       | boolean to fire the function for snap To Edge                 | -       |
| `scrollEvent`                  | `func`            | No       | returns offset of header to apply custom animations           | -       |
| `tabs`                         | `arrayOf(string)` | No       | array of tab names                                            | -       |
| `tabTextStyle`                 | `shape({})`       | No       | Text styles of tab                                            | -       |
| `tabTextActiveStyle`           | `shape({})`       | No       | Text styles of active tab                                     | -       |
| `tabTextContainerStyle`        | `shape({})`       | No       | Container styles of tab                                       | -       |
| `tabTextContainerActiveStyle`  | `shape({})`       | No       | Container styles of active tab                                | -       |
| `tabsContainerBackgroundColor` | `string`          | No       | Background color of tab bar container                         | -       |
| `tabsWrapperStyle`             | `shape({})`       | No       | Tabs Wrapper styles                                           | -       |

