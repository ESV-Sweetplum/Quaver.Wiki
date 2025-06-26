---
name: Plugins
---

# Plugins

## What are plugins?

Plugins are your own customizable editor extensions that let you perform
automated tasks very easily. They can be customized in functions, layout and
style, and let you do nearly everything the normal editor would let you do. They
are written in the Lua language.

## Adding plugins
*Adding* a plugin is done by adding a folder that contains a `plugin.lua` file
and a `settings.ini` file to your `Quaver/Plugins` folder. You can find your
Quaver folder by searching for "Open Game folder" in the in-game settings, or by
going to your Steam Library, right-clicking Quaver and selecting **Manage > Browse
local files**.

The plugin should show up in your Plugins dropdown the next time you open the
editor. Do keep in mind that the plugin will show up with the name specified in
the `settings.ini` file, and not the name of the folder.

However, the process of *getting* a plugin can be slightly different depending
on where the plugin is hosted or shared. In the case that the plugin is
hosted on GitHub, you can click on the green "Download Code" button, and
download it as a ZIP file. Then unzip the file to get a folder.

## Quaver Plugin Guide

The best resource to start plugin development with is the
[Quaver Plugin Guide](https://github.com/IceDynamix/QuaverPluginGuide/blob/master/quaver_plugin_guide.md)
composed by IceDynamix. It covers core development concepts for Lua and plugins
in general, provides examples for many GUI elements, and lists all possible
functions you can use to interact with the editor.

Another useful resource is the
[ImGuiWrapper.cs](https://github.com/Quaver/Quaver/blob/ui-redesign/Quaver.Shared/Scripting/ImGuiWrapper.cs)
file, which contains all GUI related functions you're able to use.

## Example plugins

If you're looking for examples to work off, feel free to look at the
[example plugins folder](https://github.com/Quaver/Quaver.Wiki/tree/master/example_plugins).
The already included tools in the editor for BPM Calculator, BPM Detector and
"Go to objects" were also written in Lua and can be found
[here](https://github.com/Quaver/Quaver.Resources/tree/master/Quaver.Resources/Scripts/Lua/Editor).

## User-created plugins

A few users have already gone ahead and made plugins for everyone to use! There
isn't any complete list of them yet, but here are the most known and popular
ones so far:

* [iceSV](https://github.com/IceDynamix/iceSV), a large plugin for general SV
  pattern creation and editing
* [StackedNotesFinder](https://github.com/Illuminati-CRAZ/StackedNotesFinder), a
  plugin to find overlapped notes in a map
* [UnsnappedNotesFinder](https://github.com/Illuminati-CRAZ/UnsnappedNotesFinder),
  a plugin to find unsnapped notes in a map
* [KeepStill](https://github.com/Illuminati-CRAZ/KeepStill), a plugin to create
  a specific SV effect that makes notes look frozen in place
* [BeatSnapColorOverride](https://github.com/Illuminati-CRAZ/BeatSnapColorOverride),
  a plugin to conveniently place timing points in a way to force snap notes to a
  specific color in-game
* [PatternMaster](https://github.com/Adrriii/PatternMaster),
  a plugin to help enforcing consistency and maintainability of copy/pasted patterns
