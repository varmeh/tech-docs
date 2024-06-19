# Understanding Geohashing: A Comprehensive Guide

- [Understanding Geohashing: A Comprehensive Guide](#understanding-geohashing-a-comprehensive-guide)
  - [What is Geohashing?](#what-is-geohashing)
  - [Why is Geohashing Required?](#why-is-geohashing-required)
    - [Advantages of Geohashing](#advantages-of-geohashing)
  - [Understanding Geohash Precision](#understanding-geohash-precision)
    - [Geohash Precision Table](#geohash-precision-table)
  - [How is Geohash Calculated?](#how-is-geohash-calculated)
    - [Step-by-Step Calculation Using a 4-Character Geohash](#step-by-step-calculation-using-a-4-character-geohash)
    - [1. Convert Latitude and Longitude to Binary](#1-convert-latitude-and-longitude-to-binary)
      - [Latitude Conversion](#latitude-conversion)
      - [Longitude Conversion (-122.084)](#longitude-conversion--122084)
    - [2. Interleave the Bits](#2-interleave-the-bits)
    - [3. Convert Interleaved Binary to Base32](#3-convert-interleaved-binary-to-base32)
    - [Resulting Geohash](#resulting-geohash)
  - [Benefits of Interleaving](#benefits-of-interleaving)
  - [References](#references)

## What is Geohashing?

Geohashing is a method for encoding geographic coordinates (latitude and longitude) into a short string of letters and digits. This encoded string, called a geohash, offers a compact, human-readable way to represent geographic locations. Developed by Gustavo Niemeyer in 2008, geohashing is widely used in geographic information systems (GIS), spatial databases, and location-based services.

## Why is Geohashing Required?

### Advantages of Geohashing

1. `Compact Representation`:
   - Geohashes condense geographic coordinates into a short string
   - This makes them easier to store and communicate
  
2. `Spatial Locality`:
   - Geohashes maintain spatial locality, meaning that points close to each other geographically will have similar geohashes
   - This is crucial for efficient spatial querying and indexing
  
3. `Hierarchical Structure`:
   - Geohashes provide a hierarchical structure where each additional character increases the precision of the location
   - This allows for variable levels of granularity in spatial data representation

4. `Efficient Querying`:
   - Geohashes enable efficient spatial queries, such as finding all points within a certain area or bounding box
   - This is especially useful for applications like geocoding, mapping, and location-based services

## Understanding Geohash Precision

Precision in geohashing refers to the accuracy of the geographic location represented by the geohash.

The length of the geohash string determines its precision: `longer geohashes correspond to smaller areas`, thus representing locations more accurately.

Here is a table showing the precision of geohashes based on their length:

### Geohash Precision Table

| Geohash Length | Latitude Bits | Longitude Bits | Cell Width  | Cell Height |
|----------------|---------------|----------------|-------------|-------------|
| 1              | 2             | 3              | ≤ 5,000 km  | ≤ 5,000 km  |
| 2              | 5             | 5              | 1,250 km    | 625 km      |
| 3              | 7             | 8              | 156 km      | 156 km      |
| 4              | 10            | 10             | 39.1 km     | 19.5 km     |
| 5              | 12            | 13             | 4.89 km     | 4.89 km     |
| 6              | 15            | 15             | 1.22 km     | 0.61 km     |
| 7              | 17            | 18             | 153 m       | 153 m       |
| 8              | 20            | 20             | 38.2 m      | 19.1 m      |
| 9              | 22            | 23             | 4.77 m      | 4.77 m      |
| 10             | 25            | 25             | 1.19 m      | 0.596 m     |
| 11             | 27            | 28             | 149 mm      | 149 mm      |
| 12             | 30            | 30             | 37.2 mm     | 18.6 mm     |

- [Experience effect of precision on geohash](https://www.movable-type.co.uk/scripts/geohash.html)

## How is Geohash Calculated?

### Step-by-Step Calculation Using a 4-Character Geohash

Let's derive a 4-character (*precision*) geohash for Google's headquarters located at approximately latitude `37.422` and longitude `-122.084`

We'll break down the process into detailed steps.

### 1. Convert Latitude and Longitude to Binary

#### Latitude Conversion

Latitude ranges from -90 to +90.

| Step | Range              | Midpoint        | 37.422 Comparison  | Binary | New Range              |
|------|--------------------|-----------------|--------------------|--------|------------------------|
| 1    | [-90, 90]          | 0               | 37.422 > 0         | 1      | [0, 90]                |
| 2    | [0, 90]            | 45              | 37.422 < 45        | 10     | [0, 45]                |
| 3    | [0, 45]            | 22.5            | 37.422 > 22.5      | 101    | [22.5, 45]             |
| 4    | [22.5, 45]         | 33.75           | 37.422 > 33.75     | 1011   | [33.75, 45]            |
| 5    | [33.75, 45]        | 39.375          | 37.422 < 39.375    | 10110  | [33.75, 39.375]        |
| 6    | [33.75, 39.375]    | 36.5625         | 37.422 > 36.5625   | 101101 | [36.5625, 39.375]      |
| 7    | [36.5625, 39.375]  | 37.96875        | 37.422 < 37.96875  | 1011010| [36.5625, 37.96875]    |
| 8    | [36.5625, 37.96875]| 37.265625       | 37.422 > 37.265625 | 10110101| [37.265625, 37.96875] |
| 9    | [37.265625, 37.96875]| 37.6171875    | 37.422 < 37.6171875| 101101010| [37.265625, 37.6171875]|
| 10   | [37.265625, 37.6171875]| 37.44140625 | 37.422 < 37.44140625| 1011010100| [37.265625, 37.44140625]|

Latitude binary: `1011010100`

#### Longitude Conversion (-122.084)

Longitude ranges from -180 to +180.

| Step | Range              | Midpoint        | -122.084 Comparison | Binary | New Range              |
|------|--------------------|-----------------|---------------------|--------|------------------------|
| 1    | [-180, 180]        | 0               | -122.084 < 0        | 0      | [-180, 0]              |
| 2    | [-180, 0]          | -90             | -122.084 < -90      | 00     | [-180, -90]            |
| 3    | [-180, -90]        | -135            | -122.084 > -135     | 001    | [-135, -90]            |
| 4    | [-135, -90]        | -112.5          | -122.084 < -112.5   | 0010   | [-135, -112.5]         |
| 5    | [-135, -112.5]     | -123.75         | -122.084 > -123.75  | 00101  | [-123.75, -112.5]      |
| 6    | [-123.75, -112.5]  | -118.125        | -122.084 < -118.125 | 001010 | [-123.75, -118.125]    |
| 7    | [-123.75, -118.125]| -120.9375       | -122.084 < -120.9375| 0010100| [-123.75, -120.9375]   |
| 8    | [-123.75, -120.9375]| -122.34375     | -122.084 > -122.34375| 00101001| [-122.34375, -120.9375]|
| 9    | [-122.34375, -120.9375]| -121.640625 | -122.084 < -121.640625| 001010010| [-122.34375, -121.640625]|
| 10   | [-122.34375, -121.640625]| -121.9921875 | -122.084 < -121.9921875| 0010100100| [-122.34375, -121.9921875]|

Longitude binary: `0010100100`

### 2. Interleave the Bits

Interleave the bits of longitude and latitude to create a single binary string.

`(Longitude, Latitude) -> (01)`

| Step | Latitude | Longitude | Interleaved    |
|------|----------|-----------|----------------|
| 1    | 1        | 0         | 01             |
| 2    | 0        | 0         | 0100           |
| 3    | 1        | 1         | 010011         |
| 4    | 1        | 0         | 01001101       |
| 5    | 0        | 1         | 0100110110     |
| 6    | 1        | 0         | 010011011001   |
| 7    | 0        | 0         | 01001101100100 |
| 8    | 1        | 1         | 0100110110010011|
| 9    | 0        | 0         | 010011011001001100|
| 10   | 0        | 0         | 01001101100100110000|

Interleaved binary: `01001101100100110000`

### 3. Convert Interleaved Binary to Base32

The binary string `01001101100100110000` needs to be converted to base32.

1. **Pad the binary string to make its length a multiple of 5**: The length of `01000110100100110000` is 20, which is already a multiple of 5, so no padding is needed.

2. **Divide the binary string into 5-bit groups**:

   ```
   01001 10110 01001 10000
   ```

3. **Convert each 5-bit group to its decimal equivalent**:
   - `01001` = 9
   - `10110` = 28
   - `01001` = 9
   - `10000` = 16

4. **Map each decimal value to the corresponding base32 character**:
   Base32 alphabet: `0123456789bcdefghjkmnpqrstuvwxyz`
   - 9  -> 9
   - 28 -> u
   - 9  -> 9
   - 16 -> h

Therefore, the binary string `01000110100100110000` converts to the base32 string `9u9h`.

### Resulting Geohash

The 4-character geohash for Google's headquarters (latitude 37.422, longitude -122.084) is `9u9h`.

## Benefits of Interleaving

1. `Spatial Locality`:
   - Interleaving ensures that geohashes for nearby locations are similar, preserving spatial locality. This is critical for spatial queries and indexing.

2. `Efficient Range Queries`:
   - Interleaved geohashes allow for efficient bounding box queries, which are common in geographic searches. This makes it easier to query all points within a certain area.

3. `Balanced Precision`:
   - By interleaving, both latitude and longitude contribute equally to the precision of the geohash. This avoids skewed precision that could occur if one coordinate is given more bits than the other.

4. `Hierarchical Subdivision`:
   - Interleaving provides a hierarchical structure where each additional character refines the location, allowing for varying levels of granularity.

## References

- [What is geohashing](https://medium.com/@bkawk/geohashing-20b282fc9655)
- [Geohash Implementation Explained](https://yatmanwong.medium.com/geohash-implementation-explained-2ed9627a61ff)
- [Geohash Calculator](https://www.movable-type.co.uk/scripts/geohash.html)
