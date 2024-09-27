# Code Documentation: Jupyter Notebook Modification Script

## Table of Contents

1. **Introduction**
2. **Code Implementation**
    * **Dependencies**
    * **Script Logic**
        * Input File Handling
        * Cell Content Modification
        * Output File Handling
3. **Usage**
4. **Example**

## Introduction

This script modifies a Jupyter Notebook file by setting the `k` property of a specific cell type to `0`. The script targets Jupyter Notebook files stored as JSON files, specifically targeting the `content` object within the JSON data.

## Code Implementation

### Dependencies

The script relies on the following Node.js modules:

| Module | Description |
|---|---|
| `fs` | File system module for reading and writing files |
| `process` | Process module for accessing command line arguments |

### Script Logic

#### Input File Handling

1. The script first retrieves the filename from the second command-line argument (`process.argv[2]`).
2. It then uses the `fs.readFileSync` function to read the contents of the specified file into a variable named `raw`.

#### Cell Content Modification

1. The `raw` data is parsed as JSON using `JSON.parse` and stored in the `pstate` variable.
2. The script iterates through the keys of the `pstate.content` object using `Object.keys`.
3. For each cell key, it retrieves the corresponding `cellContent` from the `pstate.content` object.
4. The script checks if the `cellContent` object has a property with index `4` using `hasOwnProperty`. If the property doesn't exist, the loop continues to the next cell.
5. If the property exists, the script sets the `k` property of the `cellContent[4]` object to `0`.

#### Output File Handling

1. After processing all cells, the script uses `JSON.stringify` to convert the modified `pstate` object back into a JSON string.
2. Finally, the `fs.writeFileSync` function is used to write the modified JSON string back to the original filename.

## Usage

To use the script, run the following command in your terminal:

```bash
node script.js <filename>
```

Replace `<filename>` with the actual filename of the Jupyter Notebook file you want to modify.

## Example

**Input File (notebook.json):**

```json
{
  "content": {
    "cell1": {
      "4": {
        "k": 1
      }
    },
    "cell2": {
      "4": {
        "k": 2
      }
    }
  }
}
```

**Output File (notebook.json) after running the script:**

```json
{
  "content": {
    "cell1": {
      "4": {
        "k": 0
      }
    },
    "cell2": {
      "4": {
        "k": 0
      }
    }
  }
}
```
