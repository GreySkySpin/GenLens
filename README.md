# Demographic Visualisation of AI Research

## Overview

This project critically examines the demographic landscape of researchers in **Artificial Intelligence (AI)** by analysing author metadata from open-access repositories such as **arXiv** and **Zenodo**. It uses inferred name data to group authors by likely gender and cultural name origin, connecting them to research topics via keywords. The resulting dataset is visualised with D3.js to explore patterns of visibility, dominance, and underrepresentation.

## Motivation

The research space is disproportionately dominated by names familiar to Western and especially Anglo-European audiences. This bias is not only structural but also psychological—researchers with names that are more recognisable and pronounceable to Western readers are more easily cited, remembered, or included in networks. While names of East Asian origin, especially Chinese and Japanese, are increasingly present, their visibility is often contingent on assimilation into Western academic norms.

This project aims to reveal and challenge such biases, amplify underrepresented voices, and create a platform that allows for more inclusive discovery of work in AI. It intentionally avoids sorting or listing dominant categories first in the interface or database, reflecting a deliberate rejection of inherited hierarchies.

## Goals

- Extract paper metadata including titles, authors, keywords, and publication dates.
- Use APIs to infer gender and cultural origin based on names.
- Allow manual correction of author data, to accommodate trans, non-binary, intersex, and other identities.
- Visualise connections between demographics and research topics using an interactive network.
- Offer filters to promote researchers from marginalised and underrepresented name groups.
- Maintain ethical transparency about the limits and assumptions of inference.

## Data Schema

Each paper record follows this structure:

```json
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
```

## APIs and Tools

### Metadata Sources

- [Zenodo API](https://developers.zenodo.org/)
- [arXiv API](https://arxiv.org/help/api/)

### Demographic Inference APIs

- [genderize.io](https://genderize.io/) — Predicts likely gender based on first names.
- [NamSor API](https://www.namsor.com/) — Predicts both gender and cultural name origin.

### Python Libraries

- `requests` — HTTP requests for APIs
- `pandas` — Data processing
- `python-dateutil` — Parsing and comparing timestamps

Install with:

```bash
pip install requests pandas python-dateutil
```

## Sample Code (Zenodo Extraction)

```python
import requests
from datetime import datetime

# Load last update timestamp
with open("last_updated.txt", "r") as f:
    last_updated = f.read().strip()

# Query Zenodo for new records
url = f"https://zenodo.org/api/records/?q=artificial+intelligence&sort=mostrecent&size=100&updated:>{last_updated}"
response = requests.get(url)
data = response.json()

# Update timestamp
now = datetime.utcnow().isoformat() + "Z"
with open("last_updated.txt", "w") as f:
    f.write(now)

# Process records
for record in data['hits']['hits']:
    title = record['metadata']['title']
    authors = [creator['name'] for creator in record['metadata'].get('creators', [])]
    print(title, authors)
```

## Visualisation Approach

- **Bubble or network chart** using D3.js
- Nodes represent research topics or keywords
- Demographic breakdown shown via colour or pie segments
- Avoid stereotypical colours (e.g., no pink/blue for gender)
- Filters by demographic group, keyword, publication date
- Tooltips include author list and links to original papers

## Manual Data Correction

Use a CSV or lightweight database with these fields:

- `full_name`
- `corrected_gender`
- `corrected_origin`
- `notes`

Corrections are merged with inferred data and flagged for clarity.

## Ethical Considerations

Name-based inference is probabilistic, approximate, and cannot capture the full spectrum of lived identity. This project does not aim to categorise individuals but to expose systemic imbalances in recognition and representation. Manual correction is supported. Unknown or uncertain values should always be marked clearly, never assumed.

## Next Steps

- Finalise core keywords for AI paper search
- Build data extraction script for Zenodo
- Integrate with gender and origin APIs
- Add manual override system
- Begin initial D3.js visualisations
- Add script for timestamp-aware incremental updates

## Licence

This project is released under the MIT Licence. Data is used solely for critical analysis and educational purposes.
