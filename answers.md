# api-ethics-assignment
PII classification and Ethics

Task 1  Classify and Handle PII Fields

full_name	    Direct PII	                        Drop or Pseudonymize
email	        Direct PII	                        Drop
date_of_birth	Indirect PII	                    Mask (e.g., year only)
zip_code	    Indirect PII	                    Mask (e.g., first 3 digits) 
job_title	    Indirect PII	                    Keep or Generalize
diagnosis_notes	Sensitive Personal Data (Health)	Pseudonymize + Clean

Task 2 Audit the API Script for Ethical Compliance

Violation 1: Hardcoded API Key (Security Risk)

Problem:
API key is exposed in code which can be misused
Violates most API Terms of Service.

Fix:
Use environment variables instead
#Fix for voilation 1:

```python
import os
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

records = []
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    data = response.json()
    records.extend(data["results"])
```

Violation 2: Bulk Data Scraping Without Limits / Consent

Problem:
Fetching 100 pages blindly may:
Violate API rate limits
Breach Terms of Service
Collect more data than necessary (violates data minimization)
No check for user consent or allowed usage

Fix:
Add:
Rate limiting
Respect API guidelines
Fetch only required data

Fix for voilation 2:

```python
import os
import requests
import time

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

records = []
MAX_PAGES = 20  # Limit to required data

for page in range(1, MAX_PAGES + 1):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})

    if response.status_code != 200:
        break

    data = response.json()
    records.extend(data.get("results", []))

    time.sleep(1)  # Respect API rate limits
```
