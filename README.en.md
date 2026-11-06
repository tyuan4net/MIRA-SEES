# MIRA Spacetime Expression and Exchange Standard (MIRA-SEES)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Latest Release](https://img.shields.io/github/v/release/tyuan4net/MIRA-SEES?color=blue)](https://github.com/tyuan4net/MIRA-SEES/releases/latest)
[![Format](https://img.shields.io/badge/Format-EPUB_%7C_PDF-orange)](https://github.com/tyuan4net/MIRA-SEES/releases/latest)
[![Blog](https://img.shields.io/badge/Blog-Blogger-ff69b4?logo=blogger)](https://mirahumanatlas.blogspot.com/)

> A spacetime expression language that people can write, discuss, exchange, parse, and compute.

**Status: Draft — open for discussion and implementation-driven validation**

MIRA-SEES addresses a deceptively simple question:

> When we write down a date, time, and place, what kind of time do those numbers represent, and how much geographic information accompanies them?

“1960-06-28 14:45” might mean the local clock time recorded on a birth certificate, local standard time with daylight saving excluded, UTC, local mean time, or local apparent solar time. If only the numbers survive an exchange, timezone rules, DST, longitude, Equation of Time, and the meaning of the original record can silently disappear.

MIRA-SEES provides short, visible, machine-readable expressions for these distinctions. Its purpose is to let the same spacetime record move between people, software, and different calculation systems without losing its original meaning.

## Why it is needed

Many spacetime errors are not caused by an inaccurate algorithm. They begin because the input was not described clearly enough.

- A birth certificate normally records the administrative clock time observed at that place and date.
- Astronomical calculations usually need a normalized instant and an explicitly identified time scale.
- Houses, ASC, MC, and other location-dependent calculations also require coordinates.
- Some schools of Bazi, Zi Wei Dou Shu, and other traditional systems may use local mean or apparent solar time.
- Historical timezone rules, daylight-saving rules, and administrative boundaries change over time.
- Future civil-time rules may not yet be known and should not replace a known canonical event instant.

Two programs can therefore be internally correct and still disagree because they interpreted the same-looking input differently.

MIRA-SEES does not invent a new time system. It makes assumptions that were previously hidden in context explicit and exchangeable.

## From one birth record to an exchangeable human atlas

MIRA is not limited to one charting system. Human Design, Western and classical astrology, Hindu astrology and Jyotish, Bazi, Zi Wei Dou Shu, Cayce Astrology, Jungian Astrology, and other systems can eventually begin with the same normalized and traceable birth-spacetime record, then produce their own structured outputs.

```text
One original birth record
          │
          ▼
MIRA-SEES spacetime normalization
          │
          ├──► Human Design structured data
          ├──► Western / classical astrology structured data
          ├──► Hindu astrology / Jyotish structured data
          ├──► Bazi and Zi Wei Dou Shu structured data
          ├──► Cayce Astrology structured data
          └──► Jungian Astrology structured data
                         │
                         ▼
       Linked with life events, observations,
                 tags, and relationships
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
       Human reading  AI-assisted  Third-party
                      analysis      software
```

These outputs should not be limited to one-time reports or images that can only be viewed. They can become data with defined fields, provenance, versions, and calculation conditions, allowing tools written by different people and at different times to parse and exchange them.

Birth spacetime is the shared anchor. Each system retains its own schema, terminology, rules, and provenance. MIRA-SEES does not collapse different interpretive traditions into one rule set; it first ensures that they refer to a clearly identified time and place and that the transformation from the source record can be reproduced.

Once structured system outputs are connected with observed life events, people or AI can compare systems, investigate correspondences, propose interpretations, and test hypotheses. Third-party programs can participate without first guessing MIRA-specific time and location conventions.

Calculation and interpretation remain separate layers. The same reproducible structured result may support different schools, authors, programs, or AI interpretations without rewriting the underlying birth record.

## Core model

A MIRA-SEES expression distinguishes two kinds of information:

1. **Which representation of time is currently being used**;
2. **Which kind of geographic information accompanies or governs it**.

### Time representation codes

| Code | Meaning | Typical use |
| --- | --- | --- |
| `I` | IANA local administrative time | Birth certificates, wall-clock time, ordinary local time |
| `IS` | Local standard time with DST excluded | Comparison, research, or an explicit no-seasonal-adjustment view |
| `U` | A UTC instant | Cross-system exchange, astronomical core, timelines, returns |
| `LM` | Local Mean Time | Historical LMT or mean solar time at a specified longitude |
| `LS` | Local Solar Time | A calculation explicitly using longitude and Equation of Time |

### Geographic information codes

| Code | Meaning | Question answered |
| --- | --- | --- |
| `A` | IANA timezone level; the default and normally omitted | Which administrative-time rule set applies? |
| `B` | Local administrative area | Which city, town, or locality is meant? |
| `C` | Explicit latitude and longitude | Which terrestrial position is used for calculation? |
| `D` | LMT reference longitude | Which reference meridian governed a historical local clock? |

`D` is not a more precise level than `C`. It represents a different kind of information. The actual event location and the reference longitude used by a historical clock may differ and must not be stored in the same field.

## Writable by people, readable by programs

Documents, reports, and conversations can use a compact notation:

```text
I<1990.3.20.8.30>
IB<1990.3.20.8.30>
IC<1960.6.28.14.45>
U<1960-06-28T05:45:00Z>
LMD<1870.5.10.8.20>
LSC<1960.6.28.14.43.20>
```

For example, `IC<1960.6.28.14.45>` says that the value is an IANA local administrative time accompanied by explicit coordinates. A complete record still carries the required fields:

```text
Birth spacetime: IC
Local time:        1960-06-28 14:45:00
IANA timezone:     Asia/Taipei
Resolved offset:   UTC+09:00
UTC:               1960-06-28T05:45:00Z
Area:              Taiwan / Taipei City
Place:             Taipei Postal Hospital
Coordinates:       25.0288072263 N, 121.5189180277 E
```

The prefix does not replace the complete record. It is a concise semantic marker: a reader can immediately see which time representation is being discussed and what kind of geographic information is available. Required timezone, coordinate, offset, algorithm, and version fields remain in the payload.

Because `<` and `>` have special meanings in many shells, a CLI can use:

```text
IC:1960.6.28.14.45
```

or separate the semantic code from the value:

```text
--time-style IC --time 1960.6.28.14.45 \
  --tz Asia/Taipei \
  --lat 25.0288072263 \
  --lon 121.5189180277
```

## Normalization: preserve the source, establish a common computation value

```text
[ Handwritten record / birth certificate / phone / other software ]
                              │
                              ▼
             [ Identify time and geographic semantics ]
                              │
                              ▼
       [ Resolve offset, UTC, and reproducibility metadata ]
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
          Astronomy / HD   Houses and   Solar-time
              core          angles       systems
            UTC / JD        UTC + C     LM / LS + EOT
```

Normalization must not erase the source record. It should retain, when applicable:

- the original local date-time and its representation type;
- the IANA timezone identifier;
- the resolved UTC offset and UTC value;
- administrative area, named place, and coordinates;
- the selected occurrence of an ambiguous DST time;
- the relevant timezone-database, time-scale, EOT, or algorithm version.

Different systems can then share a common computation layer while still knowing how the original record was understood.

## Never guess silently

MIRA-SEES is intended to expose ambiguity, not hide it behind defaults.

- During a DST fall-back, one local time may map to two UTC instants. The system must request a choice or preserve the known offset.
- During a DST spring-forward, some local times do not exist. The system must reject them or request correction.
- An administrative area name is not an IANA timezone and does not necessarily identify one uniquely.
- `B` means that an administrative area is known; it does not pretend that precise coordinates are known.
- `C` means that explicit coordinates are present, while their source and uncertainty may still be recorded separately.
- Future civil-time rules may change. Long-range events should retain a canonical UTC instant.

“Unambiguous” does not mean that a short prefix can create missing information. It means that a complete payload exposes unresolved ambiguity, forbids silent guessing, and preserves enough information to reproduce the transformation.

## Cross-system examples

### Human Design

```text
IC local birth record
→ normalization
→ U
→ existing UTC calculation core
```

The Human Design core does not need to be rewritten merely to accept local-time input.

### Western astrology

```text
IC
→ UTC / astronomical time + latitude + longitude
→ planets, points, ASC, MC, and houses
```

Planetary positions and houses require different information. MIRA-SEES makes that distinction explicit.

### Bazi, Zi Wei Dou Shu, and other traditional systems

```text
IC original record
→ U
→ longitude-based mean solar time
→ optional EOT
→ LM or LS calculation profile
```

Whether a school uses local mean time, local apparent solar time, or another convention is determined by that system or calculation profile. MIRA-SEES does not decide on behalf of a tradition; it makes the selected rule identifiable, exchangeable, and reproducible.

### Timelines, transits, and returns

A long-range astronomical event is first retained as a unique UTC instant:

```text
U<2078-03-04T06:13:22Z>
```

It can later be projected as `I`, `IS`, `LM`, or `LS` for display. This prevents the event itself from drifting when future timezone or DST rules change.

### Cayce Astrology and Jungian Astrology

Cayce Astrology and Jungian Astrology are planned MIRA output and research layers. They can share normalized birth spacetime and lower-level astronomical data while expressing their own concepts, mappings, sources, and versions through independent schemas.

This allows later research to distinguish reproducible calculation values, structures created by a particular system, and interpretations proposed by a person or AI.

## Design principles

1. **Human- and machine-friendly** — suitable for handwriting, conversation, and reports, while mapping cleanly to CLI, JSON, APIs, and databases.
2. **Separate semantics from formatting** — how a timestamp is written and what kind of time it represents are different questions.
3. **Keep source and derived values together** — normalization establishes common computation values without overwriting provenance.
4. **Make ambiguity visible** — missing data, repeated times, nonexistent times, and inferred values must not be handled silently.
5. **Support incremental adoption** — each program may implement and declare only the subset it needs.
6. **Preserve reproducibility** — relevant timezone-database, time-scale, EOT, and algorithm versions should be recordable.

## Current status

MIRA-SEES is an open draft being validated through real implementations. It is not a completed international standard and has not been approved by a standards organization.

Initial validation areas include:

1. `I` / `IB` / `IC` local input for Human Design;
2. the `U` computation interface for `planet_dump`;
3. actual `LM` / `LS`, longitude, and EOT requirements in Bazi and Zi Wei Dou Shu;
4. whether `IC` is sufficient for planets, points, and houses in Western astrology;
5. shared birth-spacetime and astronomical foundations for Cayce Astrology, Jungian Astrology, and other systems;
6. linking structured system outputs with life events, observations, and interpretation layers;
7. canonical UTC and display projections for timelines and returns;
8. JSON Schema, CLI options, and cross-implementation conformance vectors.

A complete specification, schemas, and conformance examples will be developed gradually through implementation. When real data reveals a necessary spacetime meaning that the current codes cannot express, the draft should be revised rather than forcing reality into the existing codes.

## The convenience MIRA-SEES is meant to create

MIRA-SEES should make conversations like these ordinary:

```text
“My birth record is IB: the city and timezone are known,
but precise coordinates are not.”

“This calculation requires IC; please provide the coordinates to use.”

“This event is stored as U and converted to the viewer's civil time
only for display.”

“This Bazi result uses LSC and records the EOT algorithm version.”
```

A person should not need to understand an entire database schema before exchanging a concept with clear boundaries. A program should not need to guess assumptions that previously existed only in human context.

That is the shared language MIRA-SEES is intended to create.
