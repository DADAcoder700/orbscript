--[[
    ORBIT ORBS V1.0 — без AI
    Поддержка мобильных | До 100 шаров
--]]

-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

-- UI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "OrbUI"
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame", ScreenGui)
Frame.Position = UDim2.new(0, 20, 0.3, 0)
Frame.Size = UDim2.new(0, 220, 0, 260)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel", Frame)
Title.Text = "Orbit Orbs"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

-- Variables
local orbs = {}
local orbitSpeed = 2
local orbColor = Color3.fromRGB(255, 255, 255)

-- Create Orb
local function createOrb()
    if #orbs >= 100 then return end
    local orb = Instance.new("Part")
    orb.Shape = Enum.PartType.Ball
    orb.Size = Vector3.new(1, 1, 1)
    orb.Anchored = true
    orb.CanCollide = false
    orb.Material = Enum.Material.Neon
    orb.Color = orbColor
    orb.Parent = workspace

    table.insert(orbs, orb)
end

-- Remove last orb
local function removeOrb()
    local orb = table.remove(orbs)
    if orb then orb:Destroy() end
end

-- Clear all orbs
local function clearOrbs()
    for _, orb in ipairs(orbs) do orb:Destroy() end
    orbs = {}
end

-- UI Buttons
local function createButton(text, yPos, callback)
    local button = Instance.new("TextButton", Frame)
    button.Text = text
    button.Position = UDim2.new(0, 10, 0, yPos)
    button.Size = UDim2.new(0, 200, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.Gotham
    button.TextSize = 14
    button.MouseButton1Click:Connect(callback)
    return button
end

createButton("Добавить шар", 40, createOrb)
createButton("Удалить шар", 80, removeOrb)
createButton("Очистить все", 120, clearOrbs)

-- Цвета
createButton("Цвет: Красный", 160, function() orbColor = Color3.fromRGB(255, 0, 0) end)
createButton("Цвет: Зелёный", 190, function() orbColor = Color3.fromRGB(0, 255, 0) end)
createButton("Цвет: Синий", 220, function() orbColor = Color3.fromRGB(0, 128, 255) end)

-- Обновление позиции
local angle = 0
RunService.RenderStepped:Connect(function(dt)
    if not Character:FindFirstChild("HumanoidRootPart") then return end
    angle += orbitSpeed * dt
    for i, orb in ipairs(orbs) do
        local theta = angle + (math.pi * 2 / #orbs) * i
        local radius = 4
        local offset = Vector3.new(math.cos(theta) * radius, 2, math.sin(theta) * radius)
        orb.Position = Character.HumanoidRootPart.Position + offset
    end
end)
