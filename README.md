# Honorius Neogy

I maintain **gp-omm-conformance**, a test corpus for the satellite-catalog migration. On 11 July 2026 the catalog passed 99,999 objects, the most a TLE's five-digit field can hold, and provider data now carries six-digit, nine-digit and lettered (Alpha-5) catalog numbers; libraries reading them may not expect these forms. The corpus holds seventeen cases built from real CelesTrak data, with no invented element sets, plus a runner and adapters for testing your own parser.

So far: eight libraries run against all seventeen cases, eleven reports filed with ten projects, and one fix merged into python-sgp4, not yet in a release.

- Results and a live tracker: https://gpconf.neogy.dev
- The corpus: https://github.com/hneogy/gp-omm-conformance, concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654)
- https://neogy.dev

NEOGY LLC, orbital data tooling.
