# Sheffield Security Planning Project — Technical Workflow

This workflow follows the steps recorded in the supplied project report. It is written to make the documented process understandable and reproducible without inventing missing processing parameters.

## 1. Project inputs

The project brief lists:
- Sheffield Lower Super Output Areas (LSOAs), 2021 — 343 polygons.
- Sheffield geographic boundary shapefile.
- Sheffield crime incident points for 1–30 September 2023, including crime types and point locations.
- `Sheffield_Crime_Count_All.csv`, containing crime counts by LSOA code.
- Sheffield conservation-area polygon shapefile.
- Existing CCTV camera locations from Sheffield City Council Open Dataset.
- Plan A proposed installation shapefile: 10 new cameras, each with a 2 km range.
- Plan B proposed upgrade shapefile: 10 existing cameras, each with a 3 km range.

The source PDF does not provide the actual GIS data files, coordinate reference system, QGIS version or exact processing parameter settings.

## 2. Crime aggregation by LSOA

1. Load the September 2023 crime incident point layer and the 2021 LSOA polygon layer in QGIS.
2. Use **Select by Location** to identify crime points falling within each LSOA.
3. Create a new crime-count layer recording the number of crime points within each LSOA.
4. The methodology summary in the report also describes the aggregation as **Join Attributes by Location**. The detailed project section specifically names **Select by Location**. Both descriptions are retained here because the source uses both.

## 3. Crime choropleth and high-incident selection

1. Generate a choropleth map of total crime incidents by LSOA.
2. Identify LSOAs with a monthly crime count greater than or equal to 30.
3. The report states that 41 LSOAs met this threshold.
4. The report interprets this threshold as identifying areas with at least one crime per day during the month.

The exact choropleth classification method and class breaks are not specified in the PDF.

## 4. Existing CCTV buffer and service areas

1. Load existing CCTV camera points.
2. Create a **2 km buffer** around each CCTV point, as specified in the brief.
3. Dissolve all buffers into one coverage geometry.
4. Use **Intersect** between the dissolved CCTV buffer and the LSOA polygons to identify the portions of LSOAs covered by the existing CCTV service area.
5. The report stresses that the intersect output is incomplete as a representation of all LSOAs because LSOAs not touched by a buffer are absent from the intersected layer.

## 5. Calculate area and percentage coverage

Within the intersected layer's attribute table, calculate:
- `Area_Ha` — area of the relevant LSOA.
- `Covered_Ha` — area of the LSOA portion serviced by the CCTV buffer, in hectares.
- `%covered` — percentage of the LSOA area covered by the CCTV buffer.

The report identifies these as the three key fields for the subsequent analysis. The exact field-calculator expressions are not included in the PDF and are therefore not reconstructed here.

## 6. Join the intersect output back to the full LSOA layer

1. Use **Join Attributes by Field Value**.
2. Use `lsoa21cd` as the join field.
3. Join the intersected coverage attributes to the original LSOA layer.
4. This restores LSOAs that were not intersected by any CCTV buffer, allowing the analysis to distinguish between LSOAs with zero coverage and those with partial coverage.

The report describes `lsoa21cd` as a unique code assigned to each LSOA.

## 7. Priority classification

Apply the following expression as reproduced in the report:

```sql
CASE
WHEN "Crime_Coun" >= 30 AND "% Covered" = 0
THEN 'VERY HIGH'
WHEN "Crime_Coun" >= 30 AND "% Covered" > 0 AND "% Covered" <=50
THEN 'HIGH'
WHEN "Crime_Coun" >= 30 AND "% Covered" > 50
THEN 'MEDIUM'
WHEN "Crime_Coun" <30 AND "% Covered" = 0
THEN 'LOW'
WHEN "Crime_Coun" < 30 AND "% Covered" > 0
THEN 'LEAST'
END
```

The categories mean:
- **VERY HIGH:** 30 or more monthly incidents and zero CCTV coverage.
- **HIGH:** 30 or more monthly incidents and CCTV coverage greater than zero and up to 50%.
- **MEDIUM:** 30 or more monthly incidents and CCTV coverage greater than 50%.
- **LOW:** Fewer than 30 monthly incidents and zero CCTV coverage.
- **LEAST:** Fewer than 30 monthly incidents and some CCTV coverage.

