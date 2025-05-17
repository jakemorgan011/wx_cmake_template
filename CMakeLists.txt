cmake_minimum_required(VERSION 3.16 FATAL_ERROR) # 3.16 is minimum for juce obj-c++

project(multi_dependencies LANGUAGES C CXX OBJCXX)

include(FetchContent)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(wxBUILD_SHARED OFF)

message(STATUS "Fetching wxWidgets...")

FetchContent_Declare(
   wxWidgets
   GIT_REPOSITORY https://github.com/wxWidgets/wxWidgets.git
   GIT_SHALLOW ON
)

message(STATUS "Fetching JUCE...")

FetchContent_Declare(
  juce
  GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
  GIT_SHALLOW ON 
)

FetchContent_MakeAvailable(wxWidgets juce)

set(SRCS main.cpp)

if(APPLE)
    # create bundle on apple compiles
    add_executable(main MACOSX_BUNDLE ${SRCS})
    # juce needs these apple frameworks in order to run properly on macos
    target_link_libraries(main PRIVATE "-framework Cocoa" "-framework CoreAudio" "-framework WebKit")

    # Set a custom plist file for the app bundle - needed for Mac OS Retina display
    set_target_properties(main PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist)
else()
    # the WIN32 is needed for Windows in order for it to look for WinMain
    # instead of the main function. This is ignored on other systems,
    # so it works on all platforms
    add_executable(main WIN32 ${SRCS} main.exe.manifest)
endif()

# =======================================
#
#             audio processor
#             audio basics
#             audio devices
#
# =======================================

target_link_libraries(main PRIVATE wxcore wxnet juce::juce_audio_basics juce::juce_audio_devices juce::juce_audio_processors)
