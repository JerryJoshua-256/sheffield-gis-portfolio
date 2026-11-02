# Sheffield Security Planning — GIS Portfolio Case Study

A GitHub Pages-ready, single-page portfolio case study based on the supplied **Sheffield_project.pdf**.

## Project focus
A QGIS-based evaluation of crime patterns, existing CCTV service areas, priority LSOAs and two proposed CCTV schemes in Sheffield, United Kingdom.

## Website
- `index.html` — responsive portfolio case study with a clean, static editorial hero.
- `assets/` — original project-page images rendered from the supplied PDF.
- `technical-workflow.md` — detailed, reproducible workflow based only on the documented operations.
- `source-notes.md` — source limitations and numerical discrepancies retained from the PDF.

## Publish with GitHub Pages
1. Create a public GitHub repository.
2. Upload the contents of this folder (not the ZIP file itself).
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select the `main` branch and `/ (root)` folder, then save.
6. Wait for GitHub Pages to publish the site. The public URL will appear in the Pages settings.

Google Fonts are loaded from a public CDN. The portfolio itself is static HTML/CSS and does not use a 3D scene or animated star.

## Source and fidelity
Project-specific wording, values, categories and conclusions are reproduced from the supplied PDF. The original map-page images are included as page renders and have not been redrawn. Where the source contains conflicting figures, both are retained and the discrepancy is disclosed.

## Important data note
The source PDF describes the input datasets but does not include the underlying GIS data package. This repository therefore documents the workflow and results; it does not contain the crime points, LSOA polygons, CCTV point layers or proposed installation shapefiles needed to rerun the spatial analysis.
