# Expo React Native File Download with Progress

This example demonstrates how to download a file in **React Native using Expo File System**, display the download progress, and provide a cancel button UI.

> **Note:** The code below is kept exactly as provided without changing any code.

## Complete Code

```jsx
import { Button, StyleSheet, Text, View } from "react-native";
import { File, Paths } from "expo-file-system";
import { useState } from "react";

export default function DownloadScreen() {
    const [progress, setProgress] = useState(0);
    const [downloading, setDownloading] = useState(false);

    const downloadPDF = async () => {
        const url =
            "https://drive.google.com/uc?export=download&id=1OcCmHAkIkhDPVOZJ9a-xbu_hhx36FvyX";

        const destination = new File(Paths.cache, "Video.mp4");

        setDownloading(true);
        setProgress(0);

        try {
            const file = await File.downloadFileAsync(url, destination, {
                idempotent: true,

                onProgress: ({ bytesWritten, totalBytes }) => {
                    if (totalBytes > 0) {
                        const res = (bytesWritten / totalBytes) * 100;
                        setProgress(res);
                    }
                },
            });

            console.log("Downloaded file:", file.uri);
        } catch (error) {
            console.log("Download error:", error);
        }

        setDownloading(false);
    };

    const cancelDownload = () => {
        // File.downloadFileAsync() cannot be cancelled
        // using AbortController in this API.
        console.log("Cancel clicked");
    };

    return (
        <View style={styles.container}>
            <Text style={styles.title}>PDF Downloader</Text>

            <Button
                title="Download Resume"
                onPress={downloadPDF}
            />

            <Text style={styles.progress}>
                {Math.round(progress)}%
            </Text>

            <Button
                title="Cancel Download"
                onPress={cancelDownload}
                disabled={!downloading}
            />
        </View>
    );
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
        padding: 20,
        backgroundColor: "white",
    },

    title: {
        fontSize: 24,
        fontWeight: "bold",
        marginBottom: 20,
    },

    progress: {
        fontSize: 20,
        margin: 20,
    },
});
```

---

# Explanation

## 1. React Native Imports

```jsx
import { Button, StyleSheet, Text, View } from "react-native";
```

These are React Native components used to build the UI.

### `View`

Works like a container.

```jsx
<View>
    ...
</View>
```

It is similar to a `<div>` in web development.

### `Text`

Used to display text.

```jsx
<Text>Hello</Text>
```

### `Button`

Creates a clickable button.

```jsx
<Button
    title="Download Resume"
    onPress={downloadPDF}
/>
```

### `StyleSheet`

Used to create component styles.

```jsx
const styles = StyleSheet.create({
    ...
});
```

---

# 2. Expo File System Import

```jsx
import { File, Paths } from "expo-file-system";
```

`expo-file-system` is used for working with files on the device.

### `File`

The `File` class represents a file on the device.

### `Paths`

Provides standard filesystem locations.

In this example:

```jsx
Paths.cache
```

means the application's cache directory.

---

# 3. `useState`

```jsx
import { useState } from "react";
```

`useState` is used to store values that can change during the component's lifecycle.

This code uses two states:

```jsx
const [progress, setProgress] = useState(0);
const [downloading, setDownloading] = useState(false);
```

---

# 4. Progress State

```jsx
const [progress, setProgress] = useState(0);
```

This stores the download percentage.

Initially:

```text
progress = 0
```

During download it can become:

```text
10
25
50
75
100
```

To update it:

```jsx
setProgress(res);
```

---

# 5. Downloading State

```jsx
const [downloading, setDownloading] = useState(false);
```

This stores whether a download is currently running.

Initially:

```text
downloading = false
```

When download starts:

```jsx
setDownloading(true);
```

When download finishes or fails:

```jsx
setDownloading(false);
```

This state is also used to enable or disable the Cancel button.

---

# 6. `downloadPDF()` Function

```jsx
const downloadPDF = async () => {
```

This function is responsible for downloading the file.

It is marked `async` because `File.downloadFileAsync()` returns a Promise.

---

# 7. Download URL

```jsx
const url =
    "https://drive.google.com/uc?export=download&id=1OcCmHAkIkhDPVOZJ9a-xbu_hhx36FvyX";
```

This contains the URL from which the file is downloaded.

The URL points to a Google Drive file.

