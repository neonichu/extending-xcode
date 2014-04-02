# Extending Xcode

## CocoaHeads Hamburg, April 2014

### Boris Bügling - @NeoNacho

![](Xcode.png)

![20%, original, inline](contentful.png)

---

![100%](oh-hi-there-turtle.jpg)

---

# Agenda

- Xcode
- Using plugins
- Developing your own plugin

![](agenda.jpg)

---

# AppCode?

.

![90%, original](appcode.png)

---

# Xcode

![](Welcome_Xcode.png)

---

Plugins!

![100%, right](Xcode_Plugins_Folder.png)

---

# Possibilities

- Color Schemes
- File Templates
- Project Templates
- Plugins

![](possibilities.jpg)

---

# Using Plugins

![](using.jpg)

---

## Alcatraz

The Xcode package manager

http://alcatraz.io

Marin and Delisa

![80%, left, inline](Alcatraz_Team.png)

![right](Alcatraz.png)

---

![100%, left](alcatraz_menu.png)

opens

![100%, right](alcatraz_screenshot.png)

---

### How does it work?

- *packages.json* contains GitHub URL
- Clones the repository
- Runs *xcodebuild* with some parameters

![](background.jpg)

---

# Some cool plugins

![](Frozen-Ice-Cube-Wallpaper.jpg)

---

### ClangFormat

![](clangformat-author.png)

---

### FuzzyAutoComplete

![](fuzzyautocomplete-author.png)

---

### KSImageNamed

![](ksimagenamed-author.png)

---

### OMColorSense

![](omcolorsense-author.png)

---

### ShowInGithub

![](showingithub-author.png)

---

### VVDocumenter-Xcode

![](vvdocumenter-author.png)

---

### Xcode\_beginning\_of\_line

![](beginning-of-line-author.png)

---

### XcodeColors

![](xcodecolors-author.png)

---

# Developing your own plugin

![](Xcode_quit_1.png)

---

# Getting started

- Clone *https://github.com/kattrali/Xcode5-Plugin-Template*
- Put it into *~/Library/Developer/Xcode/Templates/Project Templates/Application Plug-in/Xcode5 Plugin.xctemplate*

![](background.jpg)

---

![100%, original](New_plugin.png)

---

# Plugin Template

- Xcode 5.1 compatible
- Shows a menu item
- On build, plugin ends up here: *~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/*
- Just restart and it shows up

![](background.jpg)

---

### Once you build and restart Xcode

.

![100%, original](Dummy_Plugin_In_Action.png)

---

![](The-Office.gif)

---

https://github.com/neonichu/extending-xcode/

![](ironcat.jpg)
