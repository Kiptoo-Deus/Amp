# fm-torchknob

This is a JUCE plugin project integrated with PyTorch.

## Prerequisites

- **JUCE**: Clone the JUCE repository or download it from [JUCE](https://juce.com/get-juce).
- **PyTorch**: Download the libtorch C++ distribution from [PyTorch](https://pytorch.org/get-started/locally/) and place it in the `libtorch` directory.

## Setting Up the Project

1. **Clone the repository:**


2. **Download libtorch and place it in the `libtorch` directory:**

    ```sh
    mkdir -p libtorch
    cd libtorch
    # Download the appropriate version of libtorch for your platform
    tar -xzf libtorch-shared-with-deps-latest.zip
    cd ..
    ```

4. **Create a build directory and run CMake:**

    ```sh
    mkdir build
    cd build
    cmake ..
    cmake --build .
    ```

## Building the Plugin

To build the plugin, run:

```sh
cmake --build . --config Release


## Contributing
Contributions are welcome! Please fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

