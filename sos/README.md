# SOS Morse Code LED Blink

STM32CubeMX-generated project for the **NUCLEO-F446RE** board that blinks the
onboard user LED (LD2, PA5) in **SOS** Morse code, on a continuous loop.

## Hardware

- Board: NUCLEO-F446RE
- MCU: STM32F446RET6 (Cortex-M4)
- LED: LD2 (green), connected to PA5

## Pattern

`S` = 3 short pulses, `O` = 3 long pulses, `S` = 3 short pulses, then a pause
before repeating.

| Unit        | Duration |
|-------------|----------|
| Dot (short) | 200 ms   |
| Dash (long) | 600 ms   |
| Gap between pulses | 200 ms |
| Gap between letters | 600 ms |
| Gap between words (loop restart) | 1400 ms |

Implemented in `Src/main.c` via `sos_blink()`, called repeatedly from the main
loop.

## Building

Requires the GNU ARM toolchain (bundled with STM32CubeIDE):

```bash
export PATH="/opt/st/stm32cubeide_2.2.0/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.14.3.rel1.linux64_1.0.100.202602081740/tools/bin:$PATH"
make -j4
```

Produces `build/SOS_LED.elf`, `.hex`, and `.bin`.

## Flashing

Using STM32CubeProgrammer CLI (bundled with STM32CubeIDE), with the board
connected via its onboard ST-LINK:

```bash
STM32_Programmer_CLI -c port=SWD -w build/SOS_LED.bin 0x08000000 -v -rst
```

## Notes

- This project was generated with STM32CubeMX in headless/scripted mode
  targeting the NUCLEO-F446RE board definition (default pin mapping, HAL
  drivers).
- If the ST-LINK fails to connect (`DEV_CONNECT_ERR`), check SWD jumpers
  (CN2), try a different USB port (some USB 3.0/xHCI controllers return a
  `can't set config #1, error -71` enumeration failure for this probe — a
  USB 2.0 port or different controller resolves it), and confirm the board's
  power LED is lit.
