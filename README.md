# 🌍 Israeli Soil Profiles — Research Database & Tools

> A digitization and interactive mapping project for legacy Israeli soil survey field books  
> **Department of Environmental Science & Technology · University of Maryland**

---

## Overview

This repository hosts the public-facing web interface for the **Israeli Soil Profiles Research Database** — a project digitizing over 111 pedon descriptions from Hebrew-language field books into a modern relational database, with interactive tools for researchers and stakeholders.

The database covers soil profiles spanning the full ecological range of Israel: Mediterranean coastal plains, Galilee and Golan highlands, Jordan Valley, Negev desert, and the Arava Rift Valley.

---

## 🔗 Live Site

**[siddhirohan.github.io/israeli-soil-profiles](https://siddhirohan.github.io/israeli-soil-profiles/)**

The site contains two tools accessible from the top navigation:

---

## 📄 Tab 1 — Pedon Description Generator

Generate USDA-format pedon descriptions for any of the 111 soil profiles in the database. No login required — open to all visitors.

**Features:**
- Dropdown selection of all 111 profiles
- Automatic generation of structured pedon descriptions from live database data
- Preview in-browser before downloading
- One-click download as a formatted `.docx` Word document (Times New Roman, USDA series format)
- Field Book code reference legend (ABK, SBK, PR, MA, etc.)

**Example output includes:**
- Taxonomic class
- Typical pedon horizon-by-horizon description (Munsell color, texture, structure, roots, boundary)
- Type location and geographic setting
- Parent material and climate

---

## 💬 Tab 2 — Research Chatbot

A natural language interface to the soil database, powered by Claude (Anthropic) and queried via live PostgreSQL. **Login required** — access is restricted to authorized researchers.

**Features:**
- Ask questions in plain English: *"What are the most common subgroups?"*, *"Compare average clay % in Vertisols vs Alfisols"*
- Claude generates SQL, executes it against the live database, and explains results
- Results displayed as formatted data tables alongside natural language explanations
- Shows the generated SQL for full transparency
- Suggests follow-up questions
- Full chat history per user session
- Admin panel: add/disable users, view role assignments
- Audit log: timestamped record of every query, login, and system event

**Access:** Contact the lab to request researcher credentials.

---

## 🗄️ Database Summary

| Table | Description | Rows |
|-------|-------------|------|
| `soils_info` | Core soil type metadata, taxonomy, classification | 111 |
| `soil_profile` | Profile identifiers and site linkage | 111 |
| `soil_layers` | Horizon-by-horizon field descriptions | 656 |
| `particle_breakdown` | Particle size analysis (sand/silt/clay %) | 651 |
| `exchangeable_nutrients` | CEC, Ca, Mg, K, Na, SAR | 633 |
| `salinity_saturation_analysis` | EC, soluble ions, gypsum | 531 |
| `mineral_composition` | Clay mineralogy (smectite, kaolinite, illite, etc.) | 419 |
| `available_nutrients` | Organic C, pH, P, N | 185 |
| `mechanical_ancillary_tests` | Atterberg limits, COLE, ESP | 103 |

**Coverage:**
- **111 soil profiles** across 6 soil orders and 46 subgroups
- **9 soil orders represented:** Alfisols, Aridisols, Entisols, Inceptisols, Mollisols, Vertisols
- **Temperature regimes:** 83 hyperthermic, 28 thermic
- **Mineralogy classes:** halloysitic, kaolinitic, smectitic, illitic, mixed
- **Classification standard:** USDA Keys to Soil Taxonomy (older nomenclature as published in source surveys)

---

## 🏗️ Architecture

```
┌─────────────────────────────────┐
│      GitHub Pages (this repo)   │
│         index.html              │
│   ┌─────────┬────────────────┐  │
│   │  Pedon  │   Chatbot      │  │
│   │  Tab    │   Tab          │  │
│   └────┬────┴───────┬────────┘  │
└────────│────────────│───────────┘
         │            │
         ▼            ▼
  Supabase REST   Supabase Edge Function
  (soil DB,       (chatbot-auth project)
   anon key,         │
   public read)      ├── Auth & sessions
                     ├── Audit logging
                     ├── Claude API (SQL gen)
                     └── Soil DB query (read-only)
```

**Infrastructure:**
- **Database:** PostgreSQL 17 on Supabase (`us-east-1`)
- **Pedon generation:** PostgreSQL function `generate_pedon_description()` via Supabase REST API
- **Chatbot backend:** Supabase Edge Function (Deno) on separate auth project
- **AI model:** Claude Sonnet (Anthropic API) for text-to-SQL and result interpretation
- **Frontend:** Single-page HTML — no build step, no framework, no dependencies except `docx.js` for Word export
- **Hosting:** GitHub Pages (free, permanent URL)

---

## 📚 Data Sources

Profiles were digitized from:

- **Dan, J. & Hurvitz, N. (1981).** *Morphological and Analytical Data of Soil Profiles from Israel.* Volcani Center, Agricultural Research Organization, Israel.
- Additional survey field books from the Volcani Center soil survey archive (1960s–1980s).

All Hebrew horizon descriptions were translated to English and coded to **USDA NRCS Field Book for Describing and Sampling Soils, Version 4.0** standards.

---

## 🔬 Research Context

This project supports the long-term goal of **quantitatively developing criteria for Israeli soil series** — particularly the Alfizechron series and related Typic Rhodoxeralf families — by building a structured, queryable dataset from legacy survey materials that previously existed only in physical field books.

**Key analytical capabilities:**
- Cross-profile comparison of particle size, CEC, clay mineralogy
- Family-level taxonomy completion (particle size class, mineralogy class, temperature regime)
- Spatial analysis via ArcGIS Pro integration (Israel Transverse Mercator, WKID 2039)
- Pedon description generation in USDA series format

---

## 👤 Project Team

| Role | Person |
|------|--------|
| Principal Investigator | Dr. Brian A. Needelman |
| Data Engineer & Graduate Researcher | Siddhi Rohan Chakka |
| Institution | Department of Environmental Science & Technology, University of Maryland |

---

## 📬 Contact

For researcher access to the chatbot, data inquiries, or collaboration:  
**Department of Environmental Science & Technology · University of Maryland**

---

## ⚖️ License

Database contents derived from published Israeli soil survey materials. Web interface code: MIT License.

---

*Built with PostgreSQL · Supabase · Claude (Anthropic) · GitHub Pages*
