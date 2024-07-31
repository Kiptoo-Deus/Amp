cmake_minimum_required(VERSION 3.15)
project(minimal_plugin VERSION 0.0.1)

# Path to JUCE folder
set(JUCE_DIR ${CMAKE_SOURCE_DIR}/JUCE)

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

juce_add_plugin(minimal_plugin
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

juce_generate_juce_header(minimal_plugin)

target_sources(minimal_plugin
    PRIVATE
    src/PluginEditor.cpp
    src/PluginProcessor.cpp
)

target_compile_definitions(minimal_plugin
    PUBLIC
    JUCE_ALSA=1
    JUCE_DIRECTSOUND=1
    JUCE_DISABLE_CAUTIOUS_PARAMETER_ID_CHECKING=1
    JUCE_USE_OGGVORBIS=1
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
)

target_link_libraries(minimal_plugin
    PRIVATE
    juce::juce_audio_utils
    PUBLIC
    juce::juce_recommended_config_flags
    juce::juce_recommended_lto_flags
    juce::juce_recommended_warning_flags
)
