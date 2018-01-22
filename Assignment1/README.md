<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Assignment1

### Personal Interest
I'm interested in match equations whether the bitwise data parallel expression matching equation is reliable or not. Can these manipulations be improved more?

$$ Match(m, C) = Advance(CharClass(C) ∧ m) $$
$$ Match(m, RS) = Match(Match(m, R), S) $$
$$ Match(m, R|S) = Match(m, R) ∨ Match(m, S)) $$
$$ Match(m, C∗) = MatchStar(m, CharClass(C)) $$
$$ Match(m, R∗) = m ∨ Match(Match(m, R), R∗) $$
$$ Advance(m) = m + m $$
$$ MatchStar(m, C) = (((m ∧ C) + C) ⊕ C) ∨ m $$


### Problem Description
When I use command "cmake", I get the following warning.
```zsh
CMake Warning at /Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:5
Imported targets and dependency information not available for Boost version
(all versions older than 1.33)
Call Stack (most recent call first):
/Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:907 (_Boost_COMPONENT_DEPENDENCIES)
/Applications/CMake.app/Contents/share/cmake-3.10/Modules/FindBoost.cmake:1542 (_Boost_MISSING_DEPENDENCIES)
CMakeLists.txt:53 (find_package)
```

### Figure out my boost version and cmake version
Then I check the path  `/usr/local/Cellar/boost/1.66.0`

I know that maybe the reason that my boost version is higher than `1.33` that leads this problem.

I use command `cmake -version` to confirm my cmake version is `3.101`.

When I try to figure out the problem, I find the following url about
https://gitlab.kitware.com/cmake/cmake/blob/release/Modules/FindBoost.cmake
```c
if(Boost_VERSION VERSION_LESS 103300)
message(WARNING "Imported targets and dependency information not available for Boost version ${Boost_VERSION} (all versions older than 1.33)")
set(_Boost_IMPORTED_TARGETS FALSE)
```
It really confused my that since my cmake version is `1.66.0`. In the judgment statement, version is not less than 1.33.0
into the if structure. However, the statement output version within the structure is older than `1.33.0`



According to the following statement, I found that it does not recognize that boost version is `1.66.0`
```c
set(_Boost_KNOWN_VERSIONS ${Boost_ADDITIONAL_VERSIONS}
"1.65.1" "1.65.0" "1.65"
"1.64.0" "1.64" "1.63.0" "1.63" "1.62.0" "1.62" "1.61.0" "1.61" "1.60.0" "1.60"
"1.59.0" "1.59" "1.58.0" "1.58" "1.57.0" "1.57" "1.56.0" "1.56" "1.55.0" "1.55"
"1.54.0" "1.54" "1.53.0" "1.53" "1.52.0" "1.52" "1.51.0" "1.51"
```


The I find a issue reported on github. However, the issue has been closed.
https://github.com/dealii/dealii/issues/3765

It said:
No, that's not a problem
- we don't use imported targets in our build system configuration
- (it is already a mess - and with two different strategies of keeping track of external
- libraries it will get worse).

This answer makes me more confused.

