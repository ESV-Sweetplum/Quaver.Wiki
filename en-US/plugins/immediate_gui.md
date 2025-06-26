---
name: Immediate GUI
---

# ImGui

## Introduction to Immediate GUI
Assuming you're following the guide, you should now have a very basic plugin with a very basic window. Remember that `draw()` is being called every frame, meaning the window is "created" and "destroyed" each frame. The advantage of this is that you will not have to deal with frame managements, event listeners, or other inter-frame structures. Instead, since everything is rendered fresh each frame (drastic oversimplification), all of your logic only has to worry about what the user is doing in the current frame.

## Display Elements
Things like text and separators are easy to render because they don't change between frames. Let's try it:
```lua
-- YourPlugin/plugin.lua

function draw()
  imgui.Begin("Talking Box")

  imgui.Text("I'm text number 1.")
  imgui.Separator()
  imgui.Text("And I'm text number 2!")

  imgui.End()
end
``` 
## State
Of course, if the plugin is being "refreshed" every frame, then variables will be "refreshed" as well, which is not ideal. To bypass this, we will use the built-in `state` global, which allows you to save data in-between frames. Take the following code as an example:

```lua
-- YourPlugin/plugin.lua

function draw()
  imgui.Begin("Your Plugin")

  local foo = 5
  _, foo = imgui.InputInt("Foo Input", foo)

  imgui.End()
end
```
###### Don't worry about the inputs, we'll go over that later.
Try using this code for your plugin. You have an input, but changing it doesn't do anything, and it will always revert to 5. This is because we are setting `foo` to be 5 each frame, so no matter what input you choose, the displayed value will always go back to 5. To fix this, we will "save" and "load" the value of `foo` like so:
```lua
-- YourPlugin/plugin.lua

function draw()
  imgui.Begin("Your Plugin")

  local foo = state.GetValue("foo", 5)
  _, foo = imgui.InputInt("Foo Input", foo)
  state.SetValue("foo", foo)

  imgui.End()
end
```
You can think of state as set of lockers, where we store valuable data with their corresponding labels. Here, we introduced two new functions:
* `state.GetValue(key, fallback)`: Gets the data corresponding to the given `key`. If the data doesn't exist, returns `fallback` instead.
* `state.SetValue(key, value)`: Sets the data of `key` to be `value`.

All variables that change between frames (aka anything affected by user input) needs to be saved in `state` to protect its value. In short, you can save the data of a variable through the following steps:
* Get the value of the variable.
* Create an input that manipulates the variable in some way.
* Save the variable to the state, and repeat.

Now that data can be persisted, it's time to learn how to input that data.

## Buttons, Inputs, and Checkboxes
As shown above, we can create an input field in the plugin window by calling `imgui.InputInt`. The function's overload, or type definition, looks like this:
```lua
ref, value = imgui.InputInt(label, value)
```
However, as Lua doesn't work with refs, it is conventional to discard the first return value and only care about the second. We can say that a variable is unnecessary by prefixing it with an `_`:
```lua
_, pizzas = imgui.InputInt("Number of Pizzas", pizzas)
```
All input functions (like sliders, checkboxes and text inputs) return values like the above example. The `imgui` class has plenty of different dynamic inputs to choose from, including but not limited to:
* `imgui.InputText(label, value, character_limit)`
* `imgui.SliderFloat(label, value, slider_min, slider_max)`
* `imgui.DragInt2(label, value, value_speed, value_min, value_max)`

Unlike dynamic inputs, inputs like checkboxes and radio buttons are much stricter. Try the following code in your plugin:
```lua
-- YourPlugin/plugin.lua
function draw()
  imgui.Begin("Simple Checkbox")

  local checked = state.GetValue("checkbox_active", false)
  _, checked = imgui.Checkbox("Having fun?", checked)
  state.SetValue("checkbox_active", checked)

  imgui.End()
end
```
Checkboxes are great for boolean options. With checkboxes and dynamic inputs, we can create our own little settings page. Let's make a simple plugin which will add or subtract two numbers:
```lua
-- YourPlugin/plugin.lua
function draw()
  imgui.Begin("Basic Adder")

  local values = state.GetValue("numbers", {0, 0})
  local subtractInstead = state.GetValue("subtract", false)

  _, values = imgui.InputInt2("Inputs", values)
  _, subtractInstead = imgui.Checkbox("Subtract?", subtractInstead)

  state.SetValue("numbers", values)
  state.SetValue("subtract", subtractInstead)

  imgui.End()
end
```
Some way to confirm an action would be nice; that's where `imgui.Button(label)` comes in. It returns a boolean value which is `true` if the button was clicked on this frame, so we can write code like this:
```lua
-- YourPlugin/plugin.lua
function draw()
  imgui.Begin("Basic Adder")

  local values = state.GetValue("numbers", {0, 0})
  local subtractInstead = state.GetValue("subtract", false)

  _, values = imgui.InputInt2("Inputs", values)
  _, subtractInstead = imgui.Checkbox("Subtract?", subtractInstead)

  if (imgui.Button("Compute!")) then
    if (subtractInstead) then
      print(values[1] - values[2])
    else
      print(values[1] + values[2])
    end
  end

  state.SetValue("numbers", values)
  state.SetValue("subtract", subtractInstead)

  imgui.End()
end
```
If you are curious to learn more about the built-in `ImGui` functions, check out the [ImGuiRedirect.cs](https://github.com/Quaver/Quaver/blob/main/Quaver.Shared/Scripting/ImGuiRedirect.cs) file in Quaver's repository.
