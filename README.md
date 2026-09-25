# React Native Contacts

```jsx
import {
  View,
  Text,
  Button,
  Image,
  TextInput,
  FlatList,
  Alert,
  StyleSheet,
} from "react-native";
import * as Contacts from "expo-contacts";
import { useState } from "react";

export default function ContactsScreen() {
  const [contact, setContact] = useState([]);
  const [searchContact, setSearchContact] = useState("");

  const handleGetPermission = async () => {
    const permission = await Contacts.getPermissionsAsync();

    if (!permission.granted) {
      Alert.alert(
        "Permission Denied",
        "Contact permission is required"
      );
      return;
    }

    const { data } = await Contacts.getContactsAsync({
      sort: Contacts.ContactTypes.FirstName,
    });

    setContact(data);
  };

  const searchData = contact.filter((ele) => {
    const name = ele.name?.toLowerCase() || "";
    const phone = ele.phoneNumbers?.[0]?.number || "";

    return (
      name.includes(searchContact.toLowerCase()) ||
      phone.includes(searchContact)
    );
  });

  const renderContact = ({ item }) => (
    <View style={styles.contactContainer}>
      {item.image?.uri ? (
        <Image
          source={{ uri: item.image.uri }}
          style={styles.image}
        />
      ) : (
        <View style={styles.avatar}>
          <Text style={styles.avatarText}>
            {item.name?.charAt(0)?.toUpperCase() || "?"}
          </Text>
        </View>
      )}

      <View style={styles.info}>
        <Text style={styles.name}>
          {item.name || "No contact name"}
        </Text>

        <Text style={styles.phone}>
          {item.phoneNumbers?.[0]?.number || "No phone number"}
        </Text>
      </View>
    </View>
  );

  return (
    <View style={styles.container}>
      <Text style={styles.title}>My Contacts</Text>

      <Button
        title="Get Contacts"
        onPress={handleGetPermission}
      />

      <TextInput
        style={styles.input}
        placeholder="Search contact"
        value={searchContact}
        onChangeText={setSearchContact}
      />

      <FlatList
        data={searchData}
        renderItem={renderContact}
        keyExtractor={(item) => item.id}
        contentContainerStyle={styles.list}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#F8FAFC",
    padding: 16,
  },

  title: {
    fontSize: 24,
    fontWeight: "bold",
    marginBottom: 15,
  },

  input: {
    height: 45,
    backgroundColor: "#FFFFFF",
    borderWidth: 1,
    borderColor: "#CBD5E1",
    borderRadius: 8,
    paddingHorizontal: 12,
    marginTop: 15,
    marginBottom: 10,
  },

  list: {
    paddingTop: 5,
  },

  contactContainer: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: "#FFFFFF",
    padding: 12,
    marginBottom: 10,
    borderRadius: 10,
    borderWidth: 1,
    borderColor: "#E2E8F0",
  },

  image: {
    width: 50,
    height: 50,
    borderRadius: 25,
  },

  avatar: {
    width: 50,
    height: 50,
    borderRadius: 25,
    backgroundColor: "#CBD5E1",
    justifyContent: "center",
    alignItems: "center",
  },

  avatarText: {
    fontSize: 20,
    fontWeight: "bold",
  },

  info: {
    marginLeft: 12,
    flex: 1,
  },

  name: {
    fontSize: 16,
    fontWeight: "bold",
    marginBottom: 4,
  },

  phone: {
    fontSize: 14,
    color: "#64748B",
  },
});
```

## Install Package

```bash
npx expo install expo-contacts
```

## Run

```bash
npx expo start
```
