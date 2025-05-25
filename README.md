# GenLens

## Demographic Visualisation of AI Research

### Overview

This project aims to critically examine the demographic landscape of researchers in Artificial Intelligence (AI) by analysing author metadata from open-access repositories like arXiv and Zenodo. It uses inferred name data to group authors by likely gender and cultural name origin, connecting them to research topics via keywords. The results are visualised with D3.js to highlight visibility, disparities, and underrepresentation.

### Goals

Extract paper metadata including titles, authors, keywords, and publication dates.

Infer gender and cultural origin from names using public APIs.

Allow manual correction of author data (e.g. for trans, non-binary, or intersex individuals).

Visualise data as a network or cluster chart to explore intersections of demographic and topic.

Encourage reflection on structural biases in academic visibility and publication.


### Data Schema

Each paper record includes:

{
  "title": "Sample Paper Title",
  "authors": [
    {
      "full_name": "First Last",
      "first_name": "First",
      "last_name": "Last",
      "gender_guess": "non-binary",
      "origin_guess": "Indian",
      "gender_source": "inferred",
      "origin_source": "inferred"
    }
  ],
  "keywords": ["artificial intelligence", "machine learning"],
  "abstract": "Abstract text...",
  "publication_date": "2025-04-12",
  "source": "Zenodo",
  "url": "https://zenodo.org/record/xxxx",
  "last_updated": "2025-05-25T14:23:00Z"
}

APIs and Tools

APIs

Zenodo API

arXiv API

genderize.io — Predicts gender from first name

NamSor — Predicts gender and cultural name origin


### Python Libraries

requests — API querying

pandas — data handling

python-dateutil — date parsing and formatting


pip install requests pandas python-dateutil

JavaScript Libraries

D3.js — Data-driven visualisation


### Sample Code (Zenodo Extraction)

import requests
from datetime import datetime

#### Load last update timestamp
with open("last_updated.txt", "r") as f:
    last_updated = f.read().strip()

#### Query Zenodo for new records
url = f"https://zenodo.org/api/records/?q=artificial+intelligence&sort=mostrecent&size=100&updated:>{last_updated}"
response = requests.get(url)
data = response.json()

#### Update timestamp
now = datetime.utcnow().isoformat() + "Z"
with open("last_updated.txt", "w") as f:
    f.write(now)

#### Process records
for record in data['hits']['hits']:
    title = record['metadata']['title']
    authors = [creator['name'] for creator in record['metadata'].get('creators', [])]
    print(title, authors)

### Visualisation Approach

Bubble or network chart using D3.js

Nodes = keywords/topics

Demographic breakdown = node colour or pie segments

Avoid stereotypical colours (e.g. no pink/blue for gender)

Add filters by demographic, keyword, publication date

Tooltip with list of authors and links to original paper


### Manual Data Correction

A CSV or database can support manual overrides, with fields:

full_name

corrected_gender

corrected_origin

notes


This is merged with inferred data and clearly marked.

### Ethical Note

Name-based inference of identity is inherently approximate and potentially inaccurate. This project is not about classification but about making visible the overrepresentation of dominant groups and questioning who gets visibility in AI research. Manual correction is supported, and uncertain results should always be marked as such.

### Next Steps

[ ] Define the AI-related keywords to search for

[ ] Build data extraction for Zenodo (start with static dataset)

[ ] Connect to NamSor or genderize APIs

[ ] Design correction interface (CSV or Airtable)

[ ] Build D3.js visualisation

[ ] Add automatic update script using ISO 8601 timestamps


### Licence

This project is released under the MIT Licence. Data is used solely for critical analysis and educational purposes.

