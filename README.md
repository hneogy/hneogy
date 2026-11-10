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

20 libraries so far, each run at the version and on the date named, and every finding reproduced on the library's own code before it was reported. These are results for a specific version on a specific date, not verdicts on the projects. The status after each finding is as of 2026-11-10. Many of the first findings are Alpha-5 TLE failures, but CelesTrak emits no Alpha-5 at all, so users who take their TLEs from CelesTrak don't meet those failures today.

**PyEphem 4.2.1, 2026-09-24**
- Alpha-5 fields read as catalog number 0, silently: [#296](https://github.com/brandon-rhodes/pyephem/issues/296), open.
- a letter in the epoch field accepted and the epoch read up to it: [#297](https://github.com/brandon-rhodes/pyephem/issues/297), closed by the maintainer, no change linked.

**satellite.js 7.1.0, 2026-09-24**
- OMM JSON epochs truncated to the millisecond: [#185](https://github.com/shashwatak/satellite-js/issues/185), [PR #186](https://github.com/shashwatak/satellite-js/pull/186), my PR merged 2026-09-26, not yet released.
- no decoder for Alpha-5 catalog fields (TLE ids come back as strings): [PR #187](https://github.com/shashwatak/satellite-js/pull/187), my PR merged 2026-09-28, not yet released.
- a stray letter in a TLE field read with no error: [#190](https://github.com/shashwatak/satellite-js/issues/190), [PR #197](https://github.com/shashwatak/satellite-js/pull/197), my PR merged 2026-10-14, not yet released.

**Gpredict 2.6, 2026-09-24**
- the mean motion lost its last digit on every record: [#426](https://github.com/csete/gpredict/issues/426), [PR #428](https://github.com/csete/gpredict/pull/428), my PR, released in v2.7.
- a letter in the epoch field accepted: [#427](https://github.com/csete/gpredict/issues/427), [PR #430](https://github.com/csete/gpredict/pull/430), fixed by Wahaj18, released in v2.7.

**gods-eye-view main ce671ce, 2026-09-24**
- Alpha-5 satellites collapsed onto one entry keyed NaN, the rest dropped: [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751), [PR #767](https://github.com/bilawalsidhu/gods-eye-view/pull/767), my PR, released in v0.3.0.
- a set missing a line dropped the rest of its group: [#906](https://github.com/bilawalsidhu/gods-eye-view/issues/906), [PR #936](https://github.com/bilawalsidhu/gods-eye-view/pull/936), fixed by xiehuanyi, released in v0.3.0.

**SatDump 1.2.2 and master f3d82ad, 2026-09-24**
- Alpha-5 sets dropped silently; SupGP CSV rejected whole on master: [#1221](https://github.com/SatDump/SatDump/issues/1221), closed by me after the maintainers said both parts were already planned.

**libsgp4 master 661e057 (2026-09-24) and v3.0 (2026-09-27)**
- Alpha-5 fields refused (results posted on the open PR): [PR #42](https://github.com/dnwrnr/sgp4/pull/42), [PR #46](https://github.com/dnwrnr/sgp4/pull/46), fixed by the maintainer's PR #46, released in v3.0.
- epoch rounded by 8 µs; `FromCsv` reads stray microseconds: [#45](https://github.com/dnwrnr/sgp4/issues/45), open.

**tle.js 5.0.3, 2026-09-24**
- Alpha-5 fields give NaN; two-digit years pivot at 50: [#62](https://github.com/davidcalhoun/tle.js/issues/62), [PR #64](https://github.com/davidcalhoun/tle.js/pull/64), my PR, released in v6.0.0.
- `getEpochDay()` reads up to a stray letter and `isValidTLE()` calls the TLE valid: [#63](https://github.com/davidcalhoun/tle.js/issues/63), open.

**astroz main d558933, 2026-09-25**
- Alpha-5 decoded without skipping I and O: [#97](https://github.com/ATTron/astroz/issues/97), fixed by the maintainer, released in v0.13.0.
- `Tle.epoch` off by hundreds of days: [#98](https://github.com/ATTron/astroz/issues/98), fixed by the maintainer, released in v0.13.0.
- seconds passed through a half-precision float, up to 15.5 ms off: [#102](https://github.com/ATTron/astroz/issues/102), fixed by the maintainer, released in v0.14.0.

**python-sgp4 2.27, 2026-09-21 to 2026-11-08**
- an empty `OBJECT_ID` in OMM XML raised `TypeError`: [#171](https://github.com/brandon-rhodes/python-sgp4/issues/171), [PR #172](https://github.com/brandon-rhodes/python-sgp4/pull/172), my PR merged 2026-09-24, not yet released.
- `Satrec.sgp4init` writes I and O as Alpha-5 letters: [#173](https://github.com/brandon-rhodes/python-sgp4/issues/173), open.
- an ephemeris type 4 (SGP4-XP) TLE propagated as plain SGP4: [#178](https://github.com/brandon-rhodes/python-sgp4/issues/178), open.
- line 2's catalog number not checked against line 1's: [#179](https://github.com/brandon-rhodes/python-sgp4/issues/179), open; the maintainer keeps this check out of scope.

**Vallado's SGP4 code in CelesTrak's repository, 4b04ddc**
- `twoline2rv` reads an Alpha-5 field beginning with I or O as a five-digit number: [#172](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/172), open.
- `twoline2rv` returns the element set and revolution numbers with the checksum digit attached: [#174](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/174), open.

**strf 92d2425, 2026-09-23**
- no range check in `number_to_alpha5` above 339999: [#88](https://github.com/cbassa/strf/issues/88), [PR #89](https://github.com/cbassa/strf/pull/89), open; I tested the open fix, PR #89 by ppapadeas.

**satvis 71e1e3b, 2026-11-08**
- `ommToTle` writes element set 999 for `ELEMENT_SET_NO` 0: [#84](https://github.com/anttikuosmanen-rgb/satvis/issues/84), [PR #85](https://github.com/anttikuosmanen-rgb/satvis/pull/85), fixed by the maintainer on master; the project has no releases.

**Basilisk develop 016955f, 2026-11-08**
- `satOmm2elem` skipped CelesTrak's KVN (`MEAN_ELEMENT_THEORY = SGP/SGP4`): [#1673](https://github.com/AVSLab/basilisk/issues/1673), [PR #1675](https://github.com/AVSLab/basilisk/pull/1675), fix PR #1675 by schaubh open; I tested it.

**satkit 0.24.2, 2026-11-08**
- `TLE.from_lines` drops a whole file for one bad set: [#286](https://github.com/ssmichael1/satkit/issues/286), open.

**SatelliteKit 2.1.2, 2026-10-06 to 2026-11-09**
- B* read from a TLE as e-53 instead of e-05: [#16](https://github.com/gavineadie/SatelliteKit/issues/16), [PR #17](https://github.com/gavineadie/SatelliteKit/pull/17), open; fix PR #17 by djsincla open.
- deep-space Lyddane check on `abs(inclination)` where Vallado's is signed: [#24](https://github.com/gavineadie/SatelliteKit/issues/24), open.

**SGPKit (swift-sgp4) 3.1.0 on 2026-10-06 and 4.4.0 on 2026-11-10**
- satellites numbered 100000 and up refused (3.1.0): [#4](https://github.com/csanfilippo/swift-sgp4/issues/4), fixed by the maintainer, released in 4.1.0.
- `TLEDecoder.decodeCollection` drops every TLE in a file for one bad set (4.4.0): [#16](https://github.com/csanfilippo/swift-sgp4/issues/16), open.

**SGP.NET 1.6.0, 2026-11-10**
- Alpha-5 TLEs refused (an open feature request; my repro, and my PR): [#35](https://github.com/parzivail/SGP.NET/issues/35), [PR #47](https://github.com/parzivail/SGP.NET/pull/47), open.

**brahe 1.8.1, 2026-11-10**
- `OMM.from_str` refuses CelesTrak's ndm-wrapped XML and KVN with an empty CREATION_DATE; `parse_norad_id("A+123")` accepted; Alpha-5 encoding below 100000 unpadded: [#620](https://github.com/duncaneddy/brahe/issues/620), open.

**akhenakh/sgp4 v1.0.2, 2026-11-10**
- Alpha-5 catalog numbers refused; a type 4 TLE propagated as SGP4: [#16](https://github.com/akhenakh/sgp4/issues/16), fixed by the maintainer, released in v1.0.4.

**sgp4 crate (Rust) 2.4.0, 2026-11-10**
- a type 4 (SGP4-XP) TLE propagated as plain SGP4: [#23](https://github.com/neuromorphicsystems/sgp4/issues/23), open.

## What has landed

Every report, with who fixed what and which release has it, is on one page: **[the upstream record](https://gpconf.neogy.dev/upstream/)**. The short version, as of 2026-11-10:

- 35 findings across 20 libraries.
- 11 fixed and in a release.
- 5 fixed and merged, not yet in a release.
- 17 open.
- 2 ended another way: one closed by the maintainer without a change, and one I closed after the maintainers said both parts were already planned.

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

[GPKit](https://github.com/hneogy/GPKit) reads GP data — OMM and TLE, six-digit catalog numbers included — and passes gpconf. v1.0.0, MIT, on the [Swift Package Index](https://swiftpackageindex.com/hneogy/GPKit). Reports to the other Swift readers are listed above, under SatelliteKit and SGPKit.

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [gpconf](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
