# Changelog

All notable changes to `@intended-inc/open-intent-layer` are documented here.
This project follows [Semantic Versioning](https://semver.org/) — within a
major version, the OIL identifier set is **append-only** (codes never get
reused or renumbered).

## 2.0.2 — 2026-04-30

### Documentation
- License section in the README is now the standard `Apache License 2.0.
  See LICENSE.` (previously editorialized on top of the license).
- Removed two GitHub deep links from the "Companion runtime" section that
  pointed into a private repo and 404'd for public visitors. Kept the two
  `intended.so/*` links which resolve.

No taxonomy or API changes. Safe to upgrade from 2.0.1.

## 2.0.1 — 2026-04-29

### Documentation
- Fixed the README usage example. The 2.0.0 README showed
  `import { OIL_TAXONOMY, findCategory }` which do not exist. Real
  exports are `{ oil, domains, getCategory, getDomain,
  getDomainForCategory }`. Example now matches reality, including a
  correct `getCategory("OIL-1502")` result of `Pick & Place`.

No taxonomy or API changes. Safe to upgrade from 2.0.0.

## 2.0.0 — 2026-04-28

### Added — physical / embodied operations expansion
- 15 new domains spanning physical-AI:
  - OIL-1500 Manipulation
  - OIL-1600 Locomotion & Mobility
  - OIL-1700 Sensing & Perception
  - OIL-1800 Actuation & Control
  - OIL-1900 Manufacturing & Production
  - OIL-2000 Autonomous Vehicles
  - OIL-2100 Aerial Systems
  - OIL-2200 Surgical & Medical Robotics
  - OIL-2300 Agricultural Operations
  - OIL-2400 Construction & Excavation
  - OIL-2500 Hazardous Environments
  - OIL-2600 Energy & Utilities
  - OIL-2700 Mining & Resource Extraction
  - OIL-2800 Logistics & Material Handling
  - **OIL-2900 Embodied AI Safety** (load-bearing safety primitives —
    e-stop, geofence enforcement, exclusion zones, light curtains,
    attestation requirements, operator-in-the-loop escalation)
- 93 new categories under those domains (subtotal across the new
  physical / embodied side).
- v2.0 totals: **29 domains, 173 categories**.

### Unchanged from v1.0
- The 14 digital-operations domains (OIL-100 through OIL-1400) and
  their 80 categories are stable. Codes were not renumbered.
- Lookup helper API (`oil`, `domains`, `getDomain`, `getCategory`,
  `getDomainForCategory`, `allCategoryCodes`).

### Migration
- Consumers of v1.0 can upgrade in place. No code changes required;
  the new domains simply become available.
- Code that pattern-matches a closed set of OIL codes should review
  whether it now needs to handle physical / embodied codes.

## 1.0.0 — 2026-03-21

### Initial release
- 14 enterprise digital domains (OIL-100 — OIL-1400)
- 80 categories
- Apache 2.0 licensed taxonomy with TypeScript helpers
