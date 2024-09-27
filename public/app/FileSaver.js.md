## FileSaver.js Internal Documentation 

**Table of Contents** 

* [Introduction](#introduction)
* [Implementation Details](#implementation-details)
    * [Variables](#variables)
    * [Functions](#functions)
    * [FileSaver Class](#filesaver-class)
    * [saveAs Function](#saveas-function)
    * [IE10+ (native saveAs)](#ie10-native-saveas) 
    * [FileSaver Prototype](#filesaver-prototype)
* [Usage](#usage) 


### Introduction 

This code implements the `saveAs()` function, providing a cross-browser mechanism to download files directly from the browser. The `saveAs()` function utilizes various browser APIs and workarounds to ensure compatibility across different browsers, including:

* **`a.download`**: The most common method for modern browsers.
* **Web Filesystem**: As a fallback for browsers that don't support `a.download`.
* **Object URLs**: Used for browsers where other methods are not viable.
* **IE10+ (native saveAs)**: Leverages the native `navigator.msSaveOrOpenBlob` API for compatibility.

### Implementation Details 

#### Variables 

| Variable | Description |
|---|---|
| `doc` | Represents the current document object. |
| `get_URL` | A function to retrieve the appropriate URL object for the current browser (e.g., `window.URL`). |
| `save_link` | An `<a>` element used for initiating the download process. |
| `can_use_save_link` | A boolean indicating whether the `download` attribute is supported in the current browser. |
| `click` | A function to simulate a click event on a DOM element. |
| `is_safari` | A boolean indicating whether the current browser is Safari. |
| `is_chrome_ios` | A boolean indicating whether the current browser is Chrome on iOS. |
| `throw_outside` | A function to handle errors by throwing them in a separate execution context. |
| `force_saveable_type` | The default MIME type for saving files when the actual type is unknown. |
| `arbitrary_revoke_timeout` | The delay (in milliseconds) after which object URLs are revoked. |
| `revoke` | A function to revoke object URLs or remove File objects. |
| `dispatch` | A function to dispatch events related to the save process. |
| `auto_bom` | A function to prepend a Byte Order Mark (BOM) to the file data for specific MIME types (UTF-8 XML and text/*). |

#### Functions 

| Function | Description |
|---|---|
| `get_URL()` | Retrieves the appropriate URL object for the current browser. |
| `click(node)` | Simulates a click event on a DOM element. |
| `throw_outside(ex)` | Handles errors by throwing them in a separate execution context. |
| `revoke(file)` | Revokes object URLs or removes File objects. |
| `dispatch(filesaver, event_types, event)` | Dispatches events related to the save process. |
| `auto_bom(blob)` | Prepends a Byte Order Mark (BOM) to the file data for specific MIME types. |

#### FileSaver Class

The `FileSaver` class represents a file to be saved. 

| Property | Description |
|---|---|
| `readyState` | Indicates the current state of the save process. |
| `INIT` | The initial state (0). |
| `WRITING` | The state when the file is being saved (1). |
| `DONE` | The state when the file has been saved successfully (2). |

| Method | Description |
|---|---|
| `abort()` | Cancels the save process. |
| `onwritestart` | Event handler for the "writestart" event. |
| `onprogress` | Event handler for the "progress" event. |
| `onwrite` | Event handler for the "write" event. |
| `onabort` | Event handler for the "abort" event. |
| `onerror` | Event handler for the "error" event. |
| `onwriteend` | Event handler for the "writeend" event. |

#### saveAs Function

The `saveAs()` function is the main entry point for saving files. 

| Parameter | Description |
|---|---|
| `blob` | The Blob object representing the file data. |
| `name` (optional) | The desired filename for the downloaded file. |
| `no_auto_bom` (optional) | A boolean indicating whether to skip automatic BOM prepending. |

#### IE10+ (native saveAs)

If the browser supports the native `navigator.msSaveOrOpenBlob` API (IE10+), this implementation is used directly for saving files.

#### FileSaver Prototype

The `FileSaver` class prototype defines the following methods:

| Method | Description |
|---|---|
| `abort()` | Cancels the save process. |
| `readyState` | Indicates the current state of the save process. |
| `INIT` | The initial state (0). |
| `WRITING` | The state when the file is being saved (1). |
| `DONE` | The state when the file has been saved successfully (2). |
| `error` | Event handler for the "error" event. |
| `onwritestart` | Event handler for the "writestart" event. |
| `onprogress` | Event handler for the "progress" event. |
| `onwrite` | Event handler for the "write" event. |
| `onabort` | Event handler for the "abort" event. |
| `onerror` | Event handler for the "error" event. |
| `onwriteend` | Event handler for the "writeend" event. |

### Usage 

The `saveAs()` function can be used as follows:

```javascript
// Save a Blob object as "myFile.txt" 
saveAs(myBlob, "myFile.txt");

// Save a Blob object with a specific MIME type as "myData.json"
saveAs(new Blob([JSON.stringify(myData)], { type: 'application/json' }), "myData.json");
```

The `saveAs()` function supports various event handlers, such as `onwritestart`, `onprogress`, `onwrite`, `onabort`, `onerror`, and `onwriteend`, which can be used to monitor the download process and handle specific events. 
