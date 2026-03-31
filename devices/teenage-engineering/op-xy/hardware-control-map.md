# OP-XY Control Map (v0)

Date: 2026-02-16

## Confirmed device dimensions (official)
- Width: 288 mm
- Depth: 102 mm
- Height: 29 mm

Source: https://teenage.engineering/store/op-xy

## Required controls (official guide references)
- Power: physical switch on the right side.
- COM: "to the far right under sample you will find com."
- SHIFT: in transport controls (bottom-left area), used in combos.
- M1: "the button labelled ( 1 ) is referred to as M1."
- Projects: dedicated "projects" button in layout guide.

Sources:
- https://teenage.engineering/guides/op-xy/hardware-overview
- https://teenage.engineering/guides/op-xy/layout

## Preliminary placement model for the robot fixture
Coordinate frame:
- Origin: top-left corner of OP-XY top surface.
- +X to the right (0 to 288 mm).
- +Y downward (0 to 102 mm).

Official layout hotspot references from the OP-XY guide page markup
(normalized values, not final key-center coordinates):
- `modules` (contains M1-M4): x=0.2448979592, y=0.0989795918
- `transport controls` (contains SHIFT): x=0.0571428571, y=0.1989795918
- `projects`: x=0.0724489796, y=0.0571428571
- `com`: x=0.7010204082, y=0.0571428571
- `volume`: x=0.1336734694, y=0.0193877551
- `encoders`: x=0.4857142857, y=0.0387755102

Current confidence levels:
- COM: high (clear in guide and imagery).
- SHIFT: medium-high (clear label in transport section).
- M1: high (explicit M1 label under screen module row).
- Projects: medium (guide naming is clear; icon-level mapping still needs one calibration photo on your unit).
- Power switch: medium (right-side placement confirmed; exact along-side offset still to be measured on your unit).

## Actuator mapping to build against now
- Station A: right-side power switch actuator (lateral slide/toggle motion).
- Station B: top-surface COM plunger.
- Station C: top-surface SHIFT plunger.
- Station D: top-surface PROJECTS plunger.
- Station E: top-surface M1 plunger.

## Next measurement pass (to make coordinates exact for your unit)
1. Place OP-XY on graph paper.
2. Take one top-down photo with a ruler in frame.
3. Mark these controls in that image: Power, COM, SHIFT, Projects, M1.
4. Convert each center point to mm in the frame above.
5. Use those mm coordinates as hardpoints for Lego Technic actuator mounting.
