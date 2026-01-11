# TivaWare Out-of-Tree Module Setup

This document describes how TivaWare has been configured as an out-of-tree Zephyr module.

This is AI-Generated Documentation. 

## Directory Structure

```
~/projects/tivaware/                       # TivaWare module root
├── zephyr/                                # Zephyr integration directory
│   ├── module.yml                         # Module metadata (REQUIRED)
│   ├── CMakeLists.txt                     # Build integration
│   └── Kconfig                            # Configuration options
├── driverlib/                             # TivaWare peripheral drivers
├── inc/                                   # Hardware definitions
├── README.zephyr.md                       # This file
└── [other TivaWare directories]
```

## Integration Methods

There are three ways to use this out-of-tree TivaWare module:

### Method 1: Using ZEPHYR_MODULES (Recommended for development)

Set the `ZEPHYR_MODULES` CMake variable to point to the TivaWare directory:

```bash
# Command line:
west build -b tm4c129x_dk samples/hello_world -- -DZEPHYR_MODULES=~/projects/tivaware

# Or in your application's CMakeLists.txt (before find_package(Zephyr)):
set(ZEPHYR_MODULES ~/projects/tivaware)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
```

### Method 2: Using EXTRA_ZEPHYR_MODULES

To add TivaWare in addition to other modules discovered by west:

```bash
west build -b tm4c129x_dk samples/hello_world -- -DEXTRA_ZEPHYR_MODULES=~/projects/tivaware
```

### Method 3: Using West Workspace (Recommended for projects)

Create a custom west.yml in your application directory:

```yaml
manifest:
  projects:
    - name: zephyr
      url: https://github.com/zephyrproject-rtos/zephyr
      revision: main
      import: true

    - name: tivaware
      path: ../projects/tivaware
```

Then initialize the workspace:

```bash
cd your-app
west init -l .
west update
```

## Module Configuration

The `zephyr/module.yml` file defines:
- **Module name**: `tivaware`
- **Build files**: CMakeLists.txt and Kconfig in `zephyr/` directory
- **No additional roots**: Uses Zephyr's standard paths

## Building Applications

### Build a TM4C129 application:

```bash
# Using ZEPHYR_MODULES:
west build -b tm4c129x_dk samples/hello_world -- \
  -DZEPHYR_MODULES=~/projects/tivaware

# Using environment variable:
export ZEPHYR_MODULES=~/projects/tivaware
west build -b tm4c129x_dk samples/hello_world

# Clean build:
west build -b tm4c129x_dk --pristine samples/hello_world -- \
  -DZEPHYR_MODULES=~/projects/tivaware
```

### Build BU_fpga sample:

```bash
west build -b tm4c129x_dk samples/boards/tm4c129/BU_fpga -- \
  -DZEPHYR_MODULES=~/projects/tivaware
```

## Verification

To verify the module is being loaded:

```bash
# Check CMake output for module discovery:
west build -b tm4c129x_dk samples/hello_world -- \
  -DZEPHYR_MODULES=~/projects/tivaware 2>&1 | grep -i tivaware

# Expected output should show:
# -- Configuring module: tivaware (~/projects/tivaware)
```

## Module Variables

When TivaWare is loaded, these CMake variables are available:

- `ZEPHYR_TIVAWARE_MODULE_DIR`: Path to TivaWare root
- `ZEPHYR_TIVAWARE_CMAKE_DIR`: Path to CMakeLists.txt directory (zephyr/)

## Troubleshooting

**Problem**: Module not found
- **Solution**: Verify ZEPHYR_MODULES path is correct and absolute
- **Check**: `ls ~/projects/tivaware/zephyr/module.yml` should exist

**Problem**: Build errors referencing old tivaware location
- **Solution**: Clean build directory: `west build -t clean` or `rm -rf build`

**Problem**: TI HAL module still trying to load tivaware
- **Solution**: Verify `~/zephyrproject/modules/hal/ti/CMakeLists.txt` has tivaware commented out

## Development Workflow

For active TivaWare development:

1. Make changes in `~/projects/tivaware/`
2. Rebuild Zephyr applications normally
3. Changes take effect immediately (no need to reinstall)
4. Can version control TivaWare separately from Zephyr

## Module Dependencies

TivaWare has no dependencies on other Zephyr modules. It is a standalone HAL providing:

## References

- Zephyr Modules Documentation: `~/zephyrproject/zephyr/doc/develop/modules.rst`
- West Manifest Documentation: `~/zephyrproject/zephyr/doc/develop/west/manifest.rst`
- TivaWare Documentation: https://www.ti.com/tool/SW-TM4C
