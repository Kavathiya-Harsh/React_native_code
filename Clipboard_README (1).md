# React Native Clipboard

```jsx
import { useState } from "react";
import {
  View,
  Text,
  TextInput,
  Button,
  Image,
  StyleSheet,
} from "react-native";
import * as Clipboard from "expo-clipboard";

export default function ClipboardApp() {
  const [text, setText] = useState("");
  const [clipboardText, setClipboardText] = useState("");
  const [image, setImage] = useState(null);

  const handleCopyText = async () => {
    await Clipboard.setStringAsync(text);
  };

  const CopyUrl = async () => {
    const url = "https://www.google.com";
    await Clipboard.setStringAsync(url);
  };

  const handleGetCopiedText = async () => {
    const value = await Clipboard.getStringAsync();
    setClipboardText(value);
  };

  const pasteImage = async () => {
    const hasImage = await Clipboard.hasImageAsync();

    if (!hasImage) {
      return;
    }

    const result = await Clipboard.getImageAsync({
      format: "png",
    });

    setImage(result);
    console.log(result);
  };

  return (
    <View style={styles.container}>
      <TextInput
        placeholder="Enter Text..."
        value={text}
        onChangeText={setText}
        style={styles.input}
      />

      <Button
        title="Copy Text"
        onPress={handleCopyText}
      />

      <View style={styles.buttonContainer}>
        <Button
          title="Copy URL"
          onPress={CopyUrl}
        />
      </View>

      <View style={styles.buttonContainer}>
        <Button
          title="Paste Text OR URL"
          onPress={handleGetCopiedText}
        />
      </View>

      <View style={styles.buttonContainer}>
        <Button
          title="Paste Image"
          onPress={pasteImage}
        />

        {image && (
          <Image
            source={{ uri: image.data }}
            style={styles.image}
          />
        )}
      </View>

      <Text style={styles.clipboardText}>
        {clipboardText}
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: "center",
    backgroundColor: "#F8FAFC",
  },

  input: {
    borderWidth: 1,
    borderColor: "#CBD5E1",
    backgroundColor: "#FFFFFF",
    padding: 12,
    marginBottom: 10,
    borderRadius: 8,
  },

  buttonContainer: {
    marginTop: 10,
  },

  image: {
    width: 250,
    height: 250,
    marginTop: 15,
    alignSelf: "center",
  },

  clipboardText: {
    marginTop: 20,
    fontSize: 18,
  },
});
```

## Install Package

```bash
npx expo install expo-clipboard
```

## Run

```bash
npx expo start
```
