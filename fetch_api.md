```jsx
import React, { useEffect, useState } from "react";
import {
  View,
  Text,
  FlatList,
  TextInput,
  StyleSheet,
  ActivityIndicator,
} from "react-native";

export default function App() {
  const [users, setUsers] = useState([]);
  const [search, setSearch] = useState("");
  const [loading, setLoading] = useState(true);

  const getUsers = async () => {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/users"
    );

    const data = await response.json();

    setUsers(data);
    setLoading(false);
  };

  useEffect(() => {
    getUsers();
  }, []);

  // Search users
  const filteredUsers = users.filter((user) =>
    user.name.toLowerCase().includes(search.toLowerCase())
  );

  const renderUser = ({ item }) => {
    return (
      <View style={styles.card}>
        <Text style={styles.name}>{item.name}</Text>
        <Text>Email: {item.email}</Text>
        <Text>Phone: {item.phone}</Text>
        <Text>City: {item.address.city}</Text>
      </View>
    );
  };

  if (loading) {
    return (
      <View style={styles.loading}>
        <ActivityIndicator size="large" />
        <Text>Loading...</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Users List</Text>

      {/* Search Box */}
      <TextInput
        style={styles.search}
        placeholder="Search user..."
        value={search}
        onChangeText={setSearch}
      />

      <FlatList
        data={filteredUsers}
        renderItem={renderUser}
        keyExtractor={(item) => item.id.toString()}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    paddingTop: 50,
  },

  title: {
    fontSize: 25,
    fontWeight: "bold",
    marginBottom: 15,
  },

  search: {
    height: 50,
    borderWidth: 1,
    borderColor: "gray",
    borderRadius: 10,
    paddingHorizontal: 15,
    marginBottom: 15,
  },

  card: {
    padding: 15,
    marginBottom: 10,
    backgroundColor: "#eee",
    borderRadius: 10,
  },

  name: {
    fontSize: 18,
    fontWeight: "bold",
    marginBottom: 5,
  },

  loading: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
});
```
