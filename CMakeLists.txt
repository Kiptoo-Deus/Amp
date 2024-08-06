cmake_minimum_required(VERSION 3.15)
project(fm-torchknob VERSION 0.0.1)

# Path to JUCE folder
set(JUCE_DIR ${CMAKE_SOURCE_DIR}/JUCE)

# Path to libtorch folder
set(LIBTORCH_DIR ${CMAKE_SOURCE_DIR}/libtorch)

# Set CMake prefix path for libtorch
set(CMAKE_PREFIX_PATH "${LIBTORCH_DIR}/share/cmake/Torch")

# Ensure JUCE is added correctly
if(NOT EXISTS ${JUCE_DIR}/CMakeLists.txt)
    message(FATAL_ERROR "JUCE directory not found at ${JUCE_DIR}")
endif()

# Add JUCE directory
add_subdirectory(${JUCE_DIR} ${CMAKE_BINARY_DIR}/JUCE)

# Include JUCE modules path
list(APPEND CMAKE_MODULE_PATH ${JUCE_DIR}/extras/Build/CMake)

# Include JUCE helper functions
include(JUCEUtils)

# Find libtorch
find_package(Torch REQUIRED)

# Define the JUCE plugin
juce_add_plugin(fm-torchknob
    COMPANY_NAME "scandvia"
    IS_SYNTH TRUE
    NEEDS_MIDI_INPUT TRUE
    IS_MIDI_EFFECT FALSE
    NEEDS_MIDI_OUTPUT TRUE
    COPY_PLUGIN_AFTER_BUILD TRUE
    PLUGIN_MANUFACTURER_CODE Yeek
    PLUGIN_CODE Abc1
    FORMATS AU VST3 Standalone
    PRODUCT_NAME "AIPlugin"
)

juce_generate_juce_header(fm-torchknob)

target_sources(fm-torchknob
    PRIVATE
    src/PluginEditor.cpp
    src/PluginProcessor.cpp
)

target_compile_definitions(fm-torchknob
    PUBLIC
    JUCE_ALSA=1
    JUCE_DIRECTSOUND=1
    JUCE_DISABLE_CAUTIOUS_PARAMETER_ID_CHECKING=1
    JUCE_USE_OGGVORBIS=1
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
)

# Link JUCE libraries
target_link_libraries(fm-torchknob
    PRIVATE
    juce::juce_audio_utils
    PUBLIC
    juce::juce_recommended_config_flags
    juce::juce_recommended_lto_flags
    juce::juce_recommended_warning_flags
)

# Link against libtorch
target_link_libraries(fm-torchknob
    PRIVATE
    "${TORCH_LIBRARIES}"
)

# Include libtorch headers
target_include_directories(fm-torchknob
    PRIVATE
    "${TORCH_INCLUDE_DIRS}"
)

# Copy all libtorch DLLs to the output directory
if (MSVC)
    file(GLOB TORCH_DLLS "${LIBTORCH_DIR}/lib/*.dll")
    add_custom_command(TARGET fm-torchknob
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${TORCH_DLLS}
        $<TARGET_FILE_DIR:fm-torchknob>)
endif (MSVC)
