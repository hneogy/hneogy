# Honorius Neogy

Orbital data tooling · NEOGY LLC

[![neogy.dev](https://img.shields.io/badge/neogy.dev-website-0B5FFF?style=flat-square)](https://neogy.dev)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0007--4516--7727-A6CE39?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/0009-0007-4516-7727)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-honorius--neogy-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/honorius-neogy)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.22867654-1682D4?style=flat-square)](https://doi.org/10.5281/zenodo.22867654)
[![corpus CI](https://img.shields.io/github/actions/workflow/status/hneogy/gp-omm-conformance/ci.yml?branch=main&style=flat-square&label=corpus%20CI)](https://github.com/hneogy/gp-omm-conformance/actions/workflows/ci.yml)

## The satellite catalog passed 99,999. Does your software know?

On 11 July 2026 the US Space Force catalog assigned number 100000 (to the Portuguese CubeSat SARAMAGO) after exhausting the five-digit range, which ends at 69999 (CelesTrak); provider data now carries six-digit and nine-digit catalog numbers, Space-Track serves the lettered (Alpha-5) form, and libraries reading them may not expect these forms.

**[gpconf](https://github.com/hneogy/gp-omm-conformance)** is a free test kit that tells you whether your satellite software handles catalog numbers above 99,999 — built from real CelesTrak data, with every expected answer traced to its source. It is the GP/OMM conformance corpus: eighteen cases with no invented element sets, a runner that installs with `pip install gpconf`, presets that test a library with no adapter written, and a GitHub Action for CI.

## Libraries against gpconf

20 libraries so far, each run at the version and on the date its report names, and every finding reproduced on the library's own code before it was reported. These are results for a specific version on a specific date, not verdicts on the projects. Many of the first findings are Alpha-5 TLE failures, but CelesTrak emits no Alpha-5 at all, so users who take their TLEs from CelesTrak don't meet those failures today. States and releases as of 2026-11-10; the whole record, with who fixed what, is on **[the upstream record](https://gpconf.neogy.dev/upstream/)**.

37 findings: **8 fixed by my pull requests**, **9 fixed by maintainers and contributors after my report**, **17 still open**, and 3 that ended another way.

### Fixed by my pull requests (8)

- **python-sgp4**: an empty `OBJECT_ID` in OMM XML raised `TypeError`. [#171](https://github.com/brandon-rhodes/python-sgp4/issues/171), [PR #172](https://github.com/brandon-rhodes/python-sgp4/pull/172), merged 2026-09-24, not yet released.
- **satellite.js**: OMM JSON epochs truncated to the millisecond. [#185](https://github.com/shashwatak/satellite-js/issues/185), [PR #186](https://github.com/shashwatak/satellite-js/pull/186), merged 2026-09-26, not yet released.
- **satellite.js**: no decoder for Alpha-5 catalog fields. [PR #187](https://github.com/shashwatak/satellite-js/pull/187), merged 2026-09-28, not yet released.
- **satellite.js**: a stray letter in a TLE field read with no error. [#190](https://github.com/shashwatak/satellite-js/issues/190), [PR #197](https://github.com/shashwatak/satellite-js/pull/197), merged 2026-10-14, not yet released.
- **Gpredict**: the mean motion lost its last digit on every record. [#426](https://github.com/csete/gpredict/issues/426), [PR #428](https://github.com/csete/gpredict/pull/428), merged 2026-10-08, released in v2.7.
- **gods-eye-view**: Alpha-5 satellites collapsed onto one entry keyed NaN, the rest dropped. [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751), [PR #767](https://github.com/bilawalsidhu/gods-eye-view/pull/767), merged 2026-10-17, released in v0.3.0.
- **tle.js**: Alpha-5 fields gave NaN; two-digit years pivoted at 50. [#62](https://github.com/davidcalhoun/tle.js/issues/62), [PR #64](https://github.com/davidcalhoun/tle.js/pull/64), merged 2026-10-19, released in v6.0.0.
- **SGP.NET**: Alpha-5 TLEs refused. [#35](https://github.com/parzivail/SGP.NET/issues/35), [PR #47](https://github.com/parzivail/SGP.NET/pull/47), merged 2026-11-10, not yet released.

### Fixed by maintainers and contributors after my report (9)

- **astroz**: Alpha-5 decoded without skipping I and O. [#97](https://github.com/ATTron/astroz/issues/97), [PR #99](https://github.com/ATTron/astroz/pull/99), fixed by ATTron, the maintainer, released in v0.13.0.
- **astroz**: `Tle.epoch` off by hundreds of days. [#98](https://github.com/ATTron/astroz/issues/98), [PR #99](https://github.com/ATTron/astroz/pull/99), fixed by ATTron, the maintainer, released in v0.13.0.
- **astroz**: seconds passed through a half-precision float, up to 15.5 ms off. [#102](https://github.com/ATTron/astroz/issues/102), [PR #104](https://github.com/ATTron/astroz/pull/104), fixed by ATTron, the maintainer, released in v0.14.0.
- **libsgp4**: every Alpha-5 field refused (results posted on the open PR). [PR #42](https://github.com/dnwrnr/sgp4/pull/42), [PR #46](https://github.com/dnwrnr/sgp4/pull/46), fixed by dnwrnr, the maintainer, released in v3.0.
- **Gpredict**: a letter in the epoch field accepted. [#427](https://github.com/csete/gpredict/issues/427), [PR #430](https://github.com/csete/gpredict/pull/430), fixed by Wahaj18, a contributor, released in v2.7.
- **gods-eye-view**: a set missing a line dropped the rest of its group. [#906](https://github.com/bilawalsidhu/gods-eye-view/issues/906), [PR #936](https://github.com/bilawalsidhu/gods-eye-view/pull/936), fixed by xiehuanyi, a contributor, released in v0.3.0.
- **SGPKit (swift-sgp4)**: satellites numbered 100000 and up refused. [#4](https://github.com/csanfilippo/swift-sgp4/issues/4), fixed by csanfilippo, the maintainer, released in 4.1.0.
- **satvis**: `ommToTle` wrote element set 999 for `ELEMENT_SET_NO` 0. [#84](https://github.com/anttikuosmanen-rgb/satvis/issues/84), [PR #85](https://github.com/anttikuosmanen-rgb/satvis/pull/85), fixed by anttikuosmanen-rgb, the maintainer, on master; the project has no releases.
- **akhenakh/sgp4**: Alpha-5 catalog numbers refused; a type 4 TLE propagated as SGP4. [#16](https://github.com/akhenakh/sgp4/issues/16), fixed by Fabrice Aneche (akhenakh), the maintainer, released in v1.0.4.

### Reported, still open (17)

- **PyEphem**: Alpha-5 fields read as catalog number 0, silently. [#296](https://github.com/brandon-rhodes/pyephem/issues/296), open.
- **python-sgp4**: `Satrec.sgp4init` writes I and O as Alpha-5 letters. [#173](https://github.com/brandon-rhodes/python-sgp4/issues/173), open.
- **python-sgp4**: a type 4 (SGP4-XP) TLE propagated as plain SGP4. [#178](https://github.com/brandon-rhodes/python-sgp4/issues/178), open.
- **python-sgp4**: line 2's catalog number not checked against line 1's. [#179](https://github.com/brandon-rhodes/python-sgp4/issues/179), open; the maintainer keeps this check out of scope.
- **Vallado's SGP4 code (CelesTrak's repository)**: `twoline2rv` reads an Alpha-5 field beginning with I or O as a five-digit number. [#172](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/172), open.
- **Vallado's SGP4 code (CelesTrak's repository)**: `twoline2rv` returns the element set and revolution numbers with the checksum digit attached. [#174](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/174), open.
- **strf**: no range check in `number_to_alpha5` above 339999. [#88](https://github.com/cbassa/strf/issues/88), [PR #89](https://github.com/cbassa/strf/pull/89), open; a fix by ppapadeas in review.
- **libsgp4**: epoch rounded by 8 µs; `FromCsv` reads stray microseconds. [#45](https://github.com/dnwrnr/sgp4/issues/45), open.
- **tle.js**: `getEpochDay()` reads up to a stray letter and `isValidTLE()` calls the TLE valid. [#63](https://github.com/davidcalhoun/tle.js/issues/63), open.
- **Basilisk**: `satOmm2elem` skipped CelesTrak's KVN (`MEAN_ELEMENT_THEORY = SGP/SGP4`). [#1673](https://github.com/AVSLab/basilisk/issues/1673), [PR #1675](https://github.com/AVSLab/basilisk/pull/1675), open; a fix by schaubh in review.
- **satkit**: `TLE.from_lines` drops a whole file for one bad set. [#286](https://github.com/ssmichael1/satkit/issues/286), open.
- **SatelliteKit**: B* read from a TLE as e-53 instead of e-05. [#16](https://github.com/gavineadie/SatelliteKit/issues/16), [PR #17](https://github.com/gavineadie/SatelliteKit/pull/17), open; a fix by djsincla in review.
- **SatelliteKit**: deep-space Lyddane check on `abs(inclination)` where Vallado's is signed. [#24](https://github.com/gavineadie/SatelliteKit/issues/24), open.
- **SGPKit (swift-sgp4)**: `TLEDecoder.decodeCollection` drops every TLE in a file for one bad set. [#16](https://github.com/csanfilippo/swift-sgp4/issues/16), open.
- **brahe**: `OMM.from_str` refuses CelesTrak's ndm-wrapped XML and KVN with an empty CREATION_DATE; `parse_norad_id("A+123")` accepted; Alpha-5 encoding below 100000 unpadded. [#620](https://github.com/duncaneddy/brahe/issues/620), open.
- **sgp4 crate (Rust)**: a type 4 (SGP4-XP) TLE propagated as plain SGP4. [#23](https://github.com/neuromorphicsystems/sgp4/issues/23), open.
- **SGP.NET**: `OmmKvnParser` refuses a day-of-year EPOCH and a unit in brackets, two forms CCSDS 502.0-B-3 allows. [#48](https://github.com/parzivail/SGP.NET/issues/48), open.

### Ended another way (3)

- **PyEphem**: a letter in the epoch field accepted and the epoch read up to it. [#297](https://github.com/brandon-rhodes/pyephem/issues/297), closed by the maintainer without a change.
- **SatDump**: Alpha-5 sets dropped silently; SupGP CSV rejected whole on master. [#1221](https://github.com/SatDump/SatDump/issues/1221), closed by me after the maintainers said both parts were already planned.
- **python-sgp4**: nine-digit NORAD_CAT_ID refused by the OMM reader: someone else's report (michaelbaisch), where I tested the open pull request. [#169](https://github.com/brandon-rhodes/python-sgp4/issues/169), open.

### Per library

| library | fixed by my PR | fixed by others | open | other |
|---|---|---|---|---|
| akhenakh/sgp4 |  | 1 |  |  |
| astroz |  | 3 |  |  |
| Basilisk |  |  | 1 |  |
| brahe |  |  | 1 |  |
| gods-eye-view | 1 | 1 |  |  |
| Gpredict | 1 | 1 |  |  |
| libsgp4 |  | 1 | 1 |  |
| PyEphem |  |  | 1 | 1 |
| python-sgp4 | 1 |  | 3 | 1 |
| SatDump |  |  |  | 1 |
| satellite.js | 3 |  |  |  |
| SatelliteKit |  |  | 2 |  |
| satkit |  |  | 1 |  |
| satvis |  | 1 |  |  |
| SGP.NET | 1 |  | 1 |  |
| sgp4 crate (Rust) |  |  | 1 |  |
| SGPKit (swift-sgp4) |  | 1 | 1 |  |
| strf |  |  | 1 |  |
| tle.js | 1 |  | 1 |  |
| Vallado's SGP4 code (CelesTrak's repository) |  |  | 2 |  |

## Releases

[gpconf](https://github.com/hneogy/gp-omm-conformance) is at v0.8.0, on [PyPI](https://pypi.org/project/gpconf/) and archived on Zenodo under the concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654), which always resolves to the latest release; 0.8.0's own is [10.5281/zenodo.23853937](https://doi.org/10.5281/zenodo.23853937). From 0.7.0 it is under Apache-2.0; releases up to 0.6.2 stay MIT. Since it became installable with pip at 0.3.0:

- 0.4.0 added the eighteenth case, the first to hand a parser corrupt input.
- 0.5.0 let an adapter say "unsupported" for an operation it does not have, counted each record once, and made the package builds reproducible.
- 0.5.1 cut a user's fetch from 61 requests and 12.5 MB to 50 requests and 3.2 MB, and made it stop at any response it does not expect.
- 0.6.0 gave the JSON report a summary a script can rely on, took the name gpconf, and cut the fetch to 46 requests.
- 0.6.1 made the fetch name the kit and its version in its `User-Agent`.
- 0.6.2 fixed two things an audit found: a mistyped case name passed silently, and `check-tle` on a missing file printed a traceback.
- 0.7.0 is about the grader itself. SM-26 answered [my audit ask](https://github.com/hneogy/gp-omm-conformance/issues/1) with seven wrong-on-purpose parsers that passed 0.6.1, and two blind audits wrote twenty-one more. What got through is closed, all of them are now tests, and more is graded before any fetch. No published case verdict moved.
- 0.8.0 grades every copy of a catalog number: when a file holds the same id more than once, each copy a parser returns is checked against the record it matches, so a parser that returns the last copy twice no longer passes. Before the release I ran a mutation test of the grading code; the tests caught 75.5% of the changes, and the survivors are listed for 0.8.1. One published verdict moved: python-sgp4's mean-motion derivative case now passes within tolerance.

## Quick start

```bash
pip install gpconf
gpconf fetch                                                  # once; fetches the fixtures under CelesTrak's usage policy
gpconf run --preset reference                                 # the control
gpconf presets                                                # libraries testable with no adapter written
gpconf run --adapter mypkg.gpconf_adapter:Parser --json report.json   # your parser
```

## GPKit, for Swift

[GPKit](https://github.com/hneogy/GPKit) reads GP data — OMM and TLE, six-digit catalog numbers included — and passes gpconf: 17 of its 18 cases, none failing (the eighteenth checks data and runs no parser). [v1.3.0](https://github.com/hneogy/GPKit/releases/tag/v1.3.0), MIT, on the [Swift Package Index](https://swiftpackageindex.com/hneogy/GPKit). Each release also carries a playground module, GPKit built for WebAssembly, held by CI to the native build's answers; it runs [the playground](https://gpkit.gppasses.com/playground/), a page that reads a TLE or an OMM and works out the next day's passes in the browser. Reports to the other Swift readers are listed above, under SatelliteKit and SGPKit.

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [gpconf](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
