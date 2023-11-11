project('STM_SDK',
        'c',
        version : '0.2')

c_args = []
stm_sdk_srcs = []
stm_sdk_incs = []

chip = get_option('chip')
peripherals = get_option('peripherals')
use_hal = get_option('use_hal')
family = chip.substring(5,7)

# CMSIS Core


# ST CMSIS device
add_project_arguments('-DSTM32' + family, language:'c')
chipoption = '-D' + chip
add_project_arguments(chipoption, language:'c')
add_project_arguments('-DUSE_FULL_LL_DRIVER', language:'c')
add_project_arguments('-DSTM32G474xx', language: 'c')

if use_hal
  add_project_arguments('-DUSE_HAL_DRIVER', language:'c')
endif

arm_cmsis_include = include_directories('cmsis_core/Core/Include')
stm_sdk_incs += arm_cmsis_include

cmsis_device_root = 'cmsis_device/cmsis_device_' + family.to_lower()
cmsis_include_dir = cmsis_device_root / 'Include'
cmsis_include = include_directories(cmsis_include_dir)

startup_file = 'startup_' + chip.to_lower() + '.s'
system_file = 'system_stm32' + family.to_lower() + 'xx.c'
cmsis_sources = files(cmsis_device_root / 'Source/Templates/gcc' / startup_file,
                      cmsis_device_root / 'Source/Templates' / system_file)

stm_sdk_incs += cmsis_include
stm_sdk_srcs += cmsis_sources

# LL + HAL
hal_root = 'HAL' / 'stm32' + family.to_lower() + 'xx_hal_driver' 
hal_include_dir = hal_root / 'Inc'
hal_include = include_directories(hal_include_dir)
hal_sources = []
ll_sources = []

hal_src_dir = hal_root / 'Src'
foreach peripheral : peripherals
  ll_file_name = 'stm32' + family.to_lower() + 'xx_ll_' + peripheral + '.c'
  ll_sources += hal_src_dir / ll_file_name
  
  hal_file_name = 'stm32' + family.to_lower() + 'xx_hal_' + peripheral + '.c'
  hal_sources += hal_src_dir / hal_file_name
endforeach

stm_sdk_incs += hal_include
stm_sdk_srcs += ll_sources
if use_hal
  stm_sdk_srcs += hal_sources
endif

stm_sdk = static_library(
  'stm_sdk',
  stm_sdk_srcs,
  install: false,
  # native: false,
  include_directories: stm_sdk_incs,
  build_by_default: meson.is_subproject() == false,
)

stm_sdk_dep = declare_dependency(
              include_directories: stm_sdk_incs,
              link_with: stm_sdk,
              )
