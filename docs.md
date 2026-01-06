# Dollarware UI Library Documentation

A feature-rich UI library for Roblox exploits.

## Table of Contents
- [Getting Started](#getting-started)
- [UI Object](#ui-object)
- [Windows](#windows)
- [Menus](#menus)
- [Sections](#sections)
- [Controls](#controls)
  - [Toggle](#toggle)
  - [Button](#button)
  - [Label](#label)
  - [Slider](#slider)
  - [Textbox](#textbox)
  - [Dropdown](#dropdown)
  - [Color Picker](#color-picker)
  - [Hotkey](#hotkey)
- [Config Manager](#config-manager)
- [Notifications](#notifications)
- [Events](#events)
- [Themes](#themes)

---

## Getting Started

```lua
local ui = loadstring(game:HttpGet('https://raw.githubusercontent.com/myethg/dollarware/main/library.lua'))({
    theme = 'grape',  -- Theme name or custom theme table
    rounding = true,  -- Enable rounded corners
    smoothDragging = true  -- Smooth window dragging
})

-- Create a window
local Window = ui.newWindow({ text = 'My Script' })

-- Create a menu (tab)
local Menu = Window:addMenu({ text = 'Main' })

-- Create sections
local LeftSection = Menu:addSection({ text = 'Options', side = 'left' })
local RightSection = Menu:addSection({ text = 'Settings', side = 'right' })

-- Add controls
LeftSection:addToggle({ text = 'Enable Feature', state = false }, function(enabled)
    print('Toggled:', enabled)
end)
```

---

## UI Object

The main `ui` object returned by the library.

### Properties
| Property | Type | Description |
|----------|------|-------------|
| `ui.windows` | table | Array of all created windows |
| `ui.configManager` | table | Config manager instance |

### Methods

#### `ui.newWindow(settings)`
Creates a new window.

```lua
local Window = ui.newWindow({
    text = 'Window Title',
    resize = false,  -- Allow resizing (default: false)
    position = UDim2.new(0.5, 0, 0.5, 0)  -- Optional starting position
})
```

#### `ui.notify(settings)`
Shows a notification.

```lua
ui.notify({
    title = 'Success',
    message = 'Config saved!',
    duration = 3  -- Seconds (default: 2)
})
```

#### `ui.setTheme(themeTable)`
Changes the UI theme (requires custom theme table).

#### `ui.destroy()`
Destroys the entire UI.

#### `ui.createSettingsMenu(window, settings)`
Creates a pre-built settings menu with config management.

```lua
ui.createSettingsMenu(Window, {
    folder = 'MyScript_Configs',  -- Config folder name
    menuName = 'Settings'  -- Tab name
})
```

---

## Windows

### Methods

#### `Window:addMenu(settings)`
Adds a menu tab to the window.

```lua
local Menu = Window:addMenu({ text = 'Tab Name' })
```

#### `Window:setTitle(title)`
Changes the window title.

#### `Window:setIcon(iconId)`
Sets the window icon (Roblox asset ID).

#### `Window:setPosition(position)`
Sets window position (UDim2).

#### `Window:setSize(size)`
Sets window size (UDim2).

#### `Window:getPosition()`
Returns current window position.

#### `Window:getSize()`
Returns current window size.

### Events
- `onDestroy` - Fired when window is destroyed

---

## Menus

Menus are tabs within a window.

### Methods

#### `Menu:addSection(settings)`
Adds a section to the menu.

```lua
local Section = Menu:addSection({
    text = 'Section Name',
    side = 'left'  -- 'left' or 'right'
})
```

---

## Sections

Sections contain controls and are organized in left/right columns.

### Methods

All `add*` methods return the created control object.

#### `Section:addToggle(settings, callback)`
#### `Section:addButton(settings, callback)`
#### `Section:addLabel(settings)`
#### `Section:addSlider(settings, callback)`
#### `Section:addTextbox(settings, callback)`
#### `Section:addDropdown(settings, callback)`
#### `Section:addColorPicker(settings, callback)`
#### `Section:addHotkey(settings)`

---

## Controls

### Toggle

A checkbox/switch control.

```lua
local Toggle = Section:addToggle({
    text = 'Enable Feature',
    state = false,  -- Initial state
    flag = 'myToggle'  -- Optional config key
}, function(enabled)
    print('Toggled:', enabled)
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Toggle:getState()` | boolean | Get current state |
| `Toggle:getValue()` | boolean | Alias for getState |
| `Toggle:isEnabled()` | boolean | Alias for getState |
| `Toggle:setState(bool)` | self | Set the toggle state |
| `Toggle:setValue(bool)` | self | Alias for setState |
| `Toggle:setTooltip(text)` | self | Set hover tooltip |

#### Events
- `onToggle(state)` - Fired when toggled
- `onEnable` - Fired when enabled
- `onDisable` - Fired when disabled

---

### Button

A clickable button.

```lua
local Button = Section:addButton({
    text = 'Click Me',
    style = 'small'  -- 'small' or nil for normal
}, function()
    print('Clicked!')
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Button:setTooltip(text)` | self | Set hover tooltip |

#### Events
- `onClick` - Fired when clicked

---

### Label

A text label (non-interactive).

```lua
local Label = Section:addLabel({
    text = 'Some info text'
})
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Label:setText(text)` | self | Change the text |
| `Label:getText()` | string | Get current text |

---

### Slider

A numeric slider control.

```lua
local Slider = Section:addSlider({
    text = 'Speed',
    min = 0,
    max = 100,
    default = 50,
    decimals = 0,  -- Decimal places (default: 0)
    flag = 'speedSlider'
}, function(value)
    print('Value:', value)
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Slider:getValue()` | number | Get current value |
| `Slider:setValue(num)` | self | Set the value |
| `Slider:setTooltip(text)` | self | Set hover tooltip |

#### Events
- `onNewValue(value)` - Fired when value changes

---

### Textbox

A text input field.

```lua
local Textbox = Section:addTextbox({
    text = 'Username',  -- Label shown when empty/unfocused
    placeholder = 'Enter name...',
    default = '',
    flag = 'usernameBox'
}, function(text)
    print('Text changed:', text)
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Textbox:getValue()` | string | Get stored value |
| `Textbox:setValue(text)` | self | Set the value |
| `Textbox:getText()` | string | Get display text |
| `Textbox:setText(text)` | self | Set display text |
| `Textbox:setTooltip(text)` | self | Set hover tooltip |

#### Events
- `onTextChange(text)` - Fired while typing
- `onFocus` - Fired when clicked/focused
- `onFocusLost(text)` - Fired when unfocused (enter/click away)

---

### Dropdown

A selection dropdown menu.

```lua
local Dropdown = Section:addDropdown({
    text = 'Select Option',
    options = {'Option 1', 'Option 2', 'Option 3'},
    default = 'Option 1',
    multiSelect = false,  -- Allow multiple selections
    flag = 'myDropdown'
}, function(selected)
    print('Selected:', selected)
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Dropdown:getSelected()` | string/table | Get selected value(s) |
| `Dropdown:setSelected(value)` | self | Set selection |
| `Dropdown:setOptions(options)` | self | Update options list |
| `Dropdown:refresh(options)` | self | Alias for setOptions |
| `Dropdown:toggle()` | self | Open/close dropdown |
| `Dropdown:close()` | self | Close dropdown |

#### Events
- `onChanged(selected)` - Fired when selection changes
- `onOpen` - Fired when dropdown opens
- `onClose` - Fired when dropdown closes

---

### Color Picker

A color selection control.

```lua
local Picker = Section:addColorPicker({
    text = 'Highlight Color',
    color = Color3.fromRGB(255, 0, 0),
    flag = 'highlightColor'
}, function(color)
    print('Color:', color)
end)
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Picker:getColor()` | Color3 | Get current color |
| `Picker:setColor(color)` | self | Set the color |
| `Picker:setTooltip(text)` | self | Set hover tooltip |

#### Events
- `onNewColor(color)` - Fired when color changes
- `onClick` - Fired when picker button clicked

---

### Hotkey

A keybind input control.

```lua
local Hotkey = Section:addHotkey({
    text = 'Toggle Key',
    hotkey = Enum.KeyCode.F,  -- Default key
    flag = 'toggleHotkey'
})

-- Link to another control
Hotkey:linkTo(SomeToggle)  -- Will toggle when key pressed
```

#### Methods
| Method | Returns | Description |
|--------|---------|-------------|
| `Hotkey:setHotkey(keyCode)` | self | Set the keybind |
| `Hotkey:getHotkey()` | KeyCode | Get current keybind |
| `Hotkey:linkTo(control)` | self | Link to toggle/control |
| `Hotkey:setTooltip(text)` | self | Set hover tooltip |

---

## Config Manager

The config manager handles saving/loading settings.

Access via `ui.configManager` or `cm` inside library.

### Methods

#### `cm:init(folderName)`
Initialize with a folder name.

```lua
ui.configManager:init('MyScript_Configs')
```

#### `cm:saveConfig(name)`
Save current settings to a config file.

```lua
ui.configManager:saveConfig('default')
```

#### `cm:loadConfig(name)`
Load settings from a config file.

```lua
ui.configManager:loadConfig('default')
```

#### `cm:deleteConfig(name)`
Delete a config file.

#### `cm:getConfigs()`
Returns array of config names.

#### `cm:ignore(flagId)`
Exclude a control from config saving.

```lua
ui.configManager:ignore('Settings_Config Name')
```

#### `cm:registerControl(flagId, getter, setter)`
Manually register a control for config saving.

```lua
ui.configManager:registerControl('customValue', 
    function() return myValue end,
    function(v) myValue = v end
)
```

#### `cm:onSave(callback)`
Hook into save events.

#### `cm:onLoad(callback)`
Hook into load events.

---

## Notifications

```lua
ui.notify({
    title = 'Title',
    message = 'Notification message',
    duration = 3  -- Seconds
})
```

Notifications appear in bottom-right and auto-dismiss.

---

## Events

All controls support event binding:

```lua
-- Bind to an event
Control:bindToEvent('eventName', function(...)
    -- Handler
end)

-- Fire an event
Control:fireEvent('eventName', arg1, arg2)
```

### Common Events
| Event | Arguments | Controls |
|-------|-----------|----------|
| `onToggle` | state | Toggle |
| `onEnable` | - | Toggle |
| `onDisable` | - | Toggle |
| `onClick` | - | Button, ColorPicker |
| `onNewValue` | value | Slider |
| `onTextChange` | text | Textbox |
| `onFocus` | - | Textbox |
| `onFocusLost` | text | Textbox |
| `onChanged` | selected | Dropdown |
| `onNewColor` | color | ColorPicker |
| `onOpen` | - | Dropdown |
| `onClose` | - | Dropdown |

---

## Themes

### Built-in Themes
- `cherry` - Red
- `orange` - Orange
- `lemon` - Yellow
- `lime` - Green
- `raspberry` - Cyan
- `blueberry` - Blue
- `grape` - Purple (default)
- `watermelon` - Legacy red-aqua

### Usage
```lua
local ui = loadstring(...)({
    theme = 'grape'
})
```

### Custom Theme
```lua
local ui = loadstring(...)({
    theme = {
        Primary = Color3.fromRGB(134, 53, 255),
        Secondary = Color3.fromRGB(211, 53, 255),
        Window1 = Color3.fromRGB(41, 41, 51),
        Window2 = Color3.fromRGB(35, 35, 44),
        Tab1 = Color3.fromRGB(41, 41, 51),
        Tab2 = Color3.fromRGB(48, 48, 60),
        Button1 = Color3.fromRGB(48, 48, 60),
        Button2 = Color3.fromRGB(55, 55, 68),
        Button3 = Color3.fromRGB(65, 65, 80),
        Button4 = Color3.fromRGB(75, 75, 92),
        ScrollBar = Color3.fromRGB(75, 75, 92),
        TextPrimary = Color3.fromRGB(255, 255, 255),
        TextSecondary = Color3.fromRGB(180, 180, 180),
        TextDark = Color3.fromRGB(100, 100, 100),
        TextStroke = Color3.fromRGB(0, 0, 0),
        Stroke = Color3.fromRGB(65, 65, 80),
        ControlGradient1 = Color3.fromRGB(255, 255, 255),
        ControlGradient2 = Color3.fromRGB(192, 192, 192),
    }
})
```

---

## Full Example

```lua
local ui = loadstring(game:HttpGet('https://raw.githubusercontent.com/myethg/dollarware/main/library.lua'))({
    theme = 'grape',
    rounding = true
})

local Window = ui.newWindow({ text = 'My Script v1.0' })

-- Main Tab
local MainMenu = Window:addMenu({ text = 'Main' })
local Section1 = MainMenu:addSection({ text = 'Features', side = 'left' })
local Section2 = MainMenu:addSection({ text = 'Settings', side = 'right' })

-- Features
local SpeedToggle = Section1:addToggle({ 
    text = 'Speed Hack', 
    state = false,
    flag = 'speedHack'
}, function(enabled)
    -- Your code here
end)

local SpeedSlider = Section1:addSlider({
    text = 'Speed Value',
    min = 16,
    max = 100,
    default = 16,
    flag = 'speedValue'
}, function(value)
    -- Your code here
end)

Section1:addButton({ text = 'Teleport' }, function()
    ui.notify({ title = 'Teleport', message = 'Teleported!' })
end)

-- Settings
Section2:addDropdown({
    text = 'Mode',
    options = {'Safe', 'Normal', 'Rage'},
    default = 'Normal',
    flag = 'mode'
}, function(selected)
    print('Mode:', selected)
end)

Section2:addColorPicker({
    text = 'ESP Color',
    color = Color3.fromRGB(255, 0, 0),
    flag = 'espColor'
})

-- Config Tab
ui.createSettingsMenu(Window, {
    folder = 'MyScript_Configs',
    menuName = 'Settings'
})

ui.notify({ title = 'Loaded', message = 'Script loaded successfully!', duration = 3 })

```
