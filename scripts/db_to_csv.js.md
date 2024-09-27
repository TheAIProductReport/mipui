# Database Summary CSV Generator

## Table of Contents

* [Overview](#overview)
* [Usage](#usage)
* [Code Breakdown](#code-breakdown)
* [Example Output](#example-output)
* [Dependencies](#dependencies)


## Overview

This code snippet generates a CSV file summarizing information from a database. It reads a JSON file containing database records and extracts relevant information such as ID (mid), creation timestamp, number of operations, and a readonly link. 

## Usage

To use this script, you need to provide the path to the JSON database file as an argument when running the script. Here's an example:

```bash
node db-summary.js path/to/database.json
```

The script will then output the summary information to the console in CSV format.

## Code Breakdown

```javascript
// Prints the db summary as csv.

const fs = require('fs'); // Import the file system module for reading the JSON file. 
const JSONStream = require('JSONStream'); // Import the JSONStream module for parsing the JSON file.

const stream = fs.createReadStream(process.argv[2]); // Create a readable stream from the JSON file provided as command line argument.
const parser = JSONStream.parse('maps.$*'); // Create a JSONStream parser to extract data from the 'maps' property of the JSON.

parser.on('data', data => { // For each piece of data parsed from the JSON:
  const mid = data.key; // Extract the ID (mid).
  const created = new Date(data.value.metadata.created).toISOString(); // Extract the creation timestamp in ISO format.
  const readonlyLink = 'https://www.mipui.net/app/index.html?mid=' + mid; // Construct the readonly link using the ID.
  const numOperations = 
      data.value.payload && data.value.payload.latestOperation ? // Check if payload and latestOperation exist
        data.value.payload.latestOperation.i.n : -1; // If they exist, extract the number of operations, otherwise set to -1.
  console.log(`${mid},${created},${numOperations},${readonlyLink}`); // Log the extracted information as a CSV row to the console.
});

stream.pipe(parser); // Pipe the JSON file stream into the parser to process the data.
```

## Example Output

```csv
mid,created,numOperations,readonlyLink
12345,2023-07-20T10:00:00.000Z,10,https://www.mipui.net/app/index.html?mid=12345
67890,2023-07-21T15:30:00.000Z,-1,https://www.mipui.net/app/index.html?mid=67890
```

## Dependencies

* **fs:** The Node.js file system module for reading files.
* **JSONStream:** A library for parsing JSON data streams. 
