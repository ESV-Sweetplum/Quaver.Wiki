# Getting Started
## Locating the `Quaver/Plugins` folder
You will need to locate the Quaver plugin folder in order to start developing. The simplest way to locate your Quaver folder is to **select a song in the song selection screen**, then click on **open song folder**. Once you've navigated backwards to the `Quaver` folder, there will be a folder called `Plugins`, which is the desired folder.

## Setting up a code editor
Any text editor is frankly suitable, but given the large amounts of defined global variables, [VSCode](https://code.visualstudio.com/) with the [sumneko LuaLS extension](https://github.com/LuaLS/lua-language-server) is strongly recommended. If you need help installing the lua language server, instructions can be found [here](https://luals.github.io/#install).

## Making your first plugin
Assuming you've located your `Quaver/Plugins` folder, using a text editor of your choice, create a new folder within it. For the sake of this example, we will call the folder `myFirstPlugin`. Inside `myFirstPlugin`, create two files:

`myFirstPlugin/settings.ini`:

```ini
[Settings]
Name = MyFirstPlugin
Author = <YOUR_NAME>
Description = <YOUR_DESCRIPTION>
```

`myFirstPlugin/plugin.lua`:
```lua
function draw()
    imgui.Begin("My First Plugin")
    imgui.End()
end
```

Once saved, enter the editor and look for the `Plugins` tab at the top. Within the dropdown should be an item called `MyFirstPlugin`. When hovering on that item, click on the button that says `enable`. This will create an empty window. Congratulations! You've created your very first plugin (albeit with no features, but that will change later).

All of your important code will be located in `myFirstPlugin/plugin.lua`. When run, the plugin calls the `draw()` function every frame. `imgui.Begin()` creates the window (where the window name is "My First Plugin"), and `imgui.End()` finalizes the window. In the next section, we will learn to add text, buttons, plots, and more.
