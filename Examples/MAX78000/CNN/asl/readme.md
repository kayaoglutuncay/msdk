## Description

- **What:** A small CNN example for American Sign Language (ASL) classification targeting the MAX78000 MCU. The example demonstrates embedding pretrained network weights into firmware and running on-device inference.
- **Goal:** Show how to build and flash a compiled CNN example for the MAX78000, run inference on sample data, and view results over serial.

## Software / Build

Prerequisites:

- MSDK checkout and compatible toolchain (use the same toolchain referenced by your MSDK release).
- GNU `make` and the cross-toolchain (see MSDK docs for exact tool versions).

Quick build and flash (typical):

```bash
cd Examples/MAX78000/CNN/asl
make
make flash
```

If flashing fails, confirm your `project.mk`/toolchain `PREFIX` settings match your installed toolchain.

## Running and Serial Output

- Connect the board and open a serial terminal (115200 baud typical).
- After flashing, the example runs on reset and prints:
  - Model initialization messages.
  - Inference timings (cycles or ms).
  - Predicted class and confidence/probabilities.
  - Optional debug or sample data comparisons.

## Notes on Model & Conversion

- This example embeds compiled weights (`weights.h`) rather than loading a separate `.kmodel` file. To use a new model, convert and export weights into the same format expected by the firmware, then regenerate `weights.h` and rebuild.
- Typical conversion steps (high level): export a trained model → quantize/pack weights → produce header/source with weight arrays → replace `weights.*` and rebuild.

## Required Connections

- Power and debugger (SWD) or programmer for flashing.
- UART-to-USB for serial logging (115200 baud recommended).

## Debugging Tips

- If output is empty or predictions are incorrect:
  - Verify input preprocessing matches training (image size, channel order, normalization).
  - Ensure the embedded weights match the network architecture built into `cnn.c`.
  - Check linker/memory settings if the program crashes on startup.
