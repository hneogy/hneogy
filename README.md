# Honorius Neogy

Orbital data tooling · NEOGY LLC

[![neogy.dev](https://img.shields.io/badge/neogy.dev-website-0B5FFF?style=flat-square)](https://neogy.dev)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0007--4516--7727-A6CE39?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/0009-0007-4516-7727)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-honorius--neogy-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/honorius-neogy)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.22867654-1682D4?style=flat-square)](https://doi.org/10.5281/zenodo.22867654)
[![corpus CI](https://img.shields.io/github/actions/workflow/status/hneogy/gp-omm-conformance/ci.yml?branch=main&style=flat-square&label=corpus%20CI)](https://github.com/hneogy/gp-omm-conformance/actions/workflows/ci.yml)

## The problem, and the corpus

On 11 July 2026 the US Space Force catalog assigned number 100000 (to the Portuguese CubeSat SARAMAGO) after exhausting the five-digit range, which ends at 69999 (CelesTrak); provider data now carries six-digit, nine-digit and lettered (Alpha-5) catalog numbers, and libraries reading them may not expect these forms. **[gp-omm-conformance](https://github.com/hneogy/gp-omm-conformance)** is a test corpus for that migration: seventeen cases built from real CelesTrak data with no invented element sets, a runner that installs with `pip install gpconf`, presets that test a library with no adapter written, and a GitHub Action for CI.

## Eight libraries against the corpus

Each run by hand against all seventeen cases on 2026-09-24, at the version named, and every finding reproduced on the library's own code before it was reported. These are results against a specific version on a specific date, not verdicts on the projects. All eight failed on Alpha-5 TLEs in these runs, but CelesTrak emits no Alpha-5 at all, so users who take their TLEs from CelesTrak don't meet those failures today.

> **PyEphem 4.2.1** — [#296](https://github.com/brandon-rhodes/pyephem/issues/296)  
> Five-digit sets exact, epoch within a microsecond.  
> Alpha-5 fields read as catalog number 0, silently.

> **satellite.js 7.1.0** — [#185](https://github.com/shashwatak/satellite-js/issues/185), [PR #186](https://github.com/shashwatak/satellite-js/pull/186), [PR #187](https://github.com/shashwatak/satellite-js/pull/187)  
> All 604 TLE records exact, nine-digit OMM ids accepted.  
> OMM JSON epochs truncated to ms (#186 merged); TLE ids as strings (#187 open).

> **Gpredict 2.6** — [#426](https://github.com/csete/gpredict/issues/426)  
> Five-digit ids right, every element but one exact.  
> Alpha-5 fields become 0; the mean motion loses its last digit on every record.

> **gods-eye-view main ce671ce** — [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751), [PR #767](https://github.com/bilawalsidhu/gods-eye-view/pull/767)  
> Five-digit ids right, 1998 epoch pivots.  
> Alpha-5 satellites collapse onto one entry keyed NaN; the rest dropped (#767 open).

> **SatDump 1.2.2 and master** — [#1221](https://github.com/SatDump/SatDump/issues/1221)  
> Five-digit sets exact, six-digit CSV ids on master.  
> Alpha-5 sets dropped silently; SupGP CSV rejected whole on master.

> **libsgp4 master 661e057** — [#45](https://github.com/dnwrnr/sgp4/issues/45), [#44](https://github.com/dnwrnr/sgp4/issues/44#issuecomment-5848881779)  
> Five-digit sets exact but for an 8 µs epoch rounding, CSV takes six-digit ids.  
> Every Alpha-5 set refused; v3.0 decodes them correctly, #45's rounding unresolved.

> **tle.js 5.0.3** — [#62](https://github.com/davidcalhoun/tle.js/issues/62)  
> Five-digit sets exact, checksums count letters as 0.  
> Alpha-5 fields give NaN with no error; two-digit years pivot at 50.

> **astroz main d558933** — [#97](https://github.com/ATTron/astroz/issues/97), [#98](https://github.com/ATTron/astroz/issues/98), [#102](https://github.com/ATTron/astroz/issues/102)  
> Carried elements exact, nine-digit JSON ids as integers.  
> Alpha-5 (#97) and epoch (#98) fixed in v0.13.0, seconds (#102) in v0.14.0; now MIT.

## What has landed

- Four fixes merged upstream: two pull requests from this account, python-sgp4 [PR #172](https://github.com/brandon-rhodes/python-sgp4/pull/172) (the empty `OBJECT_ID` in OMM XML) and satellite.js [PR #186](https://github.com/shashwatak/satellite-js/pull/186) (OMM JSON epochs kept to the microsecond), neither yet in a release; and astroz's own [PR #99](https://github.com/ATTron/astroz/pull/99), for #97 and #98, released in v0.13.0, and [PR #104](https://github.com/ATTron/astroz/pull/104), for #102, released in v0.14.0.
- Twelve reports filed with ten projects, the eight above plus python-sgp4 and strf, astroz accounting for three, each stating what was run and how to reproduce it.
- Corpus v0.3.0 released, on PyPI as [gpconf](https://pypi.org/project/gpconf/) and archived on Zenodo: concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654), version DOI [10.5281/zenodo.22986178](https://doi.org/10.5281/zenodo.22986178).

## Quick start

```bash
pip install gpconf
gpconf fetch                                                  # once; fetches the fixtures under CelesTrak's usage policy
gpconf run --preset reference                                 # the control
gpconf presets                                                # libraries testable with no adapter written
gpconf run --adapter mypkg.gpconf_adapter:Parser --json report.json   # your parser
```

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [the corpus](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
