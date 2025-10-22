# GitHub Pages Artifact Validator

## Summary
This repository provides a minimal, production-ready web page (index.html) that links to and validates a specific set of project files required by automated tests. The page focuses on functionality and content verification over UI polish. It also supports validating a remote GitHub Pages deployment via a query parameter.

Required assets this page expects:
- ashravan.txt — 300–400 word story about Ashravan after Shai restores him
- dilemma.json — AV moral dilemma with a specific schema
- about.md — exactly three words
- pelican.svg — valid SVG of a pelican riding a bicycle; rated by a built-in, offline “LLM-style” heuristic
- restaurant.json — a Delhi restaurant recommendation with consistent data
- prediction.json — Fed Funds rate prediction for December 2025
- LICENSE — MIT License text
- uid.txt — must match the provided attachment

The page:
- Links to each file directly
- Validates file presence and content
- Displays a simple SVG preview and heuristic rating for pelican.svg
- Handles the query parameter ?url=... to validate assets hosted elsewhere (e.g., another GitHub Pages site)

## Setup
You can use this repository as-is or integrate index.html into your own project.

- GitHub Pages:
  - Push index.html (and your required files) to the default branch of your repository.
  - In your repository settings, enable GitHub Pages (Pages → Build and deployment → Deploy from branch).
  - Your site will be available at: https://<your-user>.github.io/<your-repo>/

- Local testing:
  - You can open index.html directly in a modern browser. Some validations fetch files over HTTP; for full functionality, serve via a local server or load via your GitHub Pages URL.

No external dependencies are required.

## Usage
- Open index.html on GitHub Pages. The page will:
  - Detect the current site as the base URL and run all checks automatically.
  - Show links to each required file.
  - Display pass/warn/fail status and details for every requirement.

- Validating another site with ?url
  - Append ?url=<base> to the index page to validate a remote deployment.
  - Example:
    - https://your-user.github.io/your-repo/index.html?url=https%3A%2F%2Fanother-user.github.io%2Fanother-repo%2F
  - The app will fetch and validate files from that base URL.
  - You can also use the input field at the top of the page to set any base URL and re-run checks.

## Code Explanation
index.html is fully self-contained and includes:
- A list of required files and per-file validators implemented in plain JavaScript
- A simple UI to set the base URL and re-run validation
- A viewer and heuristic “LLM-style” rater for pelican.svg

Validators implemented:
- ashravan.txt
  - Counts words and verifies presence of “Ashravan” and “Shai”
  - Expects 300–400 words
- dilemma.json
  - Validates JSON and schema:
    - { people: 3, case_1: { swerve: boolean, reason: string }, case_2: { swerve: boolean, reason: string } }
- about.md
  - Ensures exactly three words
- pelican.svg
  - Checks for a valid <svg> root and common vector elements (circle/path)
  - Inline preview in the UI
  - Applies a deterministic heuristic rating (1–10) based on presence of vector features and themed keywords; shows verdict and notes
- restaurant.json
  - Ensures fields exist: city, lat, long, name, what_to_eat
  - Confirms city is “Delhi” and latitude/longitude fall roughly within the Delhi region
- prediction.json
  - Requires rate ∈ [0, 1] and a reason length ≥ 20
  - Provides a plausibility hint for 0–10% range (non-blocking warning if outside)
- LICENSE
  - Confirms presence of “MIT License” and the standard permission grant text
- uid.txt
  - Fetches and displays the SHA-256 hash and length so that it can be compared to the provided attachment

The page also:
- Normalizes base URLs (ensures trailing slash)
- Exposes a “Copy shareable link” option that embeds the chosen base as ?url=...

Accessibility and performance:
- Lightweight CSS, no frameworks
- Works without external network requests aside from fetching the target files
- All logic is client-side and modern-browser compatible

## License
This project is provided under the MIT License. See the LICENSE file in your repository for full text.