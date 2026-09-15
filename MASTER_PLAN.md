# STM32 Motor Vibration Monitor — Master Plan

## Purpose

Build a real embedded system that measures motor vibration, processes the signal on an STM32, and eventually classifies abnormal operating conditions with a small machine-learning model.

The project should demonstrate real engineering ability: C, microcontroller peripherals, interrupts, DMA, FreeRTOS, UART protocols, debugging, signal processing, testing, and edge ML.

## Final outcome by December

- Working STM32 firmware on the NUCLEO-U575ZI-Q
- LSM6DSOX vibration sensor operating over SPI
- Motor controlled through a TB6612 driver
- Timestamped data acquisition using interrupts and DMA
- FreeRTOS tasks for acquisition, processing, and communication
- Python logger and visualization tools
- Baseline threshold detector plus a simple ML classifier
- Measured sampling rate, jitter, dropped samples, inference latency, memory usage, and accuracy
- Public GitHub repository with tests, screenshots, measurements, documentation, and a short demo

## Hardware

- STM32 NUCLEO-U575ZI-Q
- Adafruit LSM6DSOX accelerometer/gyroscope breakout
- TB6612 motor-driver breakout
- 3–6 V TT motor and wheel
- Four-AA battery holder and batteries
- Breadboard, Dupont wires, stranded wire, heat-shrink, zip ties, M3 hardware
- USB logic analyzer

Use a guarded, low-voltage motor setup. Do not power the motor directly from an STM32 pin. Use SJSU Makerspace equipment for soldering and 3D printing if needed.

## Repository structure

```text
stm32-motor-vibration-monitor/
├── README.md
├── ROADMAP.md
├── MASTER_PLAN.md
├── PROJECT_STATUS.md
├── CLAUDE.md
├── firmware/
├── host_python/
├── hardware/
├── tests/
└── docs/
    ├── screenshots/
    ├── wiring/
    └── measurements/
```

GitHub is the permanent source of truth. `PROJECT_STATUS.md` records the current milestone and next task. `ROADMAP.md` records the planned sequence. `MASTER_PLAN.md` records the full semester strategy.

## Fall 2026 timeline

### Week 1 — September 14–20

- Finish AI and repository setup.
- Create the STM32CubeIDE project.
- Compile, flash, and debug an onboard-LED blink test.
- Save build and hardware evidence.

### Week 2 — September 21–27

- Implement the cache simulator foundation in C.
- Add set associativity, LRU, write policies, counters, and tests.
- Compare array-order and blocked-matrix traces.
- Add sanitizers, Makefile, README, and tag `v1.0`.

### Week 3 — September 28–October 4

- Bring up STM32 GPIO and UART.
- Practice breakpoints, register inspection, and debugging.
- Record the board pinout and project architecture.

### Week 4 — October 5–11

- Implement the LSM6DSOX SPI driver.
- Verify device identity, configure the sensor, read samples, and calibrate.
- Capture an SPI transaction with the logic analyzer.

### Week 5 — October 12–18

- Add data-ready interrupt handling.
- Add timer timestamps and SPI DMA.
- Implement a ping-pong buffer or ring buffer.
- Measure sampling rate and timing jitter.

### Week 6 — October 19–25

- Create a versioned UART packet format with sequence number and CRC.
- Add host-side Python parsing, logging, and plotting.
- Test corrupted and missing packets.

### Week 7 — October 26–November 1

- Separate acquisition, processing, and communication into FreeRTOS tasks.
- Use queues or task notifications.
- Add a watchdog and document task priorities.

### Week 8 — November 2–8

- Build and safely mount the motor rig.
- Record normal, loose-mount, and controlled-imbalance conditions at multiple speeds.
- Store raw data with speed, condition, timestamp, and session metadata.

### Week 9 — November 9–15

- Extract RMS, peak-to-peak, crest factor, and frequency-band features.
- Build a simple threshold-based detector first.
- Plot features and identify failure cases.

### Week 10 — November 16–22

- Train logistic regression or a tiny MLP.
- Split data by recording session, not adjacent samples.
- Report confusion matrix and macro-F1.

### Week 11 — November 23–29

- Deploy the simplest useful model to the STM32.
- Measure inference latency, RAM, Flash, CPU load, and dropped samples.

### Week 12 — November 30–December 6

- Run a 30–60 minute soak test.
- Test sensor disconnects, corrupted packets, and forced resets.
- Fix reliability issues and finish the enclosure or mounting.

### Finals and post-finals — December 7–20

- Do not add major features during finals.
- Finish the README, architecture diagram, results table, screenshots, and demo video.
- Tag `v1.0` and update the résumé using only measured results.

## Spring extension

Build a fault-tolerant firmware updater using the STM32U575 dual-bank flash:

1. Minimal bootloader and application jump
2. Framed UART/USB image transfer
3. CRC and version validation
4. Inactive-bank installation
5. Boot confirmation and rollback
6. Python packager/flasher
7. Vetted signature-verification library
8. Raspberry Pi Pico reset injector
9. 100–200 interrupted-update trials

Afterward, contribute firmware to one SJSU robotics, racing, or research team rather than starting another large solo project.

## Definition of done

- Fresh clone builds successfully.
- Another person can follow the setup instructions.
- Tests cover important logic and failure cases.
- Measurements are reproducible.
- Screenshots, logic-analyzer captures, and videos are saved.
- README begins with a short demo and clearly explains architecture.
- Git history shows incremental work.
- Every résumé claim can be demonstrated and explained.

## AI workflow

### ChatGPT Project

Use ChatGPT for teaching, planning, debugging explanations, checkpoint reviews, and deciding what evidence to collect. Link the GitHub repository and ask it to inspect the latest `README.md`, `ROADMAP.md`, `MASTER_PLAN.md`, and `PROJECT_STATUS.md` before giving project advice.

### Claude Code

Run Claude Code inside the cloned repository. It can inspect repository files, create and edit files, run shell commands, compile code, run tests, inspect errors, and prepare Git commits. It cannot physically see your hardware, know whether a circuit works, or invent measurements. Review all changes before committing.

### Session procedure

1. Ask ChatGPT for the single task and its goal.
2. Give that task to Claude Code.
3. Have Claude explain its proposed changes.
4. Approve only the intended changes.
5. Run the program and perform the hardware test yourself.
6. Save required screenshots or measurements.
7. Ask Claude to update `PROJECT_STATUS.md`.
8. Review and commit the changes to GitHub.
9. Tell ChatGPT what happened and request the next task.

## Rules

- Never claim unfinished work is complete.
- Do not paste code you cannot explain.
- Do not add features merely to collect keywords.
- Cut displays, Bluetooth, cloud services, and large neural networks before cutting tests or measurements.
- Update the status file after every meaningful checkpoint.
