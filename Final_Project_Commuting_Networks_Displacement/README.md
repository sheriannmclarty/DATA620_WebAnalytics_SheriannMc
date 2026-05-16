# Final Project: Commuting Networks & Displacement

**Course:** DATA 620 — Web Analytics  
**Student:** Sheriann McLarty  
**Project:** Gentrification and Network Centrality Across NYC's Five Boroughs  

## Project Overview

This final project examines whether gentrification and displacement pressure are reflected in NYC commuting network patterns and 311 complaint language.

The project incorporates both major DATA 620 course themes:

- **Network analysis:** A directed weighted commuting network using 2021 Census LODES tract-to-tract commuting flows.
- **Text processing:** NYC 311 complaint terms and complaint categories analyzed by gentrification stage.

The workflow includes data cleaning, network construction, centrality measures, Kruskal-Wallis tests, spatial visualization, 311 complaint analysis, and interpretation of findings.

## Main Files

- `DATA620_FinalProject_SheriannMcLarty.ipynb` — reproducible notebook
- `DATA620_FinalProject_Report_SheriannMcLarty.html` — exported report
- `DATA620_FinalProject_Presentation_SheriannMcLarty.pptx` — final presentation

## Research Question

Do census tracts experiencing gentrification and displacement pressure in NYC's five boroughs occupy significantly different positions in the citywide commuting network than stable, affordable, or deeply disinvested tracts?

## Summary

The project finds that out-degree centrality provides the clearest network signal for displacement and disinvestment. In-degree centrality is more useful for identifying job and institutional hubs, while out-degree centrality captures how broadly residents commute outward from their home tracts. The 311 complaint analysis adds a text-processing layer by showing how housing distress and quality-of-life complaint language differ across gentrification stages.
