# LIB MEM DMABUF

## Overview

`lib_mem_dmabuf` is a userspace library package for interacting with Linux DMA buffers. It provides C++ APIs and Ament CMake build integration, making it easy to use in ROS 2 projects.

**Feature Highlights**

* Underlying file descriptor (fd) import and access.
* Flexible buffer management with automatic or manual release.
* Support for buffer release callback registration.

> [!NOTE]
> Prerequisites: Linux kernel version 5.12 or later is required for kernel dma-buf support.

## Quickstart

### Build

This package uses the Ament CMake build system for easy integration into ROS 2 projects.

```bash
git clone https://github.com/quic-qrb-ros/lib_mem_dmabuf.git
colcon build
```

For the Qualcomm QCLinux platform, we provide two ways to build this package.

<details>
<summary>On-Device Compilation with Docker</summary>

1. Set up the QCLinux Docker environment following the [QRB ROS Docker Setup](https://github.com/quic-qrb-ros/qrb_ros_docker?tab=readme-ov-file#quickstart).

2. Clone and build the source code:

    ```bash
    cd /home/qrb_ros_ws/src/qrb_ros_docker/scripts && \
    bash docker_run.sh

    git clone https://github.com/quic-qrb-ros/lib_mem_dmabuf.git
    colcon build
    ```

</details>

<details><summary>Cross Compilation with QIRP SDK</summary>

1. Set up the QIRP SDK environment: Refer to [QRB ROS Documents: Getting Started](https://quic-qrb-ros.github.io/getting_started/environment_setup.html)

2. Create a workspace and clone the source code:

    ```bash
    mkdir -p <qirp_decompressed_workspace>/qirp-sdk/ros_ws
    cd <qirp_decompressed_workspace>/qirp-sdk/ros_ws

    git clone https://github.com/quic-qrb-ros/lib_mem_dmabuf.git
    ```

3. Build the source code with QIRP SDK:

    ```bash
    colcon build --merge-install --cmake-args \
      -DCMAKE_TOOLCHAIN_FILE=${OE_CMAKE_TOOLCHAIN_FILE} \
      -DPYTHON_EXECUTABLE=${OECORE_NATIVE_SYSROOT}/usr/bin/python3 \
      -DPython3_NumPy_INCLUDE_DIR=${OECORE_NATIVE_SYSROOT}/usr/lib/python3.12/site-packages/numpy/core/include \
      -DPYTHON_SOABI=cpython-312-aarch64-linux-gnu \
      -DCMAKE_MAKE_PROGRAM=/usr/bin/make \
      -DBUILD_TESTING=OFF
    ```

</details>

### Usage

This section shows how to use `lib_mem_dmabuf` to interact with Linux DMA buffers in your projects.

Add dependencies in your `package.xml`:

```xml
<depend>lib_mem_dmabuf</depend>
```

Configure dependencies in your `CMakeLists.txt`:

```cmake
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
```

Allocate a DMA buffer with C++ APIs:

```c++
#include "lib_mem_dmabuf/dmabuf.hpp"

// Allocate dmabuf with size and DMA heap name
auto buf = lib_mem_dmabuf::DmaBuffer::alloc(1024, "/dev/dma_heap/system");

// Get fd of buffer
std::cout << "fd: " << buf->fd() << std::endl;

// Get CPU accessible address
if (buf->map()) {
    std::cout << "CPU address: " << buf->addr() << std::endl;
}

// fd will auto close when buf leaves scope
```

## Contributing

We would love to have you as a part of the QRB ROS community. Whether you are helping us fix bugs, proposing new features, improving our documentation, or spreading the word, please refer to our [contribution guidelines](./CONTRIBUTING.md) and [code of conduct](./CODE_OF_CONDUCT.md).

- Bug report: If you see an error message or encounter failures, please create a [bug report](../../issues)
- Feature Request: If you have an idea or if there is a capability that is missing and would make development easier and more robust, please submit a [feature request](../../issues)

## Authors

* **Peng Wang** - *Maintainer* - [penww](https://github.com/penww)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

Project is licensed under the [BSD-3-Clause License](https://spdx.org/licenses/BSD-3-Clause.html). See [LICENSE](./LICENSE) for the full license text.
