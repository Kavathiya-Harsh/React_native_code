# React Native Location & Map

A simple React Native location screen using **Expo Location** and **React Native Maps**.

## Features

* Request location permission
* Get current location
* Display latitude
* Display longitude
* Display location accuracy
* Show current location on map
* Show marker on current location
* Automatically update location every 10 seconds
* Reverse geocoding code included as comments

## Packages

```bash
npx expo install expo-location
npm install react-native-maps
```

## Code

```jsx
import React, { useEffect, useState } from "react";
import { View, Button, StyleSheet, Text } from "react-native";
import MapView, { Marker } from "react-native-maps";
import * as Location from "expo-location";

export default function LocationScreen() {
  const [location, setLocation] = useState(null);
  const [accuracy, setAccuracy] = useState(null);
  const [latitude, setLatitude] = useState(null);
  const [longitude, setLongitude] = useState(null);
  // const [address, setAddress] = useState(null);

  const handleGrantPermission = async () => {
    const { granted } =
      await Location.requestForegroundPermissionsAsync();

    if (granted) {
      alert("Location Permission Granted");
    } else {
      alert("Location Permission Denied");
    }
  };

  const handleGetCurrentLocation = async () => {
    const { granted } =
      await Location.requestForegroundPermissionsAsync();

    if (!granted) {
      alert("Permission Denied");
      return;
    }

    const currentLocation = await Location.getCurrentPositionAsync({
      accuracy: Location.Accuracy.Highest,
    });

    setLocation(currentLocation);
    setAccuracy(currentLocation.coords.accuracy);
    setLatitude(currentLocation.coords.latitude);
    setLongitude(currentLocation.coords.longitude);
  };

  // const handleGetAddress = async () => {
  //   const { granted } =
  //     await Location.requestForegroundPermissionsAsync();

  //   if (!granted) {
  //     alert("Permission Denied");
  //     return;
  //   }

  //   const currentLocation = await Location.getCurrentPositionAsync({
  //     accuracy: Location.Accuracy.Highest,
  //   });

  //   const result = await Location.reverseGeocodeAsync({
  //     latitude: currentLocation.coords.latitude,
  //     longitude: currentLocation.coords.longitude,
  //   });

  //   if (result.length > 0) {
  //     const place = result[0];

  //     setAddress(
  //       `${place.name || ""}\n${place.street || ""}\n${place.city || ""}, ${
  //         place.region || ""
  //       }\n${place.postalCode || ""}\n${place.country || ""}`
  //     );
  //   } else {
  //     setAddress("Address not found");
  //   }
  // };

  useEffect(() => {
    const getLocation = async () => {
      const { granted } =
        await Location.requestForegroundPermissionsAsync();

      if (!granted) return;

      const currentLocation = await Location.getCurrentPositionAsync({
        accuracy: Location.Accuracy.Highest,
      });

      setLocation(currentLocation);
    };

    getLocation();

    const timer = setInterval(() => {
      getLocation();
    }, 10000);

    return () => clearInterval(timer);
  }, []);

  if (!location) {
    return (
      <View style={styles.loadingContainer}>
        <Text>Loading Location...</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Button
        title="Grant Permission"
        onPress={handleGrantPermission}
      />

      <View style={{ height: 15 }} />

      <Button
        title="Get Current Location"
        onPress={handleGetCurrentLocation}
      />

      <View style={styles.infoContainer}>
        <Text>Accuracy: {accuracy}</Text>
        <Text>Latitude: {latitude}</Text>
        <Text>Longitude: {longitude}</Text>
      </View>

      <MapView
        style={styles.map}
        region={{
          latitude: location.coords.latitude,
          longitude: location.coords.longitude,
          latitudeDelta: 0.01,
          longitudeDelta: 0.01,
        }}
      >
        <Marker
          coordinate={{
            latitude: location.coords.latitude,
            longitude: location.coords.longitude,
          }}
          title="My Location"
        />
      </MapView>

      {/* <View style={{ marginTop: 10 }}>
        <Button title="Get Address" onPress={handleGetAddress} />
      </View>

      {address && (
        <View style={styles.addressContainer}>
          <Text style={{ fontWeight: "bold" }}>Address:</Text>
          <Text>{address}</Text>
        </View>
      )} */}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#F8FAFC",
    padding: 16,
  },

  loadingContainer: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#F8FAFC",
  },

  infoContainer: {
    marginTop: 16,
    marginBottom: 8,
    padding: 16,
    backgroundColor: "#FFFFFF",
    borderRadius: 16,
    borderWidth: 1,
    borderColor: "#E2E8F0",
    shadowColor: "#000",
    shadowOffset: {
      width: 0,
      height: 3,
    },
    shadowOpacity: 0.08,
    shadowRadius: 8,
    elevation: 3,
  },

  map: {
    flex: 1,
    marginTop: 12,
    borderRadius: 18,
    overflow: "hidden",
  },

  addressContainer: {
    marginTop: 15,
    padding: 14,
    backgroundColor: "#FFFFFF",
    borderRadius: 12,
    borderWidth: 1,
    borderColor: "#E2E8F0",
  },
});
```

## Run

```bash
npx expo start
```

## File

```text
LocationScreen.jsx
```
