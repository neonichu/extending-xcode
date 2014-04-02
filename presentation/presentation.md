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
- Use plugins
- Develop your own plugin

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

# Use Plugins

![](using.jpg)

---

### How do I even...

![](no-idea.jpg)

---

@_supermarin

![](supermarin.jpg)

---

## Alcatraz

The Xcode package manager

http://alcatraz.io

Marin, Delisa and Jurre

![77%, left, inline](Alcatraz_Team.png)

![right](Alcatraz.png)

---

*curl -fsSL https://raw.github.com/supermarin/Alcatraz/master/Scripts/install.sh | sh*

![](background.jpg)

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

# Develop your own plugin

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

### Let's build something useful...

.

![100%, original](Orta_Tweet.png)

---

# General points

![](generalrelativity.jpg)

---

# Compatibility UUIDs

### This might appear in your *system.log*

    [MT] PluginLoading: Required plug-in compatibility UUID 640F884E-CE55-4B40-87C0-8869546CAB7A
    for plug-in at path '~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/CocoaPodsPlugIn.xcplugin'
    not present in DVTPlugInCompatibilityUUIDs

    $ defaults read /Applications/Xcode51-DP2.app/Contents/Info DVTPlugInCompatibilityUUID
    640F884E-CE55-4B40-87C0-8869546CAB7A

### Add that UUID to your plugin's *Info.plist*

![](background.jpg)

---

### This will happen all the time

![100%, filtered](Xcode_quit_1.png)

![100%, filtered](Xcode_quit_2.png)

---

# Your only friends...

    $ tail -f /var/log/system.log

    $ rm -rf ~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/*

- Debug from the command line with *lldb*...
- or with a second instance of Xcode

![](background.jpg)

---

# Headers

Use *class-dump* yourself, or just grab
https://github.com/luisobo/Xcode5-RuntimeHeaders

![](background.jpg)

---

# What we are looking for

- How to detect if the user types in the editor?
- How to hide the debug pane?

![](background.jpg)

---

`grep -ri editor *`

![](background.jpg)

---

    @interface IDEWorkspaceWindowController : NSWindowController
          <NSWindowDelegate,
           IDEEditorAreaContainer,
           DVTStatefulObject,
           DVTTabbedWindowControlling,
           DVTEditor,
           DVTInvalidation>

    [...]

    @property(readonly) IDEEditorArea *editorArea;

    [...]

    @end

![](background.jpg)

---

    @interface IDEEditorArea : IDEViewController <IDEDebuggerBarEditorInfoProvider>

    [...]

    - (void)toggleDebuggerVisibility:(id)arg1;
    - (void)activateConsole:(id)arg1;
    @property BOOL showDebuggerArea;

    [...]

    @end

![](background.jpg)

---

    - (void)toggleDebuggersIfNeeded {
      for (NSWindowController *workspaceWindowController in
            [objc_getClass("IDEWorkspaceWindowController")
              workspaceWindowControllers]) {
        id editorArea = [workspaceWindowController editorArea];
        if ([editorArea showDebuggerArea]) {
            [editorArea toggleDebuggerVisibility:nil];
        }
      }
    }

![](background.jpg)

---

    @interface NSObject (ShutUpWarnings)

    -(id)editorArea;
    -(BOOL)showDebuggerArea;
    -(void)toggleDebuggerVisibility:(id)arg;
    -(NSArray*)workspaceWindowControllers;

    @end

![](background.jpg)

---

# Grepping through _subtreeDescription

^ Logged view hierarchy under IDEWorkspaceWindowController

`grep -i source *`

    [   AF O P LU ] h=--- v=--- NSClipView 0x7f822e93e990 f=(35,0,885,662) b=(0,637,-,-)
      TIME drawRect: min/mean/max 0.00/0.00/0.00 ms
    [   AF O   LU ] h=-&- v=-&- DVTSourceTextView 0x7f822c723f00 f=(0,0,885,1339) b=(-)
      TIME drawRect: min/mean/max 0.00/0.00/0.00 ms
    [   A      LU ] h=--- v=--- DVTMessageBubbleView 0x7f822eb5c080 f=(638,975,247,12) b=(-)
      TIME drawRect: min/mean/max 0.23/0.42/0.68 ms

![](background.jpg)

---

DVTSourceTextView

![](background.jpg)

---

    - (void)swizzleDidChangeTextInSourceTextView {
        [[objc_getClass("DVTSourceTextView") new]
            yl_swizzleSelector:@selector(didChangeText)
                     withBlock:^void(id sself) {
                       [self toggleDebuggersIfNeeded];

                       [sself yl_performSelector:@selector(didChangeText)
                                   returnAddress:NULL
                               argumentAddresses:NULL];
                      }];
    }

![](background.jpg)

---

![](wat.jpg)

---

![200%, original](swizzle.jpg)

---

    @interface NSObject (YOLO)

    -(void)yl_performSelector:(SEL)aSelector
                returnAddress:(void *)result
            argumentAddresses:(void *)arg1, ...;
    -(void)yl_swizzleSelector:(SEL)originalSelector
                    withBlock:(id)block;

    @end

![](background.jpg)

---

![100%, original](plugin.gif)

---

# Ship it

    {
      "name": "My Life-Changing Xcode Plugin",
      "url": "https://github.com/me/xcode-life-changing-plugin",
      "description": "Makes Xcode stop, collaborate and listen."
    }

Send a pull request to the Alcatraz packages repo

https://github.com/supermarin/alcatraz-packages

![](background.jpg)

---

# Using DTrace

- Powerful dynamic tracing framework
- Can be used to log any objc_msgSend()
- Useful for seeing call trees of a specific class
- http://chen.do/blog/2013/10/22/reverse-engineering-xcode-with-dtrace/

![](background.jpg)

---

https://github.com/kattrali/xcode-devtools
https://coderwall.com/p/-mgtww

![](background.jpg)

---

alcatraz.io

![](supermarin.jpg)

---

https://github.com/neonichu/extending-xcode/
https://github.com/neonichu/BBUDebuggerTuckAway/

![](ironcat.jpg)
