# Angled Wall SVG Mask Generator Documentation

## Table of Contents

- [Overview](#overview)
- [Function Breakdown](#function-breakdown)
  - [createPolygon()](#createpolygon)
  - [sectionsFromDirs()](#sectionsfromdirs)
  - [createAngledWallSvgMask()](#createangledwallsvgmask)
  - [createPolygons()](#createpolygons)


## Overview

This code defines functions to generate SVG masks for angled walls. An angled wall is composed of 17 individual parts (A-Q), and 8 potential connections (t, r, b, l, tr, br, bl, tl) control which parts are walls and which are not. The `sectionsFromDirs()` function determines which parts should be included based on the connection configuration, while `createPolygon()` generates the SVG polygon definition for each part. 

The `createAngledWallSvgMask()` function takes a connection configuration as input and generates a complete SVG mask string representing the angled wall. The final mask is defined as a data URI, allowing it to be directly embedded into other HTML elements.

## Function Breakdown

### createPolygon()

This function creates the SVG polygon definition for a given wall part. It takes a single character representing the part (e.g., 'A', 'B', 'C', etc.) as input and returns an SVG `<polygon>` element with the appropriate coordinates.

| Part  | Coordinates |
|---|---|
| A | 7 0, 14 7, 7 7 |
| B | 31 0, 32 0, 32 8, 23 8 |
| C | 0 7, 7 7, 7 14 |
| D | 14 7, 24 7, 19 12 |
| E | 31 7, 38 7, 31 14 |
| F | 7 7, 14 7, 19 12, 12 19, 7 14 |
| G | 24 7, 32 7, 32 13, 26 19, 19 12 |
| H | 7 14, 12 19, 7 24 |
| I | 19 12, 26 19, 19 26, 12 19 |
| J | 32 13, 32 25, 26 19 |
| K | 12 19, 19 26, 13 32, 6 32, 6 25 |
| L | 26 19, 32 25, 32 32, 25 32, 19 26 |
| M | 7 24, 7 32, 0 32, 0 31 |
| N | 19 26, 25 32, 13 32 |
| O | 31 24, 39 32, 31 32 |
| P | 7 31, 14 31, 7 38 |
| Q | 24 31, 32 31, 32 39 |

### sectionsFromDirs()

This function determines which wall parts should be included based on the provided connection configuration. It takes a dictionary `d` containing boolean values for each potential connection (e.g., `d['t'] = true` if top connection exists) and returns an array of wall part names. 

The logic for determining which parts to include is detailed in the inline comments within the function.

### createAngledWallSvgMask()

This function generates the complete SVG mask string for an angled wall. It takes a single integer representing the connection configuration as input (where each connection is represented by a bit flag: `t=1`, `r=2`, `b=4`, `l=8`, `tr=16`, `br=32`, `bl=64`, `tl=128`) and returns a data URI string containing the SVG mask.

The function first converts the connection configuration integer into a dictionary of boolean values for each connection. It then calls `createPolygons()` to get an array of SVG polygons and constructs the complete SVG string, including the `<defs>` section with the `<mask>` element, and the `<rect>` element that uses the mask.

### createPolygons()

This function generates an array of SVG polygon definitions for all the wall parts that should be included based on the connection configuration. It takes a dictionary of connection values (same as `sectionsFromDirs()` input) and calls `sectionsFromDirs()` to determine the included parts. It then calls `createPolygon()` for each included part and returns the resulting array of polygon strings. 
