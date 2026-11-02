# Source Notes and Fidelity

This portfolio is based on the supplied 26-page `Sheffield_project.pdf`.

## Preservation
- Project objective, narrative, workflow descriptions, priority labels, statistics, comparison values and final Plan A decision are retained from the source.
- Original map pages are included as page renders; they are not redrawn.
- No new crime counts, coverage figures, GIS settings or conservation-area statistics have been introduced.

## Items explicitly not supplied by the PDF
The source does not state the QGIS version, coordinate reference system, choropleth class breaks, exact field-calculator formulas, or detailed buffer parameters beyond the stated distances and dissolve step.

## Discrepancies and terminology retained
1. The methodology summary describes crime aggregation with `Join Attributes by Location`; the detailed step describes `Select by Location`.
2. The area calculation discussion names `Area_Ha`, `Covered_Ha`, and `%covered`; the priority expression uses `"% Covered"`.
3. Plan A's table reports 8,226.69 ha / 22.34%, while the later interpretation and comparison table report 7,719.41 ha / 20.96%.
4. The Plan B table gives 28.90778%, while the interpretation rounds this to 28.91%.
5. The brief asks for definitions of crime density and surveillance service areas, but the project write-up does not provide a standalone formal definition for both terms. The website describes the operational service-area method as buffer intersection and coverage percentage, without adding an external definition.

## Public repository note
Before making the repository public, confirm that the user has permission to publish the source document's maps and any underlying third-party datasets. The original data package is not included in this generated website.
