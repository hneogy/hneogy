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

## Eight libraries against gpconf

Each run by hand at the version named, six on 2026-09-24 against the seventeen cases of the time and libsgp4 and astroz on 2026-09-27 against v0.4.0's eighteen, and every finding reproduced on the library's own code before it was reported. These are results against a specific version on a specific date, not verdicts on the projects. All eight failed on Alpha-5 TLEs when first run, on 2026-09-24, but CelesTrak emits no Alpha-5 at all, so users who take their TLEs from CelesTrak don't meet those failures today.

> **PyEphem 4.2.1** — [#296](https://github.com/brandon-rhodes/pyephem/issues/296)  
> Five-digit sets exact, epoch within a microsecond.  
> Alpha-5 fields read as catalog number 0, silently.

> **satellite.js 7.1.0** — [#185](https://github.com/shashwatak/satellite-js/issues/185), [PR #186](https://github.com/shashwatak/satellite-js/pull/186), [PR #187](https://github.com/shashwatak/satellite-js/pull/187), [#190](https://github.com/shashwatak/satellite-js/issues/190), [PR #197](https://github.com/shashwatak/satellite-js/pull/197)  
> All 604 five-digit TLE sets exact, nine-digit OMM ids accepted.  
> OMM JSON epochs truncated to ms (#186) and TLE ids as strings (#187); a stray letter in a TLE field read with no error (#190), fixed by my PR #197. All three merged, in no release yet.

> **Gpredict 2.6** — [#426](https://github.com/csete/gpredict/issues/426), [PR #428](https://github.com/csete/gpredict/pull/428), [#427](https://github.com/csete/gpredict/issues/427), [PR #430](https://github.com/csete/gpredict/pull/430)  
> Five-digit ids right, every element but one exact.  
> Alpha-5 fields become 0. The mean motion lost its last digit on every record: my PR #428 fixed it, merged 2026-10-08. A letter in the epoch field was accepted: Wahaj18 fixed that in PR #430, merged 2026-10-10. Neither is in a release yet.

> **gods-eye-view main ce671ce** — [#751](https://github.com/bilawalsidhu/gods-eye-view/issues/751), [PR #767](https://github.com/bilawalsidhu/gods-eye-view/pull/767), [#906](https://github.com/bilawalsidhu/gods-eye-view/issues/906)  
> Five-digit ids right, 1998 epoch pivots.  
> Alpha-5 satellites collapse onto one entry keyed NaN; the rest dropped (#767 open). A set missing a line dropped the rest of its group (#906): xiehuanyi fixed that in [PR #936](https://github.com/bilawalsidhu/gods-eye-view/pull/936), merged 2026-10-12, in no release yet.

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

Every report, with who fixed what and which release has it, is on one page: **[the upstream record](https://gpconf.neogy.dev/upstream/)**. The short version, as of 2026-10-15:

- 23 findings reported to 12 projects.
- 5 fixed by pull requests of mine that the maintainers merged: python-sgp4 [#172](https://github.com/brandon-rhodes/python-sgp4/pull/172) (an empty `OBJECT_ID` in OMM XML), satellite.js [#186](https://github.com/shashwatak/satellite-js/pull/186) (OMM epochs kept to the microsecond) and [#187](https://github.com/shashwatak/satellite-js/pull/187) (an Alpha-5 decoder), satellite.js [#197](https://github.com/shashwatak/satellite-js/pull/197) (a stray letter in a TLE field read with no error), Gpredict [#428](https://github.com/csete/gpredict/pull/428) (the mean motion's lost digit). None of the five is in a release yet.
- 7 fixed by someone else after I reported them or posted results on their thread. The maintainers fixed five: astroz in [#99](https://github.com/ATTron/astroz/pull/99) (v0.13.0) and [#104](https://github.com/ATTron/astroz/pull/104) (v0.14.0), libsgp4 in [#46](https://github.com/dnwrnr/sgp4/pull/46) (v3.0), and swift-sgp4 in [4.1.0](https://github.com/csanfilippo/swift-sgp4/issues/4#issuecomment-6105448861). Two contributors fixed one each: Wahaj18, Gpredict [#430](https://github.com/csete/gpredict/pull/430), and xiehuanyi, gods-eye-view [#936](https://github.com/bilawalsidhu/gods-eye-view/pull/936).
- 8 open, among them tle.js [PR #64](https://github.com/davidcalhoun/tle.js/pull/64) and python-sgp4 [#173](https://github.com/brandon-rhodes/python-sgp4/issues/173). Two more are open in Dave Vallado's reference code, hosted in CelesTrak's repository, and are not on that page yet: [#172](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/172) and [#174](https://github.com/CelesTrak/fundamentals-of-astrodynamics/issues/174).
- 3 ended another way: one closed by the maintainer without a change, one I closed after the maintainers said both parts were already planned, and one that was someone else's report, where I only tested the open pull request.

## Releases

gpconf is at v0.7.0, on [PyPI](https://pypi.org/project/gpconf/) and archived on Zenodo under the concept DOI [10.5281/zenodo.22867654](https://doi.org/10.5281/zenodo.22867654), which always resolves to the latest release; 0.7.0's own is [10.5281/zenodo.23382463](https://doi.org/10.5281/zenodo.23382463). From 0.7.0 it is under Apache-2.0; releases up to 0.6.2 stay MIT. Since it became installable with pip at 0.3.0:

- 0.4.0 added the eighteenth case, the first to hand a parser corrupt input.
- 0.5.0 let an adapter say "unsupported" for an operation it does not have, counted each record once, and made the package builds reproducible.
- 0.5.1 cut a user's fetch from 61 requests and 12.5 MB to 50 requests and 3.2 MB, and made it stop at any response it does not expect.
- 0.6.0 gave the JSON report a summary a script can rely on, took the name gpconf, and cut the fetch to 46 requests.
- 0.6.1 made the fetch name the kit and its version in its `User-Agent`.
- 0.6.2 fixed two things an audit found: a mistyped case name passed silently, and `check-tle` on a missing file printed a traceback.
- 0.7.0 is about the grader itself. SM-26 answered [my audit ask](https://github.com/hneogy/gp-omm-conformance/issues/1) with seven wrong-on-purpose parsers that passed 0.6.1, and two blind audits wrote twenty-one more. What got through is closed, all of them are now tests, and more is graded before any fetch. No published case verdict moved.

## Quick start

```bash
pip install gpconf
gpconf fetch                                                  # once; fetches the fixtures under CelesTrak's usage policy
gpconf run --preset reference                                 # the control
gpconf presets                                                # libraries testable with no adapter written
gpconf run --adapter mypkg.gpconf_adapter:Parser --json report.json   # your parser
```

## GPKit, for Swift

[GPKit](https://github.com/hneogy/GPKit) reads GP data — OMM and TLE, six-digit catalog numbers included — and passes gpconf. v1.0.0, MIT, on the [Swift Package Index](https://swiftpackageindex.com/hneogy/GPKit). Two reports to the Swift readers: [SatelliteKit #16](https://github.com/gavineadie/SatelliteKit/issues/16) (B* read 48 orders of magnitude too small), open, and [swift-sgp4 #4](https://github.com/csanfilippo/swift-sgp4/issues/4) (satellites from 100000 up refused), fixed by the maintainer in 4.1.0.

## Elsewhere

NEOGY LLC, orbital data tooling. Results and a live tracker at [gpconf.neogy.dev](https://gpconf.neogy.dev) · [gpconf](https://github.com/hneogy/gp-omm-conformance) · [neogy.dev](https://neogy.dev)
