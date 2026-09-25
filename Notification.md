# Schedule Notification - React Native + Expo

A beginner-friendly React Native and Expo project that demonstrates how to schedule a local notification after 24 hours using `expo-notifications`.

## Features

- Request notification permission
- Enter a custom notification title
- Enter a custom notification message
- Schedule a local notification
- Schedule the notification after 24 hours
- Play the default notification sound
- Show the notification in the notification banner and list
- Validate empty input fields
- Show success and permission-denied alerts
- Clear the input fields after scheduling
- Simple and beginner-friendly UI

## Technologies Used

- React Native
- Expo
- JavaScript
- `expo-notifications`

## Installation

Install `expo-notifications`:

```bash
npx expo install expo-notifications
```

## Complete Code

```jsx
import React, { useState } from "react";

import {
  View,
  Text,
  Button,
  TextInput,
  Alert,
  StyleSheet,
} from "react-native";

import * as Notifications from "expo-notifications";

// --------------------------------------------------
// Notification Handler
// --------------------------------------------------

Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldShowBanner: true,
    shouldShowList: true,
    shouldPlaySound: true,
    shouldSetBadge: false,
  }),
});

// --------------------------------------------------
// Notification Screen
// --------------------------------------------------

export default function NotificationScreen() {
  const [title, setTitle] = useState("");
  const [text, setText] = useState("");

  // ------------------------------------------------
  // Schedule Notification
  // ------------------------------------------------

  const handleNotification = async () => {
    if (!title.trim() || !text.trim()) {
      Alert.alert("Error", "Please enter title and text");
      return;
    }

    const permission =
      await Notifications.requestPermissionsAsync();

    if (!permission.granted) {
      Alert.alert(
        "Permission Denied",
        "Please allow notification permission"
      );
      return;
    }

    await Notifications.scheduleNotificationAsync({
      content: {
        title: title.trim(),
        body: text.trim(),
        sound: "default",
      },

      trigger: {
        type:
          Notifications.SchedulableTriggerInputTypes.TIME_INTERVAL,
        seconds: 24 * 60 * 60,
        repeats: false,
      },
    });

    Alert.alert(
      "Success",
      "Notification scheduled successfully!"
    );

    setTitle("");
    setText("");
  };

  return (
    <View style={styles.container}>
      <Text style={styles.heading}>
        Schedule Notification
      </Text>

      <TextInput
        style={styles.input}
        placeholder="Enter notification title"
        value={title}
        onChangeText={setTitle}
      />

      <TextInput
        style={[styles.input, styles.messageInput]}
        placeholder="Enter notification message"
        value={text}
        onChangeText={setText}
        multiline
      />

      <View style={styles.buttonContainer}>
        <Button
          title="Schedule Notification"
          onPress={handleNotification}
        />
      </View>
    </View>
  );
}

// --------------------------------------------------
// Styles
// --------------------------------------------------

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    padding: 20,
    backgroundColor: "#f5f5f5",
  },

  heading: {
    fontSize: 26,
    fontWeight: "bold",
    marginBottom: 30,
  },

  input: {
    width: "100%",
    backgroundColor: "#ffffff",
    borderWidth: 1,
    borderColor: "#cccccc",
    borderRadius: 10,
    padding: 12,
    marginBottom: 15,
    fontSize: 16,
  },

  messageInput: {
    height: 100,
    textAlignVertical: "top",
  },

  buttonContainer: {
    width: "100%",
    marginTop: 5,
  },
});
```

## How It Works

### 1. Notification Handler

`Notifications.setNotificationHandler()` controls how the notification is displayed.

```js
Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldShowBanner: true,
    shouldShowList: true,
    shouldPlaySound: true,
    shouldSetBadge: false,
  }),
});
```

### 2. Store User Input

Two `useState` variables store the notification title and message:

```js
const [title, setTitle] = useState("");
const [text, setText] = useState("");
```

### 3. Validate Input

The app checks that both fields contain text:

```js
if (!title.trim() || !text.trim()) {
  Alert.alert("Error", "Please enter title and text");
  return;
}
```

### 4. Request Permission

The app requests notification permission:

```js
const permission =
  await Notifications.requestPermissionsAsync();
```

If permission is denied, the notification is not scheduled.

### 5. Schedule Notification

The notification is scheduled with:

```js
await Notifications.scheduleNotificationAsync({
  content: {
    title: title.trim(),
    body: text.trim(),
    sound: "default",
  },
  trigger: {
    type:
      Notifications.SchedulableTriggerInputTypes.TIME_INTERVAL,
    seconds: 24 * 60 * 60,
    repeats: false,
  },
});
```

### 6. Schedule After 24 Hours

The notification uses:

```js
seconds: 24 * 60 * 60
```

Calculation:

```text
24 hours × 60 minutes × 60 seconds
= 86,400 seconds
```

### 7. One-Time Notification

The code uses:

```js
repeats: false
```

Therefore, the notification appears only once.

### 8. Clear Input

After scheduling:

```js
setTitle("");
setText("");
```

Both input fields are cleared.

## Important APIs

### `Notifications.setNotificationHandler()`

Controls notification presentation.

### `Notifications.requestPermissionsAsync()`

Requests notification permission.

### `Notifications.scheduleNotificationAsync()`

Schedules a local notification.

### `Notifications.SchedulableTriggerInputTypes.TIME_INTERVAL`

Specifies a time-interval trigger.

## Notification Flow

```text
User enters title
        ↓
User enters message
        ↓
Press Schedule Notification
        ↓
Validate input
        ↓
Request permission
        ↓
Check permission
        ↓
Schedule notification
        ↓
Wait 24 hours
        ↓
Notification appears
```

## UI Components

### Text

Displays the heading.

### TextInput

Used for the notification title and message.

### Button

Starts notification scheduling.

### Alert

Displays validation, permission, and success messages.

### StyleSheet

Provides simple styling.

## Project Structure

```text
project/
│
├── App.js
├── package.json
└── README.md
```

## Example

User enters:

```text
Title:
Daily Reminder

Message:
Remember to complete your study work.
```

After pressing **Schedule Notification**, the app requests permission and schedules the notification for 24 hours later.

## Notes

- Install `expo-notifications` before running the project.
- This example uses local notification scheduling.
- The notification is scheduled for 24 hours.
- `repeats: false` makes it a one-time notification.
- The title and message come from the `TextInput` fields.
- Notification permission must be granted before scheduling.
- The UI uses simple React Native styling.

## Conclusion

The basic notification flow is:

**User Input → Permission → Schedule Notification → 24-Hour Timer → Notification**

This project demonstrates the basic concepts of `expo-notifications`, notification permissions, notification handlers, and time-based local notification scheduling.
