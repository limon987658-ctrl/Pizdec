Pizdec.lua
-- [[ PIZDEC RIVALS v1.0 ]] --
-- Специально для Delta Executor (Mobile)
-- Разработано с нуля под сенсорное управление

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local VirtualUser = game:GetService("VirtualUser")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

-- [[ НАСТРОЙКИ ]] --
local Settings = {
    Aimbot = true,
    AimbotFOV = 250,
    Smoothness = 0.2,
    SilentAim = true,
    AutoFire = true,
    Speed = 45,
    JumpPower = 80,
    ESP = true,
    NoRecoil = true,
    AntiAFK = true,
}

-- [[ СОЗДАНИЕ GUI ДЛЯ ТЕЛЕФОНА ]] --
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PizdecGUI"
ScreenGui.Parent = LocalPlayer.PlayerGui
ScreenGui.ResetOnSpawn = false

-- Главное меню (адаптировано под тач)
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 460)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -230)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 30)
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

-- Размытие фона (эффект стекла)
local Blur = Instance.new("BlurEffect")
Blur.Size = 16
Blur.Parent = game:GetService("Lighting")

-- Заголовок
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.Text = "✦ PIZDEC RIVALS ✦"
Title.TextColor3 = Color3.fromRGB(255, 20, 120)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.BackgroundTransparency = 1
Title.Parent = MainFrame

-- Кнопка открытия меню (на экране)
local ToggleButton = Instance.new("ImageButton")
ToggleButton.Size = UDim2.new(0, 55, 0, 55)
ToggleButton.Position = UDim2.new(0.02, 0, 0.15, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 20, 120)
ToggleButton.BackgroundTransparency = 0.3
ToggleButton.BorderSizePixel = 0
ToggleButton.Image = "rbxassetid://6031095073"
ToggleButton.Parent = ScreenGui

-- Rounded corners
local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 15)
Corner.Parent = MainFrame

local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(0, 15)
ToggleCorner.Parent = ToggleButton

-- [[ ФУНКЦИЯ СОЗДАНИЯ КНОПОК ]] --
local function CreateButton(text, yPos, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0.85, 0, 0, 45)
    Button.Position = UDim2.new(0.075, 0, 0, yPos)
    Button.Text = text
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextScaled = true
    Button.Font = Enum.Font.GothamSemibold
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 70)
    Button.BackgroundTransparency = 0.3
    Button.BorderSizePixel = 0
    Button.Parent = MainFrame
    
    local BtnCorner = Instance.new("UICorner")
    BtnCorner.CornerRadius = UDim.new(0, 10)
    BtnCorner.Parent = Button
    
    Button.MouseButton1Click:Connect(callback)
    return Button
end

local function CreateSlider(text, yPos, min, max, default, callback)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(0.85, 0, 0, 50)
    SliderFrame.Position = UDim2.new(0.075, 0, 0, yPos)
    SliderFrame.BackgroundTransparency = 1
    SliderFrame.Parent = MainFrame
    
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 0.4, 0)
    Label.Text = text .. " | " .. default
    Label.TextColor3 = Color3.fromRGB(200, 200, 255)
    Label.TextScaled = true
    Label.Font = Enum.Font.GothamSemibold
    Label.BackgroundTransparency = 1
    Label.Parent = SliderFrame
    
    local Slider = Instance.new("Frame")
    Slider.Size = UDim2.new(1, 0, 0.4, 0)
    Slider.Position = UDim2.new(0, 0, 0.6, 0)
    Slider.BackgroundColor3 = Color3.fromRGB(40, 40, 80)
    Slider.BorderSizePixel = 0
    Slider.Parent = SliderFrame
    
    local Fill = Instance.new("Frame")
    Fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    Fill.BackgroundColor3 = Color3.fromRGB(255, 20, 120)
    Fill.BorderSizePixel = 0
    Fill.Parent = Slider
    
    local SliderCorner = Instance.new("UICorner")
    SliderCorner.CornerRadius = UDim.new(0, 5)
    SliderCorner.Parent = Slider
    
    local dragging = false
    Slider.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            local x = math.clamp((input.Position.X - Slider.AbsolutePosition.X) / Slider.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * x)
            Fill.Size = UDim2.new(x, 0, 1, 0)
            Label.Text = text .. " | " .. value
            callback(value)
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.Touch then
            local x = math.clamp((input.Position.X - Slider.AbsolutePosition.X) / Slider.AbsoluteSize.X, 0, 1)
            local value = math.floor(min + (max - min) * x)
            Fill.Size = UDim2.new(x, 0, 1, 0)
            Label.Text = text .. " | " .. value
            callback(value)
        end
    end)
end

-- [[ СОЗДАНИЕ ЭЛЕМЕНТОВ УПРАВЛЕНИЯ ]] --
local yPos = 50

CreateButton("🔴 Aimbot (On/Off)", yPos, function()
    Settings.Aimbot = not Settings.Aimbot
end)
yPos = yPos + 55

