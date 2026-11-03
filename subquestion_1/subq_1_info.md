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