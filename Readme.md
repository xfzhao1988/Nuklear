# Nuklear

[![](https://github.com/Immediate-Mode-UI/Nuklear/workflows/C%2FC++%20CI/badge.svg )](https://github.com/Immediate-Mode-UI/Nuklear/actions)

This is a minimal-state, immediate-mode graphical user interface toolkit
written in ANSI C and licensed under public domain. It was designed as a simple
embeddable user interface for application and does not have any dependencies,
a default render backend or OS window/input handling but instead provides a
highly modular, library-based approach, with simple input state for input and
draw commands describing primitive shapes as output. So instead of providing a
layered library that tries to abstract over a number of platform and
render backends, it focuses only on the actual UI.
这是一个最小状态、即时模式的图形用户界面工具包，用 ANSI C 编写，并在公共领域获得许可。它被
设计为应用程序的简单嵌入式用户界面，没有任何依赖项、默认渲染后端或操作系统窗口/输入处理，
而是提供高度模块化、基于库的方法，使用简单的输入状态作为输入，使用描述原始形状的绘制命令作为输出。
因此，它只关注实际的 UI，而不是提供试图抽象多个平台和渲染后端的分层库。

## Features 特征

- Immediate-mode graphical user interface toolkit
  即时模式图形用户界面工具包
- Single-header library
  单头库
- Written in C89 (ANSI C)
  使用 C89 (ANSI C) 编写
- Small codebase (~18kLOC) //LOC:lines of code的缩写
  代码库较小 (~18kLOC)
- Focus on portability, efficiency and simplicity
  注重可移植性、效率和简单性
- No dependencies (not even the standard library if not wanted)
  没有依赖项（如果不需要，甚至没有标准库）
- Fully skinnable and customizable
  完全可换肤和可定制
- Low memory footprint with total control of memory usage if needed / wanted
  低内存占用，并可根据需要完全控制内存使用情况
- UTF-8 support
  UTF-8 支持
- No global or hidden state
  没有全局或隐藏状态
- Customizable library modules (you can compile and use only what you need)
  可定制的库模块（您可以只编译和使用您需要的部分）
- Optional font baker and vertex buffer output
  可选的字体烘焙器和顶点缓冲区输出
- [Documentation](https://Immediate-Mode-UI.github.io/Nuklear/doc/nuklear.html)

## Building 构建

This library is self-contained in one single header file and can be used either
in header-only mode or in implementation mode. The header-only mode is used
by default when included and allows including this header in other headers
and does not contain the actual implementation.
此库包含在一个单独的头文件中，可在仅标头模式或实现模式下使用。包含时默认使用仅标头模式，
允许将此标头包含在其他标头中，但不包含实际实现。

The implementation mode requires defining the preprocessor macro
`NK_IMPLEMENTATION` in *one* .c/.cpp file before `#include`ing this file, e.g.:
实现模式需要在一个 .c/.cpp 文件中定义预处理器宏“NK_IMPLEMENTATION”，然后才能包含改文件，例如：
```c
#define NK_IMPLEMENTATION
#include "nuklear.h"
```
IMPORTANT: Every time you include "nuklear.h" you have to define the same optional flags.
This is very important; not doing it either leads to compiler errors, or even worse, stack corruptions.
重要提示：每次包含“nuklear.h”时，都必须定义相同的可选标志。
这非常重要；不这样做会导致编译器错误，甚至更糟的是，导致堆栈损坏。

## Gallery 画廊

![screenshot](https://cloud.githubusercontent.com/assets/8057201/11761525/ae06f0ca-a0c6-11e5-819d-5610b25f6ef4.gif)
![screen](https://cloud.githubusercontent.com/assets/8057201/13538240/acd96876-e249-11e5-9547-5ac0b19667a0.png)
![screen2](https://cloud.githubusercontent.com/assets/8057201/13538243/b04acd4c-e249-11e5-8fd2-ad7744a5b446.png)
![node](https://cloud.githubusercontent.com/assets/8057201/9976995/e81ac04a-5ef7-11e5-872b-acd54fbeee03.gif)
![skinning](https://cloud.githubusercontent.com/assets/8057201/15991632/76494854-30b8-11e6-9555-a69840d0d50b.png)
![gamepad](https://cloud.githubusercontent.com/assets/8057201/14902576/339926a8-0d9c-11e6-9fee-a8b73af04473.png)

## Example

```c
/* init gui state */
struct nk_context ctx;
nk_init_fixed(&ctx, calloc(1, MAX_MEMORY), MAX_MEMORY, &font);

enum {EASY, HARD};
static int op = EASY;
static float value = 0.6f;
static int i =  20;

if (nk_begin(&ctx, "Show", nk_rect(50, 50, 220, 220),
    NK_WINDOW_BORDER|NK_WINDOW_MOVABLE|NK_WINDOW_CLOSABLE)) {
    /* fixed widget pixel width */
    nk_layout_row_static(&ctx, 30, 80, 1);
    if (nk_button_label(&ctx, "button")) {
        /* event handling */
    }

    /* fixed widget window ratio width */
    nk_layout_row_dynamic(&ctx, 30, 2);
    if (nk_option_label(&ctx, "easy", op == EASY)) op = EASY;
    if (nk_option_label(&ctx, "hard", op == HARD)) op = HARD;

    /* custom widget pixel width */
    nk_layout_row_begin(&ctx, NK_STATIC, 30, 2);
    {
        nk_layout_row_push(&ctx, 50);
        nk_label(&ctx, "Volume:", NK_TEXT_LEFT);
        nk_layout_row_push(&ctx, 110);
        nk_slider_float(&ctx, 0, &value, 1.0f, 0.1f);
    }
    nk_layout_row_end(&ctx);
}
nk_end(&ctx);
```
![example](https://cloud.githubusercontent.com/assets/8057201/10187981/584ecd68-675c-11e5-897c-822ef534a876.png)

## Bindings 绑定
There are a number of nuklear bindings for different languages created by other authors.
其他作者为不同语言创建了许多核心绑定。
I cannot attest for their quality since I am not necessarily proficient in any of these
languages. Furthermore there are no guarantee that all bindings will always be kept up to date:
我无法保证它们的质量，因为我不一定精通这些语言中的任何一种。此外，不能保证所有绑定始终保持最新：

- [Java](https://github.com/glegris/nuklear4j) by Guillaume Legris
- [D](https://github.com/Timu5/bindbc-nuklear) by Mateusz Muszyński
- [Golang](https://github.com/golang-ui/nuklear) by golang-ui@github.com
- [Rust](https://github.com/snuk182/nuklear-rust) by snuk182@github.com
- [Chicken](https://github.com/wasamasa/nuklear) by wasamasa@github.com
- [Nim](https://github.com/zacharycarter/nuklear-nim) by zacharycarter@github.com
- Lua
  - [LÖVE-Nuklear](https://github.com/keharriso/love-nuklear) by Kevin Harrison
  - [MoonNuklear](https://github.com/stetre/moonnuklear) by Stefano Trettel
- Python
  - [pyNuklear](https://github.com/billsix/pyNuklear) by William Emerison Six (ctypes-based wrapper)
  - [pynk](https://github.com/nathanrw/nuklear-cffi) by nathanrw@github.com (cffi binding)
- [CSharp/.NET](https://github.com/cartman300/NuklearDotNet) by cartman300@github.com
- [V](https://github.com/nsauzede/vnk) by Nicolas Sauzede

## Credits 致谢
Developed by Micha Mettke and every direct or indirect contributor to the GitHub.
由 Micha Mettke 和 GitHub 的每一位直接或间接贡献者开发。


Embeds `stb_texedit`, `stb_truetype` and `stb_rectpack` by Sean Barrett (public domain)
Embeds `ProggyClean.ttf` font by Tristan Grimmer (MIT license).
嵌入肖恩·巴雷特的“stb_texedit”、“stb_truetype”和“stb_rectpack”（公共领域）
嵌入Tristan Grimmer的ProggyClean. ttf字体（MIT许可证）。

Big thank you to Omar Cornut (ocornut@github) for his [imgui](https://github.com/ocornut/imgui) library and
giving me the inspiration for this library, Casey Muratori for handmade hero
and his original immediate-mode graphical user interface idea and Sean
Barrett for his amazing single-header [libraries](https://github.com/nothings/stb) which restored my faith
in libraries and brought me to create some of my own. Finally Apoorva Joshi for his single-header [file packer](http://apoorvaj.io/single-header-packer.html).
非常感谢 Omar Cornut (ocornut@github) 提供的 [imgui](https://github.com/ocornut/imgui) 库，它给了我创建这个库的灵感；
感谢 Casey Muratori 提供的 handmade hero 以及他原创的即时模式图形用户界面创意；
感谢 Sean Barrett 提供的令人惊叹的单头 [库 (https://github.com/nothings/stb)]，它让我重拾了对库的信心，并让我创建了一些自己的库。
最后，感谢 Apoorva Joshi 提供的单头 [文件打包器](http://apoorvaj.io/single-header packer.html)。

## License 许可证
```
------------------------------------------------------------------------------
This software is available under 2 licenses -- choose whichever you prefer.
该软件有 2 种许可证 - 请选择您喜欢的那种。
------------------------------------------------------------------------------
ALTERNATIVE A - MIT License
Copyright (c) 2017 Micha Mettke
Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
备选A - MIT许可
MIT许可
版权所有 (c) 2017 Micha Mettke

特此免费授予获得本软件及相关文档文件（“软件”）副本的任何人处理本软件的权利，包括但不限于使用、复制、
修改、合并、发布、分发、再许可和/或销售本软件副本的权利，并允许本软件提供给他人，遵守以下条件：
上述版权声明和本许可声明应包含在本软件的所有副本或主要部分中。

本软件按“原样”提供，不提供任何明示或暗示的担保，包括但不限于对适销性、特定用途的适用性和非侵权的担保。
在任何情况下，作者或版权持有人均不对因本软件或使用本软件或其他交易中产生的任何索赔、损害或其他责任负责，
无论是在合同诉讼、侵权行为或其他方面。

------------------------------------------------------------------------------
ALTERNATIVE B - Public Domain (www.unlicense.org)
This is free and unencumbered software released into the public domain.
Anyone is free to copy, modify, publish, use, compile, sell, or distribute this
software, either in source code form or as a compiled binary, for any purpose,
commercial or non-commercial, and by any means.
In jurisdictions that recognize copyright laws, the author or authors of this
software dedicate any and all copyright interest in the software to the public
domain. We make this dedication for the benefit of the public at large and to
the detriment of our heirs and successors. We intend this dedication to be an
overt act of relinquishment in perpetuity of all present and future rights to
this software under copyright law.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN
ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
备选B - 公有领域（Public Domain）
这是一款释放到公有领域的自由且不受约束的软件。
任何人都可以自由地复制、修改、发布、使用、编译、出售或分发本软件，无论是源代码形式还是编译后的二进制形式，出于任何目的，
无论是商业用途还是非商业用途，均可使用。

在承认版权法的司法管辖区内，本软件的作者或作者将所有版权利益献给公有领域。我们做此奉献是为了全体公众的利益，
并损害我们的继承人和继任者。我们希望这种奉献成为放弃现在和未来对本软件所有权利的公开行为。

本软件按“原样”提供，不提供任何明示或暗示的担保，包括但不限于对适销性、特定用途的适用性和非侵权的担保。
在任何情况下，作者均不对因本软件或使用本软件或其他交易中产生的任何索赔、损害或其他责任负责，
无论是在合同诉讼、侵权行为或其他方面。

-----------------------------------------------------------------------------
```

## Reviewers guide 审稿人指南

When reviewing pull request there are common things a reviewer should keep
in mind.
在审查拉取请求时，审查者应该记住一些常见事项。

Reviewing changes to `src/*` and `nuklear.h`:
审查对 `src/*` 和 `nuklear.h` 的更改：

* Ensure C89 compatibility.
  确保 C89 兼容性。
* The code should work for several backends to an acceptable degree.
  该代码应该可以在可接受的程度上适用于多个后端。
* Check no other parts of `nuklear.h` are related to the PR and thus nothing is missing.
  检查“nuklear.h”中的其他部分是否与 PR 相关，因此没有遗漏任何内容。
* Recommend simple optimizations.
  建议进行简单的优化。
  * Pass small structs by value instead of by pointer.
    通过值而不是指针传递小结构。
  * Use local buffers over heap allocation when possible.
    尽可能使用本地缓冲区而不是堆分配。
* Check that the coding style is consistent with code around it.
  检查编码风格是否与周围的代码一致。
  * Variable/function name casing.
    变量/函数名称大小写。
  * Indentation.
    缩进。
  * Curly bracket (`{}`) placement.
    花括号（`{}`）的位置
* Ensure that the contributor has bumped the appropriate version in
  确保贡献者已将适当的版本添加到
  [clib.json](https://github.com/Immediate-Mode-UI/Nuklear/blob/master/clib.json)
  and added their changes to the
  并将他们的更改添加到
  [CHANGELOG](https://github.com/Immediate-Mode-UI/Nuklear/blob/master/src/CHANGELOG).
* Have at least one other person review the changes before merging.
  合并之前请至少另外一个人审核更改。

Reviewing changes to `demo/*`, `example/*` and other files in the repo:
审查仓库中 `demo/*`、`example/*` 和其他文件的变更：

* Focus on getting working code merged.
  专注于合并工作代码。
  * We want to make it easy for people to get started with Nuklear, and any
    `demo` and `example` improvements helps in this regard.
    我们希望让人们能够轻松开始使用 Nuklear，任何“演示”和“示例”改进都有助于实现这一点。
* Use of newer C features, or even other languages is not discouraged.
  不鼓励使用较新的 C 特性，甚至其他语言。
  * If another language is used, ensure that the build process is easy to figure out.
    如果使用其他语言，请确保构建过程易于理解。
* Messy or less efficient code can be merged so long as these outliers are pointed out
  and easy to find.
  只要指出这些异常值并易于查找，就可以合并混乱或效率较低的代码。
* Version shouldn't be bumped for these changes.
  不应因这些更改而改变版本。
* Changes that improves code to be more inline with `nuklear.h` are ofc always welcome.
  改进代码以使其与 `nuklear.h` 更加一致的变化当然总是受欢迎的。

