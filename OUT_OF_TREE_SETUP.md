# Out-of-Tree TivaWare Module Setup - Summary

The Following is AI Generated Documentation.

## How to Use

### Method 1: Command Line (Quick Testing)
```bash
west build -b tm4c129x_dk <application> -- \
  -DZEPHYR_MODULES=~/projects/tivaware
```

### Method 2: Environment Variable
```bash
export ZEPHYR_MODULES=~/projects/tivaware
west build -b tm4c129x_dk <application>
```

### Method 3: Application CMakeLists.txt
```cmake
# Add before find_package(Zephyr)
set(ZEPHYR_MODULES ~/projects/tivaware)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
```

## Directory Structure

```
~/projects/tivaware/
├── zephyr/
│   ├── module.yml          ← Module metadata
│   ├── CMakeLists.txt      ← Build integration
│   └── Kconfig             ← Configuration
├── driverlib/              ← TivaWare drivers
├── inc/                    ← Hardware definitions
├── README.zephyr.md        ← Usage documentation
└── [other TivaWare files]
```

## Verification

The module is properly configured when CMake outputs:
```
-- Configuring module: tivaware (~/projects/tivaware)
```

## Next Steps

To build an application:
```bash
# Example: Build hello_world
cd ~/zephyrproject/zephyr
west build -b tm4c129x_dk samples/hello_world -- \
  -DZEPHYR_MODULES=~/projects/tivaware
```