CreateButton("🎯 Silent Aim", yPos, function()
    Settings.SilentAim = not Settings.SilentAim
end)
yPos = yPos + 55

CreateButton("🔫 Auto Fire", yPos, function()
    Settings.AutoFire = not Settings.AutoFire
end)
yPos = yPos + 55

CreateButton("💨 SpeedHack", yPos, function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        if LocalPlayer.Character.Humanoid.WalkSpeed == 16 then
            LocalPlayer.Character.Humanoid.WalkSpeed = Settings.Speed
        else
            LocalPlayer.Character.Humanoid.WalkSpeed = 16
        end
    end
end)
yPos = yPos + 55

CreateSlider("🎯 FOV", yPos, 50, 500, Settings.AimbotFOV, function(v)
    Settings.AimbotFOV = v
end)
yPos = yPos + 60

CreateSlider("⚡ Speed", yPos, 16, 100, Settings.Speed, function(v)
    Settings.Speed = v
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = v
    end
end)

-- Открытие/закрытие меню
local menuOpen = true
ToggleButton.MouseButton1Click:Connect(function()
    menuOpen = not menuOpen
    MainFrame.Visible = menuOpen
end)

-- [[ ОСНОВНАЯ ЛОГИКА AIMBOT ]] --
function GetClosestPlayer()
    local closest = nil
    local shortest = math.huge
    local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            local headPos = player.Character.Head.Position
            local screenPos, onScreen = Camera:WorldToScreenPoint(headPos)
            
            if onScreen then
                local dist = (Vector2.new(screenPos.X, screenPos.Y) - center).Magnitude
                if dist < Settings.AimbotFOV and dist < shortest then
                    shortest = dist
                    closest = player
                end
            end
        end
    end
    return closest
end

-- [[ РАБОТА В КАЖДОМ КАДРЕ ]] --
RunService.RenderStepped:Connect(function()
    -- Aimbot
    if Settings.Aimbot then
        local target = GetClosestPlayer()
        if target and target.Character and target.Character:FindFirstChild("Head") then
            local headPos = target.Character.Head.Position
            
            if Settings.SilentAim then
                -- Silent Aim через прозрачное наведение
                pcall(function()
                    Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
                end)
            else
                -- Плавное наведение
                local targetCFrame = CFrame.new(Camera.CFrame.Position, headPos)
                Camera.CFrame = Camera.CFrame:Lerp(targetCFrame, Settings.Smoothness)
            end
            
            -- Auto Fire
            if Settings.AutoFire then
                pcall(function()
                    Mouse1Click()
                    wait(0.1)
                end)
            end
        end
    end
    
    -- Anti-AFK (для мобилы)
    if Settings.AntiAFK then
        pcall(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end)
    end
end)

-- [[ НАСТРОЙКИ ПРИ РЕСПАВНЕ ]] --
LocalPlayer.CharacterAdded:Connect(function(char)
    wait(0.5)
    local hum = char:FindFirstChild("Humanoid")
    if hum then
        hum.WalkSpeed = Settings.Speed
        hum.JumpPower = Settings.JumpPower
    end
end)

-- [[ СОЗДАНИЕ ESP ДЛЯ ТЕЛЕФОНА ]] --
local function CreateMobileESP(player)
    if player == LocalPlayer then return end
    local char = player.Character
    if not char then return end
    
    local esp = Instance.new("BillboardGui")
    esp.Name = "PizdecESP"
    esp.Size = UDim2.new(0, 100, 0, 50)
    esp.StudsOffset = Vector3.new(0, 3, 0)
    esp.AlwaysOnTop = true
    esp.Parent = char:FindFirstChild("Head") or char
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundColor3 = Color3.fromRGB(255, 0, 100)
    frame.BackgroundTransparency = 0.5
    frame.BorderSizePixel = 0
    frame.Parent = esp
    
    local name = Instance.new("TextLabel")
    name.Size = UDim2.new(1, 0, 0.4, 0)
    name.Position = UDim2.new(0, 0, 1, 0)
    name.Text = player.Name
    name.TextColor3 = Color3.fromRGB(255, 255, 255)
    name.TextScaled = true
    name.Font = Enum.Font.GothamBold
    name.BackgroundTransparency = 1
    name.Parent = esp
end

-- Включение ESP при старте
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        task.spawn(CreateMobileESP, player)
    end
end

Players.PlayerAdded:Connect(CreateMobileESP)

-- [[ ВИЗУАЛЬНЫЙ ЭФФЕКТ ]] --
local effect = Instance.new("ColorCorrectionEffect")
effect.Parent = game:GetService("Lighting")
effect.Saturation = -0.2
effect.Brightness = 0.1

print("✦ PIZDEC RIVALS v1.0 ЗАГРУЖЕН! ✦")
print("Наслаждайся игрой на телефоне! 🎮")
