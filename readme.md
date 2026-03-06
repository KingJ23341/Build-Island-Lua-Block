# Welcome!

Thank you for visiting this repository. 

The [Build Island](https://www.roblox.com/games/8424197431/Build-Island-F3X-Building-Tools) Lua Code Block utilizes **Lua 5.1** rather than standard Luau (Roblox Lua). While this environment presents a steeper learning curve due to different global functions and limitations, it is highly efficient once mastered. The following documentation provides tutorials to help you begin programming.

---

## How to Insert the Program Block

Follow these steps to locate and place the Lua 5.1 block:

* **Equip Tools**: Ensure you have the F3X Building Tools equipped.
* **Open Insert Menu**: Press **J** on your keyboard or click the **Plus (+)** icon in the UI.
* **Navigate Categories**: Select the **Experimental** category.
* **Place Block**: Select **Lua 51 Program** to insert it into the workspace.

> [!IMPORTANT]  
> Ensure you select **Experimental**, not *ExperimentalOperations*, as the latter contains different logic components.

# Picture Steps
1. <img width="1010" height="498" alt="image" src="https://github.com/user-attachments/assets/ad0a5a11-2139-4ddc-976b-aeba5c836966" />
2. <img width="682" height="512" alt="image" src="https://github.com/user-attachments/assets/d66abc3d-2ea5-4fdc-8a77-05ae097dda24" />
3. <img width="285" height="258" alt="image" src="https://github.com/user-attachments/assets/0d1fde14-a747-4cf1-869c-6c5a2498ef4f" />
4. <img width="279" height="241" alt="image" src="https://github.com/user-attachments/assets/f152bf95-3b89-4871-a64b-174809866a18" />

This is what the block looks like.
<img width="372" height="356" alt="image" src="https://github.com/user-attachments/assets/966dd3fc-376f-4af1-a446-050eea1858ff" />

To edit the code, click on the block.

Once you've clicked on it, you should see the following:
<img width="2050" height="1046" alt="image" src="https://github.com/user-attachments/assets/b3daa452-3f77-4947-ba34-19ff471760f7" />

This is the default code inside of the Lua 5.1 Block.
```lua
--- PLC8IO: The Buildverse Group

--- ////////// NOTICE: Any vulnerabilities or exploits discovered should be immediately reported to staff.
--- //////////         failure to do so will result in a ban regardless of if the exploit was used or not.
--- ////////// NOTICE: Any apis are prone to change at any point without backwards compatability.

-- The text label that text is written to can be modified only with trust elevation
-- so the code below would error for regular players, don't try to do this.
--scriptroot.SurfaceGui.Frame.TextLabel.BackgroundColor3 = Color3.fromRGB(27, 41, 170)
--scriptroot.SurfaceGui.Frame.TextLabel.TextColor3 = Color3.new(1, 1, 1)

print("Starting the sample program")
print("This runs lua 5.1, not roblox luau. Some things may be different but they mostly similar")

-- Access digital ports (boolean true/false on/off)
local inputPort1 = io.port.input.new(1)
local outputPort1 = io.port.output.new(1)

-- Make sure the ports are boolean (on / off red wiring)
inputPort1.Type = "boolean"
outputPort1.Type = "boolean"

-- Test to show that some things are restricted
task.spawn(function()
	
	-- Users with trust elevation have less restrictions, so the code below to change some things
	-- would have ran. We dont want to do anything to the game so dont run it.
	if trustelevation() ~= 0 then
		print("Running as an elevated script, the code below would have ran normally.")
		return
	end

	-- Test to ensure that access to instances is restricted
	print("Testing sandbox game.name")
	game.Name = "ChangeInName"
	print("Testing sandbox game.players.name")
	game.Players.Name = "ChangeInName"
	error("This should error")
	print("This should not execute after an error")

	local i = 0
	while i <= 10 do
		i = i + 1
		local v = inputPort1:GetValue()
		print('input1switch', i, v)
		if v then
			outputPort1:SetValue(not outputPort1:GetValue())
		end
	end
end)

task.spawn(function()
	
	-- This script runs inside of a virtual environment, so there is no "script".
	-- Instead, access to the part itself is provided via `scriptroot`
	local myPart = scriptroot
	
	-- You should be able to modify your own parts
	myPart.BrickColor = BrickColor.Random()
	
	if trustelevation() ~= 0 then
		return "Running as an elevated script, the code below would have ran normally."
	end
	
	-- You should not be able to create anything new
	-- Nothing after Instance.new will run. it will error right then and there.
	local badInstance = Instance.new("Part", workspace)
	badInstance.Name = "I'll never exist"
	print("I'll never be seen")
	
end)

-- "string", "boolean", and "number" types are supported on ports
local inputNumberPort = io.port.input.new(8)
local outputNumberPort = io.port.output.new(8)

inputNumberPort.Type = "number"
outputNumberPort.Type = "number"

inputNumberPort.Changed:Connect(function()
	print("The input number is now", inputNumberPort.Value)
	outputNumberPort.Value = inputNumberPort.Value * 2467 * 2
end)

outputNumberPort.Changed:Connect(function()
	print("The number output was set to", outputNumberPort.Value)
end)

inputPort1.Changed:Connect(function()
	print("Input port 1 changed. New value: "..tostring(inputPort1:GetValue()))
	print("Setting output port 1 to inverse")
	outputPort1:SetValue(not inputPort1:GetValue())
end)

outputPort1.Changed:Connect(function()
	print("Output port 1 changed. New value: "..tostring(outputPort1:GetValue()))
end)

```

### Code Breakdown

This script serves as a technical template for **Build Island Lua 5.1**, demonstrating how to interact with the environment while respecting its security "sandbox."

---

#### 1. Port Logic (I/O)
The script manages digital and analog "wires" to connect code to build components:
* **Boolean (Port 1):** Handles **On/Off** signals. It uses `.Changed` to automatically flip the output when the input toggles.
* **Number (Port 8):** Processes numerical data. It multiplies an input by a constant ($2467 \times 2$) and sends the result to an output.

#### 2. Sandbox & Security
To maintain game stability, the environment enforces strict safety limits:
* **Instance Protection:** Blocks `Instance.new` for regular players to prevent unauthorized spawning.
* **Global Lockdown:** Prevents scripts from modifying core properties like `game.Name` or `game.Players`.
* **Trust Elevation:** Uses `trustelevation()` to skip restricted UI or global code if the user lacks permissions.

#### 3. Key Differences
* **`scriptroot`:** Use this global to reference the part running the code; the standard `script` global does not exist.
* **Efficiency:** Uses **Event-Driven** programming (`:Connect`) instead of loops to ensure the code only runs when values update, preventing lag.

---


## What can you do with the Lua 5.1 Compiler BLock?
You can do a variety of different things, here's a brief FAQ of things you can do with the Lua 5.1 Compiler Block.

<details>
	<summary>Can I insert a part?</summary>
	As of March 6th, you are **not** able to insert a part using the Lua 5.1 Compiler, as it requires Trust Elevation.
</details>

---
<details>
	<summary>Can I clone a part?</summary>
	As of March 6th, you are **not** able to clone parts using the Lua 5.1 Compiler, as it requires Trust Elevation.
</details>

---
<details>
	<summary>Can I change the name of a part?</summary>
		As of March 6th, you are **not** able to change the name/rename parts using the Lua 5.1 Compiler, as it requires Trust Elevation.
</details>

---
<details>
	<summary>Can I do game:GetService()?</summary>
		As of March 6th, you are **not** able to use game:GetService on the Lua 5.1 Compiler, since this is a Lua Sandbox, it does not run Roblox lua.
</details>

---
<details>
	<summary>Can I kick players?</summary>
		As of March 6th, you are **not** able to kick a player from the game using the Lua 5.1 Compiler, as it requires Trust Elevation.
</details>

---
<details>
	<summary>Can I remove parts?</summary>
	Yes! You **can** remove parts using the Compiler.
	There are numerous ways to do this, such as...
	- Setting the parent to nil
	- Running Remove()
	- Running Destroy()
</details>

---
<details>
	<summary>Can I run .Touched events?</summary>
	Yes, you can run a .Touched event, and it works exactly how it would on Roblox!
</details>

---
<details>
	<summary>Can I run task.spawn()</summary>
	Yes, you can run task.spawn(), and it works exactly how it would on Roblox.
</details>

---
<details>
<summary>How do I print messages to see if my code works?</summary>
You can use the standard print() function. These messages will appear in the Output tab of the code editor window (shown in the "Picture Steps" section) or the F9 Developer Console.
</details>

---
<details>
<summary>Can I save data between game sessions?</summary>
As of March 6th, local variables and port values are not persistent. If the server restarts or the block is reloaded, the data will reset to its default state.
</details>

---
<details>
<summary>Can I use 'Wait' or 'Delay'?</summary>
Yes. You can use task.wait(n) to pause execution for a specific number of seconds. Avoid using the old wait() as task.wait() is more efficient for this environment.
</details>

---
<details>
<summary>Can I change the color or transparency of a part?</summary>
Yes! Since you have access to scriptroot, you can modify physical properties of the block itself (like .Transparency, .Color, or .CanCollide) without needing Trust Elevation.
</details>

---
<details>
	<summary>When I go to save my build, does the code in the Compiler save?</summary>
	Yes! The code will be saved, and will run and work 100% when loading it. Please do keep in mind that if a function is changed to require Trust Elevation, and you loa dyour build that uses that code, it will no-longer work.
</details>

---
<details>
<summary>Can I use standard Lua libraries like 'math' or 'string'?</summary>
Yes. Most standard Lua 5.1 libraries like math, string, and table are fully supported for complex calculations and data management.
</details>

---
<details>
<summary>Can I communicate with other Program Blocks?</summary>
Yes! The primary way to communicate between blocks is through **I/O Ports**. You can set a value on an output port in one block and read it from an input port in another, effectively creating a "wired" network for complex systems.
</details>

## Tutorials
Below are numerous tutorials, as well as share codes of working programs for you to try out, with step-by-step explanations if you wanted to learn how they worked.

<details>
	<summary>Proximity Prompt Jump Power Increaser</summary>
	
Description: Increase the player's jump power by +25 when they trigger a proximity prompt.

Instructions
To create, insert an Interactive Trigger into workspace, then a Lua 5.1 Compiler.

Paste this code into the compiler:
```lua
local promptPart = workspace.InteractivePrompt
local prompt = promptPart.ProximityPrompt
-- By default, proximity parts will have the Object Text say From [your name], and action text be "Interactive", however, we can change it using some code.

prompt.ActionText = "+25 Jump Power"
prompt.ObjectText = "Jump Higher"

prompt.Triggered:Connect(function(trigger)
trigger.Character.Humanoid.JumpPower = trigger.Character.Humanoid.JumpPower + 25
-- We do need to re-assign the action and object text here since the game will reset it back to the default.
prompt.ActionText = "+25 Jump Power"
prompt.ObjectText = "Jump Higher"
-- For Debugging
print("Added 25 jump power to "..trigger.Name..". They now have "..trigger.Character.Humanoid.JumpPower.." jump power.")
end)
```

Save Code: 6C9B2234C234C2702F29341C5557AE810224A470C86C2F777B6AF166C03889B1

Images:
<img width="203" height="216" alt="image" src="https://github.com/user-attachments/assets/c54febe7-41eb-4ea1-89fb-308c79102cc3" />
<img width="434" height="277" alt="image" src="https://github.com/user-attachments/assets/72e84e45-140e-46f7-b19f-683d5c3765b8" />

</details>
