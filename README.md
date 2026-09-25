# Honorius Neogy

Orbital data tooling · NEOGY LLC

[![neogy.dev](https://img.shields.io/badge/neogy.dev-website-0B5FFF?style=flat-square)](https://neogy.dev)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0007--4516--7727-A6CE39?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/0009-0007-4516-7727)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-honorius--neogy-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/honorius-neogy)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.22867654-1682D4?style=flat-square)](https://doi.org/10.5281/zenodo.22867654)
[![corpus CI](https://img.shields.io/github/actions/workflow/status/hneogy/gp-omm-conformance/ci.yml?branch=main&style=flat-square&label=corpus%20CI)](https://github.com/hneogy/gp-omm-conformance/actions/workflows/ci.yml)

## The problem, and the corpus

On 11 July 2026 the satellite catalog passed 99,999 objects, the most a TLE's five-digit field can hold; provider data now carries six-digit, nine-digit and lettered (Alpha-5) catalog numbers, and libraries reading them may not expect these forms. **[gp-omm-conformance](https://github.com/hneogy/gp-omm-conformance)** is a test corpus for that migration: seventeen cases built from real CelesTrak data with no invented element sets, a runner, and adapters for testing your own parser.

## Eight libraries against the corpus

Each run by hand against all seventeen cases on 2026-09-24, at the version named, and every finding reproduced on the library's own code before it was reported. These are results against a specific version on a specific date, not verdicts on the projects; six of the eight read only TLE, and CelesTrak's TLE feed omits the objects that trigger these failures, so their users are not affected today.

| Library | Version run | Report | What the corpus found |
|---|---|---|---|
| PyEphem | 4.2.1 | [#296](https://github.com/brandon-rhodes/pyephem/issues/296) | Five-digit sets exact, epoch within a microsecond; Alpha-5 fields read as catalog number 0, silently |
| satellite.js | 7.1.0 | [#185](https://github.com/shashwatak/satellite-js/issues/185) | All 604 TLE records exact, nine-digit OMM ids accepted; OMM JSON epochs truncated to milliseconds, the TLE catalog field left as a string |
| Gpredict | 2.6 | [#426](https://github.com/csete/gpredict/issues/426) | Five-digit ids right, every element but one exact; Alpha-5 fields become 0, and the mean motion loses its last digit on every record |
| gods-eye-view | main ce671ce | [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751) | Five-digit ids right, 1998 epoch pivots; Alpha-5 satellites collapse onto one entry keyed NaN, the rest dropped |
| SatDump | 1.2.2 and master | [#1221](https://github.com/SatDump/SatDump/issues/1221) | Five-digit sets exact, six-digit CSV ids on master; Alpha-5 sets dropped silently, SupGP CSV rejected whole on master |
| libsgp4 | master and PR #42 | [#45](https://github.com/dnwrnr/sgp4/issues/45), [PR #42](https://github.com/dnwrnr/sgp4/pull/42#issuecomment-5824123874) | Five-digit sets exact but for an 8 µs epoch rounding, CSV takes six-digit ids; master refuses every Alpha-5 set, the PR's letter table drops X and shifts Y and Z |
| tle.js | 5.0.3 | [#62](https://github.com/davidcalhoun/tle.js/issues/62) | Five-digit sets exact, checksums count letters as 0; Alpha-5 fields give NaN with no error, two-digit years pivot at 50 |
| astroz | main d558933 | [#97](https://github.com/ATTron/astroz/issues/97), [#98](https://github.com/ATTron/astroz/issues/98) | Carried elements exact, nine-digit JSON ids as integers; Alpha-5 decoded without skipping I and O, public epoch field off by hundreds of days |

## What has landed

- One fix merged upstream: python-sgp4 [PR #172](https://github.com/brandon-rhodes/python-sgp4/pull/172), the empty `OBJECT_ID` in OMM XML, merged 2026-09-24 and not yet in a release.
- Eleven reports filed with ten projects, the eight above plus python-sgp4 and strf, astroz accounting for two, each stating what was run and how to reproduce it.
- Corpus v0.2.1 archived on Zenodo: concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654), version DOI [10.5281/zenodo.22926017](https://doi.org/10.5281/zenodo.22926017).

## Quick start

```bash
git clone https://github.com/hneogy/gp-omm-conformance.git && cd gp-omm-conformance
python3 tools/fetch.py                                              # once; fetches the fixtures under CelesTrak's usage policy
python3 -m gpconf run --adapter tests.adapters.reference:Parser    # the control
python3 -m gpconf run --adapter mypkg.gpconf_adapter:Parser --json report.json   # your parser
```

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [the corpus](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
