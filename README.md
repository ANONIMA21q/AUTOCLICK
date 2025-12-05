--// SERVICES
local VirtualInputManager = game:GetService("VirtualInputManager")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

--// GUI SETUP
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local Panel = Instance.new("Frame")
Panel.Parent = ScreenGui
Panel.Size = UDim2.new(0, 250, 0, 150)
Panel.Position = UDim2.new(0.5, -125, 0.5, -75)
Panel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Panel.BorderSizePixel = 0
Panel.Visible = false

local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = Panel
ToggleButton.Size = UDim2.new(1, -20, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Text = "Auto Click: OFF"
ToggleButton.TextScaled = true

--// AUTO CLICK VARIABLES
local isAutoClickEnabled = false
local isCtrlHeld = false

local function simulateClick()
    local viewport = workspace.CurrentCamera.ViewportSize
    local center = Vector2.new(viewport.X/2, viewport.Y/2)

    VirtualInputManager:SendMouseButtonEvent(center.X, center.Y, 0, true, game, 0)
    task.wait(0.0000001)
    VirtualInputManager:SendMouseButtonEvent(center.X, center.Y, 0, false, game, 0)
end

-- AUTO CLICK LOOP: depende do botão no painel + CTRL segurado
task.spawn(function()
    while true do
        if isAutoClickEnabled and isCtrlHeld then
            simulateClick()
        end
        task.wait(0.0000001)
    end
end)

-- BOTÃO DO PAINEL
ToggleButton.MouseButton1Click:Connect(function()
    isAutoClickEnabled = not isAutoClickEnabled
    ToggleButton.Text = "Auto Click: " .. (isAutoClickEnabled and "ON" or "OFF")
end)

-- DETECTAR CTRL segurado/solto
UserInputService.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        isCtrlHeld = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.RightControl then
        isCtrlHeld = false
    end
end)

--// TRIPLE CLICK LEFT + RIGHT TO OPEN/CLOSE PANEL
local comboCount = 0
local lastClickTime = 0

UserInputService.InputBegan:Connect(function(input)
    local now = tick()

    -- Checar se ESQUERDO e DIREITO estão pressionados juntos
    if UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton1)
        and UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then

        if now - lastClickTime <= 0.5 then
            comboCount += 1
        else
            comboCount = 1
        end

        lastClickTime = now

        if comboCount >= 3 then
            Panel.Visible = not Panel.Visible
            comboCount = 0
        end
    end
end)
