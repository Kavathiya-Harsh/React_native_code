# Share API Demo - React Native + Expo

A beginner-friendly React Native and Expo project that demonstrates how
to share text, URLs, product details, contact details, order details,
referral links, images, and files.

## Features

-   Share custom text entered by the user
-   Share a website URL
-   Share product information
-   Share contact information
-   Share order information
-   Share a referral link
-   Pick and share an image
-   Pick and share any file
-   Check whether file sharing is available
-   Show share success/cancel results
-   Simple and beginner-friendly UI

## Technologies Used

-   React Native
-   Expo
-   JavaScript
-   `react-native` Share API
-   `expo-sharing`
-   `expo-image-picker`
-   `expo-document-picker`

## Installation

Create an Expo project and install the required packages:

``` bash
npx expo install expo-sharing
npx expo install expo-image-picker
npx expo install expo-document-picker
```

## Complete Code

``` jsx
import React, { useState } from "react";

import {
  View,
  Text,
  TextInput,
  Pressable,
  StyleSheet,
  Share,
  Alert,
} from "react-native";

import * as Sharing from "expo-sharing";
import * as ImagePicker from "expo-image-picker";
import * as DocumentPicker from "expo-document-picker";

export default function App() {
  const [text, setText] = useState("");
  const [shareResult, setShareResult] = useState("");

  // --------------------------------
  // 1. SHARE TEXT
  // --------------------------------

  const shareText = async () => {
    if (!text.trim()) {
      Alert.alert("Empty Text", "Please enter some text first.");
      return;
    }

    const result = await Share.share({
      message: text.trim(),
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "Content was shared successfully.");

      if (result.activityType) {
        console.log("Activity Type:", result.activityType);
      }
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 2. SHARE URL
  // --------------------------------

  const shareURL = async () => {
    const result = await Share.share({
      title: "React Native Website",
      message: "Check this website:\nhttps://reactnative.dev",
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "URL was shared successfully.");
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 3. SHARE PRODUCT
  // --------------------------------

  const shareProduct = async () => {
    const product = {
      name: "React Native Course",
      price: 999,
      url: "https://example.com/course",
    };

    const message = `
Check out this product!

Name: ${product.name}
Price: ₹${product.price}

${product.url}
`;

    const result = await Share.share({
      message: message,
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "Product details shared.");
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 4. SHARE CONTACT
  // --------------------------------

  const shareContact = async () => {
    const contact = {
      name: "Rajesh Sharma",
      phone: "+91 9876543210",
      email: "rajesh@example.com",
    };

    const message = `
Contact Details

Name: ${contact.name}
Phone: ${contact.phone}
Email: ${contact.email}
`;

    const result = await Share.share({
      message: message,
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "Contact details shared.");
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 5. SHARE ORDER
  // --------------------------------

  const shareOrder = async () => {
    const order = {
      id: "ORD-1001",
      product: "Laptop",
      amount: 55000,
    };

    const message = `
Order Details

Order ID: ${order.id}
Product: ${order.product}
Amount: ₹${order.amount}
`;

    const result = await Share.share({
      message: message,
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "Order details shared.");
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 6. SHARE REFERRAL LINK
  // --------------------------------

  const shareReferral = async () => {
    const referralCode = "RAJESH123";

    const referralLink =
      `https://example.com/invite/${referralCode}`;

    const result = await Share.share({
      message: `Join me on this app!\n\n${referralLink}`,
    });

    setShareResult(result.action);

    if (result.action === Share.sharedAction) {
      Alert.alert("Success", "Referral link shared.");
    }

    if (result.action === Share.dismissedAction) {
      Alert.alert("Cancelled", "Share dialog was dismissed.");
    }
  };

  // --------------------------------
  // 7. PICK AND SHARE IMAGE
  // --------------------------------

  const shareImage = async () => {
    const available = await Sharing.isAvailableAsync();

    if (!available) {
      Alert.alert(
        "Unavailable",
        "File sharing is not available."
      );

      return;
    }

    const result =
      await ImagePicker.launchImageLibraryAsync({
        mediaTypes: ["images"],
        allowsEditing: false,
        quality: 1,
      });

    if (result.canceled) {
      return;
    }

    const image = result.assets[0];

    console.log("IMAGE URI:", image.uri);

    await Sharing.shareAsync(
      image.uri,
      {
        mimeType:
          image.mimeType || "image/jpeg",

        dialogTitle: "Share Image",
      }
    );

    Alert.alert("Success", "Image sharing opened.");
  };

  // --------------------------------
  // 8. PICK AND SHARE FILE
  // --------------------------------

  const shareFile = async () => {
    const available = await Sharing.isAvailableAsync();

    if (!available) {
      Alert.alert(
        "Unavailable",
        "File sharing is not available."
      );

      return;
    }

    const result =
      await DocumentPicker.getDocumentAsync({
        type: "*/*",
        copyToCacheDirectory: true,
      });

    if (result.canceled) {
      return;
    }

    const file = result.assets[0];

    console.log("FILE:", file);
    console.log("FILE URI:", file.uri);

    await Sharing.shareAsync(
      file.uri,
      {
        mimeType: file.mimeType,
        dialogTitle: "Share File",
      }
    );

    Alert.alert("Success", "File sharing opened.");
  };

  // --------------------------------
  // UI
  // --------------------------------

  return (
    <View style={styles.container}>

      <Text style={styles.title}>
        Share API Demo
      </Text>

      <Text style={styles.subtitle}>
        React Native + Expo
      </Text>

      <TextInput
        style={styles.input}
        placeholder="Enter text to share"
        value={text}
        onChangeText={setText}
        multiline
      />

      <Pressable
        style={styles.button}
        onPress={shareText}
      >
        <Text style={styles.buttonText}>
          Share Text
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareURL}
      >
        <Text style={styles.buttonText}>
          Share URL
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareProduct}
      >
        <Text style={styles.buttonText}>
          Share Product
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareContact}
      >
        <Text style={styles.buttonText}>
          Share Contact
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareOrder}
      >
        <Text style={styles.buttonText}>
          Share Order
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareReferral}
      >
        <Text style={styles.buttonText}>
          Share Referral Link
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareImage}
      >
        <Text style={styles.buttonText}>
          Pick & Share Image
        </Text>
      </Pressable>

      <Pressable
        style={styles.button}
        onPress={shareFile}
      >
        <Text style={styles.buttonText}>
          Pick & Share File
        </Text>
      </Pressable>

      {shareResult !== "" && (
        <Text style={styles.result}>
          Last Result: {shareResult}
        </Text>
      )}

    </View>
  );
}

