# STM Device SDK for Meson Builds

This project exists to help provide the STM drivers to embedded firmware projects using the meson build system (or not!  You can also simply include it on its own).

Code is imported from the [STM32Cube MCU Overall Offer](https://github.com/STMicroelectronics/STM32Cube_MCU_Overall_Offer).


[STM32 device families](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html)

## How to use

In a meson project, create a directory called `subprojects`.  In that directory, create a new file named `stm_sdk.wrap` with the following contents:

````
[wrap-git]
url = https://github.com/akobyl/STM_SDK
revision = main
clone-recursive = true

[provide]
stm_sdk = stm_sdk_dep
````

In the project root, run the command `meson subprojects download` to initially download it. `meson subprojects update` can be used to update an existing subproject after it is downloaded.  See [Meson Subprojects Command](https://mesonbuild.com/Subprojects.html#meson-subprojects-command)

To use the subproject in a `meson.build` file, first create a list of options you need.  You then can crate a subproject object to import it, and then import the dependency `stm_sdk` :

```
stm_sdk_options  = [
  'family=g4',
  'chip=stm32g474',
  'peripherals=gpio,adc,utils',
  'use_hal=false']

stm_sdk_project = subproject('sdk',
                             default_options: stm_sdk_options)
stm_sdk = dependency('stm_sdk')
```

## Options

| Option | Description |
|--------|-------------|
| `chip` | Chip part number (exluding package  code) |
| `peripherals` | List of peripherals to import |
| `use_hal` | Import HAL code (LL code is always imported) |

