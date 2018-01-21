//===----------------------------------------------------------------------===//
//                   Bug reports on Cmake
//===----------------------------------------------------------------------===//

When I use command "cmake", I get the following warning.

CMake Warning at /Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:567 (message):
Imported targets and dependency information not available for Boost version
(all versions older than 1.33)
Call Stack (most recent call first):
/Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:907 (_Boost_COMPONENT_DEPENDENCIES)
/Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:1542 (_Boost_MISSING_DEPENDENCIES)
CMakeLists.txt:53 (find_package)

Then I use check the path "/usr/local/Cellar/boost/1.66.0", I know that maybe the reason that my boost version is higher than 1.33 that leads this problem.

I use command "cmake -version" to confirm my cmake version is 3.101.

When I try to figure out the problem, I find the following url.

https://gitlab.kitware.com/cmake/cmake/commit/ee1f8903322b443b263ec9638ab4851e7e5edf21

Up to the newest version Cake:
set(_Boost_KNOWN_VERSIONS ${Boost_ADDITIONAL_VERSIONS}
"1.65.1" "1.65.0" "1.65"
"1.64.0" "1.64" "1.63.0" "1.63" "1.62.0" "1.62" "1.61.0" "1.61" "1.60.0" "1.60"
"1.59.0" "1.59" "1.58.0" "1.58" "1.57.0" "1.57" "1.56.0" "1.56" "1.55.0" "1.55"
"1.54.0" "1.54" "1.53.0" "1.53" "1.52.0" "1.52" "1.51.0" "1.51"

It still cannot support boost version with 1.66.0.
The logic is:
if(NOT Boost_VERSION VERSION_LESS 103300 AND Boost_VERSION VERSION_LESS 103500)
...........
elseif(NOT Boost_VERSION VERSION_LESS 105900 AND Boost_VERSION VERSION_LESS 106000)
.............
else()
message(WARNING "Imported targets not available for Boost version ${Boost_VERSION}"
set(_Boost_IMPORTED_TARGETS FALSE)
endif()

The I find a issue reported on github. However, the issue has been closed.
It said:
No, that's not a problem - we don't use imported targets in our build system configuration (it is already a mess - and with two different strategies of keeping track of external libraries it will get worse).
