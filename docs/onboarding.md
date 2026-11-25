# Onboarding

Welcome to the avionics team! This page will hopefully get you up to speed for merging your first pull request.

## Project overview

This repository holds the firmware for our EuRoC rocket:

- **Flight computer** (`flight-computer/`) - runs the flight logic.
- **DAQ** (`daq/`) - data acquisition board.
- **Shared code** (`shared/`) - drivers and utilities used by both boards.

See [repository-structure.md](repository-structure.md) for the full layout.

## Tools to install

- [ ] Git
- [ ] STM32CubeMX - for opening and editing the `.ioc` files
- [ ] CMake and the ARM GNU toolchain (`arm-none-eabi-gcc`)
- [ ] VS Code with the STM32 extension - the preferred editor
- [ ] Python 3 - for scripts in `tools/`

> **Note:** STM32CubeIDE is not preferred. The projects are built with CMake, and VS Code integrates with that workflow well, while CubeIDE uses its own build system.

**Developing on a Mac?** Contact @mati5kova for tips on setting up and developing on macOS, or with any other questions.

## Getting the code

via HTTPS

```sh
git clone https://github.com/MakerLab-Rocketry/avionics-firmware.git
cd avionics-firmware
```

via SSH

```sh
git clone git@github.com:MakerLab-Rocketry/avionics-firmware.git
cd avionics-firmware
```

## Building and flashing

<!-- TODO: add build and flash commands once the CubeMX projects are in place. -->

## Drivers

- Read the [driver development guidelines](driver-guidelines.md) before writing or changing a driver.
- Drivers shared by both boards go in `shared/`; board-specific code goes in that board's `App/` folder.
- Never put your code in CubeMX-generated folders (`Core/`, `Drivers/`) outside `USER CODE` blocks.

## Git workflow

1. Pick or create an issue.
2. Create a branch from an up-to-date `main`, e.g. `feature/tmp102-driver` or `fix/uart-timeout`:

    ```sh
    git status                  # make sure you have no uncommitted changes
    git switch main             # switch to main
    git pull --ff-only          # get the latest main
    git switch -c feature/tmp102-driver
    ```

3. Commit small, focused changes with clear messages.
4. Open a pull request and fill in the template (including `Closes #<issue>`).
5. Get a review before merging.

## Testing

- Add tests to `tests/` where possible.
- For hardware-related changes, test on real hardware and note the result in the pull request.

## Where to get help

- Check the [FAQ](faq.md).
- Ask the team.
