# Coordinates Belgium — ArcGIS Experience Builder Widget

A custom widget for [ArcGIS Experience Builder](https://www.esri.com/en-us/arcgis/products/arcgis-experience-builder/overview) that displays map coordinates in real time or on click, with support for multiple coordinate systems and on-the-fly format switching.

Based on the original [Coordinates widget](https://github.com/Esri/solutions-components) by Esri, and further modified to add runtime format selection.

---

## Features

- **Real-time coordinates** — displays coordinates as the cursor moves over the map
- **Click-to-locate** — pin a point on the map and copy its coordinates
- **Multiple coordinate systems** — configure any number of output CRS by WKID (geographic and projected)
- **Runtime format switching** — switch the display format on the fly without changing the app configuration:
  - `DD` — Decimal Degrees
  - `DDM` — Degrees Decimal Minutes
  - `DMS` — Degrees Minutes Seconds
  - `MGRS` — Military Grid Reference System
  - `USNG` — US National Grid
  - `UTM` — Universal Transverse Mercator
  - `Long/Lat` — labeled decimal degrees
- **3D scene support** — shows elevation and eye altitude for SceneViews
- **Two widget styles** — Classic (toolbar) and Modern (card)
- **Datum transformation** — configurable datum transformation WKID per coordinate system
- **Copy to clipboard** — one-click copy of the displayed coordinates
- **Localized** — ships with translations for 40+ locales

## Format override behaviour

When a format is selected at runtime (e.g. MGRS, UTM), the widget projects the map point to the geographic CRS associated with the configured coordinate system's spheroid — not hardcoded to WGS84. This ensures datum consistency for non-WGS84 reference systems (e.g. RGF93, ETRS89). If the CRS definition has no GEOGCS entry, it falls back to WGS84 (WKID 4326).

## Installation

This widget is designed to be dropped into an ArcGIS Experience Builder custom widgets folder.

1. Copy the `coordinates-be` folder into your Experience Builder client's `widgets/` directory:

   ```
   <exb-root>/client/your-extensions/widgets/coordinates-be
   ```

2. Restart the Experience Builder dev server.

3. The widget appears in the widget panel under its configured label.

> [!NOTE]
> This widget targets Experience Builder **v1.17**. The `exbVersion` field in `manifest.json` must match your installed version.

## Configuration

Open the widget settings panel in build mode:

| Setting | Description |
|---|---|
| **Map widget** | Select the Map or Scene widget to connect to |
| **Widget style** | Classic (inline toolbar) or Modern (card with footer) |
| **Output coordinate systems** | Add one or more CRS by WKID; each can have its own display unit, decimal precision, and datum transformation |
| **Display unit** | Per-system unit: meters, feet, DD, DDM, DMS, MGRS, USNG, etc. |
| **Elevation unit** | Metric or Imperial (for 3D scenes) |
| **Decimal places** | Separate precision settings for coordinates and altitude |
| **Thousand separators** | Toggle digit grouping |
| **Display order** | Longitude/Latitude (X, Y) or Latitude/Longitude (Y, X) |
| **Default coordinate format** | Format shown to end users on first load: DD, DDM, DMS, MGRS, USNG, UTM, or Long/Lat (default: DD) |

### Adding a coordinate system

1. Click **New coordinate system** in the settings panel.
2. Enter a valid [WKID](https://developers.arcgis.com/rest/services-reference/enterprise/using-spatial-references.htm).
3. Optionally set a display label, display unit, and datum transformation WKID.
4. Save — the system appears in the runtime dropdown.

## Project structure

```
coordinates-be/
├── manifest.json          # Widget metadata and ExB version
├── config.json            # Default widget configuration
├── icon.svg               # Widget icon
├── src/
│   ├── config.ts          # TypeScript interfaces and enums
│   ├── runtime/
│   │   ├── widget.tsx     # Main runtime component
│   │   ├── style.ts       # Runtime styles
│   │   └── components/    # Sub-components (ElevEye, TextAutoFit)
│   ├── setting/
│   │   ├── setting.tsx    # Settings panel
│   │   ├── system-config.tsx  # Per-system configuration panel
│   │   └── style.ts       # Settings styles
│   ├── utils/
│   │   └── index.ts       # Coordinate conversion utilities
│   └── json/
│       └── simple_geogcs.json  # Geographic CRS lookup table
└── dist/                  # Compiled output (loaded by ExB at runtime)
```

## License

Apache 2.0 — see [LICENSE](http://www.apache.org/licenses/LICENSE-2.0).
