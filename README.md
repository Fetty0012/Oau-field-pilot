# OAU Field Atlas

A browser-based interactive map interface for Obafemi Awolowo University in Ile-Ife. It uses MapLibre GL JS and OpenStreetMap tiles, with no build step or package installation required.

## Run it

For the full map, use the [GitHub Pages preview](https://fetty0012.github.io/Oau-field-pilot/). Opening `index.html` directly still shows the interface and lets you inspect the code, but deliberately disables hosted map tiles and geocoding. The hosted map, routing, and weather need an internet connection; geolocation requires a secure browser context.

## Add campus GIS data

Use **Import GIS file** or drop a file onto the map. The app accepts GeoJSON (`.geojson` or `.json`) and KML (`.kml`). Shapefiles and GeoPackages should be exported to GeoJSON first, for example with QGIS.

GeoJSON coordinates must use WGS 84 longitude/latitude (EPSG:4326). Feature names are read from `name`, `Name`, `NAME`, `title`, `building`, or `ref`. Categories are inferred from `category`, `type`, `amenity`, or `building`. Add `description`, `opening_hours`, `accessibility`, `facilities`, `phone`, and `website` properties to enrich the location detail panel.

Optional `average_rating` (or `avg_rating`, `rating`, `stars`) and `review_count` properties show an imported rating. Visitors can also select a personal 1–5-star rating; it is saved in that browser only and is not sent to other visitors. Building polygons can provide `height` or `levels` properties for 3D extrusions; otherwise the 3D view uses a generic height. Imported data stays in the current browser session and is not uploaded or saved.

## Get an OSM starter export

The workspace currently contains no campus GIS files. To make a starter GeoJSON from volunteer-contributed OpenStreetMap data, open [Overpass Turbo](https://overpass-turbo.eu/), run the query below, then choose **Export → GeoJSON** and import the downloaded file into the map.

```overpassql
[out:json][timeout:25];
(
	nwr(around:1800,7.5189,4.5274)["building"];
	nwr(around:1800,7.5189,4.5274)["amenity"];
	nwr(around:1800,7.5189,4.5274)["leisure"];
);
out geom;
```

This is community-mapped data, not an official or complete OAU facilities dataset. Verify locations and descriptions with the university before relying on them. OSM data is licensed under ODbL and requires attribution/share-alike obligations; do not add copied copyrighted campus maps.

## Data and service boundaries

- Basemap vector tiles use [OpenFreeMap](https://openfreemap.org/) with data from OpenMapTiles and OpenStreetMap. The public instance needs no API key or request/view limit, but has no SLA.
- The app no longer requests tiles from `tile.openstreetmap.org`. Do not add standard OSM tile prefetching, offline downloads, or bulk scraping.
- Nominatim address search is only triggered by explicit form submission, is limited to one request per 1.1 seconds, and is unavailable from `file://` pages. Never use it for autocomplete or bulk GIS extraction. See the [Nominatim usage policy](https://operations.osmfoundation.org/policies/nominatim/).
- Walking directions use the public OpenStreetMap foot-routing demo service; it is not intended as a production availability guarantee.
- Weather is fetched from Open-Meteo for the approximate campus center.
- Official campus alerts, service status, indoor maps, and verified building details are not connected yet. Add authoritative campus GIS and feeds before presenting these as live or verified information.

## Publish with GitHub Pages

The workflow in `.github/workflows/deploy-pages.yml` deploys the static site whenever changes are pushed to the `main` branch.

1. Create a public GitHub repository and upload the project files, keeping `index.html` at the repository root and preserving the `.github` folder.
2. In the repository, open **Settings → Pages** and set the build source to **GitHub Actions**.
3. Wait for the **Deploy static site to GitHub Pages** workflow to finish. GitHub displays the public URL in the Pages settings and the workflow deployment summary.
4. Edit `index.html` or the GIS data later and push changes to `main`; Pages will redeploy automatically.

## Next integration step

Export the university GIS layers to GeoJSON and load them in the map. For durable campus routing and indoor navigation, add a verified pedestrian network and floor-level data, then connect an institution-approved routing/data service.
# OAU Field Atlas

A browser-based interactive map interface for Obafemi Awolowo University in Ile-Ife. It uses MapLibre GL JS and OpenStreetMap tiles, with no build step or package installation required.

## Run it

For the full map, use the [GitHub Pages preview](https://fetty0012.github.io/Oau-field-pilot/). Opening `index.html` directly still shows the interface and lets you inspect the code, but deliberately disables OSM tiles and geocoding because `file://` pages cannot send the valid web referrer those volunteer services require. The hosted map, routing, and weather need an internet connection; geolocation requires a secure browser context.

## Add campus GIS data

Use **Import GIS file** or drop a file onto the map. The app accepts GeoJSON (`.geojson` or `.json`) and KML (`.kml`). Shapefiles and GeoPackages should be exported to GeoJSON first, for example with QGIS.

GeoJSON coordinates must use WGS 84 longitude/latitude (EPSG:4326). Feature names are read from `name`, `Name`, `NAME`, `title`, `building`, or `ref`. Categories are inferred from `category`, `type`, `amenity`, or `building`. Add `description`, `opening_hours`, `accessibility`, `facilities`, `phone`, and `website` properties to enrich the location detail panel.

Optional `average_rating` (or `avg_rating`, `rating`, `stars`) and `review_count` properties show an imported rating. Visitors can also select a personal 1–5-star rating; it is saved in that browser only and is not sent to other visitors. Building polygons can provide `height` or `levels` properties for 3D extrusions; otherwise the 3D view uses a generic height. Imported data stays in the current browser session and is not uploaded or saved.

## Get an OSM starter export

The workspace currently contains no campus GIS files. To make a starter GeoJSON from volunteer-contributed OpenStreetMap data, open [Overpass Turbo](https://overpass-turbo.eu/), run the query below, then choose **Export → GeoJSON** and import the downloaded file into the map.

```overpassql
[out:json][timeout:25];
(
	nwr(around:1800,7.5189,4.5274)["building"];
	nwr(around:1800,7.5189,4.5274)["amenity"];
	nwr(around:1800,7.5189,4.5274)["leisure"];
);
out geom;
```

This is community-mapped data, not an official or complete OAU facilities dataset. Verify locations and descriptions with the university before relying on them. OSM data is licensed under ODbL and requires attribution/share-alike obligations; do not add copied copyrighted campus maps.

## Data and service boundaries

- Map tiles and place search use OpenStreetMap services.
- Map tiles are requested only for the area a visitor is viewing; do not add offline download, tile prefetch, or bulk tile scraping. Visible copyright attribution is shown on the map.
- Nominatim address search is only triggered by explicit form submission, is limited to one request per 1.1 seconds, and is unavailable from `file://` pages. Never use it for autocomplete or bulk GIS extraction. See the [Nominatim usage policy](https://operations.osmfoundation.org/policies/nominatim/).
- Walking directions use the public OpenStreetMap foot-routing demo service; it is not intended as a production availability guarantee.
- Weather is fetched from Open-Meteo for the approximate campus center.
- Official campus alerts, service status, indoor maps, and verified building details are not connected yet. Add authoritative campus GIS and feeds before presenting these as live or verified information.

## Publish with GitHub Pages

The workflow in `.github/workflows/deploy-pages.yml` deploys the static site whenever changes are pushed to the `main` branch.

1. Create a public GitHub repository and upload the project files, keeping `index.html` at the repository root and preserving the `.github` folder.
2. In the repository, open **Settings → Pages** and set the build source to **GitHub Actions**.
3. Wait for the **Deploy static site to GitHub Pages** workflow to finish. GitHub displays the public URL in the Pages settings and the workflow deployment summary.
4. Edit `index.html` or the GIS data later and push changes to `main`; Pages will redeploy automatically.

## Next integration step

Export the university GIS layers to GeoJSON and load them in the map. For durable campus routing and indoor navigation, add a verified pedestrian network and floor-level data, then connect an institution-approved routing/data service.
