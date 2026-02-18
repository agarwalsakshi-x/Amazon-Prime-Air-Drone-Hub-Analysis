# Amazon Prime Air, Drone Hub Location Strategy for Massachusetts
**Supply Chain Analytics | MSBA Capstone Project | UMass Amherst**

> *Where should Amazon place drone hubs in Massachusetts to maximize delivery coverage, minimize flight distance, and capture the highest share of demand, all at the same time?*

This project answers that question using a **four-dimensional analytical framework** integrating Center of Gravity optimization, coverage radius modeling, and Huff's probabilistic gravity model across all 14 Massachusetts counties.


## The Business Problem

Amazon's drone delivery market is projected to grow from $1.5B to $18B by 2032. Massachusetts, with 7M residents, ~98% broadband access, and a median household income of $101K, is one of the strongest early-deployment candidates in the US.

But drone range is limited to 10–25 km. You can't serve the whole state from one hub. The question becomes: **how many hubs, where, and in what order?**


## Analytical Framework

The project evaluated four dimensions before making any location decision:

**Dimension 1: Online Business:** 75–85% of Amazon's most-purchased products are under 5 lbs and drone-eligible (apparel, cosmetics, small electronics, supplements). US e-commerce has grown from 5% of retail in 2012 to 16%+ in 2024, creating sustained demand for faster last-mile fulfillment.

**Dimension 2: Drone Technology:** Payload 1–5 kg, range 10–25 km, speed 40–70 km/h. Drones are 70–80% cheaper than vans per delivery on short routes and emit up to 90% less CO₂. Key constraints: FAA BVLOS restrictions, Logan Airport airspace in Boston, and battery limits that make suburban rather than urban hub placement optimal.

**Dimension 3: Population:** County-level dataset built from US Census ACS data covering population, median income, urbanization score, broadband access, and median age for all 14 MA counties (total population 6.99M). Eastern MA (Middlesex, Suffolk, Essex, Norfolk, Plymouth) dominates demand; Western MA counties (Berkshire, Franklin) benefit most from drones due to long traditional delivery times.

**Dimension 4: Location Analysis:** Three complementary methods applied sequentially (see below).


## Location Analysis Methods

### 1. Center of Gravity (COG)
Weighted by population, income, broadband access, urbanization, and median age, not just geographic midpoints. Each county's latitude/longitude was multiplied by its composite demand weight to identify the true demand-weighted center of each region.

| Region | COG Result | COG Coordinates |
|--------|-----------|----------------|
| Eastern MA | **Newton** | 42.3436°N, 71.1483°W |
| Central MA | **Worcester** | 42.3181°N, 71.8405°W |
| Western MA | Northampton (COG) → **Springfield** (selected) | 42.2394°N, 72.6973°W |

> Springfield was selected over Northampton because it offers 12% greater population coverage and stronger urban density, even though the pure COG fell slightly north.

### 2. Coverage Radius Modeling
Computed straight-line distances from the hub to every city/town in the coverage zone. At 60 km/h drone speed: 5 min = 5 km, 10 min = 10 km, 15 min = 15 km.

| Hub | ≤15 km (15 min) | ≤10 km (10 min) | ≤5 km (5 min) |
|-----|-----------------|-----------------|----------------|
| Newton (Eastern) | **1,724,123** | 1,299,920 | 220,381 |
| Springfield (Western) | 373,892 | 183,916 | 25,369 |
| Worcester (Central) | 357,118 | 268,046 | 265,454 |

**Estimated daily drone volume** (assuming 20% weekly order rate, 10% drone-eligible):

| Hub | Weekly Orders | Drone-Eligible | Daily Drone Volume |
|-----|--------------|----------------|--------------------|
| Newton | 344,825 | 34,482 | **~4,926/day** |
| Springfield | 74,778 | 7,478 | ~1,068/day |
| Worcester | 71,424 | 7,142 | ~1,020/day |

### 3. Huff's Gravity Model
Rather than assigning each county to its nearest hub, the Huff model computes a **probabilistic market share** for each hub based on both attractiveness (size) and distance decay (α = 2).

Formula: `E_ij = (S_j / T_ij²) / Σ(S_j / T_ij²) × C_i`

Hub attractiveness scores (S_j) were derived from regional composite weights:
- Newton: 4,384,113 (Eastern MA aggregate weight)
- Springfield: 1,538,016 (Western MA aggregate weight)  
- Worcester: 856,858 (Central MA aggregate weight)

**Results: statewide demand allocation:**

| Hub | Expected Demand (people) | Market Share |
|-----|--------------------------|--------------|
| Newton | 5,058,496 | **72.35%** |
| Worcester | 1,094,539 | 15.65% |
| Springfield | 838,818 | 12.00% |
| **Total** | **6,991,852** | **100%** |

Example calculation (Barnstable County):
- Attr(Newton) = 4,384,113 / 101.4² = 426.37
- Attr(Worcester) = 856,858 / 149.5² = 38.34
- Attr(Springfield) = 1,538,016 / 204.2² = 36.89
- → P(Newton) = 426.37 / 501.60 = **85.0%** of Barnstable demand goes to Newton

