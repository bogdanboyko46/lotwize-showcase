# Benchmarks and method

Every accuracy figure in this repository comes from a hand-checked benchmark. This page gives the
full tables, the scoring rules, and the caveats that come with them.

## The benchmark sets

| Part type | Hand-checked parts | Scored | Library searched | Date | Status |
|---|---:|---:|---:|---|---|
| Headlights | 1,640 across 37 makes | 1,630 | 308,084 photos | 8 Nov 2026 | Live |
| Rear lights (tail and center) | 1,008 | 1,008 | 258,491 photos | 9 Nov 2026 | In development |
| Fog lights and DRLs | 215 | 215 | 46,334 photos | 23 Oct 2026 | In development |

- **Real listing photos.** Headlight and rear benchmark photos come from real listings, with the same
  angles, lighting and clutter a seller uploads. The fog set is a smaller sample of product photos.
- **No benchmark photo is in the library.** Copies of benchmark photos were found in the retrieval
  library and removed. That lowered every reported score, and the lower numbers are the ones published.
- **The set is fixed.** Benchmark labels are final and are not edited to agree with a model.
- **1,630 of 1,640 headlights are scored.** The other ten fall outside what the identifier is built to recognise.

## Three outcomes per field

| Outcome | Meaning | What it costs the seller |
|---|---|---|
| **Filled in correctly** | The identifier asserted the right value. | Nothing. |
| **Dropdown** | It declined to guess and offered a short list. | One click, if the right answer is in the list. |
| **Wrong** | It asserted a value, and the value was wrong. | A return or a dispute, if it isn't caught. |

The public benchmark page reports **"correct or in the dropdown"**: a field is a success when it was
filled in correctly, or when the dropdown offered contains the right answer. A dropdown that doesn't
contain the right answer counts as a miss. The stricter **"filled in correctly"** figure is published next
to it here, so the dropdown rate is visible instead of folded into the score.

## Headlights

The identifier has two modes. **Balanced** is the default: any field it can't settle becomes a dropdown.
**Faster** auto-fills its best guess and marks it for the seller to check. Faster has fewer dropdowns
and more wrong answers, and that trade-off is the seller's choice.

### Balanced (default), 1,630 parts

| Field | Filled in correctly | Correct or in the dropdown | Dropdowns | Wrong |
|---|---:|---:|---:|---:|
| Make | 1,604 · **98.4%** | 1,627 · 99.8% | 23 | **3** |
| Model | 1,568 · **96.2%** | 1,624 · 99.6% | 56 | **6** |
| Side | 1,607 · **98.6%** | 1,628 · 99.9% | 21 | **2** |
| Year range, overlapping the true range | 1,408 · **86.4%** | 1,619 · 99.3% | 212 | **9** |
| Year range, exact window | 1,291 · **79.2%** | n/a | n/a | n/a |
| Bulb type ¹ | 1,254 / 1,437 · **87.3%** | 1,433 / 1,437 · 99.7% | 181 | **2** |
| Optic ² | n/a | 1,548 / 1,558 · 99.4% | 135 | n/a |

### Faster, 1,630 parts

| Field | Filled in correctly | Dropdowns | Wrong |
|---|---:|---:|---:|
| Make | 1,610 · **98.8%** | 16 | **4** |
| Model | 1,584 · **97.2%** | 38 | **8** |
| Side | 1,617 · **99.2%** | 6 | **7** |
| Year range, overlapping the true range | 1,447 · **88.8%** | 168 | **14** |
| Bulb type ¹ | 1,329 / 1,437 · **92.5%** | 93 | **15** |

¹ Scored only on the 1,437 headlights whose bulb a photo can settle. Many housings left the factory with
more than one bulb type and look identical from outside, so those parts are always offered as a dropdown.
They are not counted as successes.

² Projector or reflector. A value is asserted only on generations that were sold with both optics. The
public page reports it on 1,558 parts. A filled-in-correctly figure is not broken out for optic.

**Why years are scored two ways.** Generations often overlap by a model year, and the same lamp can fit
several years. The overlap score counts a range as right when it overlaps the true range. The exact score
requires the same window. The overlap score is the one shown publicly, and the exact score is the lower bound.

### Per-make breakdown

These are the four core fields (make, model, year range by overlap, side), Balanced mode, 1,630 parts.
Each part contributes four answers. Rows may not sum to exactly 100% because of rounding, and one year
field was left blank.

