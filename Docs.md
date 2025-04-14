# Loading Fluent
```lua
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
```
--------------------------------------------------------------------------------------

## Creating the Save Manager
```lua
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
```
--------------------------------------------------------------------------------------

## Loading Interface Manager
```lua
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()
```
--------------------------------------------------------------------------------------

## Creating a Window
```lua
local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    SubTitle = "by dawid",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})
```
--------------------------------------------------------------------------------------

## Tabs
```lua
local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }), --Copy this for more tabs
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}
```
--------------------------------------------------------------------------------------

## Notifications
```lua
Fluent:Notify({
        Title = "Notification",
        Content = "This is a notification",
        SubContent = "SubContent", -- Optional
        Duration = 5 -- Set to nil to make the notification not disappear
    })
```
--------------------------------------------------------------------------------------

## Buttons
```lua
    Tabs.Main:AddButton({
        Title = "Button",
        Description = "Very important button",
        Callback = function()
            Window:Dialog({ --Window Dialog is optional if you want a confirmation when you press your button.
                Title = "Title",
                Content = "This is a dialog",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })
```
--------------------------------------------------------------------------------------

## Sliders
```lua
    local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Slider",
        Description = "This is a slider",
        Default = 2,
        Min = 0,
        Max = 5,
        Rounding = 1,
        Callback = function(Value)
            print("Slider was changed:", Value)
        end
    })

    Slider:OnChanged(function(Value)
        print("Slider changed:", Value)
    end)
```
--------------------------------------------------------------------------------------

## Toggles
```lua
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Toggle", Default = false })

Toggle:OnChanged(function()
    print("Toggle changed:", Toggle.Value)
end)
```
--------------------------------------------------------------------------------------

## Dropdowns
```lua
    local Dropdown = Tabs.Main:AddDropdown("Dropdown", {
        Title = "Dropdown",
        Values = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen"},
        Multi = false,
        Default = 1,
    })

    Dropdown:OnChanged(function(Value)
        print("Dropdown changed:", Value)
    end)
```
--------------------------------------------------------------------------------------

## Multi-Dropdowns
```lua
    local MultiDropdown = Tabs.Main:AddDropdown("MultiDropdown", {
        Title = "Dropdown",
        Description = "You can select multiple values.",
        Values = {"one", "two", "three", "four", "five", "six", "seven", "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen"},
        Multi = true,
        Default = {"six", "nine"},
    })

MultiDropdown:OnChanged(function(Value)
        local Values = {}
        for Value, State in next, Value do
            table.insert(Values, Value)
        end
        print("Mutlidropdown changed:", table.concat(Values, ", "))
    end)
```
--------------------------------------------------------------------------------------

## Color Pickers
```lua
local Colorpicker = Tabs.Main:AddColorpicker("Colorpicker", {
        Title = "Colorpicker",
        Default = Color3.fromRGB(96, 205, 255)
    })

    Colorpicker:OnChanged(function()
        print("Colorpicker changed:", Colorpicker.Value)
    end)
```
--------------------------------------------------------------------------------------

## Transperancy Color Pickers
```lua
local TColorpicker = Tabs.Main:AddColorpicker("TransparencyColorpicker", {
        Title = "Colorpicker",
        Description = "but you can change the transparency.",
        Transparency = 0,
        Default = Color3.fromRGB(96, 205, 255)
    })

    TColorpicker:OnChanged(function()
        print(
            "TColorpicker changed:", TColorpicker.Value,
            "Transparency:", TColorpicker.Transparency
        )
    end)
```
--------------------------------------------------------------------------------------

## Inputs/Textboxs
```lua
    local Input = Tabs.Main:AddInput("Input", {
        Title = "Input",
        Default = "Default",
        Placeholder = "Placeholder",
        Numeric = false, -- Only allows numbers
        Finished = false, -- Only calls callback when you press enter
        Callback = function(Value)
            print("Input changed:", Value)
        end
    })

    Input:OnChanged(function()
        print("Input updated:", Input.Value)
    end)
```
--------------------------------------------------------------------------------------

## Keybinds
```lua
local Keybind = Tabs.Main:AddKeybind("Keybind", {
        Title = "KeyBind",
        Mode = "Toggle", -- Always, Toggle, Hold
        Default = "LeftControl", -- String as the name of the keybind (MB1, MB2 for mouse buttons)

        -- Occurs when the keybind is clicked, Value is `true`/`false`
        Callback = function(Value)
            print("Keybind clicked!", Value)
        end,

        -- Occurs when the keybind itself is changed, `New` is a KeyCode Enum OR a UserInputType Enum
        ChangedCallback = function(New)
            print("Keybind changed!", New)
        end
    })

    -- OnClick is only fired when you press the keybind and the mode is Toggle
    -- Otherwise, you will have to use Keybind:GetState()
    Keybind:OnClick(function()
        print("Keybind clicked:", Keybind:GetState())
    end)

    Keybind:OnChanged(function()
        print("Keybind changed:", Keybind.Value)
    end)

    task.spawn(function()
        while true do
            wait(1)

            -- example for checking if a keybind is being pressed
            local state = Keybind:GetState()
            if state then
                print("Keybind is being held down")
            end

            if Fluent.Unloaded then break end
        end
    end)

    Keybind:SetValue("MB2", "Toggle") -- Sets keybind to MB2, mode to Hold
```
--------------------------------------------------------------------------------------

## Interface Manager Configuration (REQUIRED FOR INTERFACE MANAGER TO WORK)
```lua
InterfaceManager:SetLibrary(Fluent)
InterfaceManager:BuildInterfaceSection(Tabs.Settings)
```
--------------------------------------------------------------------------------------

## Save Manager Configuration (REQUIRED FOR SAVE MANAGER TO WORK)
```lua
SaveManager:SetLibrary(Fluent)
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({})
SaveManager:SetFolder("FluentScriptHub/specific-game")
SaveManager:BuildConfigSection(Tabs.Settings)
SaveManager:LoadAutoloadConfig()
```
--------------------------------------------------------------------------------------


# Updating Modules

-------------------------------------------------------------------------------------

## Updating Toggles
```lua
 Toggle:SetValue(false)
```
--------------------------------------------------------------------------------------

## Updating Sliders
```lua
Slider:SetValue(3)
```
--------------------------------------------------------------------------------------

## Updating Dropdowns
```lua
Dropdown:SetValue("four")
```
--------------------------------------------------------------------------------------

## Updating Multi-Dropdowns
```lua
MultiDropdown:SetValue({
three = true,
five = true,
seven = false
})
```
--------------------------------------------------------------------------------------

## Updating Colorpickers
```lua
Colorpicker:SetValueRGB(Color3.fromRGB(0, 255, 140))
```
--------------------------------------------------------------------------------------

## Updating Keybinds
```lua
Keybind:SetValue("MB2", "Toggle")
```
--------------------------------------------------------------------------------------

# Created By Solentra on discord.
