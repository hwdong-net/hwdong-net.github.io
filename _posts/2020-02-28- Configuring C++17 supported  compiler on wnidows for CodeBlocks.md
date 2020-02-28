---
layout:       post
title:        Configuring C++17 supported  compiler on wnidows for CodeBlocks
subtitle:      Configuring C++17 supported  compiler on wnidows for CodeBlocks
date:         2020-02-28 15:40:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - RL    
---    

## 1.  install mingw-64 on windows

#### 1.1 download  mingw-64
goto download link to download the install program: [mingw-w64-install ](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/mingw-w64-install.exe/download)

or download the mingW version [x86_64-win32-seh](https://sourceforge.net/projects/mingw-w64/files/)

![](../imgs/mingw.png)

#### 1.2 extracted the 7z compressed file to a foler using [7z](https://www.7-zip.org/download.html)

####  1.3  add the bin folder of mingw-64 to Windows PATH environment variable.


```cpp
#include <array>
#include <iostream>
#include <string_view>
#include <tuple>
#include <type_traits>
 
namespace a::b::c
{
    inline constexpr std::string_view str{ "hello" };
}
 
template <class... T>
std::tuple<std::size_t, std::common_type_t<T...>> sum(T... args)
{
    return { sizeof...(T), (args + ...) };
}
 
int main()
{
    auto [iNumbers, iSum]{ sum(1, 2, 3) };
    std::cout << a::b::c::str << ' ' << iNumbers << ' ' << iSum << '\n';
 
    std::array arr{ 1, 2, 3 };
 
    std::cout << std::size(arr) << '\n';
 
    return 0;
}
```

Refer to 

1. [https://code.visualstudio.com/docs/cpp/config-mingw](https://code.visualstudio.com/docs/cpp/config-mingw)
