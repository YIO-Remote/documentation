---
title: "Coding guidelines"
description: "The is the coding standardization for the YIO Remote project."
menu:
  developers:
    parent: "home"
weight: 30
---

## Files

Common rules for project files:

- No spaces allowed in file and directory names
- Code and text files:
  - No tabs. Use 4 spaces instead.
  - Max line length: 120 characters.
  - UTF-8 encoding (except it's a special system file required to be in another encoding).
    - Do not use the BOM character in UTF-8 files.  
      It's of no use and certain tools will fail miserable when encountering a BOM (e.g. cpplint).
  - Use proper linefeeds:
    - Never hard code line feeds. It won't work on all platforms.  
      For C++ / Qt use: `endl`
    - Use Git `core.autocrlf true`
    - Unix shell scripts must be `\r`
- Each code file should have a GPL-3.0-or-later license header.
  - The license template is provided in remote-software/license-template.txt
  - This is mandatory for CPP files and checked with cpplint.
    - Qt Creator setting: **Tools -> Options... -> C++ -> File Naming**: `License template`
  - For JavaScript file a short one-liner is sufficient:  
    `// @License GPL-3.0-or-later - please refer to LICENSE.md file for details`

## Qt C++

We are using the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) as main reference.
See validation below for the few exceptions to the guide.

Additional rules to the Google C++ Style Guide:

- Use the modern `#pragma once` instead of the traditional `#ifndef` header guards
  - Qt Creator setting: **Tools -> Options... -> C++ -> File Naming**: Check `Include guards`
- CPP files:
  - Use lower case naming.
    - Qt Creator setting: **Tools -> Options... -> C++ -> File Naming**: Check `Lower case file names`
  - dash character `-` is not allowed.

### Validation

Style validation is performed with [cpplint](https://github.com/cpplint/cpplint) on each push with a GitHub Action.  
If the style violates the guidelines the check will fail and associated with the commit.

Filter settings for cpplint:

| Filter                | Description                         |
| --------------------- | ----------------------------------- |
| -build/c++11          | We don't restrict C++ 11 features   |
| -build/include_subdir | Header files are in the same folder |

Options for cpplint:

| Option           | Description                        |
| ---------------- | ---------------------------------- |
| --linelength=120 | The area of 3270 terminals is over |
| --root=sources   | Source directory                   |

#### Run cpplint locally

Install cpplint (Python installation required):

    $ sudo pip3 install --system cpplint

Run validation in the project's root directory:

    $ cpplint --filter=-build/include_subdir,-build/c++11 --linelength=120 --root=sources --recursive sources

Note: cpplint version 1.5.2 or newer is required, otherwise there will be false positives with Qt signal definitions!

### Qt Creator

1.  Install ClangFormat for your OS:

    - Linux Ubuntu:

          sudo apt-get install clang-format

    - macOS using [Homebrew](https://brew.sh/):

          brew install clang-format

      Clang Format command: `/usr/local/bin/clang-format`

    - Windows:
      1. Download a pre-built binary from <http://releases.llvm.org/download.html#9.0.0>.
      2. Install the package and add the path of %LLVM%\bin to your system environment.

2.  In Qt Creator go to **Help > About Plugins > C++ > Beautifier** and enable the plugin.
3.  Restart Qt.
4.  Go to **Tools -> Options -> Beautifier -> Clang Format**
5.  Verify the `Clang Format command` if it matches the installed version from step 1.
6.  For projects containing a clang format file `.clang-format` in the project's root directory:
    - Set `Use predefined style` to: `File`.
    - Set `Fallback style` to: `Google`
7.  Otherwise set `Use customized style` and add the following definition:

        BasedOnStyle: Google

        IndentWidth: 4
        ColumnLimit: 120

        AccessModifierOffset: -3
        AlignConsecutiveDeclarations: true

8.  To format a source file with the _Beautifier_ definition:  
    **Choose Tools -> Beautifier -> ClangFormat -> Format Current File**

### Examples

#### Curly Brackets

- Same line as the function definition
- The else goes on the same line as the closing bracket.
- Curly brackets are always required, even for single statements!

```
bool Foo::bar(const QString &newName) {
    bool success = m_file.rename(newName);
    if (success) {
        emit nameChanged(newName);
    } else {
        qCWarning(CLASS_LC) << "Error renaming file:" << m_file;
    }
    return success;
}
```

## Qt QML

TBD
