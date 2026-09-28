# React Native Network

## Study Code

```jsx
import { useEffect, useState } from "react";

import { View, Text, Button } from "react-native";

import * as Network from "expo-network";

export default function App() {
    const [networkState, setNetworkState] = useState(null);

    useEffect(() => {
        const subscription = Network.addNetworkStateListener((state) => {
            console.log("Network changed:", state);
            setNetworkState(state);
        });

        return () => {
            subscription.remove();
        };
    }, []);

    // const handleNetwork = async () => {
    //     const state = await Network.getNetworkStateAsync();

    //     console.log("Current Network:", state);
    //     setNetworkState(state);
    // };

    return (
        <View
            style={{
                flex: 1,
                justifyContent: "center",
                alignItems: "center",
            }}
        >
            <Text>Network Screen</Text>

            {/* <Button title="Check Network" onPress={handleNetwork} /> */}

            {networkState && (
                <Text>
                    Connected: {networkState.isConnected ? "Yes" : "No"}
                </Text>
            )}
        </View>
    );
}
```