## Why COG + Huff Together

| Method | Tells Us | Limitation |
|--------|----------|-----------|
| COG | **Where** to place hubs to minimize drone travel distance | Doesn't tell us how much demand each hub will capture |
| Huff | **How much** demand each hub will attract | Doesn't generate locations, only evaluates candidates |

Using both methods together gives Amazon **operational efficiency** (COG) and **market-share certainty** (Huff) that neither method can provide alone.


## Recommended Hub Network

### Newton: Primary Mega-Hub
- Aligns with Eastern MA center of gravity (42.34°N, 71.15°W)
- Captures **72.35% of statewide demand** (5.06M people)
- **~4,926 drone deliveries/day**, highest capacity requirement
- Serves Boston, Cambridge, Somerville, Waltham, Arlington, Brookline, and the surrounding high-income suburban corridor
- Highest broadband access + income concentration in state

### Worcester: Central Stabilizing Hub
- COG falls directly on Worcester County, perfect alignment
- Captures **100% of Central MA demand** in Huff model
- ~1,020 drone deliveries/day
- Acts as East–West balancing node; absorbs overflow from Newton at peak

### Springfield: We stern Anchor Hub
- Captures **~88% of Western MA demand** despite COG pointing slightly north to Northampton
- Covers 373,892 residents within 15 km (~374K in Pioneer Valley)
- ~1,068 drone deliveries/day
- Enables future micro-hub expansion to Amherst, Holyoke, and Northampton

## Phased Deployment Roadmap

**Phase 1: Newton (Year 1)**
Launch flagship hub. Refine flight routing, FAA compliance, and customer delivery workflows. Captures 72%+ of demand immediately.

**Phase 2: Worcester (Year 2)**
Activatthe e Central hub. Expand statewide reach, implement cross-hub load balancing algorithms.

**Phase 3: Springfield (Year 3)**
Complete statewide coverage. Pilot rural-service optimization and weather-resilience protocols for Western MA.


## Key Findings

1. **A single hub cannot serve Massachusetts.** Newton's 15-km radius covers 1.72M people, but that leaves 5.27M residents unserved. Three hubs are the minimum viable network.

2. **Newton dominates demand regardless of method.** Both COG and Huff independently point to Newton. The COG calculation produces coordinates 42.34°N, 71.15°W, mapping directly to Newton. The Huff model assigns 72.35% of statewide demand there. This dual validation makes Newton the most analytically confident hub selection.

3. **Worcester is the only possible Central hub.** With a single county (856,858 people) in the Central region, the COG literally equals Worcester County. Huff confirms it captures 100% of Central demand with no competition.

4. **Volume grows, but density drops with distance.** Newton can reach 1.3M people within 10 minutes but only 220K within 5 minutes, indicating a dense suburban ring, not an urban core. This reinforces suburban hub placement over the Boston city center.

5. **Rural counties have the highest drone value per delivery.** Franklin County (71K population) has very long traditional delivery routes, drones may reduce delivery times by 60–80% even at low volume. Springfield's hub enables this coverage.


## Limitations & Further Scope

- **No actual Amazon order data**, demand modeled from demographic proxies (population, income, broadband). Real SKU-level order density would sharpen fleet sizing estimates.
- **No GIS airspace constraints**, model assumes radial coverage; actual routes must avoid Logan Airport no-fly zones, building heights, and terrain.
- **No cost modeling**, land acquisition, permitting, charging infrastructure, and maintenance costs not evaluated. A cost-weighted optimization could shift hub placement.
- **No seasonal adjustment**, Massachusetts winters affect battery range and weather-related delivery restrictions; buffer capacity not modeled.
- **Future: micro-hub expansion**, Cambridge, Quincy, Lowell, Amherst, and coastal communities could support secondary hubs in Phase 4+.


## Files in This Repository

| File | Description |
|------|-------------|
| `COG_Calculation_Analysis.xlsx` | Weighted Center of Gravity calculations for all three MA regions |
| `Huffs_method_Analysis.xlsx` | Huff gravity model, county-level distance matrix, hub attractiveness scores, demand allocation |
| `Amazon_Prime_Air_Report.pdf` | Full 25-page analytical report with methodology, findings, and deployment roadmap |
| `Amazon_Prime_Air_Presentation.pdf` | Executive presentation deck |


## Skills Demonstrated

- Supply chain network design (multi-hub optimization)
- Center of Gravity methodology (demand-weighted spatial analysis)
- Huff's Gravity Model (probabilistic demand allocation)
- Coverage radius modeling (population × distance analysis)
- Geospatial analytics using county-level census data
- Translating quantitative outputs into executive deployment strategy


## Topics
`supply-chain` `location-analytics` `network-design` `center-of-gravity` `huff-gravity-model` `geospatial-analytics` `last-mile-logistics` `excel`
