---
title: Export Dialog Doc
---
# Introduction

This document will walk you through the implementation of the export dialog feature.

The feature allows users to export the current map view in various formats and resolutions.

We will cover:

1. Initialization of the export dialog.
2. Displaying the export options.
3. Handling the export action.
4. Processing the export output.

# Initialization of the export dialog

<SwmSnippet path="/public/app/export_dialog.js" line="1">

---

The <SwmToken path="/public/app/export_dialog.js" pos="1:2:2" line-data="class ExportDialog extends Dialog {">`ExportDialog`</SwmToken> class extends the <SwmToken path="/public/app/export_dialog.js" pos="1:6:6" line-data="class ExportDialog extends Dialog {">`Dialog`</SwmToken> class and initializes with an empty array of export buttons. This sets up the basic structure needed for the export dialog.

```
class ExportDialog extends Dialog {
  constructor() {
    super();
    this.exportButtons_ = [];
  }

  getAcceptButtonText_() { return 'Export'; }
  getActivatedAcceptButtonText_() { return 'Exporting...'; }
```

---

</SwmSnippet>

# Displaying the export options

<SwmSnippet path="/public/app/export_dialog.js" line="9">

---

The <SwmToken path="/public/app/export_dialog.js" pos="10:1:1" line-data="  showDialogContent_() {">`showDialogContent_`</SwmToken> method is responsible for displaying the export options. It calculates the current width and height of the map and dynamically creates radio buttons for each export option. Each button is associated with a specific export size and description.

```

  showDialogContent_() {
    const currentWidth =
        state.getProperty(pk.lastColumn) - state.getProperty(pk.firstColumn);
    const currentHeight =
        state.getProperty(pk.lastRow) - state.getProperty(pk.firstRow);
    const addRadioButton = (name, size, extraBoundary, ...descriptions) => {
      const container =
          createAndAppendDivWithClass(this.dialogElement_, 'menu-radio-group');
      const button = document.createElement('input');
      button.type = 'radio';
      button.name = 'export';
      button.value = name;
      const id = 'exportButton' + this.exportButtons_.length;
      button.id = id;
      container.appendChild(button);
      this.exportButtons_.push(button);
      if (this.exportButtons_.length == 1) button.checked = true;
      const label = document.createElement('label');
      const x = size * currentWidth + (extraBoundary ? size / 4 : 0);
      const y = size * currentHeight + (extraBoundary ? size / 4 : 0);
      const lines = [`${size} pixels per cell (final size ${x}x${y}).`]
          .concat(descriptions);
      if (x > 14000 || y > 14000) {
        lines.push('<span style="color: yellow">Warning: Depending on the ' +
            'browser, images with a dimension over 14,000 might not be ' +
            'properly generated.</span>');
      }
      label.innerHTML = `<b>${name}</b><br />` +
          `<div class="menu-radio-details">${lines.join('<br />')}</div>`;
      label.setAttribute('for', id);
      container.appendChild(label);
    };
```

---

</SwmSnippet>

<SwmSnippet path="/public/app/export_dialog.js" line="42">

---

The <SwmToken path="/public/app/export_dialog.js" pos="43:1:1" line-data="    addRadioButton(&#39;1:1&#39;, 32, true,">`addRadioButton`</SwmToken> function is used to add different export options. It creates radio buttons with labels that describe the export size and any additional details. This function is called multiple times to add various export options like <SwmToken path="/public/app/export_dialog.js" pos="43:4:6" line-data="    addRadioButton(&#39;1:1&#39;, 32, true,">`1:1`</SwmToken>, <SwmToken path="/public/app/export_dialog.js" pos="45:4:6" line-data="    addRadioButton(&#39;2:1&#39;, 64, true,">`2:1`</SwmToken>, 'Quick', 'Battlemap', and 'Cropped'.

```

    addRadioButton('1:1', 32, true,
        'This looks like the app looks at default zoom level.');
    addRadioButton('2:1', 64, true,
        'This looks like the app looks at default zoom level ' +
        'on high-DPI displays.');
    if (state.tilingCachingEnabled) {
      addRadioButton('Quick', 192, true,
          'This is generated faster than the other options.');
    }
    addRadioButton('Battlemap', 300, true,
        'When printing in 300 DPI, this will result in 1 inch per cell.');
    addRadioButton('Cropped', 70, false,
        'Cropped to align with grid.',
        'This is the most convenient option when importing the image in ' +
        'other apps, such as virtual tabletops.');
  }
```

---

</SwmSnippet>

# Handling the export action

<SwmSnippet path="/public/app/export_dialog.js" line="59">

---

The <SwmToken path="/public/app/export_dialog.js" pos="60:3:3" line-data="  async act_() {">`act_`</SwmToken> method is triggered when the user confirms the export action. It locks the map tiles, determines which export option was selected, and calls the <SwmToken path="/public/app/export_dialog.js" pos="65:3:3" line-data="        await downloadPng(1, 0, 0);">`downloadPng`</SwmToken> function with the appropriate parameters based on the selected option. After the export is processed, it unlocks the map tiles.

```

  async act_() {
    state.theMap.lockTiles();
    const selectedButton = this.exportButtons_.find(button => button.checked);
    switch (selectedButton.value) {
      case '1:1':
        await downloadPng(1, 0, 0);
        break;
      case '2:1':
        await downloadPng(2, 0, 0);
        break;
      case 'Quick':
        await downloadPng(192 / 32, 0, 0, true);
        break;
      case 'Battlemap':
        await downloadPng(300 / 32, 0, 0);
        break;
      case 'Cropped':
        await downloadPng(70 / 32, 4, 4);
        break;
    }
    state.theMap.unlockTiles();
  }
}
```

---

</SwmSnippet>

# Processing the export output

<SwmSnippet path="/public/app/export_dialog.js" line="83">

---

The <SwmToken path="/public/app/export_dialog.js" pos="84:4:4" line-data="async function downloadPng(">`downloadPng`</SwmToken> function handles the actual export process. It clones the tile grid imager with the specified parameters, invalidates tiles if necessary, and converts the map element to a blob. Finally, it saves the blob as a PNG file.

```

async function downloadPng(
    scale, startOffset, endOffset, useCachedTiles, name) {
  const gridImager = state.tileGridImager.clone({
    scale,
    cropLeft: startOffset,
    cropTop: startOffset,
    cropRight: endOffset,
    cropBottom: endOffset,
    margins: 0,
    disableSmoothing: true,
  });
  if (!useCachedTiles) {
    state.theMap.invalidateTiles();
  }
  const theMapElement = document.getElementById('theMap');
  const blob = await gridImager.node2blob(theMapElement,
      theMapElement.clientWidth, theMapElement.clientHeight);
  saveAs(blob, name || state.getTitle() || 'mipui.png');
}
```

---

</SwmSnippet>

This concludes the walkthrough of the export dialog feature. The implementation ensures that users can export the map in various formats and resolutions, providing flexibility and convenience.

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBbWlwdWklM0ElM0FUaGVBSVByb2R1Y3RSZXBvcnQ=" repo-name="mipui"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
