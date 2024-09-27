## Game Icon Metadata Generator Documentation

**Table of Contents:**

* [Introduction](#introduction)
* [Code Structure](#code-structure)
    * [Helper Functions](#helper-functions)
        * [hashString(s)](#hashstring-s)
        * [flatten(arr)](#flatten-arr)
        * [getPaths(dir)](#getpaths-dir)
        * [getName(path)](#getname-path)
    * [Scraping & Metadata Generation](#scraping-metadata-generation)
        * [scrapeTags(path, callback)](#scrapetags-path-callback)
        * [getIconsFromPathsStaggered(paths, icons, index, callback)](#geticonsfrompathsstaggered-paths-icons-index-callback)
        * [getIconsFromPaths(paths, callback)](#geticonsfrompaths-paths-callback)
    * [Main Execution](#main-execution)
* [Usage](#usage)

### Introduction

This script generates a JSON file containing metadata for all SVG icons present in the `public/app/assets/icons` directory. The metadata includes the icon's path, name, tags, and a hash for efficient lookup. This information is scraped from the corresponding HTML pages on [game-icons.net](https://game-icons.net).

### Code Structure

This script is organized into several functions, each responsible for a specific task. These functions work together to collect icon metadata and generate the JSON output file.

#### Helper Functions

##### hashString(s)

This function takes a string `s` and returns its hash value. It implements a simple hash function that uses bitwise operations for efficiency.

```javascript
function hashString(s) {
  // http://stackoverflow.com/a/15710692
  return s.split('').reduce((a, b) => {
    a = ((a << 5) - a) + b.charCodeAt(0);
    return a & a;
  }, 0);
}
```

##### flatten(arr)

This function flattens a nested array `arr` by recursively iterating through its elements and combining them into a single-level array.

```javascript
function flatten(arr) {
  return arr.reduce(
      (acc, val) => acc.concat(Array.isArray(val) ? flatten(val) : val), []);
}
```

##### getPaths(dir)

This function takes a directory path `dir` and returns an array of all file paths within that directory and its subdirectories, filtering out directories and converting all path separators to `/`.

```javascript
function getPaths(dir) {
  return flatten(fs.readdirSync(dir)
      .map(file => fs.statSync(path.join(dir, file)).isDirectory() ?
        getPaths(path.join(dir, file)) :
        path.join(dir, file).replace(/\\\\/g, '/')));
}
```

##### getName(path)

This function takes a file path `path` and extracts the file name without the `.svg` extension.

```javascript
function getName(path) {
  const parts = path.split('/');
  return parts[parts.length - 1].replace('.svg', '');
}
```

#### Scraping & Metadata Generation

##### scrapeTags(path, callback)

This function takes a file path `path` and a callback function `callback` as arguments. It scrapes the corresponding HTML page from [game-icons.net](https://game-icons.net) using the `scrape-it` library and extracts all tags associated with the icon. The scraped data is then passed to the callback function.

```javascript
function scrapeTags(path, callback) {
  const sitePath = 'https://game-icons.net' +
      path
          .replace('/svg/000000/transparent', '')
          .replace('public/app/assets/icons', '')
          .replace('.svg', '.html');
  console.log('Scraping ' + sitePath);
  scrapeIt(sitePath, {
    tags: {
      listItem: 'a[rel = "tag"]',
    },
  }).then(page => {
    console.log(`Scraped from ${sitePath}: ${JSON.stringify(page)}`);
    callback(page.tags);
  });
}
```

##### getIconsFromPathsStaggered(paths, icons, index, callback)

This function takes an array of file paths `paths`, an array to store icon metadata `icons`, an index `index` to track progress, and a callback function `callback` as arguments. It iterates through the file paths, scraping tags for each icon and adding the metadata to the `icons` array. The function introduces a delay between each scrape to avoid overloading the server. 

```javascript
function getIconsFromPathsStaggered(paths, icons, index, callback) {
  if (paths.length == icons.length) {
    callback(icons);
    return;
  }
  const path = paths[index];
  console.log(path);
  scrapeTags(path, tags => {
    const icon = {
      path,
      name: getName(path),
      tags,
      hash: hashString(path),
    };
    icons.push(icon);
    setTimeout(() => {
      getIconsFromPathsStaggered(paths, icons, index + 1, callback);
    }, 300);
  });
}
```

##### getIconsFromPaths(paths, callback)

This function takes an array of file paths `paths` and a callback function `callback` as arguments. It calls the `getIconsFromPathsStaggered` function with an empty `icons` array and an initial index of 0 to start the scraping and metadata generation process.

```javascript
function getIconsFromPaths(paths, callback) {
  const icons = [];
  getIconsFromPathsStaggered(paths, icons, 0, callback);
}
```

#### Main Execution

The main execution section of the script retrieves all SVG file paths from the `public/app/assets/icons` directory, filters out non-SVG files, and passes the resulting paths array to the `getIconsFromPaths` function. The `getIconsFromPaths` function then calls the callback function after generating the `icons` array. Finally, the `icons` array is written to the `public/app/assets/game_icons.json` file.

```javascript
const paths =
    getPaths('public/app/assets/icons/').filter(path => path.endsWith('.svg'));
getIconsFromPaths(paths, icons => {
  fs.writeFile('public/app/assets/game_icons.json',
      JSON.stringify(icons, null, 2), 'utf8');
});
```

### Usage

1. **Install Dependencies:** Ensure that the necessary packages are installed:
    * `fs` (for file system operations)
    * `path` (for path manipulation)
    * `scrape-it` (for web scraping)
    * `process` (for environment variables)

2. **Run the Script:** Execute the script to generate the `game_icons.json` file:
    ```bash
    node your_script_name.js
    ```
    
3. **Check the Output:** Verify that the `public/app/assets/game_icons.json` file has been created and contains the icon metadata. 

This script provides a convenient way to manage icon metadata and can be easily adapted for other icon sets or directories.  
