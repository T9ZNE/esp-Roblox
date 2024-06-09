# esp-Roblox
ITS A ROBLOX ESP ACTIVATE WHITE °H° AND REPRESS TO DESACTIVATE
                                                                                                                                                                                                                           -- StarterPlayer > StarterPlayerScripts > ShowHitboxesAndChangeSkyWithUI

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local GuiService = game:GetService("GuiService")

local showHitboxes = false
local hitboxColor = Color3.new(1, 0, 0) -- Rouge
local originalSkyColor = Lighting:GetAttribute("SkyColor") or Color3.new(0.5, 0.5, 0.5) -- Couleur du ciel par défaut

local function toggleHitboxesAndChangeSky()
    showHitboxes = not showHitboxes
    
    if showHitboxes then
        -- Afficher les hitboxes
        for _, player in ipairs(Players:GetPlayers()) do
            local character = player.Character
            if character then
                for _, part in ipairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        local boxHandleAdornment = Instance.new("BoxHandleAdornment")
                        boxHandleAdornment.Name = "Hitbox"
                        boxHandleAdornment.Size = part.Size
                        boxHandleAdornment.Adornee = part
                        boxHandleAdornment.Color3 = hitboxColor
                        boxHandleAdornment.Transparency = 0.5
                        boxHandleAdornment.AlwaysOnTop = true
                        boxHandleAdornment.ZIndex = 5
                        boxHandleAdornment.Parent = part
                    end
                end
            end
        end
        
        -- Modifier la couleur du ciel
        Lighting:SetAttribute("SkyColor", Color3.fromRGB(148, 0, 211)) -- Violet
    else
        -- Cacher les hitboxes
        for _, player in ipairs(Players:GetPlayers()) do
            local character = player.Character
            if character then
                for _, part in ipairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part:FindFirstChild("Hitbox") then
                        part.Hitbox:Destroy()
                    end
                end
            end
        end
        
        -- Rétablir la couleur du ciel d'origine
        Lighting:SetAttribute("SkyColor", originalSkyColor)
    end
end

local function createGui()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "HitboxGui"
    screenGui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 120, 0, 40)
    frame.Position = UDim2.new(0, 10, 0, 10)
    frame.BackgroundTransparency = 0.5
    frame.BackgroundColor3 = Color3.new(1, 1, 1)
    frame.BorderSizePixel = 2
    frame.BorderColor3 = Color3.new(0, 0, 0)
    frame.Parent = screenGui
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Position = UDim2.new(0, 0, 0, 0)
    label.TextScaled = true
    label.Font = Enum.Font.SourceSansBold
    label.TextColor3 = Color3.new(0, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = "Hitboxes Off"
    label.Parent = frame
    
    local function updateLabel()
        label.Text = showHitboxes and "Hitboxes On" or "Hitboxes Off"
    end
    
    updateLabel()
    
    UserInputService.InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.H then
            toggleHitboxesAndChangeSky()
            updateLabel()
        end
    end)
end

createGui()
