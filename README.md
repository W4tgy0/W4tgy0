--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]

-- إعداد الحجم الافتراضي والتمكين
_G.HeadSize = 50
_G.Disabled = false

-- إعداد واجهة المستخدم
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
local mainFrame = Instance.new("Frame")
local sizeInput = Instance.new("TextBox")
local applyButton = Instance.new("TextButton")
local creditsButton = Instance.new("TextButton")
local creditsLabel = Instance.new("TextLabel")

-- إعداد ScreenGui
screenGui.Name = "HitboxGui"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- إعداد MainFrame
mainFrame.Size = UDim2.new(0, 300, 0, 150)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
mainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.Parent = screenGui

-- إعداد TextBox
sizeInput.Size = UDim2.new(0, 200, 0, 50)
sizeInput.Position = UDim2.new(0.5, -100, 0.2, 0)
sizeInput.PlaceholderText = "Enter Hitbox Size"
sizeInput.Text = tostring(_G.HeadSize)
sizeInput.TextScaled = true
sizeInput.Parent = mainFrame

-- إعداد Button
applyButton.Size = UDim2.new(0, 200, 0, 50)
applyButton.Position = UDim2.new(0.5, -100, 0.6, 0)
applyButton.Text = "Apply Size"
applyButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
applyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
applyButton.Parent = mainFrame

-- إعداد Credits Button
creditsButton.Size = UDim2.new(0, 100, 0, 30)
creditsButton.Position = UDim2.new(1, -110, 0, 10)
creditsButton.Text = "Credits"
creditsButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
creditsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsButton.Parent = screenGui

-- إعداد Credits Label
creditsLabel.Size = UDim2.new(0, 200, 0, 50)
creditsLabel.Position = UDim2.new(0.5, -100, 0.5, -25)
creditsLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
creditsLabel.BackgroundTransparency = 1
creditsLabel.Text = "Script by w4tgy0"
creditsLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
creditsLabel.TextStrokeTransparency = 0.5
creditsLabel.TextSize = 20 -- زيادة حجم النص
creditsLabel.Visible = false
creditsLabel.Parent = screenGui

-- ميزة سحب الـ Frame
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

-- تطبيق حجم hitboxes على اللاعبين
local function applyHitboxSize()
    local newSize = tonumber(sizeInput.Text)
    if newSize then
        _G.HeadSize = newSize
        for _, v in pairs(game:GetService('Players'):GetPlayers()) do
            if v.Name ~= player.Name then
                pcall(function()
                    if v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        v.Character.HumanoidRootPart.Size = Vector3.new(_G.HeadSize, _G.HeadSize, _G.HeadSize)
                        v.Character.HumanoidRootPart.Transparency = 0.7
                        v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Really blue")
                        v.Character.HumanoidRootPart.Material = Enum.Material.Neon
                        v.Character.HumanoidRootPart.CanCollide = false
                    end
                end)
            end
        end
    else
        print("Invalid size entered.")
    end
end

-- إعداد زر تطبيق الحجم
applyButton.MouseButton1Click:Connect(applyHitboxSize)

-- إظهار/إخفاء credits عند النقر على الزر
creditsButton.MouseButton1Click:Connect(function()
    creditsLabel.Visible = not creditsLabel.Visible
end)

-- تكرار تطبيق الحجم بناءً على التغير في الحجم
game:GetService('RunService').RenderStepped:Connect(function()
    if not _G.Disabled then
        for _, v in pairs(game:GetService('Players'):GetPlayers()) do
            if v.Name ~= player.Name then
                pcall(function()
                    if v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        v.Character.HumanoidRootPart.Size = Vector3.new(_G.HeadSize, _G.HeadSize, _G.HeadSize)
                        v.Character.HumanoidRootPart.Transparency = 0.7
                        v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Really blue")
                        v.Character.HumanoidRootPart.Material = Enum.Material.Neon
                        v.Character.HumanoidRootPart.CanCollide = false
                    end
                end)
            end
        end
    end
end)
