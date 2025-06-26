---
name: Editor Interaction
---

# Interacting With The Editor
## Workflow
Hopefully, now that you're familiar with some `ImGui` basics, let's add stuff to the editor. Anything can be created, edited, or destroyed using the following steps:
* If necessary, instantiate the objects to create using the `utils` global.
* Create an editor action to tell Quaver what to do with the objects.
* Perform the editor action(s) using the `actions` global.

Let's see this workflow in action. In the following example, we will write some code to place a note in the 1st column at the given song time. Here is the scaffolding:
```lua
-- YourPlugin/plugin.lua

function draw()
  imgui.Begin("NoteMaker")

  local noteTime = state.GetValue("noteTime", 0)
  _, noteTime = imgui.InputInt("Time of Note", noteTime)
  state.SetValue("noteTime", noteTime)

  if (imgui.Button("Place!")) then placeNote(noteTime) end

  imgui.End()
end
```
Of course, the function place note is not defined yet. For brevity, all the function code will be written separately, and inserted at the end. Let's start following the steps:
### Instantiate the object
```lua
function placeNote(startTime)
  local note = utils.CreateHitObject(startTime, 1)
end
```
###### Note that `utils`, `action_type`, and `actions` are global variables. That means that no further work is necessary to define them, regardless of what your IDE says.
Here, we are creating a `HitObject` in code using the `utils.CreateHitObject` function. This function takes in two parameters; the first one being the time in which the note should be placed, and the second being the lane it should be placed in.
### Add the object to an editor action
```lua
function placeNote(startTime)
  local note = utils.CreateHitObject(startTime, 1)
  local editorAction = utils.CreateEditorAction(action_type.PlaceHitObject, note)
end
```
For those who are already familiar with plugin development, this process may seem convoluted and unnecessary. However, as you scale up, you will almost never only execute one action at a time, so this is the easiest way to go, especially as you scale.
### Execute the editor action
```lua
function placeNote(startTime)
  local note = utils.CreateHitObject(startTime, 1)
  local editorAction = utils.CreateEditorAction(action_type.PlaceHitObject, note)
  actions.Perform(editorAction)
end
```
Your finalized code should look like this. Try it out!
```lua
function draw()
  imgui.Begin("NoteMaker")

  local noteTime = state.GetValue("noteTime", 0)
  _, noteTime = imgui.InputInt("Time of Note", noteTime)
  state.SetValue("noteTime", noteTime)

  if (imgui.Button("Place!")) then placeNote(noteTime) end

  imgui.End()
end

function placeNote(startTime)
  local note = utils.CreateHitObject(startTime, 1)
  local editorAction = utils.CreateEditorAction(action_type.PlaceHitObject, note)
  actions.Perform(editorAction)
end
```
