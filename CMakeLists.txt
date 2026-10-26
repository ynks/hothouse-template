cmake_minimum_required(VERSION 4.0)

project(HothouseTemplate VERSION 1 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Dependencies
add_subdirectory(external/JUCE)

# Shared DSP
add_library(dsp INTERFACE)

target_include_directories(dsp INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/dsp
)

# Desktop plugin
juce_add_plugin(HothouseTemplate
    COMPANY_NAME "Xein"
    PLUGIN_MANUFACTURER_CODE Xein
    PLUGIN_CODE Temp
    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT FALSE
    NEEDS_MIDI_OUTPUT FALSE
    FORMATS VST3 Standalone
    PRODUCT_NAME "Hothouse Template"
    COPY_PLUGIN_AFTER_BUILD FALSE
)

target_sources(HothouseTemplate PRIVATE
        desktop/plugin_processor.cpp
        desktop/plugin_editor.cpp
)

target_compile_definitions(HothouseTemplate PUBLIC
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
)

target_link_libraries(HothouseTemplate
    PRIVATE
        dsp
        juce::juce_audio_utils
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)
