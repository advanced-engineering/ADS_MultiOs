set(SOURCES
  AdsDef.cpp
  AdsDevice.cpp
  AdsFile.cpp
  AdsLib.cpp
  ECatAccess.cpp
  Frame.cpp
  LicenseAccess.cpp
  Log.cpp
  MasterDcStatAccess.cpp
  RTimeAccess.cpp
  RegistryAccess.cpp
  RouterAccess.cpp
  Sockets.cpp
  SymbolAccess.cpp

  bhf/ParameterList.cpp

  standalone/AdsLib.cpp
  standalone/AmsConnection.cpp
  standalone/AmsNetId.cpp
  standalone/AmsPort.cpp
  standalone/AmsRouter.cpp
  standalone/NotificationDispatcher.cpp
)

add_library(ads ${SOURCES})
add_library(ads::ads ALIAS ads)

target_include_directories(ads PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
$<INSTALL_INTERFACE:include/ads>)

# Ensure ads target is built as a shared library
set_target_properties(ads PROPERTIES
    WINDOWS_EXPORT_ALL_SYMBOLS ON
    OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/Release
    OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/Debug
)

# Ensure NOMINMAX is defined for Windows/MSVC to avoid macro conflicts
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC" OR WIN32)
  target_compile_definitions(ads PUBLIC NOMINMAX=1)
endif()

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_link_libraries(ads PUBLIC wsock32)
endif()

if(WIN32 EQUAL 1)
    target_link_libraries(ads PUBLIC ws2_32)
endif()

target_link_libraries(ads PUBLIC Threads::Threads)
