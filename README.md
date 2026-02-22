# 3.3V / 5V Latch-Off Power Protection Architecture  
Rail-Level Overcurrent & Fault Isolation Standard

## Overview

This repository documents a standardized latch-off protection architecture for 3.3V and 5V logic rails used across multiple embedded hardware platforms.

The solution is based on the TPS2553-1 precision current-limited power switch and is intended to provide permanent rail-level protection against:

- Short circuits
- Sustained overcurrent conditions
- Incorrect battery installation
- Wiring mistakes
- Load-side electrical faults

This architecture is designed as a long-term system robustness improvement, not only a development-phase safeguard.

The complete engineering documentation is included in this repository as:

Protection_Standard.pdf

---

## Problem Context

In embedded systems powered by external batteries, field failures may occur due to:

- Wrong battery types
- Improper voltage sources
- Accidental polarity or wiring mistakes
- Short circuits during integration
- Faulty downstream modules

Without rail-level isolation, such faults can:

- Permanently damage expensive logic and communication ICs
- Overstress LDO regulators
- Cause cascading failures across subsystems
- Render entire boards unusable

Voltage regulation alone does not provide adequate fault isolation.

---

## Protection Strategy

Instead of direct regulator-to-load connection:

Regulator → Load

The architecture enforces:

Regulator → TPS2553-1 → Logic / Communication ICs

This ensures:

- Controlled current limiting during abnormal events
- Automatic shutdown during sustained faults
- Manual reset requirement before re-energizing the rail
- Prevention of repeated thermal stress
- Deterministic and safe fault behavior

---

## Selected Device

The protection architecture uses:

Texas Instruments – TPS2553DBVT-1  
Precision Adjustable Current-Limited Power Switch (Latch-Off Variant)

Key reasons for selection:

- Programmable current limit (75mA to 1700mA)
- True latch-off behavior (no automatic retry cycling)
- Independent operation from MCU
- Built-in soft-start to control inrush current
- Fault indication output

The latch-off (-1) variant prevents repeated power cycling under persistent fault conditions, improving long-term reliability.

---

## Current Limit Configuration

For the 3.3V rail, a 30kΩ R_ILIM resistor was selected, resulting in an approximate current limit of ~800–850mA (based on datasheet equations and tolerance analysis).

This configuration:

- Protects upstream LDO regulators
- Provides operational headroom
- Ensures shutdown before thermal overstress

Current limit values may be adjusted depending on load profile.

---

## Design Philosophy

This architecture shifts protection from:

Reactive replacement of damaged components

to:

Proactive rail-level fault containment

It transforms catastrophic failure into controlled shutdown.

---

## Validation Basis

The implementation is derived from:

- Device datasheet electrical characteristics
- Application circuit recommendations
- Current limit equation analysis
- Industry-standard eFuse design methodology

Extended validation testing is being incorporated into future hardware revisions.

---

## Impact

Adoption of this protection architecture provides:

- Improved field reliability
- Protection against user misuse
- Reduced hardware loss
- Deterministic fault handling
- Reusable power protection methodology for future designs
