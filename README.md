# Heatmap-Visualization
This project demonstrates how to create a geographical heatmap using Leaflet.js, OpenStreetMap (OSM) tiles, and earthquake data from the USGS Earthquake GeoJSON API. The heatmap visualizes earthquake intensity across the world using real-time data fetched from the USGS.

# Project Overview
Features:
1- Map Visualization: Uses Leaflet.js to render an interactive map with OpenStreetMap as the tile source.
2- Heatmap: Adds a heatmap layer to the map using the Leaflet.heat plugin to represent earthquake intensity.
3- Dynamic Data: The earthquake data is fetched in real-time from the USGS Earthquake API, which updates every day to show recent seismic activity.
4- Customization: The heatmap can be customized by adjusting parameters like radius, blur, and maxZoom to control the appearance of the heat spots.

# Code Breakdown
HTML Structure:
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Heatmap with Leaflet</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
    <style>
        #map {
            height: 100vh;
            width: 100%;
        }
    </style>
</head>

<body>
    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
    <script src="https://unpkg.com/leaflet.heat/dist/leaflet-heat.js"></script>

    <script>
        const map = L.map('map').setView([20.0, 5.0], 2);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        const url = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson';

        fetch(url)
            .then(response => response.json())
            .then(data => {
                const earthquakes = data.features.map(feature => {
                    const coords = feature.geometry.coordinates;
                    const magnitude = feature.properties.mag;
                    return [coords[1], coords[0], magnitude / 10];
                });

                L.heatLayer(earthquakes, {
                    radius: 35,
                    blur: 20,
                    maxZoom: 15
                }).addTo(map);
            })
            .catch(error => console.error('Error fetching earthquake data:', error));
    </script>
</body>

</html>
  ```

# JavaScript Breakdown:
!- Map Initialization:

* The map is initialized using L.map('map'), with the center set to latitude 20.0 and longitude 5.0. The zoom level is set to 2, providing a global view.
* OpenStreetMap tiles are loaded using the URL pattern 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'.
  
2- Fetching USGS Earthquake Data:

*The earthquake data is fetched from the USGS Earthquake API using the fetch() method. The URL (https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson) provides a GeoJSON feed of all recent earthquakes from the past day.

3- Processing Earthquake Data:

* The data is processed by extracting the latitude, longitude, and magnitude of each earthquake from the GeoJSON features. The magnitude is normalized (divided by 10) to better represent heat intensity in the heatmap.

4- Creating the Heatmap Layer:

* The processed data is passed to L.heatLayer() to generate a heatmap.
* The radius controls the size of each heat point, blur controls the smoothing, and maxZoom defines the maximum zoom level where the heatmap will be displayed.

# Customization

- You can adjust the following properties of the heatmap:

> Radius: Controls the size of the heat points. A larger radius makes each point more prominent.
> Blur: Controls how much the heat points are blurred. A higher blur creates a smoother transition between points.
> MaxZoom: Defines the maximum zoom level at which the heatmap is displayed. When zooming in further, the heatmap will disappear.

# Data Source
The earthquake data comes from the United States Geological Survey (USGS). The GeoJSON feed used in this project is available at:

* All Earthquakes from the Past Day: https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson
Other data feeds from USGS can be used depending on the desired timeframe:

* Past Hour: https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_hour.geojson
* Past 7 Days: https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson
* Past 30 Days: https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson

# How to Run

<img width="939" alt="image" src="https://github.com/user-attachments/assets/f0cd80dc-ac04-4871-aeae-950cb6747782">

1- Clone the repository:

git clone https://github.com/waleedmustafa971/Heatmap-Visualization.git

2- Open the index.html file in your browser. You can use an HTTP server like the Live Server extension for VSCode to avoid cross-origin issues when fetching the data.

3- The map will load, and the heatmap will be displayed after the earthquake data is fetched.

# License
This project is licensed under the MIT License. Feel free to use and modify the code as needed.



