set(GTEST_SHUFFLE 1)

add_executable(
    cpp-test
    main.cpp
    PositionEstimatorTests.cpp
    ParamParserTests.cpp
    HumanDetectorTests.cpp
    ../app/LabelParser.cpp
    ../app/PositionEstimator.cpp
    ../app/ParamParser.cpp
    ../app/utils.cpp
    ../app/Detection.cpp
    ../app/HumanDetector.cpp
    ../app/params_vec.cpp
)

set(Boost_USE_STATIC_LIBS OFF) 
set(Boost_USE_MULTITHREADED ON)  
set(Boost_USE_STATIC_RUNTIME OFF) 
find_package(Boost 1.45.0 COMPONENTS filesystem) 

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

target_include_directories(cpp-test PUBLIC ../vendor/googletest/googletest/include 
                                           ${CMAKE_SOURCE_DIR}/include)
target_link_libraries(cpp-test PUBLIC gtest ${OpenCV_LIBS} ${Boost_LIBRARIES})

include_directories(
    ${CMAKE_SOURCE_DIR}/include
)