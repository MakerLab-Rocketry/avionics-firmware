# Driver Development Guidelines

Guidelines for writing device drivers (sensors, radios, storage, etc.) used by the flight computer and DAQ firmware.

## Where drivers live

- Drivers used by both boards go in `shared/` — header in `shared/inc/`, implementation in `shared/src/`.
- Board-specific code goes in that board's `App/` folder.
- Do not put driver code in CubeMX-generated folders (`Core/`, `Drivers/`); they can be overwritten on regeneration.

See [repository-structure.md](repository-structure.md) for the full repository layout.

## Naming

- One driver per device, named after the part: `tmp102.h` / `tmp102.c`.
- Prefix all public symbols with the driver name: `tmp102_init()`, `tmp102_read_temperature()`, `TMP102_I2C_ADDR`.
- Keep internal helpers `static` inside the `.c` file.

## Interface

- Keep the driver hardware-independent where practical: pass the bus handle (e.g. `I2C_HandleTypeDef *`) and pins through a config/handle struct instead of hard-coding them.
- Use a handle struct so multiple instances of the same device can coexist:

    ```c
    typedef struct {
        I2C_HandleTypeDef *hi2c;
        uint8_t addr;
        /* configuration, state, ... */
    } tmp102_t;

    tmp102_status_t tmp102_init(tmp102_t *dev, I2C_HandleTypeDef *hi2c, uint8_t addr);
    tmp102_status_t tmp102_read(tmp102_t *dev, tmp102_data_t *out);
    ```

- Every function that can fail returns a status code; never fail silently.
- Return data in SI units (Pa, m/s², °C, ...) and document the units in the header.

## Robustness

- Always use timeouts on bus transactions — never block forever.
- Verify the device ID (`WHO_AM_I` or equivalent) during init.
- Avoid dynamic memory allocation (`malloc`/`free`).
- Do not use `HAL_Delay()` or busy-waits in code that runs from interrupts.
- Check and handle every HAL return value.

## Documentation

- Document every public function in the header (purpose, parameters, return value, units) using [Doxygen](https://www.doxygen.nl/manual/docblocks.html) comments. Editors such as VS Code show them on hover and in autocomplete.

    ```c
    /**
     * @brief  Read the current temperature from the TMP102.
     *
     * @param[in]  dev  Initialized device handle.
     * @param[out] out  Where the temperature is written, in °C.
     *
     * @return TMP102_OK on success, error code otherwise.
     */
    tmp102_status_t tmp102_read(tmp102_t *dev, tmp102_data_t *out);
    ```

    - The comment must start with `/**` (or `///`).
    - You usually don't have to write the template by hand: in many editors you can right-click the function name and pick an option like **Generate Doxygen Comment** (VS Code with the C/C++ extension). In VS Code, typing `/**` above a function and pressing Enter also works.
    - Common tags: `@brief`, `@param[in]` / `@param[out]` / `@param[in,out]`, `@return` / `@retval`, `@note`, `@warning`.

- Reference the datasheet (part number and revision) at the top of the driver file.
- Add an interface description to `docs/` if the driver exposes a protocol or data format.

## Testing

- Add tests for each driver to `tests/`.
- Before merging, test the driver on real hardware and note the result (board, date, outcome) in the pull request.