| Make | Headlights | Filled in correctly | Offered as a dropdown | Wrong |
|---|---:|---:|---:|---:|
| Toyota | 138 | 95.8% | 3.8% | 2 |
| Ford | 117 | 97.0% | 3.0% | 0 |
| BMW | 110 | 96.4% | 3.4% | 1 |
| Audi | 102 | 96.1% | 3.7% | 1 |
| Hyundai | 93 | 93.3% | 6.7% | 0 |
| Mercedes-Benz | 91 | 97.0% | 2.7% | 1 |
| Kia | 88 | 93.8% | 6.2% | 0 |
| Chevrolet | 79 | 94.0% | 4.7% | 4 |
| Honda | 75 | 96.3% | 3.7% | 0 |
| Nissan | 75 | 99.0% | 0.7% | 1 |
| Lexus | 74 | 95.3% | 4.7% | 0 |
| Subaru | 59 | 92.4% | 7.6% | 0 |
| Volkswagen | 40 | 88.8% | 9.4% | 3 |
| Mazda | 38 | 92.1% | 6.6% | 2 |
| Infiniti | 37 | 96.6% | 2.7% | 1 |
| Dodge | 35 | 97.9% | 2.1% | 0 |
| Cadillac | 32 | 93.8% | 6.2% | 0 |
| Acura | 31 | 97.6% | 2.4% | 0 |
| GMC | 29 | 96.6% | 3.4% | 0 |
| Lincoln | 29 | 90.5% | 9.5% | 0 |
| Porsche | 29 | 83.6% | 15.5% | 1 |
| Buick | 27 | 95.4% | 4.6% | 0 |
| Jeep | 26 | 93.3% | 6.7% | 0 |
| Ram | 23 | 98.9% | 1.1% | 0 |
| Land Rover | 22 | 96.6% | 2.3% | 1 |
| Volvo | 21 | 90.5% | 9.5% | 0 |
| Genesis | 19 | 88.2% | 11.8% | 0 |
| Jaguar | 12 | 95.8% | 4.2% | 0 |
| Maserati | 12 | 100.0% | 0.0% | 0 |
| Mitsubishi | 12 | 97.9% | 2.1% | 0 |
| Tesla | 10 | 90.0% | 10.0% | 0 |
| Chrysler | 9 | 91.7% | 8.3% | 0 |
| MINI | 9 | 86.1% | 11.1% | 1 |
| Fiat | 8 | 100.0% | 0.0% | 0 |
| Alfa Romeo | 7 | 100.0% | 0.0% | 0 |
| Scion | 7 | 82.1% | 17.9% | 0 |
| Rivian | 5 | 75.0% | 20.0% | 1 |
| **All 37 makes** | **1630** | **94.9%** | **4.8%** | **20** |

Small makes have wide error bars. Rivian's 5 parts and Scion's 7 say much less than Toyota's 138.

## Rear lights (in development), 1,008 parts, 9 November 2026

Scored on the same basis as the public headlight figures: correct or in the dropdown.

| Field | Correct or in the dropdown | Dropdown rate |
|---|---:|---:|
| Part type (tail or center) | 1,001 / 1,008 · 99.3% | 4.7% |
| Make | 999 / 1,008 · 99.1% | 14.6% |
| Bulb type ³ | 179 / 182 · 98.4% | 23.4% |
| Model | 982 / 1,008 · 97.4% | 23.0% |
| Side | 981 / 1,008 · 97.3% | 6.2% |
| Position (outer or inner) | 960 / 1,008 · 95.2% | 12.8% |
| Year range, overlapping the true range | 948 / 1,008 · 94.0% | 29.4% |

³ Counted only on generations sold with both halogen and LED lamps.

Expect several dropdowns per rear identification.

## Fog lights and DRLs (in development), 215 parts, 23 October 2026

This board uses a **stricter** rule than the others: a field counts only if it was filled in correctly.
A dropdown counts as not correct, even if it holds the right answer.

| Field | Filled in correctly | Asked as a dropdown |
|---|---:|---:|
| Make | 195 · 90.7% | 18 |
| Bulb type | 195 · 90.7% | 16 |
| Side | 181 · 84.2% | 30 |
| Year range, overlapping the true range | 180 · 83.7% | 32 |
| Model | 179 · 83.3% | 32 |
| Part type (fog light or DRL) | 172 · 80.0% | 38 |

The fog benchmark is small and made of product photos rather than real uploads, so treat it as an early
reading.

## How a change is judged

Each candidate change runs on the full headlight benchmark next to the live configuration, in both modes,
on the same machine. The pass/fail rules are written before the run. A typical rule set:

1. Wrong answers may not rise for make, model, side or bulb type, in either mode.
2. Exact-window year accuracy may not fall.
3. Correct answers across the core fields may not fall overall, and no single field may lose more than two.
4. The live arm must reproduce the previous benchmark exactly, or the run doesn't count.

A change that fails any rule doesn't ship, whatever else it improved.
