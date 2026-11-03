# Subquestion I.
 As the water speed increases, to what extent is there a change in disagreement between satellite measurements and field measurements for analyzing algal blooms in rivers?

## Procedure
For each river, select a square that will be tested by the USGS sensor and the Sentinel satellite imagery. The square is the Sentinel-2 pixel (20 m) containing the USGS sensor, and it must contain only water, with no riverbank.
Use the in-situ USGS sensors to detect the algal blooms at a given point in time for the slowest river identified. Use the USGS reading closest in time before the satellite overpass (about 10 a.m. local time).
Then, use Sentinel-2 imagery to detect the same algal bloom, [x minutes later].
The chlorophyll-a concentration is then measured in µg/L for both: USGS estimated chlorophyll-a, and a published Sentinel-2 chlorophyll-a algorithm that was not trained on these rivers.
The chlorophyll-a concentration is then recorded, for both in-situ and the satellite imagery. Record the river speed at the time of measurement [source TBD].
Steps 1–5 are repeated for rivers, with each trial being tested on a faster-moving river.
The trend is then graphed.


## Constants
x is kept as small as possible and recorded for every trial. It is set by the overpass time and the USGS logging interval, so it can’t be chosen.
The amount of turbidity is kept relatively constant.
This is done by ensuring that weather conditions remain mostly constant.
This is also done by ensuring that measurements are taken no less than 3 days after a storm.
This is confirmed by checking USGS turbidity readings at each site.

## Accepted Value
The in-situ USGS measurements. These are a fair reference only for the water at the sensor at its timestamp.

## Independent Variable
The speed of the river

## Dependent Variable
Difference in chlorophyll-a concentration (µg/L)


## Control
In-situ and satellite differences on a lake (~0 mph speed)



| Stage | Question it answers | Input → output | What you'd learn |
| --- | --- | --- | --- |
| 1. Site inventory | Which rivers have usable USGS chlorophyll data? | USGS site list → table of sites, dates, logging interval | Querying USGS data in Python (the official `dataretrieval` package), pandas |
| 2. Speed source | Where does "speed" come from for each site and date? | Site → speed per timestamp, or "not available" | What USGS actually measures at each site (discharge vs. velocity) |
| 3. Find images | Which Sentinel-2 overpasses are clear over each site? | Site coordinates + dates → list of scenes with overpass times | Searching satellite catalogs (Earth Engine or a STAC catalog), cloud filtering |
| 4. Extract pixel | What does the satellite see in the sensor's pixel? | Scene + coordinates → reflectance per band, plus a check that it's water-only | Rasters and coordinate systems (`rasterio` or Earth Engine) |
| 5. Apply algorithm | What chlorophyll-a does a published method give? | Reflectance → µg/L | Implementing one published equation exactly as the paper specifies |
| 6. Match | Which USGS reading pairs with each overpass? | Overpass time + USGS series → paired rows with x recorded | Timestamp handling and time zones (this is where most bugs hide) |
| 7. Filter | Which pairs meet your constants? | Pairs → pairs passing turbidity and storm rules | Writing your rules as code, decided *before* you see results |
| 8. Analyze | Does disagreement change with speed? | Pairs → graph of difference vs. speed, plus the lake control | Plotting, log-space error |