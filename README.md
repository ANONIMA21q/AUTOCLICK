--// SERVICES
local VirtualInputManager = game:GetService("VirtualInputManager")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- =====================================================
-- GUI SETUP
-- =====================================================

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local Panel = Instance.new("Frame")
Panel.Parent = ScreenGui
Panel.Size = UDim2.new(0, 260, 0, 200)
Panel.Position = UDim2.new(0.5, -130, 0.5, -100)
Panel.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
Panel.BorderSizePixel = 0
Panel.Visible = false

local Title = Instance.new("TextLabel")
Title.Parent = Panel
Title.Size = UDim2.new(1, 0, 0, 35)
Title.BackgroundTransparency = 1
Title.Text = "PAINEL"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.TextScaled = true

-- AUTO CLICK BUTTON
local AutoClickBtn = Instance.new("TextButton")
AutoClickBtn.Parent = Panel
AutoClickBtn.Size = UDim2.new(1, -20, 0, 40)
AutoClickBtn.Position = UDim2.new(0, 10, 0, 45)
AutoClickBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
AutoClickBtn.TextColor3 = Color3.new(1, 1, 1)
AutoClickBtn.TextScaled = true
AutoClickBtn.Text = "Auto Click: OFF"

-- CONTROL MODE BUTTON
local CtrlModeBtn = Instance.new("TextButton")
CtrlModeBtn.Parent = Panel
CtrlModeBtn.Size = UDim2.new(1, -20, 0, 40)
CtrlModeBtn.Position = UDim2.new(0, 10, 0, 90)
CtrlModeBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
CtrlModeBtn.TextColor3 = Color3.new(1, 1, 1)
CtrlModeBtn.TextScaled = true
CtrlModeBtn.Text = "CTRL Mode: OFF"

-- INFINITY BUTTON (AGORA É UM GATILHO)
local InfinityBtn = Instance.new("TextButton")
InfinityBtn.Parent = Panel
InfinityBtn.Size = UDim2.new(1, -20, 0, 40)
InfinityBtn.Position = UDim2.new(0, 10, 0, 135)
InfinityBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
InfinityBtn.TextColor3 = Color3.new(1, 1, 1)
InfinityBtn.TextScaled = true
InfinityBtn.Text = "Infinity E-A-D-L: RUN"

-- =====================================================
-- VARIABLES
-- =====================================================

local autoClickEnabled = false
local ctrlModeEnabled = false
local ctrlHeld = false

-- =====================================================
-- CLICK NO MOUSE
-- =====================================================

local function simulateClick()
    local pos = UserInputService:GetMouseLocation()
    VirtualInputManager:SendMouseButtonEvent(pos.X, pos.Y, 0, true, game, 0)
    task.wait(0.0001)
    VirtualInputManager:SendMouseButtonEvent(pos.X, pos.Y, 0, false, game, 0)
end

-- =====================================================
-- AUTO CLICK SYSTEM
-- =====================================================

task.spawn(function()
    while true do
        if autoClickEnabled then
            if not ctrlModeEnabled then
                simulateClick()
            elseif ctrlModeEnabled and ctrlHeld then
                simulateClick()
            end
        end
        task.wait(0.0001)
    end
end)

-- =====================================================
-- BUTTON LOGIC
-- =====================================================

AutoClickBtn.MouseButton1Click:Connect(function()
    autoClickEnabled = not autoClickEnabled
    AutoClickBtn.Text = "Auto Click: " .. (autoClickEnabled and "ON" or "OFF")
end)

CtrlModeBtn.MouseButton1Click:Connect(function()
    ctrlModeEnabled = not ctrlModeEnabled
    CtrlModeBtn.Text = "CTRL Mode: " .. (ctrlModeEnabled and "ON" or "OFF")
end)

-- =====================================================
-- INFINITY BUTTON EXECUTION
-- =====================================================

InfinityBtn.MouseButton1Click:Connect(function()
    print("Executando Infinity E-A-D-L...")

    -- ⚠️ EXECUTE SEU LOADSTRING AQUI ⚠️
    loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

-- =====================================================
-- DETECT CTRL
-- =====================================================

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        ctrlHeld = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        ctrlHeld = false
    end
end)

-- =====================================================
-- TECLA INS PARA ABRIR / FECHAR O PAINEL
-- =====================================================

local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end  -- ignora se veio de GUI

    -- Detecta a tecla Insert (INS)
    if input.KeyCode == Enum.KeyCode.Insert then
        Panel.Visible = not Panel.Visible
    end
end)

