local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local orbs = {}
local orbitSpeed = 2
local orbCount = 0
local orbColors = {"Bright red", "Bright blue", "Bright green", "White", "Hot pink"}
local maxOrbs = 200

local function createOrb(color)
    if orbCount >= maxOrbs then return end
    orbCount += 1
    local orb = Instance.new("Part", workspace)
    orb.Shape = Enum.PartType.Ball
    orb.Material = Enum.Material.Neon
    orb.Color = BrickColor.new(color).Color
    orb.Size = Vector3.new(1.5, 1.5, 1.5)
    orb.Anchored = true
    orb.CanCollide = false
    table.insert(orbs, orb)
end

for i = 1, 10 do
    createOrb(orbColors[(i % #orbColors) + 1])
end
