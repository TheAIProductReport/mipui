---
title: Index.html master page
---
# Introduction

This document will walk you through the implementation of the <SwmPath>[public/index.html](/public/index.html)</SwmPath> master page for the Mipui application.

The feature introduces a structured and informative landing page for Mipui, a collaborative web application for creating <SwmToken path="/public/index.html" pos="24:38:40" line-data="      &lt;em&gt;Mipui&lt;/em&gt; is a free and open-source collaborative web application for creating, editing and viewing grid-based maps for tabletop or role-playing games.">`grid-based`</SwmToken> maps.

We will cover:

1. The overall structure of the <SwmPath>[public/index.html](/public/index.html)</SwmPath> file.
2. The inclusion and role of Google Analytics.
3. The main content and sections of the page.
4. Additional resources and links provided at the end of the page.

# Overall structure of the <SwmPath>[public/index.html](/public/index.html)</SwmPath> file

<SwmSnippet path="/public/index.html" line="2">

---

The <SwmPath>[public/index.html](/public/index.html)</SwmPath> file sets up the basic structure of the Mipui landing page. It includes essential metadata, stylesheets, and scripts.

```
<html>
<head>
  <title>Mipui</title>
  <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
  <link rel="icon" href="favicon.ico" type="image/x-icon">
  <link rel="stylesheet" href="css/style.css" type="text/css">

  <script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
    (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
    m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
```

---

</SwmSnippet>

# Inclusion and role of Google Analytics

<SwmSnippet path="/public/index.html" line="14">

---

Google Analytics is included to track user interactions and gather usage data. This helps in understanding user behavior and improving the application.

```

    ga('create', 'UA-96544349-1', 'auto');
    ga('send', 'pageview');

  </script>
</head>
<body>
  <article>
    <h1>Mipui</h1>
    <p>
      <em>Mipui</em> is a free and open-source collaborative web application for creating, editing and viewing grid-based maps for tabletop or role-playing games.
    </p>
    <a href="app/" class="start">Start creating a map now!</a>
    <p>
      No registration is neccessary, and there's nothing to download or install.
    </p>
    <h2>Features</h2>
    <h3>Simple</h3>
    <p>
      <em>Mipui</em> employs a limited but expressive set of tools, avoiding the bloat that exists in some other map editors while still remaining rich enough for most everyday gaming needs. In particular, no textures are used, instead opting for a symbolic approach.
    </p>
    <h3>Real-time collaboration</h3>
    <p>
      <em>Mipui</em> has real-time collaboration between multiple authors.
      Just share the link with someone else and watch their edits live.
      No registration necessary.
    </p>
    <p>
      It's also possible to share a read-only view of the map, so that maps you create can easily be shared with any number of total strangers - aka the Internet. It's easy to remix existing maps with the "fork" option.
    </p>
    <h3>Friendly</h3>
    <p>
      <em>Mipui</em> contains a lot of small features that help with worry-free editing, including undo/redo support, the ability to resize the grid without deleting existing content, zoom and pan tools, copy and paste, etc.
    </p>
    <h3>Cloud storage</h3>
    <p>
      Every <em>Mipui</em> map is automatically saved to the cloud every few seconds, and never expires.
    </p>
    <h2>Licensing and Commercial Use</h2>
    You may use maps created using <em>Mipui</em> for any purpose, commercial or not, and you do not have to credit the <em>Mipui</em> tool or its author in your publication (though that would be nice of you). However, the following caveats apply:
    <ol>
      <li>The token images are not made by me and are licensed CC-BY. It means that if you publish a map that contains those you need to credit their authors, for example by linking to the <a href="http://game-icons.net/about.html#authors">detailed artist breakdown page</a>.</li>
      <li>If you fork another map and publish something built on that, it probably counts as derivative work and so is subject to the laws around that.</li>
    </ol>
    <h2>Developer Information</h2>
    <a href="docs/developer_guide.html">Visit the developer's guide</a>.
    <h2>Supported Browsers</h2>
    <em>Mipui</em> supports Chrome, Firefox and Safari. Visit the <a href="docs/supported_browsers.html">Browser Support Page</a> for more information.
    <h2>Credits</h2>
    <p>
      <em>Mipui</em> is developed and maintained by Alon Mishne.
      Contact me at <a href="mailto:contact@mipui.net">contact@mipui.net</a>.
    </p>
    <p>
      <em>Mipui</em>'s implementation uses the following resources:
    </p>
    <ul>
      <li><a href="http://firebase.google.com">Firebase by Google</a></li>
      <li><a href="https://github.com/eligrey/FileSaver.js">FireSaver.js by Eli Grey</a></li>
      <li><a href="http://game-icons.net">Game icons by various artists, available on game-icons.net</a>. Also see a <a href="http://game-icons.net/about.html#authors">detailed artist breakdown</a>.</li>
      <li><a href="https://material.io/icons/">Material icons by Google</a></li>
      <li><a href="https://blogs.msdn.microsoft.com/ericlippert/tag/shadowcasting/">Shadowcasting Tutorial by Eric Lippert</a></li>
      <li><a href="https://www.patreon.com/posts/15297542">Cross-hatching texture by Syrkres</a></li>
      <li><a href="https://github.com/parshap/node-sanitize-filename">node-sanitize-filename Node module by Sam Hocevar</a></li>
      <li><a href="https://github.com/satazor/js-spark-md5">SparkMD5 by Joseph Myers</a></li>
    </ul>
    <p>
      <em>Mipui</em> was inspired by many things, but first and foremost is <a href="http://deepnight.net/tools/tabletop-rpg-map-editor/">ANAmap by Sébastien Bénard</a>.
      In fact, <em>Mipui</em> started from me using his app and thinking "oh this is cool, but I wish it had just a few more features... I wonder if I can do that myself."
    </p>
    <p>Special thanks to my D&amp;D group, <em>Ornak's Freedom</em>, for the initial motivation!</p>
    <h2>Contact</h2>
    <p>
      Bug reports and feature requests can be posted to the <a href="https://feedback.userreport.com/7e918812-4e93-4a8f-9541-9af34d0f4231/">UserReport feedback forum</a> or as <a href="https://github.com/amishne/mipui/issues">issues on the Github project</a>. For anything else feel free to email me at <a href="mailto:contact@mipui.net">contact@mipui.net</a>.
    </p>
```