---

# 8. Destination File

```jsx
const destination = new File(Paths.cache, "Video.mp4");
```

This creates the destination where the downloaded file will be stored.

The file is saved inside the application's cache directory.

The filename is:

```text
Video.mp4
```

So conceptually:

```text
Application Cache
       ↓
   Video.mp4
```

> The variable/function/UI text uses `PDF`, but the destination filename is `Video.mp4`. This explanation preserves your code exactly as requested.

---

# 9. Set Downloading State

```jsx
setDownloading(true);
```

This tells React Native that the download has started.

The state changes:

```text
false → true
```

---

# 10. Reset Progress

```jsx
setProgress(0);
```

Before every new download, the progress is reset to:

```text
0%
```

This prevents the previous download's percentage from being displayed.

---

# 11. `try...catch`

```jsx
try {
    ...
} catch (error) {
    ...
}
```

This is used for error handling.

If downloading succeeds, the code inside `try` runs normally.

If something goes wrong, the error is caught by `catch`.

---

# 12. Download File

```jsx
const file = await File.downloadFileAsync(url, destination, {
```

This is the main download operation.

It receives:

```text
URL
+
Destination
+
Options
```

The downloaded file is returned and stored in:

```jsx
file
```

---

# 13. `idempotent`

```jsx
idempotent: true,
```

This option allows the operation to safely overwrite/reuse the destination file when appropriate instead of failing simply because the target already exists.

This is useful when the same file may be downloaded again.

---

# 14. Download Progress Callback

```jsx
onProgress: ({ bytesWritten, totalBytes }) => {
```

This callback runs while the file is downloading.

It receives two important values:

### `bytesWritten`

How many bytes have already been downloaded.

Example:

```text
bytesWritten = 500000
```

### `totalBytes`

The total size of the file.

Example:

```text
totalBytes = 1000000
```

---

# 15. Check Total File Size

```jsx
if (totalBytes > 0) {
```

This ensures that the total file size is available before calculating the percentage.

Without a valid total size, percentage calculation would not be meaningful.

---

# 16. Calculate Percentage

```jsx
const res = (bytesWritten / totalBytes) * 100;
```

The formula is:

```text
Progress % = (Downloaded Bytes / Total Bytes) × 100
```

Example:

```text
Downloaded = 500 MB
Total      = 1000 MB
```

Then:

```text
(500 / 1000) × 100
= 50%
```

So:

```jsx
res = 50
```

---

# 17. Update Progress

```jsx
setProgress(res);
```

The calculated percentage is stored inside the React state.

This causes the UI to re-render with the latest progress.

---

# 18. Get Downloaded File URI

```jsx
console.log("Downloaded file:", file.uri);
```

After the download completes, the file's URI is printed in the console.

Example:

```text
Downloaded file: file:///.../Video.mp4
```

The URI identifies where the downloaded file exists on the device.

---

# 19. Error Handling

```jsx
catch (error) {
    console.log("Download error:", error);
}
```

If the download fails, the error is printed.

Possible reasons can include:

```text
No internet connection
Invalid URL
Server error
Permission/storage-related issue
Invalid response
```

---

# 20. Finish Download

```jsx
setDownloading(false);
```

After the download process finishes, the state changes:

```text
true → false
```

This happens after either success or failure because it is outside the `try/catch`.

---

# 21. Cancel Download Function

```jsx
const cancelDownload = () => {
```

This function executes when the Cancel button is pressed.

The current implementation only prints:

```jsx
console.log("Cancel clicked");
```

The comments explain that this particular `File.downloadFileAsync()` usage cannot be cancelled with `AbortController`.

So this button currently acts as a UI action/log rather than actually stopping the active transfer.

---

# 22. Main Container

```jsx
<View style={styles.container}>
```

This is the main screen container.

Its styling is defined later inside:

```jsx
styles.container
```

---

# 23. Screen Title

```jsx
<Text style={styles.title}>PDF Downloader</Text>
```

Displays:

```text
PDF Downloader
```

---

# 24. Download Button

```jsx
<Button
    title="Download Resume"
    onPress={downloadPDF}
/>
```

When the user presses the button:

```text
Download Resume
       ↓
downloadPDF()
       ↓
Start file download
```

The `onPress` prop connects the button to the function.

---

# 25. Display Progress

