# Assignment 5: Working Guide

## Our question

How do mentions of **private amenities** and **shared amenities** vary across Manhattan apartment listings? We use the tutorial's StreetEasy descriptions, classify them with Qwen, and map the results.

## Definitions

- **Private amenities:** in-unit laundry, a dishwasher, air conditioning, private outdoor space, or explicitly private storage, parking, gym or pool. Ordinary kitchens, bathrooms, closets, heating, finishes and unspecified appliances do not count.
- **Shared amenities:** building services and facilities, including doormen, elevators, communal laundry, parking, gyms, pools, lounges, roof decks, storage, and shared gardens (including vegetable gardens).
- Access matters: a garden exclusive to the unit is private; a residents' garden is shared. Nearby public facilities and unavailable features do not count. Paid building amenities do count.

## Four labels

| Label | Meaning |
|---|---|
| 0 | Neither mentioned |
| 1 | Private amenities only |
| 2 | Shared amenities only |
| 3 | Both mentioned |

Qwen checks the two features with separate prompts, selecting an allowed amenity type or none. It supplies supporting text for review; this text can be paraphrased and should be checked against the original listing. Python calculates `int(private_amenities) + 2 * int(shared_building)`. Labels describe mentions, not confirmed availability, and are not rankings.

## Workflow

1. Define the features and write the prompt in `assignment-5.ipynb`.
2. Check Qwen against manually read sample descriptions.
3. Classify all listings, caching results against the exact prompt and model settings. A changed prompt requires new classifications.
4. Join listing coordinates to Manhattan neighborhoods and calculate counts and percentages.
5. Export both HTML maps and the neighborhood GeoJSON to `outputs`.
6. Embed the maps beside a concise explanation in `index.html`.

## Reading the maps

The neighborhood map averages color points: label 0 adds grey, label 1 adds green, label 2 adds blue, and label 3 adds both green and blue. White means no valid listings. We average color points, not the numerical category labels.

The apartment map uses the tutorial's CARTO Positron basemap and colors individual dots by category. Its legend can hide or show each category. Street tiles require internet access.

## Outputs and submission

The notebook generates `outputs/listings_classified.csv`, `outputs/classification_prompt.txt`, `outputs/classification_run.json`, `outputs/nta_amenities.geojson`, and both HTML maps. It uses the `tutorial-5` Conda environment and text-only Qwen weights. CPU mode, four threads and disabled model caches reduce resource use; cleanup stops the temporary model process after classification or an ordinary error/interrupt.

## Current status

The notebook, map-generation code and website text use **private vs shared amenities**. The saved notebook records a completed run with no cell errors, and the CSV, GeoJSON and HTML exports use the updated classification. Export cells reject older classifications instead of relabeling them.

Classification responses are retained in the cache; only matching prompts and settings are reused.

The assignment's submission also needs a map image or single-page PDF, the exact prompt, a two-sentence gallery description, and a short reflection. The interactive website and GeoJSON are working outputs.

Assignment brief: https://simmodeltwin.net/assignments/assignment-05/
