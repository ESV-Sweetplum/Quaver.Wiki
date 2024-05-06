# Basic Visual Elements (imgui)

Here is where you will start really bringing your plugin to life. The Quaver plugin graphical library, `imgui`, allows you to create almost anything you want. For a comprehensive list of supported `imgui` functions, click [here](https://github.com/Quaver/Quaver/tree/ui-redesign/Quaver.Shared/Scripting/ImGuiWrapper.cs).

## Text

Text is by far the easiest element to add to your plugin. Assuming you followed the [initial tutorial](/docs/plugins/getting_started), the following code adds a line of text with "I love Quaver":

`myFirstPlugin/plugin.lua`:
```lua
function draw()
  imgui.Begin("My First Plugin")

  imgui.Text("I love Quaver")

  imgui.End()
end
```

There are also different types of text. For example, `imgui.TextDisabled("text")` creates text that is grayed out, and `imgui.TextWrapped("large amounts of text here")` creates a paragraph of text that wraps with the plugin. The most useful one is `imgui.TextColored`, which takes in a table representing `rgba` values (**between 0-1, not between 0-255**)

`myFirstPlugin/plugin.lua`:
```lua
function draw()
  imgui.Begin("My First Plugin")

  imgui.Text("One Text")
  imgui.TextDisabled("Two Text")
  imgui.TextColored({1, 0, 0, 1}, "Red Text")
  imgui.TextColored({0, 0, 1, 1}, "Blue Text")

  imgui.End()
end
```

## Buttons

***Note: `print()` does not work if you are on the fallback branch. Please switch to stable or canary.***

Buttons work by returning a true or false value; `true` if the button was **pressed** this frame, and `false` otherwise. We can use this in two ways, either by using a variable:

`myFirstPlugin/plugin.lua`
```lua
function draw()
  imgui.Begin("My First Plugin")

  local buttonIsPressed = imgui.Button("Click me")

  if (buttonIsPressed) then
    print("The button was pressed")
  end

  imgui.End()
end
```

The alternative is to just stick the button within the `if` statement, which is much more common:

`myFirstPlugin/plugin.lua`
```lua
function draw()
  imgui.Begin("My First Plugin")

  if imgui.Button("Click me") then
    print("The button was pressed")
  end

  imgui.End()
end
```

These two code blocks are equivalent.
