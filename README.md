-- Teleport + ESP script for Steal a Brainrot

local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")

-- Teleport above base
repeat task.wait() until workspace:FindFirstChild("Base") or workspace:FindFirstChild("Baseplate")
local base = workspace:FindFirstChild("Base") or workspace:FindFirstChild("Baseplate")
if base then
    local y = base.Position.Y + (base.Size.Y / 2) + 5
    root.Anchored = true
    root.CFrame = CFrame.new(base.Position.X, y, base.Position.Z)
    task.wait(0.5)
    root.Anchored = false
end

-- ESP for all players
local function addESP(p)
    if p == player then return end
    local hrp = p.Character and p.Character:FindFirstChild("Head")
    if hrp and not hrp:FindFirstChild("ESP") then
        local gui = Instance.new("BillboardGui", hrp)
        gui.Name = "ESP"; gui.Adornee = hrp
        gui.Size = UDim2.new(0,200,0,50); gui.StudsOffset = Vector3.new(0,2,0)
        gui.AlwaysOnTop = true
        local lbl = Instance.new("TextLabel", gui)
        lbl.Size = UDim2.new(1,0,1,0); lbl.BackgroundTransparency = 1
        lbl.Font = Enum.Font.SourceSansBold; lbl.TextSize = 14
        lbl.TextColor3 = Color3.new(1,0,0); lbl.Text = p.Name
    end
end

for _,p in ipairs(game.Players:GetPlayers()) do
    if p.Character then addESP(p) end
    p.CharacterAdded:Connect(function() task.wait(1); addESP(p) end)
end
game.Players.PlayerAdded:Connect(function(p)
    p.CharacterAdded:Connect(function() task.wait(1); addESP(p) end)
end)