// --------------------------------
// SIMPLE CSS
// --------------------------------

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: "center",
  },

  title: {
    fontSize: 28,
    fontWeight: "bold",
    textAlign: "center",
    marginBottom: 5,
  },

  subtitle: {
    fontSize: 16,
    textAlign: "center",
    marginBottom: 25,
  },

  input: {
    borderWidth: 1,
    borderColor: "#999",
    borderRadius: 8,
    padding: 12,
    minHeight: 70,
    marginBottom: 15,
  },

  button: {
    padding: 14,
    borderRadius: 8,
    backgroundColor: "#eeeeee",
    alignItems: "center",
    marginBottom: 10,
  },

  buttonText: {
    fontSize: 16,
    fontWeight: "600",
  },

  result: {
    marginTop: 15,
    textAlign: "center",
    fontSize: 14,
  },
});
```

## How It Works

### 1. Share Text

The user enters text in the `TextInput`. The `Share.share()` API opens
the native share sheet.

``` js
const result = await Share.share({
  message: text.trim(),
});
```

### 2. Share URL

A URL can be shared using the React Native Share API.

``` js
const result = await Share.share({
  title: "React Native Website",
  message: "Check this website:\nhttps://reactnative.dev",
});
```

### 3. Share Product

Product information is first stored in an object and then converted into
a message.

### 4. Share Contact

Contact details such as name, phone, and email are converted into
shareable text.

### 5. Share Order

Order ID, product name, and amount are shared as formatted text.

### 6. Share Referral Link

A referral code is added to a URL and shared with a message.

### 7. Pick and Share Image

The image picker selects an image from the device. The selected image
provides a local `uri`, which is passed to `Sharing.shareAsync()`.

``` js
const image = result.assets[0];

await Sharing.shareAsync(image.uri);
```

### 8. Pick and Share File

The document picker selects a file. The file's local `uri` is passed to
`Sharing.shareAsync()`.

``` js
const file = result.assets[0];

await Sharing.shareAsync(file.uri);
```

## Important APIs

### React Native Share

Used for:

-   Text
-   URLs
-   Product information
-   Contact information
-   Order information
-   Referral links

Main method:

``` js
Share.share()
```

### Expo Sharing

Used for sharing local files.

``` js
Sharing.isAvailableAsync()
```

checks whether file sharing is available.

``` js
Sharing.shareAsync(fileUri)
```

opens the native share sheet for a local file.

### Expo Image Picker

Used to select an image from the device.

``` js
ImagePicker.launchImageLibraryAsync()
```

### Expo Document Picker

Used to select documents and other files.

``` js
DocumentPicker.getDocumentAsync()
```

## Share Result

The React Native Share API returns a result.

``` js
const result = await Share.share({
  message: "Hello",
});
```

The result can be checked using:

``` js
Share.sharedAction
```

and:

``` js
Share.dismissedAction
```

The selected activity type can also be available through:

``` js
result.activityType
```

## Text/URL vs File Sharing

Use the React Native Share API when you want to share:

-   Text
-   URLs
-   Dynamic messages
-   Product details
-   Contact details
-   Order details
-   Referral links

Use `expo-sharing` when you want to share:

-   Images
-   PDFs
-   Videos
-   Documents
-   Other local files

## Sharing Flow

### Text or URL

``` text
User Input
    ↓
Share.share()
    ↓
Native Share Sheet
```

### Image

``` text
Image Picker
    ↓
Local Image URI
    ↓
Sharing.shareAsync()
    ↓
Native Share Sheet
```

### File

``` text
Document Picker
    ↓
Local File URI
    ↓
Sharing.shareAsync()
    ↓
Native Share Sheet
```

## Project Structure

A simple project can use:

``` text
project/
│
├── App.js
├── package.json
└── README.md
```

## Notes

-   `Share.share()` is used for text and URL sharing.
-   `expo-sharing` is used for local file sharing.
-   `ImagePicker` provides a local image URI.
-   `DocumentPicker` provides a local file URI.
-   `Sharing.isAvailableAsync()` checks whether file sharing is
    available.
-   `result.canceled` is checked before using the selected image or
    file.
-   The example intentionally uses simple React Native styling and
    beginner-friendly code.

## Conclusion

This project demonstrates the main sharing flows in a React Native +
Expo application:

**Text/URL → React Native Share API**

**Local File → Expo Sharing**

**Image Picker/Document Picker → Local URI → Expo Sharing → Native Share
Sheet**
