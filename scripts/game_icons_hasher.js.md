##  Game Icon Hashing 

### Table of Contents 
* [Overview](#overview) 
* [Code Walkthrough](#code-walkthrough)
* [File Structure](#file-structure)

### Overview 
This script generates a unique hash for each icon in the `game_icons.json` file and updates the file with the new hash values. The updated file is saved as `game-icons.js` in the `public/app/assets` directory. 

### Code Walkthrough 

```javascript
// Add a hash to all entries in game_icons.json.

const fs = require('fs');
const path = require('path');
const input = require('../public/app/assets/game_icons.json'); 

// Function to generate a hash from a string.
function hashString(s) {
  // http://stackoverflow.com/a/15710692
  return s.split('').reduce((a, b) => {
    a = ((a << 5) - a) + b.charCodeAt(0);
    return a & a;
  }, 0);
}

// Iterate through each icon in the input array.
input.forEach(icon => {
  // Calculate the hash for the icon's path.
  icon.hash = hashString(icon.path); 
});

// Write the updated icon data to the game-icons.js file.
fs.writeFile('public/app/assets/game-icons.js',
    'const gameIcons = ' + JSON.stringify(input, null, 2) + '\n;', 'utf8');
```

1. **Import necessary modules:**
    * `fs` for file system operations 
    * `path` for working with file paths
    * `game_icons.json`: The input file containing the icon data. 

2. **Hashing function:**
    * The `hashString` function takes a string as input and uses a simple algorithm to generate a unique hash value. This algorithm is based on the reference:  http://stackoverflow.com/a/15710692.

3. **Hashing icon paths:** 
    * The script iterates through each `icon` in the `input` array. 
    * For each icon, the `hashString` function is called with the icon's `path` to generate a hash value.
    * The hash value is then stored in the `hash` property of the icon object. 

4. **Writing to file:**
    * The updated icon data is written to a new file called `game-icons.js` in the `public/app/assets` directory.  The data is formatted with indentation for readability.  


### File Structure 

``` 
public/
├── app/
│   └── assets/
│       └── game-icons.js 
```

**Explanation:**

* The updated icon data with hash values is written to `game-icons.js` in the `public/app/assets` directory.
* The original `game_icons.json` file is not modified.