```jsx
<Text style={styles.progress}>
    {Math.round(progress)}%
</Text>
```

This displays the download percentage.

`Math.round()` converts decimal values into a whole number.

For example:

```text
45.67 → 46
72.12 → 72
99.89 → 100
```

So the UI displays:

```text
0%
25%
50%
75%
100%
```

instead of many decimal digits.

---

# 26. Cancel Button

```jsx
<Button
    title="Cancel Download"
    onPress={cancelDownload}
    disabled={!downloading}
/>
```

The button calls:

```jsx
cancelDownload
```

when pressed.

The important part is:

```jsx
disabled={!downloading}
```

This uses the opposite value of `downloading`.

### When downloading is false

```jsx
!false
```

becomes:

```jsx
true
```

So the button is disabled.

### When downloading is true

```jsx
!true
```

becomes:

```jsx
false
```

So the button becomes enabled.

---

# 27. Styles

```jsx
const styles = StyleSheet.create({
```

React Native styles are defined here.

## Container

```jsx
container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    padding: 20,
    backgroundColor: "white",
},
```

### `flex: 1`

Makes the container use the available screen space.

### `justifyContent: "center"`

Centers content vertically.

### `alignItems: "center"`

Centers content horizontally.

### `padding: 20`

Adds 20 units of internal spacing.

### `backgroundColor: "white"`

Makes the background white.

---

# 28. Title Style

```jsx
title: {
    fontSize: 24,
    fontWeight: "bold",
    marginBottom: 20,
},
```

This makes the title:

* Larger with `fontSize: 24`
* Bold with `fontWeight: "bold"`
* Separated from the next element with `marginBottom: 20`

---

# 29. Progress Style

```jsx
progress: {
    fontSize: 20,
    margin: 20,
},
```

The progress text is larger and gets spacing around it.

---

# Download Flow

The complete flow of the application is:

```text
User presses "Download Resume"
            ↓
       downloadPDF()
            ↓
    setDownloading(true)
            ↓
      setProgress(0)
            ↓
    File.downloadFileAsync()
            ↓
      File starts downloading
            ↓
     onProgress() executes
            ↓
Calculate percentage from bytes
            ↓
      setProgress(res)
            ↓
        UI updates
            ↓
       Download finishes
            ↓
    setDownloading(false)
```

---

# Important Concepts to Study

## `useState`

Used for storing changing UI data.

```jsx
const [value, setValue] = useState(initialValue);
```

---

## `async/await`

Used for handling asynchronous operations.

```jsx
const file = await File.downloadFileAsync(...);
```

---

## Promise

File downloading is asynchronous, so `await` waits for the Promise to complete.

---

## Callback

`onProgress` is a callback function that receives download progress information.

```jsx
onProgress: ({ bytesWritten, totalBytes }) => {
    ...
}
```

---

## Percentage Calculation

```jsx
(bytesWritten / totalBytes) * 100
```

This converts downloaded bytes into a percentage.

---

## Conditional Button State

```jsx
disabled={!downloading}
```

This dynamically enables or disables the button based on state.

---

## Error Handling

```jsx
try {
    ...
} catch (error) {
    ...
}
```

Used to safely handle download failures.

---

# Important Interview Questions

### What is `useState`?

`useState` is a React Hook used to store and update state inside a functional component.

### Why is `downloadPDF` declared as `async`?

Because the file download is asynchronous and uses `await`.

### What is `bytesWritten`?

It represents the amount of data downloaded so far.

### What is `totalBytes`?

It represents the total size of the file being downloaded.

### How is download progress calculated?

```text
(bytesWritten / totalBytes) × 100
```

### Why is `Math.round(progress)` used?

To display the progress as a whole-number percentage instead of decimals.

### What does `try...catch` do?

It handles errors that may occur during the asynchronous download operation.

### Why is `setDownloading(true)` used?

It tells the UI that a download is currently running.

### Why is `disabled={!downloading}` used?

It disables the Cancel button when there is no active download.

---

# Key Takeaway

This example combines several important React Native concepts in one practical feature:

```text
React Native UI
      +
useState
      +
Async/Await
      +
Expo File System
      +
Download Progress
      +
Error Handling
```

This makes it a useful example for learning **file downloading, state management, asynchronous JavaScript, callbacks, and conditional UI in React Native**.
