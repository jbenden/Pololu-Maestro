set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)


find_package(Qt5 COMPONENTS Widgets Xml REQUIRED)

add_executable(MaestroControlCenter WIN32 MACOSX_BUNDLE
    MainWindow.h
    MainWindow.cpp
    MainWindow.ui
    QxChannelStatus.h
    QxChannelStatus.cpp
    ConfigurationFile.h
    ConfigurationFile.cpp
    main.cpp
)

target_link_libraries(MaestroControlCenter Qt5::Widgets Qt5::Xml usc)

install(TARGETS MaestroControlCenter BUNDLE DESTINATION bin)

# Automatically deploy qt
get_target_property(QT5_QMAKE_EXECUTABLE Qt5::qmake IMPORTED_LOCATION)
get_filename_component(QT5_BIN_DIR ${QT5_QMAKE_EXECUTABLE} PATH)
if (APPLE)
    find_program(QT5_DEPLOYQT_EXECUTABLE macdeployqt HINTS "${QT5_BIN_DIR}")
    install(CODE "EXECUTE_PROCESS(COMMAND ${QT5_DEPLOYQT_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/MaestroControlCenter.app)")

    install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/MaestroControlCenter.app/Contents/Frameworks DESTINATION bin/MaestroControlCenter.app/Contents)
    install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/MaestroControlCenter.app/Contents/PlugIns DESTINATION bin/MaestroControlCenter.app/Contents)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/MaestroControlCenter.app/Contents/Resources/qt.conf DESTINATION bin/MaestroControlCenter.app/Contents/Resources)
elseif (WIN32)
    find_program(QT5_DEPLOYQT_EXECUTABLE windeployqt HINTS "${QT5_BIN_DIR}")
    install(CODE "EXECUTE_PROCESS(COMMAND ${QT5_DEPLOYQT_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/MaestroControlCenter.exe)")

    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Qt5Core.dll DESTINATION bin)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Qt5Gui.dll DESTINATION bin)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Qt5Widgets.dll DESTINATION bin)
    install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/platforms DESTINATION bin)
    install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/styles DESTINATION bin)
    include(InstallRequiredSystemLibraries)
else ()
    # Assume linux
endif ()
