## Extracting a Map from JSON Stream

### Table of Contents

* [1. Introduction](#1-introduction)
* [2. Code Overview](#2-code-overview)
* [3. Usage](#3-usage)

### 1. Introduction

This script extracts a specific map from a JSON stream. The JSON stream should contain an object with a key named `maps` containing an object of key-value pairs, where the keys are map identifiers. 

### 2. Code Overview

| Line Number | Code | Description |
|---|---|---|
| 1 | `// Extracts the input map.` |  Inline comment explaining the purpose of the script. |
| 2 | `const fs = require('fs');` | Imports the Node.js `fs` module for file system operations. |
| 3 | `const JSONStream = require('JSONStream');` | Imports the `JSONStream` module for parsing JSON streams. |
| 4 | `const stream = fs.createReadStream(process.argv[3]);` | Creates a readable stream from the input file specified as the third command-line argument. |
| 5 | `const mid = process.argv[2];` | Stores the map identifier provided as the second command-line argument. |
| 6 | `const parser = JSONStream.parse('maps.$*');` | Creates a JSONStream parser to extract the `maps` object from the input stream. |
| 7 | `parser.on('data', data => {` | Defines a callback function that is executed for each data chunk emitted by the parser. |
| 8 | `if (data.key == mid) {` | Checks if the current data chunk's key matches the provided map identifier. |
| 9 | `console.log(JSON.stringify(data.value, null, 2));` | Prints the corresponding value (the map) to the console as a formatted JSON string. |
| 10 | `}` | Ends the conditional block. |
| 11 | `});` | Ends the `on('data')` event handler. |
| 12 | `stream.pipe(parser);` | Pipes the input stream to the parser, initiating the parsing process. |

### 3. Usage

To run the script, execute the following command in your terminal:

```bash
node extract_map.js <map_identifier> <input_file>
```

* Replace `<map_identifier>` with the identifier of the desired map.
* Replace `<input_file>` with the path to the JSON file containing the maps object.

For example:

```bash
node extract_map.js map1 input.json
```

This command will extract the map with the identifier `map1` from the `input.json` file and print it to the console. 
