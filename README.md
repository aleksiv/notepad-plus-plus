This is an (unofficial) fork for use with Windows Store Publishing
===================
<a href='//www.microsoft.com/store/apps/9PLHC123MQWT?ocid=badge'><img src='https://assets.windowsphone.com/85864462-9c82-451e-9355-a3d5f874397a/English_get-it-from-MS_InvariantCulture_Default.png' alt='English badge' width="100"/></a>
[![Build status](https://ci.appveyor.com/api/projects/status/4uq2fo7ki1hhmpk6?svg=true)](https://ci.appveyor.com/project/jakevis/notepad-plus-plus)
[![GitHub release](https://img.shields.io/github/release/nightri-se/notepad-plus-plus.svg)](https://github.com/NightRi-se/notepad-plus-plus/releases)

I have cloned the original repo and published changes to support publishing the app in the windows store. This is required to get notepad++ running on Win10s devices. You can find it instore at: https://www.microsoft.com/store/apps/9PLHC123MQWT

Current list of changes:
1. Removal of the run Diag (you cant open a command prompt on 10s) - see https://github.com/NightRi-se/notepad-plus-plus/blob/master/PowerEditor/src/WinControls/StaticDialog/RunDlg/RunDlg.rc for most of the required changes.
2. Added the NightRise LLC codesign cert for validation - see https://github.com/NightRi-se/notepad-plus-plus/blob/master/PowerEditor/src/ScitillaComponent/ScintillaEditView.cpp
3. Edited https://github.com/NightRi-se/notepad-plus-plus/blob/master/appveyor.yml to support building and packaging with makeappx.
4. The packaging directory, including binary plugins and config files is here: https://github.com/NightRi-se/notepad-plus-plus/tree/master/NightRise.NotepadWrapped


Packaging notes - originally the following applied. Now please see Appveyor for autobuild instructions:
1. Genuine app was downloaded and unzipped (non-installer version)
2. the exe was recompiled with listed changes above
3. exe was replaced in the original app unzipped location
4. app was packaged using Desktop App Converter (https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter) - see https://github.com/NightRi-se/notepad-plus-plus/wiki/(Re)Packaging-Instructions
6. app is published to the store here: https://www.microsoft.com/store/apps/9PLHC123MQWT

All credit to the original team, especially @donho. All licences stay the same (GPL unless otherwise stated).

If you like this app in the store, please feel free to add your comments to this thread: https://notepad-plus-plus.org/community/topic/14734/notepad-uwp-package-in-windows-store-almost-there

What is Notepad++ ?
===================

[![Join the disscussions at https://notepad-plus-plus.org/community/](https://notepad-plus-plus.org/assets/images/NppCommunityBadge.svg)](https://notepad-plus-plus.org/community/)
&nbsp;&nbsp;&nbsp;&nbsp;[![Join the chat at https://gitter.im/notepad-plus-plus/notepad-plus-plus](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/notepad-plus-plus/notepad-plus-plus?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Notepad++ is a free (free as in both "free speech" and "free beer") source code
editor and Notepad replacement that supports several programming languages and
natural languages. Running in the MS Windows environment, its use is governed by
GPL License.

Build Status
------------

[![Appveyor build status](https://ci.appveyor.com/api/projects/status/github/notepad-plus-plus/notepad-plus-plus?branch=master&svg=true)](https://ci.appveyor.com/project/donho/notepad-plus-plus)
[![GitHub release](https://img.shields.io/github/release/notepad-plus-plus/notepad-plus-plus.svg)]()

To build Notepad++ from source:
-------------------------------

There are two components that need to be built separately:

 - `notepad++.exe`: (depends on `SciLexer.dll`)
 - `SciLexer.dll` : (with nmake)

You can build Notepad++ with *or* without Boost - The release build of
Notepad++ is built **with** Boost.

Since `Notepad++` version 6.0, the build of `SciLexer.dll` that is distributed
uses features from Boost's `Boost.Regex` library.

You can build SciLexer.dll without Boost, ie. with its default POSIX regular
expression support instead of boost's PCRE one. This is useful if you would
like to debug Notepad++, but don't have boost.

## To build `notepad++.exe`:

 1. Open [`PowerEditor\visual.net\notepadPlus.vcxproj`](https://github.com/notepad-plus-plus/notepad-plus-plus/blob/master/PowerEditor/visual.net/notepadPlus.vcxproj)
 2. Build Notepad++ [like a normal Visual Studio project](https://msdn.microsoft.com/en-us/library/7s88b19e.aspx).



## To build `SciLexer.dll` with boost:

Here are the instructions to build SciLexer.dll (for both 32-bit & 64-bit) for Notepad++:

 1. Download the [Boost source code](https://sourceforge.net/projects/boost/files/boost/1.55.0/).
    v1.55 should be used with VS 2013. Then unzip it. In my case, `boost_1_55_0` is copied in `C:\sources\`
 2. Go to `scintilla\boostregex\` then run BuildBoost.bat with your boost path.
    In my case: `BuildBoost.bat C:\sources\boost_1_55_0`
	If you are compiling a 64 bit Scintilla under your *VS2013 x64 Native tool command prompt*, add `-x64` flag.
	In my case: `BuildBoost.bat C:\sources\boost_1_55_0 -x64`
 3. Go in `scintilla\win32\` then run `nmake -f scintilla.mak`



## To build `SciLexer.dll` *without* boost:

This will work with `notepad++.exe`, however some functionality in Notepad++ will be broken.

To build SciLexer.dll without PCRE support (for both 32-bit & 64-bit):

 1. For 32-bit, open a command prompt *for building* ([a.k.a. the *Developer Command Prompt for VS2013*](https://msdn.microsoft.com/en-us/library/f2ccy3wt.aspx))
    - From the IDE, you can do this by right clicking on a file in Solution Explorer,
      and clicking "Open Command Prompt". This will open up a command prompt with all the proper environment variables.
    - From the Windows Start screen/menu, type `Developer Command Prompt for VS2013`,
      and click/select the result.
    - From an *already open* command prompt, run `vcvarsall.bat`
      (e.g. "C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat").

	For 64-bit, open *VS2013 x64 Native tool command prompt*.

 2. Change directory (`cd` or `pushd`) to `scintilla\win32\`

 3. Build `SciLexer.dll` with one of the following commands:
    - `nmake NOBOOST=1 -f scintilla.mak`         (normal build)
    - `nmake NOBOOST=1 DEBUG=1 -f scintilla.mak` (debugging build)

 4. Copy `SciLexer.dll` from `scintilla\bin\` to the same directory as `notepad++.exe`.
    - For the `Unicode Release` configuration, the output directory
      (where `notepad++.exe` is) is `PowerEditor\bin\`.
    - For the `Unicode Debug` configuration, the output directory
      (where `notepad++.exe` is) is `PowerEditor\visual.net\Unicode Debug\`.


See the [Notepad++ official site](https://notepad-plus-plus.org/) for more information.

[Notepad++ Contributors](https://notepad-plus-plus.org/contributors)