The source uses the field label `"% Covered"` in the expression and separately identifies a field as `%covered` in the attribute-table discussion. Confirm the actual field name in the working dataset before running the expression; this portfolio does not silently rename the source fields.

## 8. Priority-area summary reported

| Priority category | LSOAs | Area (Ha) | Percentage of total area |
|---|---:|---:|---:|
| Very High | 8 | 449.9969 | 1.22% |
| High | 1 | 148.464 | 0.40% |
| Medium | 32 | 2,449.95 | 6.65% |
| Low | 108 | 12,262.2 | 33.29% |
| Least | 194 | 21,519.8 | 58.43% |
| **Total** | **343** | **36,830.4** | **100.00%** |

## 9. Alternative A assessment

- Plan A proposes 10 new cameras, each with a 2 km coverage range.
- Assess the proposed Plan A coverage against the five priority classes.
- The source's Plan A coverage table reports:

| Priority category | Plan A covered area (Ha) | Total area (Ha) | Coverage |
|---|---:|---:|---:|
| Very High | 397.1469 | 449.9969 | 88.26% |
| High | 0 | 148.464 | 0% |
| Medium | 904.42 | 2,449.95 | 36.92% |
| Low | 2,453.63 | 12,262.2 | 20.01% |
| Least | 4,471.49 | 21,519.8 | 20.78% |
| Total | 8,226.69 | 36,830.4 | 22.34% |

## 10. Alternative B assessment

- Plan B proposes upgrading 10 existing cameras from 2 km to 3 km coverage.
- Generate a **3 km buffer** for Alternative B.
- Assess coverage against the same five priority classes.
- The source's Plan B coverage table reports:

| Priority category | Plan B covered area (Ha) | Total area (Ha) | Coverage |
|---|---:|---:|---:|
| Very High | 26.056 | 449.9969 | 5.790262111% |
| High | 141.418 | 148.464 | 95.25406833% |
| Medium | 2,439.84 | 2,449.95 | 99.58733852% |
| Low | 612.407 | 12,262.2 | 4.994266934% |
| Least | 7,427.13 | 21,519.8 | 34.51300663% |
| Total | 10,646.851 | 36,830.4 | 28.90778% |

## 11. Comparison and final decision

The report's comparison table gives:
- Very High: Plan A 88.26%; Plan B 5.79% — Plan A performs better.
- High: Plan A 0.00%; Plan B 95.25% — Plan B performs better.
- Medium: Plan A 36.92%; Plan B 99.59% — Plan B performs better.
- Low: Plan A 20.01%; Plan B 4.99% — Plan A performs better.
- Least: Plan A 20.78%; Plan B 34.51% — Plan B performs better.
- Overall: Plan A 20.96%; Plan B 28.91% — Plan B performs better.

The final decision in the report is **Plan A**, for the following stated reasons:
1. It covers various areas around the city, not just centralised CCTV coverage.
2. Its focus on Low Priority Areas is intended to help prevent the spill of crime into those areas.
3. The report argues that because higher crime rates remain in some regions with existing central-city camera coverage, planning should also prevent crime spreading to Low and Least Priority areas rather than simply adding cameras to already-covered areas.

## 12. Conservation areas

The report visually considers proposed and existing CCTV locations in relation to conservation-area polygons. It notes that a few proposed cameras in both alternatives are located within a few conservation areas, and that some existing cameras are within or at the edge of conservation areas. The report recommends due caution in planning activities affecting these areas.

## 13. Source limitations and items not specified

The source PDF does not specify:
- QGIS version.
- Coordinate reference system or units configuration.
- Exact choropleth classification scheme and class breaks.
- Exact area and percentage field-calculator expressions.
- Detailed buffer segment, cap, join or dissolve settings beyond the stated dissolve operation.
- A quantitative conservation-area coverage summary.
- The precise processing sequence used to derive every Plan A and Plan B area figure.

These items are not invented in this workflow.

## 14. Numerical discrepancy retained from the source

The Plan A table on the source's page 70 reports **8,226.69 ha** and **22.34%** overall coverage. The interpretation on page 72 instead states **7,719.41 ha** and **20.96%**. The comparison table on page 74 also uses **20.96%** for Plan A. Both values are retained in this portfolio in their original contexts; the discrepancy should be checked against the original GIS project or data before presenting a single reconciled Plan A total.
