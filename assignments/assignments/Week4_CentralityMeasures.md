# Week 4 - Centrality Measures Plan: Brooklyn Commuting Network & Poverty Stratification
**Course:** DATA 620 - Web Analytics  
**Student:** Sheriann McLarty  
**GitHub Repository:** [DATA620_WebAnalytics_SheriannMc](https://github.com/sheriannmclarty/DATA620_WebAnalytics_SheriannMc)

---

## Dataset Identified

**Dataset:** U.S. Census Bureau LEHD LODES Origin-Destination (OD) Files + ACS 5-Year Subject Table S1701  
**Source:** Publicly available from the U.S. Census Bureau via direct file download and Census API

I will use the U.S. Census Bureau's LEHD LODES Origin-Destination (OD) files for New York State to construct a commuting flow network. The OD files include home and work geography identifiers (`h_geocode`, `w_geocode`) and job/worker counts (e.g., `S000`) that can be used as edge weights. To convert block-level commuting flows into a tract-level network, I will use the LODES geography crosswalk to map census blocks to census tracts. For node-level poverty attributes, I will use ACS 5-year Subject Table S1701 (Poverty Status in the Past 12 Months), which provides tract-level poverty estimates and percentages.

**Direct data links:**

- LODES OD file (New York, 2021): `https://lehd.ces.census.gov/data/lodes/LODES8/ny/od/ny_od_main_JT00_2021.csv.gz`
- Geography crosswalk (New York): `https://lehd.ces.census.gov/data/lodes/LODES8/ny/ny_xwalk.csv.gz`
- ACS S1701 Census API (Kings County tracts): `https://api.census.gov/data/2021/acs/acs5/subject?get=NAME,S1701_C03_001E&for=tract:*&in=state:36%20county:047`

The LODES files will be accessed via direct download from the Census Bureau's LEHD FTP server. The ACS poverty data will be pulled programmatically using the Census Bureau's public API. Neither source requires authentication or web scraping.

**Categorical variable per node:** Poverty category derived from ACS S1701 - tracts will be labeled **"High Poverty"** (poverty rate >= 20%) or **"Lower Poverty"** (poverty rate < 20%). As an alternative, I may categorize tracts into poverty-rate quartiles.

---

## Network Definition

| Element | Detail |
|---|---|
| **Nodes** | Census tracts in Kings County, NY (FIPS 36047) |
| **Edges** | Directed edge from home tract to work tract when residents commute from tract A to jobs in tract B |
| **Edge weight** | Total commuters/jobs (LODES S000) |
| **Categorical node variable** | Poverty category based on ACS S1701 poverty rate |
| **Graph type** | Directed, weighted |

---

## High-Level Loading Plan (Brooklyn-to-Brooklyn Only)

1. Download New York LODES OD files and geography crosswalk files directly from the Census Bureau LEHD server using the URLs above
2. Use the crosswalk to map `h_geocode` and `w_geocode` (block GEOIDs) to their corresponding county and tract GEOIDs
3. Filter OD flows to those where BOTH the home geography AND the work geography fall in Kings County (FIPS 36047), so the network represents commuting flows entirely within Brooklyn
4. Aggregate OD flows to tract-to-tract level and sum edge weights (`S000`)
5. Pull tract-level poverty measures from ACS S1701 via the Census API and compute poverty rate per tract; assign each tract a poverty category ("High Poverty" vs "Lower Poverty", or quartiles)
6. Build a directed weighted graph in NetworkX with tract nodes and commuting edges
7. Compute centrality measures for each tract:
   - Degree centrality, reporting in-degree and out-degree separately since the network is directed
   - Eigenvector centrality; if instability occurs in the directed weighted graph, compute eigenvector centrality on a symmetrized undirected version as a robustness check
8. Compare centrality distributions across poverty categories using a t-test or a nonparametric alternative (Mann-Whitney U) if assumptions are not met

---

## Hypothetical Outcome

**Research question:** Do high-poverty and lower-poverty census tracts in Brooklyn differ in degree centrality and eigenvector centrality within the internal Brooklyn commuting network?

**Hypothesis:** Tracts that are more central in the commuting network - especially those with higher in-degree (many commuters enter them for work) - represent job hubs and will be disproportionately lower-poverty tracts. Conversely, high-poverty tracts are expected to be more peripheral, with lower degree and eigenvector centrality, reflecting limited access to highly connected job centers within the borough.

**Why this matters:** If poverty category is associated with centrality in Brooklyn's commuting network, it suggests that economic disadvantage and network marginalization are spatially linked - high-poverty neighborhoods may be structurally disconnected from the commuting flows that connect residents to opportunity. This has direct implications for transit planning, workforce development policy, and equitable urban investment within Brooklyn.

---

## Summary

| Element | Detail |
|---|---|
| **Dataset** | LEHD LODES OD Files + ACS S1701 |
| **Source** | Census Bureau direct download + Census API |
| **Node categorical variable** | Poverty category (High Poverty >= 20% vs Lower Poverty) |
| **Graph type** | Directed, weighted |
| **Loading approach** | pandas + NetworkX; LODES via direct URL, ACS via Census API |
| **Centrality measures** | Degree centrality (in/out) and eigenvector centrality |
| **Statistical test** | Independent samples t-test or Mann-Whitney U |
| **Hypothetical prediction** | Lower-poverty tracts will have higher in-degree centrality, reflecting their role as job hubs; high-poverty tracts will be more peripheral in the commuting network |

---

*Repository: https://github.com/sheriannmclarty/DATA620_WebAnalytics_SheriannMc*
