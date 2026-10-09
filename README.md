# Honorius Neogy

Orbital data tooling · NEOGY LLC

[![neogy.dev](https://img.shields.io/badge/neogy.dev-website-0B5FFF?style=flat-square)](https://neogy.dev)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0007--4516--7727-A6CE39?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/0009-0007-4516-7727)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-honorius--neogy-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/honorius-neogy)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.22867654-1682D4?style=flat-square)](https://doi.org/10.5281/zenodo.22867654)
[![corpus CI](https://img.shields.io/github/actions/workflow/status/hneogy/gp-omm-conformance/ci.yml?branch=main&style=flat-square&label=corpus%20CI)](https://github.com/hneogy/gp-omm-conformance/actions/workflows/ci.yml)

## The satellite catalog passed 99,999. Does your software know?

On 11 July 2026 the US Space Force catalog assigned number 100000 (to the Portuguese CubeSat SARAMAGO) after exhausting the five-digit range, which ends at 69999 (CelesTrak); provider data now carries six-digit, nine-digit and lettered (Alpha-5) catalog numbers, and libraries reading them may not expect these forms.

**[gpconf](https://github.com/hneogy/gp-omm-conformance)** is a free test kit that tells you whether your satellite software handles catalog numbers above 99,999 — built from real CelesTrak data, with every expected answer traced to its source. It is the GP/OMM conformance corpus: eighteen cases with no invented element sets, a runner that installs with `pip install gpconf`, presets that test a library with no adapter written, and a GitHub Action for CI.

## Eight libraries against gpconf

Each run by hand at the version named, six on 2026-09-24 against the seventeen cases of the time and libsgp4 and astroz on 2026-09-27 against v0.4.0's eighteen, and every finding reproduced on the library's own code before it was reported. These are results against a specific version on a specific date, not verdicts on the projects. All eight failed on Alpha-5 TLEs when first run, on 2026-09-24, but CelesTrak emits no Alpha-5 at all, so users who take their TLEs from CelesTrak don't meet those failures today.

> **PyEphem 4.2.1** — [#296](https://github.com/brandon-rhodes/pyephem/issues/296)  
> Five-digit sets exact, epoch within a microsecond.  
> Alpha-5 fields read as catalog number 0, silently.

> **satellite.js 7.1.0** — [#185](https://github.com/shashwatak/satellite-js/issues/185), [PR #186](https://github.com/shashwatak/satellite-js/pull/186), [PR #187](https://github.com/shashwatak/satellite-js/pull/187)  
> All 604 five-digit TLE sets exact, nine-digit OMM ids accepted.  
> OMM JSON epochs truncated to ms (#186) and TLE ids as strings (#187), both merged, in no release yet.

> **Gpredict 2.6** — [#426](https://github.com/csete/gpredict/issues/426), [PR #428](https://github.com/csete/gpredict/pull/428)  
> Five-digit ids right, every element but one exact.  
> Alpha-5 fields become 0; the mean motion lost its last digit on every record — fixed by PR #428, merged 2026-10-08, in no release yet.

> **gods-eye-view main ce671ce** — [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751), [PR #767](https://github.com/bilawalsidhu/gods-eye-view/pull/767), [#906](https://github.com/bilawalsidhu/gods-eye-view/issues/906)  
> Five-digit ids right, 1998 epoch pivots.  
> Alpha-5 satellites collapse onto one entry keyed NaN; the rest dropped (#767 open). A set missing a line dropped the rest of its group (#906).

> **SatDump 1.2.2 and master** — [#1221](https://github.com/SatDump/SatDump/issues/1221)  
> Five-digit sets exact, six-digit CSV ids on master.  
> Alpha-5 sets dropped silently; SupGP CSV rejected whole on master.

> **libsgp4 v3.0** — [#45](https://github.com/dnwrnr/sgp4/issues/45), [#44](https://github.com/dnwrnr/sgp4/issues/44#issuecomment-5848881779)  
> Five-digit sets exact but for an 8 µs epoch rounding, CSV takes six-digit ids.  
> Alpha-5 decoded since v3.0; SupGP CSV refused whole, #45's rounding unresolved.

> **tle.js 5.0.3** — [#62](https://github.com/davidcalhoun/tle.js/issues/62), [PR #64](https://github.com/davidcalhoun/tle.js/pull/64)  
> Five-digit sets exact, checksums count letters as 0.  
> Alpha-5 fields give NaN with no error; two-digit years pivot at 50. A fix for both is up as PR #64.

> **astroz v0.14.0** — [#97](https://github.com/ATTron/astroz/issues/97), [#98](https://github.com/ATTron/astroz/issues/98), [#102](https://github.com/ATTron/astroz/issues/102)  
> Carried elements exact, nine-digit JSON ids as integers.  
> Alpha-5 (#97) and epoch (#98) fixed in v0.13.0, seconds (#102) in v0.14.0; now MIT.

## What has landed

- Six fixes merged upstream, four of them pull requests from this account: python-sgp4 [PR #172](https://github.com/brandon-rhodes/python-sgp4/pull/172) (the empty `OBJECT_ID` in OMM XML), satellite.js [PR #186](https://github.com/shashwatak/satellite-js/pull/186) (OMM epochs kept to the microsecond) and [PR #187](https://github.com/shashwatak/satellite-js/pull/187) (an Alpha-5 decoder), and Gpredict [PR #428](https://github.com/csete/gpredict/pull/428) (the mean motion's lost digit); plus astroz's own [PR #99](https://github.com/ATTron/astroz/pull/99), released in v0.13.0, and libsgp4's own [#46](https://github.com/dnwrnr/sgp4/pull/46), released in v3.0, two days after the corpus's results on [PR #42](https://github.com/dnwrnr/sgp4/pull/42#issuecomment-5824123874). None of the four from this account is in a release yet.
- Open right now: tle.js [PR #64](https://github.com/davidcalhoun/tle.js/pull/64); with CelesTrak's code repository, [#172](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/172) (the Alpha-5 letters I and O decode as numbers) and [#174](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/174) (`elnum` and `revnum` come back with the checksum digit on the end); and gods-eye-view [#906](https://github.com/bilawalsidhu/gods-eye-view/issues/906), where another contributor has a reference fix up, [kvnloo#167](https://github.com/kvnloo/gods-eye-view/pull/167), crediting the report.
- Nineteen reports filed with eleven projects, the eight above plus python-sgp4, strf and CelesTrak's fundamentals-of-astrodynamics, astroz accounting for three, each stating what was run and how to reproduce it.
- gpconf v0.6.1 released, on [PyPI](https://pypi.org/project/gpconf/) and archived on Zenodo: concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654), version DOI [10.5281/zenodo.23144821](https://doi.org/10.5281/zenodo.23144821).

## Quick start

```bash
pip install gpconf
gpconf fetch                                                  # once; fetches the fixtures under CelesTrak's usage policy
gpconf run --preset reference                                 # the control
gpconf presets                                                # libraries testable with no adapter written
gpconf run --adapter mypkg.gpconf_adapter:Parser --json report.json   # your parser
```

## GPKit, for Swift

[GPKit](https://github.com/hneogy/GPKit) reads GP data — OMM and TLE, six-digit catalog numbers included — and passes gpconf. v1.0.0, MIT, on the [Swift Package Index](https://swiftpackageindex.com/hneogy/GPKit). Two reports to the Swift readers are open: [SatelliteKit #16](https://github.com/gavineadie/SatelliteKit/issues/16) (B* read 48 orders of magnitude too small) and [swift-sgp4 #4](https://github.com/csanfilippo/swift-sgp4/issues/4) (satellites from 100000 up refused).

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [gpconf](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
