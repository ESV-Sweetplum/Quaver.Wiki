---
name: Creating a Plugin
---

# Creating Your Own Plugin
The entire guide in this wiki assumes you have basic knowledge of Lua syntax and programming structures (if-statements, loops, types, etc.)
## File Structure

Plugins are defined by a folder structure built up in following way:

* Quaver/Plugins
    * YourPlugin
        * `plugin.lua`
        * `settings.ini`
    * AnotherPlugin
        * `plugin.lua`
        * `settings.ini`
  
To start out, create a new folder within the `Quaver/Plugins` folder. For the sake of this guide, we will refer to this folder as `YourPlugin`, but you should pick any name that suits the plugin's function. Within `YourPlugin`, create a `plugin.lua` and a `settings.ini` file.

Fill in your plugin's metadata in your `settings.ini` file like this:

```ini
[Settings]
Name = Plugin Name
Author = Your Name
Description = Your Plugin Description
```

This is the data that will be shown in the **Plugins** dropdown in the editor.

## Entry Points

Now that the basic scaffolding is done, let's write some code. Any code that you'd want to run in the editor is executed through certain ***entry points***, two of which are shown below:
* `draw()` - This function is run on ***every frame***. All of your main logic should go here.
* `awake()` - This function is run when the plugin is first loaded, good for loading `state` (which will be discussed in the future).

To create our first interactive window, copy-paste the following code block into the `plugin.lua` file of your plugin.

```lua
-- YourPlugin/plugin.lua

function draw()
  imgui.Begin()
  imgui.Text("This is my first plugin.")
  imgui.End()
end
```

Once saved, any future changes made to the plugin will show up in the Quaver editor automatically, without you having to reload it. This is called **hot-reloading**, and will be very useful for debugging purposes.

For those who are unfamiliar, Quaver uses the `Dear ImGui` library for drawing elements onto the editor's screen. Since it is relatively complex, the guide for adding new features to your plugin will be in the [Immediate GUI](/docs/plugins/imgui) section.