---

</SwmSnippet>

# Main content and sections of the page

<SwmSnippet path="/public/index.html" line="14">

---

The main content of the page introduces Mipui, its features, and provides links to start using the application. It also includes information on licensing, supported browsers, and credits.

```

    ga('create', 'UA-96544349-1', 'auto');
    ga('send', 'pageview');

  </script>
</head>
<body>
  <article>
    <h1>Mipui</h1>
    <p>
      <em>Mipui</em> is a free and open-source collaborative web application for creating, editing and viewing grid-based maps for tabletop or role-playing games.
    </p>
    <a href="app/" class="start">Start creating a map now!</a>
    <p>
      No registration is neccessary, and there's nothing to download or install.
    </p>
    <h2>Features</h2>
    <h3>Simple</h3>
    <p>
      <em>Mipui</em> employs a limited but expressive set of tools, avoiding the bloat that exists in some other map editors while still remaining rich enough for most everyday gaming needs. In particular, no textures are used, instead opting for a symbolic approach.
    </p>
    <h3>Real-time collaboration</h3>
    <p>
      <em>Mipui</em> has real-time collaboration between multiple authors.
      Just share the link with someone else and watch their edits live.
      No registration necessary.
    </p>
    <p>
      It's also possible to share a read-only view of the map, so that maps you create can easily be shared with any number of total strangers - aka the Internet. It's easy to remix existing maps with the "fork" option.
    </p>
    <h3>Friendly</h3>
    <p>
      <em>Mipui</em> contains a lot of small features that help with worry-free editing, including undo/redo support, the ability to resize the grid without deleting existing content, zoom and pan tools, copy and paste, etc.
    </p>
    <h3>Cloud storage</h3>
    <p>
      Every <em>Mipui</em> map is automatically saved to the cloud every few seconds, and never expires.
    </p>
    <h2>Licensing and Commercial Use</h2>
    You may use maps created using <em>Mipui</em> for any purpose, commercial or not, and you do not have to credit the <em>Mipui</em> tool or its author in your publication (though that would be nice of you). However, the following caveats apply:
    <ol>
      <li>The token images are not made by me and are licensed CC-BY. It means that if you publish a map that contains those you need to credit their authors, for example by linking to the <a href="http://game-icons.net/about.html#authors">detailed artist breakdown page</a>.</li>
      <li>If you fork another map and publish something built on that, it probably counts as derivative work and so is subject to the laws around that.</li>
    </ol>
    <h2>Developer Information</h2>
    <a href="docs/developer_guide.html">Visit the developer's guide</a>.
    <h2>Supported Browsers</h2>
    <em>Mipui</em> supports Chrome, Firefox and Safari. Visit the <a href="docs/supported_browsers.html">Browser Support Page</a> for more information.
    <h2>Credits</h2>
    <p>
      <em>Mipui</em> is developed and maintained by Alon Mishne.
      Contact me at <a href="mailto:contact@mipui.net">contact@mipui.net</a>.
    </p>
    <p>
      <em>Mipui</em>'s implementation uses the following resources:
    </p>
    <ul>
      <li><a href="http://firebase.google.com">Firebase by Google</a></li>
      <li><a href="https://github.com/eligrey/FileSaver.js">FireSaver.js by Eli Grey</a></li>
      <li><a href="http://game-icons.net">Game icons by various artists, available on game-icons.net</a>. Also see a <a href="http://game-icons.net/about.html#authors">detailed artist breakdown</a>.</li>
      <li><a href="https://material.io/icons/">Material icons by Google</a></li>
      <li><a href="https://blogs.msdn.microsoft.com/ericlippert/tag/shadowcasting/">Shadowcasting Tutorial by Eric Lippert</a></li>
      <li><a href="https://www.patreon.com/posts/15297542">Cross-hatching texture by Syrkres</a></li>
      <li><a href="https://github.com/parshap/node-sanitize-filename">node-sanitize-filename Node module by Sam Hocevar</a></li>
      <li><a href="https://github.com/satazor/js-spark-md5">SparkMD5 by Joseph Myers</a></li>
    </ul>
    <p>
      <em>Mipui</em> was inspired by many things, but first and foremost is <a href="http://deepnight.net/tools/tabletop-rpg-map-editor/">ANAmap by Sébastien Bénard</a>.
      In fact, <em>Mipui</em> started from me using his app and thinking "oh this is cool, but I wish it had just a few more features... I wonder if I can do that myself."
    </p>
    <p>Special thanks to my D&amp;D group, <em>Ornak's Freedom</em>, for the initial motivation!</p>
    <h2>Contact</h2>
    <p>
      Bug reports and feature requests can be posted to the <a href="https://feedback.userreport.com/7e918812-4e93-4a8f-9541-9af34d0f4231/">UserReport feedback forum</a> or as <a href="https://github.com/amishne/mipui/issues">issues on the Github project</a>. For anything else feel free to email me at <a href="mailto:contact@mipui.net">contact@mipui.net</a>.
    </p>
```

---

</SwmSnippet>

# Additional resources and links

<SwmSnippet path="/public/index.html" line="89">

---

At the end of the page, additional resources such as the privacy policy, update history, and business plan are provided for users who want more information.

```

    <h2>Other resources</h2>

    <a href="docs/privacy_policy.html">Privacy policy</a><br />
    <a href="docs/updates.html">Update history</a><br />
    <a href="docs/business_plan.html">Business plan</a> (or: how is this free?)<br />
  </article>
</body>
</html>
```

---

</SwmSnippet>

This concludes the walkthrough of the <SwmPath>[public/index.html](/public/index.html)</SwmPath> master page implementation. The structure and content are designed to provide a comprehensive introduction to Mipui and its capabilities.

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBbWlwdWklM0ElM0FUaGVBSVByb2R1Y3RSZXBvcnQ=" repo-name="mipui"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
