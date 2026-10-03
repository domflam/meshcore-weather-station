# UI/UX concepts

This directory captures the product and visual direction for the MeshCore Weather Station Flutter Web / PWA frontend.

The reference deployment is the CASLF archery field at Saint-Leu-la-Forêt, but the application remains white-label and reusable by other clubs and communities.

## Concept visuals

Two initial concept boards were produced during the design phase:

- `concept-v1.png` — initial mobile, desktop/tablet and iframe weather-dashboard exploration.
- `concept-v2-field-and-3d.png` — expanded concept including real field photography, 3D-course content, history and PWA presentation.

The PNG files are design references rather than pixel-perfect implementation specifications. They can be replaced by updated iterations as the product evolves.

## Product direction

The PWA should feel like a useful local application that club members may keep on their phone, rather than an engineering telemetry dashboard.

Primary goals:

- immediate view of current conditions at the field;
- wind given strong visual priority because it matters directly to archery;
- simple access to temperature, humidity, pressure and rainfall;
- useful trends and historical charts;
- clear freshness of the last observation;
- responsive experience across mobile, tablet and desktop;
- installable PWA;
- compact `/embed` view for the existing club website;
- no login required for public weather information.

## CASLF reference theme

The CASLF deployment should take inspiration from the club's existing identity without reproducing the current website UI.

Visual direction:

- forest green as the primary identity colour;
- light/off-white backgrounds;
- blue accents for weather and water-related information;
- restrained use of target colours where useful;
- modern cards and generous spacing;
- subtle woodland / archery references;
- real photography from the CASLF field where it adds context.

## Real field photography

Production assets should progressively replace generic concept imagery with photographs taken at the actual site.

Useful photography includes:

- main shooting range;
- panoramic view of the field;
- woodland / tree line;
- 3D archery course;
- individual 3D targets or stations;
- seasonal views of the site.

Photos should remain supporting context: weather information must stay legible in bright outdoor conditions and on small screens. Use overlays/gradients where necessary rather than relying on text directly over complex photography.

## Core views

### Home

Fast outdoor-readable summary:

- station online/freshness state;
- current temperature and optional calculated apparent temperature;
- humidity;
- atmospheric pressure and trend;
- rainfall today;
- wind speed;
- gust speed;
- wind direction / compass;
- concise descriptive shooting conditions based on measured values.

### Graphs

Selectable periods such as:

- 24 hours;
- 7 days;
- 30 days;
- 1 year when sufficient history exists.

Priority charts: temperature, humidity, pressure, wind average/gusts and rainfall.

### Details

All current observations plus min/max/trend information and station freshness.

Technical station health (battery, radio/gateway status, etc.) should be available but should not dominate the public weather view.

### History

Daily and period summaries with rainfall totals, temperature ranges, wind statistics and pressure evolution.

### Embed

A compact responsive `/embed` route containing only the most useful current conditions and a link to the full PWA. It is intended to make integration into the club website a simple iframe operation.

## Optional future club content

The PWA architecture may later support contextual club content alongside weather without coupling that content to the weather telemetry protocol.

A possible CASLF-specific extension is a 3D-course view containing photographs, target/station identifiers and distances. This is exploratory and is not part of the weather-station MVP.

## White-label requirement

CASLF branding must be configuration-driven. A fork/deployment should be able to replace at least:

- application name;
- station name;
- logo;
- primary/accent colours;
- hero/background photographs;
- optional club-specific navigation/features.

The core weather components and API model must not depend on CASLF assets.

## Accessibility and outdoor use

Implementation should prioritise:

- strong contrast;
- readable type sizes;
- information not encoded by colour alone;
- touch-friendly targets;
- responsive layouts;
- good readability in direct sunlight;
- clear stale/offline states;
- timestamps preserved when cached data is shown.

## Implementation principle

These concepts establish hierarchy and visual intent. Flutter implementation should use reusable design tokens and components rather than hard-coded CASLF styling so that the same application can serve other deployments.
