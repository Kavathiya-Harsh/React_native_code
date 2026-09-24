# React Native Camera

This example uses `expo-camera` and `@react-native-community/slider` to create a simple camera screen with:

* Camera permission
* Front/back camera flip
* Flash on/off
* Zoom slider
* Photo capture
* Simple styling

## Code

```jsx
import React, { useRef, useState } from "react";
import {
  View,
  Button,
  StyleSheet,
  Text,
} from "react-native";
import Slider from "@react-native-community/slider";
import {
  CameraView,
  useCameraPermissions,
} from "expo-camera";

export default function App() {
  const cameraRef = useRef(null);
  const [permission, requestPermission] = useCameraPermissions();
  const [facing, setFacing] = useState("back");
  const [flash, setFlash] = useState("off");
  const [zoom, setZoom] = useState(0);

  if (!permission) return <View />;

  if (!permission.granted) {
    return (
      <View style={styles.permissionContainer}>
        <Button
          title="Grant Camera Permission"
          onPress={requestPermission}
        />
      </View>
    );
  }

  const takePhoto = async () => {
    try {
      const photo = await cameraRef.current.takePictureAsync();
      console.log(photo.uri);
    } catch (error) {
      console.log(error);
    }
  };

  const handleFlashCamera = () => {
    setFlash((prev) => (prev === "off" ? "on" : "off"));
  };

  return (
    <View style={styles.container}>
      <CameraView
        ref={cameraRef}
        facing={facing}
        flash={flash}
        zoom={zoom}
        style={styles.camera}
      />

      <View style={styles.sliderContainer}>
        <Text style={styles.text}>Zoom</Text>

        <Slider
          minimumValue={0}
          maximumValue={1}
          step={0.01}
          value={zoom}
          onValueChange={(value) => setZoom(value)}
          style={styles.slider}
        />
      </View>

      <View style={styles.buttonContainer}>
        <Button
          title="Flip"
          onPress={() =>
            setFacing(
              facing === "back" ? "front" : "back"
            )
          }
        />

        <Button
          title={
            flash === "off"
              ? "Flash Off"
              : "Flash On"
          }
          onPress={handleFlashCamera}
        />

        <Button
          title="Photo"
          onPress={takePhoto}
        />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 10,
    backgroundColor: "#fff",
  },

  permissionContainer: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },

  camera: {
    flex: 1,
    width: "100%",
  },

  sliderContainer: {
    padding: 10,
  },

  text: {
    fontSize: 16,
    marginBottom: 5,
  },

  slider: {
    width: "100%",
    height: 40,
  },

  buttonContainer: {
    gap: 10,
    padding: 10,
  },
});
```
