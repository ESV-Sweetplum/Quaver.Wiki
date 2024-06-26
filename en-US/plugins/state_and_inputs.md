# State and Inputs
`imgui` inputs are one of the few ways to get user information into your code. However, in `imgui`, inputs may not work the way you think.

## State Management
***Note: The first variable that is returned by an input element is a reference variable. We will tend to ignore these, and only look at the second value.***

The following code block should work, but when you try and input anything, nothing changes:

`myFirstPlugin/plugin.lua`:
```lua
function draw()
  imgui.Begin("My First Plugin")

  inputVariable = 5
  _, inputVariable = imgui.InputInt("input", inputVariable)

  imgui.End()
end
```

This is because `draw()` is called every frame, meaning that `variable` is set equal to 5 before the input field is generated, meaning the `inputVariable` is always 5. To preserve variable values between frames, we must use `state`:

`myFirstPlugin/plugin.lua`:
```lua
function draw()
  imgui.Begin("My First Plugin")

  inputVariable = state.GetValue("input") or 5

  _, inputVariable = imgui.InputInt("input", inputVariable)

  state.SetValue("input", inputVariable)

  imgui.End()
end
```

Here, we let `inputVariable` either be 5, or ***the previous value of `inputVariable`***. When changing the input in the editor, `inputVariable` is now **saved in the state**, meaning that it will keep its value across frames.

Of course, keeping state like this for every variable is very tedious. You can either simplify `state.GetValue` with this function:
```lua
function get(name, defaultValue) state.getValue(name) or defaultValue end
```

Or, you can put all state-managed variables inside a table, as such:
```lua
function draw()
  imgui.Begin("My First Plugin")

  local settings = {
    foo = 1,
    bar = "2"
  }

  retrieveStateVariables(settings)

  _, foo = imgui.InputInt("input", foo)

  saveStateVariables(settings)

  imgui.End()
end

function retrieveStateVariables(variables)
    for key in pairs(variables) do
        variables[key] = state.GetValue(key) or variables[key]
    end
end

function saveStateVariables(variables)
    for key in pairs(variables) do
        state.SetValue(key, variables[key])
    end
end
```

The above is taken from [Ice's plugin guide](https://github.com/IceDynamix/QuaverPluginGuide/blob/master/quaver_plugin_guide.md).
## Input Types

Of course, limiting inputs to strictly integers is very impractical. Instead, there are plenty of input elements for all sorts of input types. You can try out the more important ones by plugging this code into your `plugin.lua` file:

`myFirstPlugin/plugin.lua`:

```lua
function draw()
  imgui.Begin("My First Plugin")

  local MAX_CHARACTERS = 4096

  local settings = {
    inputIntVar = 1,
    inputFloatVar = 2.5,
    inputInt3Vars = {4, 5, 6},
    inputFloat2Vars = {2.71, 3.14},
    inputTextVar = "I love Quaver",
    sliderIntVar = 50,
    checkboxVar = false,
    radioButtonVar = false
  }

  retrieveStateVariables(settings)

  _, inputIntVar = imgui.InputInt("Integers Only", inputIntVar)
  _, inputFlotaVar = imgui.InputFloat("All Numbers", inputFloatVar)
  _, inputInt3Vars = imgui.InputInt3("3 Integer Input", inputInt3Vars)
  _, inputFloat2Vars = imgui.InputFloat2("2 Float Input", inputFloat2Vars)
  _, inputTextVar = imgui.InputText("Text Input", inputTextVar, MAX_CHARACTERS)
  _, sliderIntVar = imgui.SliderInt("Slider", sliderIntVar, 0, 100)
  _, checkboxVar = imgui.Checkbox("Checked?", checkboxVar)

  if (imgui.RadioButton("false", not radioButtonVar)) then
    radioButtonVar = false
  end

  if (imgui.RadioButton("true", radioButtonVar)) then
    radioButtonVar = true
  end

  saveStateVariables(settings)

  imgui.End()
end

function retrieveStateVariables(variables)
    for key in pairs(variables) do
        variables[key] = state.GetValue(key) or variables[key]
    end
end

function saveStateVariables(variables)
    for key in pairs(variables) do
        state.SetValue(key, variables[key])
    end
end
```

### ***Important Notes:***
- `imgui.InputText` requires a character limit passed to it. If it doesn't have one, it will not render.
- All `imgui.Slider____` elements require a `min` and `max` value.
- `imgui.RadioButton` works with a label and a condition in which to mark it as selected. It acts more like a button, as it runs a function when clicked. For more help with buttons, see [visual elements](/docs/plugins/visual_elements).

If you want to know the full list of `inputs`, `sliders`, and `drags`, check out the [imgui wrapper](https://github.com/Quaver/Quaver/blob/ui-redesign/Quaver.Shared/Scripting/ImGuiWrapper.cs).

## Detecting Keypresses
There are 3 different types of events linked to keypresses. If you want a function to run only once on press, use `utils.IsKeyPressed`:
```lua
if (utils.IsKeyPressed(keys.S)) then
  print("Clicked on the S key.")
end
```
If you want a function to run when a key is released, use `utils.IsKeyReleased`:
```lua
if (utils.IsKeyReleased(keys.S)) then
  print("Released the S key.")
end
```
Finally, if you want the function to be run every frame a key is held, use `utils.IsKeyDown`:
```lua
if (utils.IsKeyDown(keys.S)) then
  print("The S key is down.")
end
```
***Note: This code may be laggy for those on high FPS and low-end computers.***

## Keys
A full list of keys can be found [here](https://docs.monogame.net/api/Microsoft.Xna.Framework.Input.Keys.html). Note that Quaver will not register all of these, so play it safe and limit yourself to the alphabetical keys, space (`keys.Space`), shift (`keys.LeftShift`), control (`keys.LeftControl`), and alt (`keys.LeftAlt`).
