# Design TODO

**Status:** Proposed follow-up work; the active baseline remains [`DESIGN.md`](DESIGN.md).

This list captures five improvements for the controller prototype. Hardware assumptions must be checked against the exact board and component datasheets before they are treated as verified.

## 1. Define the MIDI contract for Mixxx

**Priority:** Before implementing the V0.1 MIDI mapping; validate end-to-end by V0.4.

Use a stable table in firmware to map each physical control to a standard MIDI message. Mixxx maps those messages to DJ actions. Define the MIDI channel and CC/Note identifiers, button press/release behavior, the relative encoding for the track-selection encoder, and messages for both jog wheels in their independently selectable NUDGE and SCRATCH modes. Define the return messages Mixxx sends for LED feedback and how they address `MAIN_L`, `MAIN_R`, `DECK_A`, and `DECK_B`.

Keep the identifiers stable across firmware revisions unless a documented compatibility change is required. Confirm the selected Mixxx version supports the intended track browsing, jog-wheel modes, and LED-feedback mapping before finalizing the contract.

**Done when:** The message table is documented and V0.4 verifies track browsing, both jog modes on each wheel, LED feedback, and reconnect behavior with Mixxx.

## 2. Resolve USB power and VBUS behavior

**Priority:** Blocking before connecting external power and USB paths together or testing USB MIDI.

Verify the exact DevKitC revision and schematic. Document the external power entry, host-VBUS sensing, isolation, and the USB-to-UART power path. Add a connection matrix for the Raspberry Pi, development computer, and external supply that marks each combination as allowed, forbidden, or unverified. Do not test unverified combinations until the board power paths have been checked.

**Done when:** Every intended configuration has a documented safe connection procedure, and testing confirms host VBUS is sensed without back-feeding either host or causing supply contention.

## 3. Record exact prototype parts and verify the pin assignment

**Priority:** Before wiring the complete prototype; final pinout verification is required before PCB work.

Record the exact DevKitC revision and ESP32-S3 module variant, two AS5048A sensor packages/modules and two [N38, 8 mm × 3 mm diametrically magnetized NdFeB discs](https://www.umagnets.com/p/20pcs-n38-8mm-x-3mm-diametrically-magnetized-neodymium-disc-magnet-small-powerfull-ndfeb-round-radial-magnets-sale/), two momentary tactile jog-mode switches, the incremental A/B track encoder and its push switch (function TBD), PCF8575 board or IC variant, SK6812 part, and power/connectors selected. Mark each BOM item as confirmed, candidate, or unverified. Check the GPIO assignment against the board schematic and module constraints; verify the AS5048A interface and the selected magnet's suitability, air gap and alignment, PCF8575 addresses, interrupt behavior, and whether the chosen breakouts already include I2C pull-ups.

**Done when:** The prototype BOM and GPIO table identify their hardware sources and have been checked against the selected parts. Keep the pinout provisional until then.

## 4. Add measurable pass criteria to prototype gates

**Priority:** Set criteria before the corresponding V0.x sign-off.

For each gate, document the test procedure, measured result, pass/fail threshold, and evidence to retain. Establish numeric targets for input-to-MIDI and jog-wheel latency, jog-angle repeatability, ADC jitter, missed or duplicate events, independent NUDGE/SCRATCH switching, track browsing, USB reconnect behavior, and extended-run stability before V0.4 sign-off. Do not infer thresholds from unmeasured assumptions; agree them before testing the relevant gate.

**Done when:** Each V0.1–V0.5 gate has an unambiguous pass/fail record and the required evidence is captured.

## 5. Measure LED power and input behavior under load

**Priority:** Before full-brightness LED operation and before freezing firmware filtering, jog-wheel processing, or power distribution.

With the selected LED part, measure peak current, voltage drop along each chain, and component temperature at controlled brightness levels. Set a tested firmware brightness limit and verify the final wiring and branch protection against measured load. Measure ADC noise and 74HC4067 settling while LEDs are active; choose sampling, calibration, filtering, and deadband from those results. With the selected AS5048A and magnet assembly, measure angle repeatability, wrap-around behavior, direction, and update rate; verify both tactile switches toggle their jog modes independently.

**Done when:** The permitted LED operating level and power distribution are supported by measurements, analog thresholds are validated under representative LED activity, and jog-wheel behavior is repeatable at the required update rate.

## Technical review

Review the MIDI contract against the selected Mixxx version and review AS5048A/magnet and other electrical conclusions against the exact hardware datasheets before treating these items as closed. This TODO does not authorize PCB release or bypass the release gates in [`DESIGN.md`](DESIGN.md#8-prototype-and-release-gates).
