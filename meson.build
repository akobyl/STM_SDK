project('STM_SDK',
        'c',
        version : '0.1')

c_args = []
stm_sdk_srcs = []
stm_sdk_incs = []

chip = get_option('chip')
family = get_option('family')
peripherals = get_option('peripherals')
use_hal = get_option('use_hal')

# CMSIS Core
arm_cmsis_include = include_directories('cmsis_core/Core/Include')

stm_sdk_incs += arm_cmsis_include

# ST CMSIS device
c_args += '-DSTM32' + 'G4'
c_args += '-D' + chip.to_upper() + 'xx'
c_args += '-DUSE_FULL_LL_DRIVER'

if use_hal
  c_args += '-DUSE_HAL_DRIVER'
endif

cmsis_device_root = 'cmsis_device/cmsis_device_' + family
cmsis_include_dir = cmsis_device_root / 'Include'
cmsis_include = include_directories(cmsis_include_dir)

startup_file = 'startup_' + chip + 'xx.s'
system_file = 'system_stm32' + family + 'xx.c'
cmsis_sources = files(cmsis_device_root / 'Source/Templates/gcc' / startup_file,
                      cmsis_device_root / 'Source/Templates' / system_file)

stm_sdk_incs += cmsis_include
stm_sdk_srcs += cmsis_sources

# LL + HAL
hal_root = 'HAL' / 'stm32' + family + 'xx_hal_driver' 
hal_include_dir = hal_root / 'Inc'
hal_include = include_directories(hal_include_dir)
hal_sources = []

hal_src_dir = hal_root / 'Src'
foreach peripheral : peripherals
  file_name = 'stm32' + family.to_lower() + '_ll_' + peripheral + '.c'
  hal_sources += hal_src_dir / file_name
endforeach

if 'adc' in peripherals
  c_args += ['-DADC1', '-DADC2', '-DADC3', '-DADC4', '-DADC5']
endif
if 'gpio' in peripherals:
  c_args += ['-DGPIOA', '-DGPIOB', '-DGPIOC', '-DGPIOD', '-DGPIOE', '-DGPIOF', '-DGPIOG']
endif


stm_sdk_incs += hal_include
stm_sdk_srcs += hal_sources

stm_sdk_lib = static_library('stm_sdk',
                             stm_sdk_srcs,
                             include_directories : stm_sdk_incs,
                             c_args : c_args,
                            )

stm_sdk_dep = declare_dependency(
              include_directories : stm_sdk_incs,
              link_with : stm_sdk_lib
              )
