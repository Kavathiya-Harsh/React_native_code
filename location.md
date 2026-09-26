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

## Get Location Parameters & divert to google map 


```jsx

import React, { useState } from "react";
import {
  View,
  Text,
  Button,
  StyleSheet,
  Linking,
} from "react-native";

import * as Location from "expo-location";

export default function LocationScreen() {
  const [location, setLocation] = useState(null);

  const getCurrentLocation = async () => {
    const { granted } =
      await Location.requestForegroundPermissionsAsync();

    if (!granted) {
      alert("Location Permission Denied");
      return;
    }

    const currentLocation =
      await Location.getCurrentPositionAsync({
        accuracy: Location.Accuracy.Highest,
      });

    setLocation(currentLocation);

    console.log(currentLocation);
  };

  const openGoogleMaps = () => {
    if (!location) {
      alert("First get your location");
      return;
    }

    const latitude = location.coords.latitude;
    const longitude = location.coords.longitude;

    const url = `https://www.google.com/maps/search/?api=1&query=${latitude},${longitude}`;

    Linking.openURL(url);
  };

  return (
    <View style={styles.container}>

      <Button
        title="GET CURRENT LOCATION"
        onPress={getCurrentLocation}
      />

      {location && (
        <View style={styles.info}>
          <Text>
            Accuracy: {location.coords.accuracy}
          </Text>

          <Text>
            Latitude: {location.coords.latitude}
          </Text>

          <Text>
            Longitude: {location.coords.longitude}
          </Text>
        </View>
      )}

      <View style={styles.space} />

      <Button
        title="OPEN IN GOOGLE MAPS"
        onPress={openGoogleMaps}
      />

    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    justifyContent: "center",
  },

  info: {
    marginTop: 20,
    padding: 20,
    backgroundColor: "#fff",
    borderRadius: 15,
    elevation: 3,
  },

  space: {
    height: 20,
  },
});

```
