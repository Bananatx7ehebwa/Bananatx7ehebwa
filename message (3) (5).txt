--// leaked by jai again omd!!!!
--// this nigga keep getting leakeddddddddddddd 😭 😭 😭 
--// LMAOOOOOOO

-- Nebula.wtf - FULL SCRIPT (Fixed Syntax Error + Visuals Tab + New Indicator)
-- Fixed: Missing ) in Replication Freeze toggle + removed duplicate crosshair section
-- Added: Visuals Tab with Color Correction
-- Updated: Replaced old indicator with new slick UI

local repo = 'https://raw.githubusercontent.com/mstudio45/LinoriaLib/main/'

local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

Library.ShowCustomCursor = true
Library.NotifySide = "Left"

local Window = Library:CreateWindow({
    Title = 'Nebula.wtf',
    Center = true,
    AutoShow = true,
    Resizable = true,
    ShowCustomCursor = true,
    UnlockMouseWhileOpen = true,
    TabPadding = 8,
    MenuFadeTime = 0.2
})

local MainTab = Window:AddTab('Main')
local LegitTab = Window:AddTab('Legitbot')
local RageTab = Window:AddTab('Ragebot')
local VisualTab = Window:AddTab('Visuals')

local AimGroup = MainTab:AddLeftGroupbox('Aimlock')
local SettingsGroup = MainTab:AddLeftGroupbox('Aimlock Settings')
local CamlockGroup = MainTab:AddRightGroupbox('Camlock Settings')
local TriggerGroup = LegitTab:AddLeftGroupbox('Triggerbot')
local HitboxGroup = LegitTab:AddRightGroupbox('Hitbox Expander')
local StrafeGroup = RageTab:AddLeftGroupbox('Target Strafe')
local ReplicationGroup = RageTab:AddRightGroupbox('Replication Freeze')

-- Services
local Players = game:GetService('Players')
local RunService = game:GetService('RunService')
local UserInputService = game:GetService('UserInputService')
local Workspace = game:GetService('Workspace')
local SoundService = game:GetService('SoundService')
local Lighting = game:GetService('Lighting')
local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera
local mouse = LocalPlayer:GetMouse()

-- === HIT LOGS NOTIFICATION LIBRARY ===
local notificationLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/laagginq/ui-libraries/main/xaxas-notification/src.lua"))()
local notifications = notificationLibrary.new({            
    NotificationLifetime = 3, 
    NotificationPosition = "Middle",
    
    TextFont = Enum.Font.Code,
    TextColor = Color3.fromRGB(255, 255, 255),
    TextSize = 15,
    
    TextStrokeTransparency = 0, 
    TextStrokeColor = Color3.fromRGB(0, 0, 0)
})

notifications:BuildNotificationUI()

-- === AIMLOCK SETTINGS VARIABLES ===
local enabled = false
local accomidationfactor = 0.06
local Plr = nil

-- Visual Settings
local showTracer = true
local showFOV = true
local showWatermark = true
local showTargetStats = true
local fovFollowTarget = true
local fovRadius = 60
local tracerToMouse = true

-- Hitsound Settings
local hitSoundEnabled = true
local selectedHitSound = "Skeet"
local hitSoundVolume = 0.5

-- Hit Effect Settings
local hitEffectEnabled = true
local selectedHitEffect = "Confetti"

-- Hit Logs Setting
local HitLogsEnabled = true

-- === TRIGGERBOT SETTINGS ===
getgenv().TB_Settings = {
    TriggerEnabled = false,
    TriggerDelay = 0.05,
    TriggerFOV = 120,
    KnifeCheck = true,
    ForceFieldCheck = true,
    KnockedCheck = true,
    AmmoCheck = true
}

local lastFire = 0

-- === TRIGGERBOT FUNCTIONS ===
local function isMelee(tool)
    if not tool then return false end
    local n = tool.Name:lower()
    return n:find("knife") or n:find("sword") or n:find("melee") or
           n:find("bat") or n:find("shovel") or n:find("katana") or
           n:find("stop sign") or n:find("machete")
end

local function FireTool()
    local char = LocalPlayer.Character
    if not char then return end
    local tool = char:FindFirstChildOfClass("Tool")
    if not tool then return end

    if getgenv().TB_Settings.KnifeCheck and isMelee(tool) then return end

    if getgenv().TB_Settings.AmmoCheck then
        local ammo = tool:FindFirstChild("Ammo")
        if not ammo or ammo.Value <= 0 then return end
    end

    pcall(function() tool:Activate() end)
    task.wait(0.02)
    pcall(function() tool:Deactivate() end)
end

-- Triggerbot Loop
RunService.RenderStepped:Connect(function()
    if not getgenv().TB_Settings.TriggerEnabled then return end

    local cx, cy = Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2
    local ray = Camera:ViewportPointToRay(cx, cy)

    local params = RaycastParams.new()
    params.FilterDescendantsInstances = {LocalPlayer.Character or {}}
    params.FilterType = Enum.RaycastFilterType.Blacklist

    local result = Workspace:Raycast(ray.Origin, ray.Direction * 5000, params)

    if result and result.Instance then
        local hitPart = result.Instance
        local targetChar = hitPart:FindFirstAncestorOfClass("Model")

        if targetChar and targetChar:FindFirstChildOfClass("Humanoid") and targetChar ~= LocalPlayer.Character then
            local humanoid = targetChar:FindFirstChildOfClass("Humanoid")
            local root = targetChar:FindFirstChild("HumanoidRootPart")
            if humanoid and root and humanoid.Health > 0 then

                if getgenv().TB_Settings.ForceFieldCheck and targetChar:FindFirstChildOfClass("ForceField") then return end

                if getgenv().TB_Settings.KnockedCheck then
                    local bodyEffects = targetChar:FindFirstChild("BodyEffects")
                    if bodyEffects and bodyEffects:FindFirstChild("K.O") and bodyEffects["K.O"].Value then return end
                end

                local screenPos = Camera:WorldToViewportPoint(root.Position)
                local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(cx, cy)).Magnitude

                if dist <= getgenv().TB_Settings.TriggerFOV and (tick() - lastFire) >= getgenv().TB_Settings.TriggerDelay then
                    lastFire = tick()
                    FireTool()
                end
            end
        end
    end
end)

-- === HITSOUNDS LIST ===
local HitSounds = {
    Bameware = "rbxassetid://3124331820",
    Bell = "rbxassetid://6534947240",
    Bubble = "rbxassetid://6534947588",
    Pick = "rbxassetid://1347140027",
    Pop = "rbxassetid://198598793",
    Rust = "rbxassetid://1255040462",
    Sans = "rbxassetid://3188795283",
    Fart = "rbxassetid://130833677",
    Big = "rbxassetid://5332005053",
    Vine = "rbxassetid://5332680810",
    Bruh = "rbxassetid://4578740568",
    Skeet = "rbxassetid://5633695679",
    Neverlose = "rbxassetid://6534948092",
    Fatality = "rbxassetid://6534947869",
    Bonk = "rbxassetid://5766898159",
    Minecraft = "rbxassetid://4018616850",
    Osu = "rbxassetid://7147454322",
    VineBoom = "rbxassetid://6308606116",
    MetalPipe = "rbxassetid://643396121",
}

-- === HIT EFFECTS ===
local hit_effects = {}

hit_effects.Confetti = function(position)
    local part = Instance.new("Part")
    part.Position = position
    part.Anchored = true
    part.Transparency = 1
    part.CanCollide = false
    part.Parent = Workspace

    local colors = {
        Color3.fromRGB(0, 255, 226),
        Color3.fromRGB(0, 25, 255),
        Color3.fromRGB(230, 255, 0),
        Color3.fromRGB(46, 255, 0),
        Color3.fromRGB(255, 0, 0)
    }

    for i = 1, 5 do
        local emitter = Instance.new("ParticleEmitter")
        emitter.Parent = part
        emitter.Acceleration = Vector3.new(0, -10, 0)
        emitter.Color = ColorSequence.new(colors[i])
        emitter.Lifetime = NumberRange.new(1, 2)
        emitter.Rate = 0
        emitter.RotSpeed = NumberRange.new(260, 260)
        emitter.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.1), NumberSequenceKeypoint.new(1, 0.1)})
        emitter.Speed = NumberRange.new(15, 15)
        emitter.SpreadAngle = Vector2.new(360, 360)
        emitter.Texture = "rbxassetid://241685484"
        emitter:Emit(20)
    end

    task.delay(3, function()
        part:Destroy()
    end)
end

hit_effects.Bubble = function(position)
    local part = Instance.new("Part")
    part.Position = position
    part.Anchored = true
    part.Transparency = 1
    part.CanCollide = false
    part.Parent = Workspace

    local color = Color3.fromRGB(91, 177, 252)

    local p1 = Instance.new("ParticleEmitter")
    p1.Parent = part
    p1.Color = ColorSequence.new(color)
    p1.Lifetime = NumberRange.new(0.5, 0.5)
    p1.LightEmission = 1
    p1.LockedToPart = true
    p1.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
    p1.Rate = 0
    p1.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(1, 10)})
    p1.Speed = NumberRange.new(1.5, 1.5)
    p1.Texture = "rbxassetid://1084991215"
    p1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1, 0), NumberSequenceKeypoint.new(0.6, 0), NumberSequenceKeypoint.new(1, 1)})
    p1.ZOffset = 1
    p1:Emit(1)

    local p2 = Instance.new("ParticleEmitter")
    p2.Parent = part
    p2.Color = ColorSequence.new(color)
    p2.Lifetime = NumberRange.new(0.5, 0.5)
    p2.LightEmission = 1
    p2.LockedToPart = true
    p2.Rate = 0
    p2.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(1, 10)})
    p2.Speed = NumberRange.new(0, 0)
    p2.Texture = "rbxassetid://1084991215"
    p2.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1, 0), NumberSequenceKeypoint.new(0.6, 0), NumberSequenceKeypoint.new(1, 1)})
    p2.ZOffset = 1
    p2:Emit(1)

    task.delay(1, function()
        part:Destroy()
    end)
end

-- === VISUALS ===
local tracer = Drawing.new('Line')
tracer.Visible = false
tracer.Color = Color3.fromRGB(255, 255, 255)
tracer.Thickness = 2

local fovCircle = Drawing.new('Circle')
fovCircle.Visible = false
fovCircle.Color = Color3.fromRGB(255, 255, 255)
fovCircle.Thickness = 2
fovCircle.NumSides = 100
fovCircle.Radius = fovRadius
fovCircle.Filled = false

local watermark = Drawing.new('Text')
watermark.Visible = false
watermark.Color = Color3.fromRGB(255, 255, 255)
watermark.Size = 13
watermark.Center = true
watermark.Outline = true
watermark.Font = 2
watermark.Text = "$$$nebula.wtf$$$"

local placemarker = Instance.new('Part')
placemarker.Anchored = true
placemarker.CanCollide = false
placemarker.Size = Vector3.new(7, 7, 7)
placemarker.Transparency = 1
placemarker.Material = Enum.Material.ForceField
placemarker.Parent = Workspace

-- Get closest player
local function getClosest()
    local closest = nil
    local shortest = math.huge
    local center = Camera.ViewportSize / 2

    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character.Humanoid.Health > 0 then
            local root = p.Character.HumanoidRootPart
            local screenPos, onScreen = Camera:WorldToViewportPoint(root.Position)
            if onScreen then
                local mag = (Vector2.new(screenPos.X, screenPos.Y) - center).Magnitude
                if mag < shortest then
                    shortest = mag
                    closest = p
                end
            end
        end
    end
    return closest
end

local function toggleLock()
    enabled = not enabled
    
    if enabled then
        Plr = getClosest()
        if not Plr then
            enabled = false
            Library:Notify('No player near center', 4)
            return
        end
        if showTracer then tracer.Visible = true end
        if showFOV then fovCircle.Visible = true end
        if showWatermark then watermark.Visible = true end
        if showTargetStats then
            if IndicatorUI then
                IndicatorUI.Enabled = true
            end
        end
        Library:Notify('Locked: ' .. Plr.DisplayName, 3)
    else
        Plr = nil
        tracer.Visible = false
        fovCircle.Visible = false
        watermark.Visible = false
        if IndicatorUI then
            IndicatorUI.Enabled = false
        end
        Library:Notify('Aimlock off', 3)
    end

    if Options and Options.AimlockToggle then
        Options.AimlockToggle:SetValue(enabled)
    end
end

-- Visual update
RunService.RenderStepped:Connect(function()
    if enabled and Plr and Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
        local root = Plr.Character.HumanoidRootPart
        local screenPos, onScreen = Camera:WorldToViewportPoint(root.Position)

        if onScreen then
            if showTracer then
                if tracerToMouse then
                    local mousePos = UserInputService:GetMouseLocation()
                    tracer.From = Vector2.new(mousePos.X, mousePos.Y)
                else
                    local center = Camera.ViewportSize / 2
                    tracer.From = Vector2.new(center.X, center.Y)
                end
                tracer.To = Vector2.new(screenPos.X, screenPos.Y)
                tracer.Visible = true
            end

            if showFOV then
                if fovFollowTarget then
                    fovCircle.Position = Vector2.new(screenPos.X, screenPos.Y)
                else
                    local center = Camera.ViewportSize / 2
                    fovCircle.Position = Vector2.new(center.X, center.Y)
                end
                fovCircle.Visible = true
            end

            if showWatermark then
                watermark.Position = Vector2.new(screenPos.X, screenPos.Y - 30)
                watermark.Visible = true
            end
        else
            tracer.Visible = false
            fovCircle.Visible = false
            watermark.Visible = false
        end
    else
        tracer.Visible = false
        fovCircle.Visible = false
        watermark.Visible = false
    end
end)

-- Prediction loop
RunService.Stepped:Connect(function()
    if enabled and Plr and Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
        local root = Plr.Character.HumanoidRootPart
        placemarker.CFrame = CFrame.new(root.Position + root.Velocity * accomidationfactor)
    end
end)

-- Silent Aim
local mt = getrawmetatable(mouse)
setreadonly(mt, false)
local old_index = mt.__index

mt.__index = newcclosure(function(self, key)
    if key == "Hit" and enabled and Plr and Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
        local root = Plr.Character.HumanoidRootPart
        return CFrame.new(root.Position + root.Velocity * accomidationfactor)
    end
    return old_index(self, key)
end)

-- === NEW SLICK INDICATOR GUI ===
local IndicatorUI = Instance.new("ScreenGui")
IndicatorUI.Name = "NebulaIndicator"
IndicatorUI.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
IndicatorUI.Parent = game:GetService("CoreGui")
IndicatorUI.Enabled = false

local MainFrame = Instance.new("Frame")
MainFrame.AnchorPoint = Vector2.new(0.5, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.5, 0, 1, -250)
MainFrame.Size = UDim2.new(0, 322, 0, 147)
MainFrame.Parent = IndicatorUI

local OuterBorder = Instance.new("Frame")
OuterBorder.BackgroundColor3 = Color3.fromRGB(96, 120, 190)
OuterBorder.BorderSizePixel = 0
OuterBorder.Position = UDim2.new(0, 1, 0, 1)
OuterBorder.Size = UDim2.new(1, -2, 1, -2)
OuterBorder.Parent = MainFrame

local InnerBorder = Instance.new("Frame")
InnerBorder.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
InnerBorder.BorderSizePixel = 0
InnerBorder.Position = UDim2.new(0, 1, 0, 1)
InnerBorder.Size = UDim2.new(1, -2, 1, -2)
InnerBorder.Parent = OuterBorder

local ContentFrame = Instance.new("Frame")
ContentFrame.BackgroundTransparency = 1
ContentFrame.BorderSizePixel = 0
ContentFrame.Position = UDim2.new(0, 1, 0, 2)
ContentFrame.Size = UDim2.new(1, -2, 1, -4)
ContentFrame.Parent = InnerBorder

local UIPadding_1 = Instance.new("UIPadding")
UIPadding_1.PaddingLeft = UDim.new(0, 6)
UIPadding_1.Parent = ContentFrame

local Holder = Instance.new("Frame")
Holder.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Holder.BorderSizePixel = 0
Holder.Position = UDim2.new(0, -3, 0, 16)
Holder.Size = UDim2.new(1, 0, 1, -18)
Holder.Name = "holder"
Holder.Parent = ContentFrame

local HolderInner1 = Instance.new("Frame")
HolderInner1.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
HolderInner1.BorderSizePixel = 0
HolderInner1.Position = UDim2.new(0, 1, 0, 1)
HolderInner1.Size = UDim2.new(1, -2, 1, -2)
HolderInner1.Parent = Holder

local HolderInner2 = Instance.new("Frame")
HolderInner2.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
HolderInner2.BorderSizePixel = 0
HolderInner2.Position = UDim2.new(0, 1, 0, 1)
HolderInner2.Size = UDim2.new(1, -2, 1, -2)
HolderInner2.Parent = HolderInner1

local UIPadding_2 = Instance.new("UIPadding")
UIPadding_2.PaddingLeft = UDim.new(0, 4)
UIPadding_2.PaddingTop = UDim.new(0, 4)
UIPadding_2.Parent = HolderInner2

local ContentArea = Instance.new("Frame")
ContentArea.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
ContentArea.BorderSizePixel = 0
ContentArea.Size = UDim2.new(1, -4, 1, -4)
ContentArea.Parent = HolderInner2

local ContentArea2 = Instance.new("Frame")
ContentArea2.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ContentArea2.BorderSizePixel = 0
ContentArea2.Position = UDim2.new(0, 1, 0, 1)
ContentArea2.Size = UDim2.new(1, -2, 1, -2)
ContentArea2.Parent = ContentArea

local ContentArea3 = Instance.new("Frame")
ContentArea3.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ContentArea3.BorderSizePixel = 0
ContentArea3.Position = UDim2.new(0, 1, 0, 1)
ContentArea3.Size = UDim2.new(1, -2, 1, -2)
ContentArea3.Parent = ContentArea2

local Gradient1 = Instance.new("UIGradient")
Gradient1.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
}
Gradient1.Parent = ContentArea3

local UIPadding_3 = Instance.new("UIPadding")
UIPadding_3.PaddingBottom = UDim.new(0, 3)
UIPadding_3.PaddingLeft = UDim.new(0, 4)
UIPadding_3.PaddingRight = UDim.new(0, 3)
UIPadding_3.PaddingTop = UDim.new(0, 4)
UIPadding_3.Parent = ContentArea3

local MainContent = Instance.new("Frame")
MainContent.BackgroundTransparency = 1
MainContent.BorderSizePixel = 0
MainContent.Size = UDim2.new(1, 0, 1, 3)
MainContent.Parent = ContentArea3

local UIListLayout_1 = Instance.new("UIListLayout")
UIListLayout_1.Padding = UDim.new(0, 4)
UIListLayout_1.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout_1.Parent = MainContent

local UIPadding_4 = Instance.new("UIPadding")
UIPadding_4.PaddingBottom = UDim.new(0, 4)
UIPadding_4.Parent = MainContent

local PlayerInfoContainer = Instance.new("Frame")
PlayerInfoContainer.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
PlayerInfoContainer.BorderSizePixel = 0
PlayerInfoContainer.Size = UDim2.new(1, -1, 1, 0)
PlayerInfoContainer.Parent = MainContent

local PlayerInfoInner1 = Instance.new("Frame")
PlayerInfoInner1.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
PlayerInfoInner1.BorderSizePixel = 0
PlayerInfoInner1.Position = UDim2.new(0, 1, 0, 1)
PlayerInfoInner1.Size = UDim2.new(1, -2, 1, -2)
PlayerInfoInner1.Parent = PlayerInfoContainer

local PlayerInfoInner2 = Instance.new("Frame")
PlayerInfoInner2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
PlayerInfoInner2.BorderSizePixel = 0
PlayerInfoInner2.Position = UDim2.new(0, 1, 0, 1)
PlayerInfoInner2.Size = UDim2.new(1, -2, 1, -2)
PlayerInfoInner2.Parent = PlayerInfoInner1

local Gradient2 = Instance.new("UIGradient")
Gradient2.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
}
Gradient2.Parent = PlayerInfoInner2

local TopBar = Instance.new("Frame")
TopBar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TopBar.BorderSizePixel = 0
TopBar.Size = UDim2.new(1, 0, 0, 2)
TopBar.Name = "bar"
TopBar.Parent = PlayerInfoInner2

local Gradient3 = Instance.new("UIGradient")
Gradient3.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(96, 120, 190)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 113, 169))
}
Gradient3.Parent = TopBar

local InfoHolder = Instance.new("Frame")
InfoHolder.BackgroundTransparency = 1
InfoHolder.BorderSizePixel = 0
InfoHolder.Position = UDim2.new(0, 1, 0, 22)
InfoHolder.Size = UDim2.new(1, -2, 1, -24)
InfoHolder.Name = "holder"
InfoHolder.Parent = PlayerInfoInner2

local UIPadding_5 = Instance.new("UIPadding")
UIPadding_5.PaddingBottom = UDim.new(0, 2)
UIPadding_5.PaddingLeft = UDim.new(0, 3)
UIPadding_5.PaddingRight = UDim.new(0, 3)
UIPadding_5.PaddingTop = UDim.new(0, -1)
UIPadding_5.Parent = InfoHolder

local PlayerInfo = Instance.new("Frame")
PlayerInfo.BackgroundTransparency = 1
PlayerInfo.BorderSizePixel = 0
PlayerInfo.Size = UDim2.new(1, 0, 1, 0)
PlayerInfo.Name = "playerinfo"
PlayerInfo.Parent = InfoHolder

local IconFrame = Instance.new("Frame")
IconFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
IconFrame.BorderSizePixel = 0
IconFrame.Size = UDim2.new(0, 68, 1, 0)
IconFrame.Name = "icon"
IconFrame.Parent = PlayerInfo

local IconInner1 = Instance.new("Frame")
IconInner1.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
IconInner1.BorderSizePixel = 0
IconInner1.Position = UDim2.new(0, 1, 0, 1)
IconInner1.Size = UDim2.new(1, -2, 1, -2)
IconInner1.Parent = IconFrame

local IconInner2 = Instance.new("Frame")
IconInner2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
IconInner2.BorderSizePixel = 0
IconInner2.Position = UDim2.new(0, 1, 0, 1)
IconInner2.Size = UDim2.new(1, -2, 1, -2)
IconInner2.Parent = IconInner1

local PlayerIcon = Instance.new("ImageLabel")
PlayerIcon.Image = "rbxthumb://type=AvatarHeadShot&id=1&w=420&h=420"
PlayerIcon.BackgroundTransparency = 1
PlayerIcon.BorderSizePixel = 0
PlayerIcon.Size = UDim2.new(1, 0, 1, 0)
PlayerIcon.Parent = IconInner2

local Gradient4 = Instance.new("UIGradient")
Gradient4.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
}
Gradient4.Parent = IconInner2

-- Health Bar
local HealthFrame = Instance.new("Frame")
HealthFrame.AnchorPoint = Vector2.new(0, 1)
HealthFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
HealthFrame.BorderSizePixel = 0
HealthFrame.Position = UDim2.new(0, 72, 1, 0)
HealthFrame.Size = UDim2.new(1, -72, 0, 14)
HealthFrame.Name = "health"
HealthFrame.Parent = PlayerInfo

local HealthInner1 = Instance.new("Frame")
HealthInner1.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
HealthInner1.BorderSizePixel = 0
HealthInner1.Position = UDim2.new(0, 1, 0, 1)
HealthInner1.Size = UDim2.new(1, -2, 1, -2)
HealthInner1.Parent = HealthFrame

local HealthInner2 = Instance.new("Frame")
HealthInner2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
HealthInner2.BorderSizePixel = 0
HealthInner2.Position = UDim2.new(0, 1, 0, 1)
HealthInner2.Size = UDim2.new(1, -2, 1, -2)
HealthInner2.Parent = HealthInner1

local Gradient5 = Instance.new("UIGradient")
Gradient5.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
}
Gradient5.Parent = HealthInner2

local HealthBarValue = Instance.new("Frame")
HealthBarValue.BackgroundColor3 = Color3.fromRGB(45, 195, 45)
HealthBarValue.BorderSizePixel = 0
HealthBarValue.Size = UDim2.new(1, 0, 1, 0)
HealthBarValue.Name = "healthbarvalue"
HealthBarValue.Parent = HealthInner2

local Gradient6 = Instance.new("UIGradient")
Gradient6.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(125, 125, 125))
}
Gradient6.Parent = HealthBarValue

local HealthText = Instance.new("TextLabel")
HealthText.Font = Enum.Font.SourceSans
HealthText.Text = "100/100"
HealthText.TextColor3 = Color3.fromRGB(180, 180, 180)
HealthText.TextSize = 12
HealthText.AnchorPoint = Vector2.new(0.5, 0.5)
HealthText.BackgroundTransparency = 1
HealthText.BorderSizePixel = 0
HealthText.Position = UDim2.new(0.5, 0, 0.5, 0)
HealthText.Size = UDim2.new(1, 0, 1, 0)
HealthText.Name = "healthvalue"
HealthText.Parent = HealthInner2

local UIStroke1 = Instance.new("UIStroke")
UIStroke1.LineJoinMode = Enum.LineJoinMode.Miter
UIStroke1.Parent = HealthText

-- Name and Distance
local InfoFrame = Instance.new("Frame")
InfoFrame.BackgroundTransparency = 1
InfoFrame.BorderSizePixel = 0
InfoFrame.Position = UDim2.new(0.27, 0, 0.029, 0)
InfoFrame.Size = UDim2.new(0, 198, 0, 31)
InfoFrame.Parent = PlayerInfo

local UIListLayout_2 = Instance.new("UIListLayout")
UIListLayout_2.Padding = UDim.new(0, 2)
UIListLayout_2.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout_2.Parent = InfoFrame

local NameLabel = Instance.new("TextLabel")
NameLabel.Font = Enum.Font.SourceSans
NameLabel.Text = "Player (@username)"
NameLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
NameLabel.TextSize = 12
NameLabel.TextXAlignment = Enum.TextXAlignment.Left
NameLabel.TextYAlignment = Enum.TextYAlignment.Top
NameLabel.BackgroundTransparency = 1
NameLabel.BorderSizePixel = 0
NameLabel.Size = UDim2.new(0.39, 0, 0.42, 0)
NameLabel.Name = "name"
NameLabel.Parent = InfoFrame

local UIStroke2 = Instance.new("UIStroke")
UIStroke2.LineJoinMode = Enum.LineJoinMode.Miter
UIStroke2.Parent = NameLabel

local DistanceLabel = Instance.new("TextLabel")
DistanceLabel.Font = Enum.Font.SourceSans
DistanceLabel.Text = "0 studs"
DistanceLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
DistanceLabel.TextSize = 12
DistanceLabel.TextXAlignment = Enum.TextXAlignment.Left
DistanceLabel.TextYAlignment = Enum.TextYAlignment.Top
DistanceLabel.BackgroundTransparency = 1
DistanceLabel.BorderSizePixel = 0
DistanceLabel.Size = UDim2.new(0.39, 0, 0.42, 0)
DistanceLabel.Name = "studs"
DistanceLabel.Parent = InfoFrame

local UIStroke3 = Instance.new("UIStroke")
UIStroke3.LineJoinMode = Enum.LineJoinMode.Miter
UIStroke3.Parent = DistanceLabel

-- Armor Bar (optional, can be hidden if no armor)
local ArmorFrame = Instance.new("Frame")
ArmorFrame.AnchorPoint = Vector2.new(0, 1)
ArmorFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
ArmorFrame.BorderSizePixel = 0
ArmorFrame.Position = UDim2.new(0, 72, 0.794, 0)
ArmorFrame.Size = UDim2.new(1, -72, 0, 14)
ArmorFrame.Name = "armor"
ArmorFrame.Visible = false
ArmorFrame.Parent = PlayerInfo

local ArmorInner1 = Instance.new("Frame")
ArmorInner1.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ArmorInner1.BorderSizePixel = 0
ArmorInner1.Position = UDim2.new(0, 1, 0, 1)
ArmorInner1.Size = UDim2.new(1, -2, 1, -2)
ArmorInner1.Parent = ArmorFrame

local ArmorInner2 = Instance.new("Frame")
ArmorInner2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ArmorInner2.BorderSizePixel = 0
ArmorInner2.Position = UDim2.new(0, 1, 0, 1)
ArmorInner2.Size = UDim2.new(1, -2, 1, -2)
ArmorInner2.Parent = ArmorInner1

local Gradient7 = Instance.new("UIGradient")
Gradient7.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 20))
}
Gradient7.Parent = ArmorInner2

local ArmorBarValue = Instance.new("Frame")
ArmorBarValue.BackgroundColor3 = Color3.fromRGB(45, 45, 195)
ArmorBarValue.BorderSizePixel = 0
ArmorBarValue.Size = UDim2.new(1, 0, 1, 0)
ArmorBarValue.Name = "armorbarvalue"
ArmorBarValue.Parent = ArmorInner2

local Gradient8 = Instance.new("UIGradient")
Gradient8.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(125, 125, 125))
}
Gradient8.Parent = ArmorBarValue

local ArmorText = Instance.new("TextLabel")
ArmorText.Font = Enum.Font.SourceSans
ArmorText.Text = "100/100"
ArmorText.TextColor3 = Color3.fromRGB(180, 180, 180)
ArmorText.TextSize = 12
ArmorText.AnchorPoint = Vector2.new(0.5, 0.5)
ArmorText.BackgroundTransparency = 1
ArmorText.BorderSizePixel = 0
ArmorText.Position = UDim2.new(0.5, 0, 0.5, 0)
ArmorText.Size = UDim2.new(1, 0, 1, 0)
ArmorText.Name = "armorvalue"
ArmorText.Parent = ArmorInner2

local UIStroke4 = Instance.new("UIStroke")
UIStroke4.LineJoinMode = Enum.LineJoinMode.Miter
UIStroke4.Parent = ArmorText

-- Section titles
local TopLabel1 = Instance.new("TextLabel")
TopLabel1.Font = Enum.Font.SourceSans
TopLabel1.Text = "Info"
TopLabel1.TextColor3 = Color3.fromRGB(136, 136, 136)
TopLabel1.TextSize = 12
TopLabel1.TextXAlignment = Enum.TextXAlignment.Left
TopLabel1.BackgroundTransparency = 1
TopLabel1.BorderSizePixel = 0
TopLabel1.Size = UDim2.new(1, 0, 1, 0)
TopLabel1.Parent = Instance.new("Frame")
TopLabel1.Parent.BackgroundTransparency = 1
TopLabel1.Parent.BorderSizePixel = 0
TopLabel1.Parent.Position = UDim2.new(0, 0, 0, 2)
TopLabel1.Parent.Size = UDim2.new(1, 0, 0, 20)
TopLabel1.Parent.Name = "top"
TopLabel1.Parent.Parent = PlayerInfoInner2

local TopLabel2 = Instance.new("TextLabel")
TopLabel2.Font = Enum.Font.SourceSans
TopLabel2.Text = "Indicator"
TopLabel2.TextColor3 = Color3.fromRGB(180, 180, 180)
TopLabel2.TextSize = 12
TopLabel2.TextXAlignment = Enum.TextXAlignment.Left
TopLabel2.BackgroundTransparency = 1
TopLabel2.BorderSizePixel = 0
TopLabel2.Size = UDim2.new(0.5, 0, 1, 0)
TopLabel2.Parent = Instance.new("Frame")
TopLabel2.Parent.BackgroundTransparency = 1
TopLabel2.Parent.BorderSizePixel = 0
TopLabel2.Parent.Size = UDim2.new(1, -4, 0, 20)
TopLabel2.Parent.Name = "top"
TopLabel2.Parent.Parent = ContentFrame

-- === INDICATOR UPDATE FUNCTION ===
local function updateIndicator()
    if not showTargetStats or not IndicatorUI then
        IndicatorUI.Enabled = false
        return
    end
    
    if enabled and Plr and Plr.Character then
        IndicatorUI.Enabled = true
        
        local humanoid = Plr.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            -- Update health
            local healthPercent = math.clamp(humanoid.Health / humanoid.MaxHealth, 0, 1)
            HealthBarValue.Size = UDim2.new(healthPercent, 0, 1, 0)
            HealthText.Text = math.floor(humanoid.Health) .. "/" .. math.floor(humanoid.MaxHealth)
            
            -- Update player icon
            PlayerIcon.Image = "rbxthumb://type=AvatarHeadShot&id=" .. Plr.UserId .. "&w=420&h=420"
            
            -- Update name
            NameLabel.Text = Plr.DisplayName .. " (@" .. Plr.Name .. ")"
            
            -- Update distance
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and Plr.Character:FindFirstChild("HumanoidRootPart") then
                local distance = (LocalPlayer.Character.HumanoidRootPart.Position - Plr.Character.HumanoidRootPart.Position).Magnitude
                DistanceLabel.Text = math.floor(distance) .. " studs"
            else
                DistanceLabel.Text = "N/A studs"
            end
            
            -- Update armor if exists
            local armor = Plr.Character:FindFirstChild("BodyEffects") and Plr.Character.BodyEffects:FindFirstChild("Armor")
            if armor then
                ArmorFrame.Visible = true
                local armorValue = armor.Value
                local armorPercent = math.clamp(armorValue / 100, 0, 1)
                ArmorBarValue.Size = UDim2.new(armorPercent, 0, 1, 0)
                ArmorText.Text = math.floor(armorValue) .. "/100"
            else
                ArmorFrame.Visible = false
            end
        end
    else
        IndicatorUI.Enabled = false
    end
end

-- Update indicator every frame
RunService.RenderStepped:Connect(updateIndicator)

-- Hitsound Object
local hitSoundObject = Instance.new("Sound")
hitSoundObject.Parent = SoundService
hitSoundObject.SoundId = HitSounds[selectedHitSound]
hitSoundObject.Volume = hitSoundVolume

-- Health cache for hit detection
local lastHealthCache = {}

-- Hit Detection (Aimlock Hits)
RunService.PostSimulation:Connect(function()
    if not enabled or not Plr or not Plr.Character or not Plr.Character:FindFirstChild("HumanoidRootPart") then 
        if Plr then lastHealthCache[Plr.UserId] = nil end
        return 
    end
    
    local humanoid = Plr.Character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    local currentHealth = humanoid.Health
    local userId = Plr.UserId
    local lastHealth = lastHealthCache[userId] or currentHealth
    local hitPart = "HumanoidRootPart"

    if currentHealth < lastHealth and currentHealth > 0 then
        if hitSoundEnabled then
            hitSoundObject.SoundId = HitSounds[selectedHitSound]
            hitSoundObject.Volume = hitSoundVolume
            hitSoundObject:Play()
        end

        if hitEffectEnabled and hit_effects[selectedHitEffect] then
            local pos = Plr.Character.HumanoidRootPart.Position
            hit_effects[selectedHitEffect](pos)
        end

        if HitLogsEnabled then
            local hitMessage = string.format(
                "+1 Hit | %s | Target: %s | Health: %d",
                hitPart,
                Plr.DisplayName,
                math.floor(currentHealth)
            )
            notifications:Notify(hitMessage)
        end
    end

    lastHealthCache[userId] = currentHealth
end)

-- === UI CONTROLS ===

-- Aimlock
AimGroup:AddToggle('AimlockToggle', {
    Text = 'Enable Aimlock',
    Default = false,
    Callback = function(val)
        if val ~= enabled then toggleLock() end
    end
})

AimGroup:AddInput('PredictionInput', {
    Text = 'Prediction',
    Default = '0.06',
    Numeric = true,
    Finished = true,
    Callback = function(text)
        local num = tonumber(text)
        if num and num >= 0 then
            accomidationfactor = num
        end
    end
})

AimGroup:AddButton({
    Text = 'Spawn Lock Button',
    Func = function()
        local oldGui = game.CoreGui:FindFirstChild("ExtortGUI")
        if oldGui then oldGui:Destroy() end

        local gui = Instance.new("ScreenGui")
        local lock_button = Instance.new("ImageButton")
        local button_corner = Instance.new("UICorner")

        gui.Name = "ExtortGUI"
        gui.Parent = game.CoreGui
        gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        gui.ResetOnSpawn = false

        lock_button.Parent = gui
        lock_button.Active = true
        lock_button.Draggable = true
        lock_button.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        lock_button.BackgroundTransparency = 0.35
        lock_button.Size = UDim2.new(0, 90, 0, 90)
        lock_button.Image = "rbxassetid://90777812714091"
        lock_button.Position = UDim2.new(0.5, -45, 0.5, -45)

        button_corner.CornerRadius = UDim.new(0.2, 0)
        button_corner.Parent = lock_button

        lock_button.MouseButton1Click:Connect(toggleLock)
        Library:Notify('Lock button spawned!', 5)
    end
})

-- Aimlock Settings
SettingsGroup:AddSlider('FOVSize', {
    Text = 'FOV Size',
    Default = 60,
    Min = 20,
    Max = 300,
    Rounding = 0,
    Callback = function(val)
        fovRadius = val
        fovCircle.Radius = val
    end
})

SettingsGroup:AddToggle('WatermarkToggle', {
    Text = 'Show Watermark',
    Default = true,
    Callback = function(val)
        showWatermark = val
    end
})

SettingsGroup:AddToggle('TracerToggle', {
    Text = 'Show Tracer',
    Default = true,
    Callback = function(val)
        showTracer = val
    end
})

SettingsGroup:AddDropdown('TracerMode', {
    Values = { 'To Mouse', 'To Screen Center' },
    Default = 1,
    Text = 'Tracer From',
    Callback = function(val)
        tracerToMouse = (val == 'To Mouse')
    end
})

SettingsGroup:AddToggle('FOVToggle', {
    Text = 'Show FOV Circle',
    Default = true,
    Callback = function(val)
        showFOV = val
    end
})

SettingsGroup:AddDropdown('FOVMode', {
    Values = { 'Follow Target', 'Screen Center' },
    Default = 1,
    Callback = function(val)
        fovFollowTarget = (val == 'Follow Target')
    end
})

SettingsGroup:AddToggle('TargetStatsToggle', {
    Text = 'Show Target Stats',
    Default = true,
    Callback = function(val)
        showTargetStats = val
        if not val and IndicatorUI then
            IndicatorUI.Enabled = false
        end
    end
})

-- Hitsound Controls
SettingsGroup:AddToggle('HitSoundToggle', {
    Text = 'Enable Hitsound',
    Default = true,
    Callback = function(val)
        hitSoundEnabled = val
    end
})

SettingsGroup:AddDropdown('HitSoundSelect', {
    Values = {'Bameware','Bell','Bubble','Pick','Pop','Rust','Sans','Fart','Big','Vine','Bruh','Skeet','Neverlose','Fatality','Bonk','Minecraft','Osu','VineBoom','MetalPipe'},
    Default = 'Skeet',
    Text = 'Hitsound Type',
    Callback = function(val)
        selectedHitSound = val
    end
})

SettingsGroup:AddSlider('HitSoundVolume', {
    Text = 'Hitsound Volume (%)',
    Default = 50,
    Min = 0,
    Max = 100,
    Rounding = 1,
    Callback = function(val)
        hitSoundVolume = val / 100
    end
})

-- Hit Effect Controls
SettingsGroup:AddToggle('HitEffectToggle', {
    Text = 'Enable Hit Effect',
    Default = true,
    Callback = function(val)
        hitEffectEnabled = val
    end
})

SettingsGroup:AddDropdown('HitEffectSelect', {
    Values = {'Confetti', 'Bubble'},
    Default = 'Confetti',
    Text = 'Hit Effect Type',
    Callback = function(val)
        selectedHitEffect = val
    end
})

-- Hit Logs Toggle
SettingsGroup:AddToggle('HitLogsToggle', {
    Text = 'Enable Hit Logs',
    Default = true,
    Callback = function(val)
        HitLogsEnabled = val
    end
})

-- === LEGITBOT UI (Triggerbot) ===
TriggerGroup:AddToggle('TriggerEnabled', {
    Text = 'Enable Triggerbot',
    Default = false,
    Callback = function(val)
        getgenv().TB_Settings.TriggerEnabled = val
    end
})

TriggerGroup:AddSlider('TriggerDelay', {
    Text = 'Trigger Delay (seconds)',
    Default = 0.05,
    Min = 0,
    Max = 0.5,
    Rounding = 3,
    Callback = function(val)
        getgenv().TB_Settings.TriggerDelay = val
    end
})

TriggerGroup:AddSlider('TriggerFOV', {
    Text = 'Trigger FOV (pixels)',
    Default = 120,
    Min = 10,
    Max = 300,
    Rounding = 0,
    Callback = function(val)
        getgenv().TB_Settings.TriggerFOV = val
    end
})

TriggerGroup:AddToggle('KnifeCheck', {
    Text = 'Melee/Knife Check',
    Default = true,
    Callback = function(val)
        getgenv().TB_Settings.KnifeCheck = val
    end
})

TriggerGroup:AddToggle('ForceFieldCheck', {
    Text = 'ForceField Check',
    Default = true,
    Callback = function(val)
        getgenv().TB_Settings.ForceFieldCheck = val
    end
})

TriggerGroup:AddToggle('KnockedCheck', {
    Text = 'Knocked Check (K.O)',
    Default = true,
    Callback = function(val)
        getgenv().TB_Settings.KnockedCheck = val
    end
})

TriggerGroup:AddToggle('AmmoCheck', {
    Text = 'Ammo Check',
    Default = true,
    Callback = function(val)
        getgenv().TB_Settings.AmmoCheck = val
    end
})

-- === CAMLOCK ===
getgenv().CamlockSettings = {
    Enabled = false,
    HitPart = "HumanoidRootPart",
    Prediction = 0.06,
    HorizontalPrediction = 0.13,
    VerticalPrediction = 0.11,
    HorizontalStrength = 1,
    VerticalStrength = 1,
    CameraSmoothness = 0.12,
    CameraShake = 0.05,
    JumpOffset = -1.2
}

local Target = nil
local lock_button = nil
local locking = false

CamlockGroup:AddToggle('EnableCamlock', {
    Text = 'Enable Camlock',
    Default = false,
    Callback = function(val)
        getgenv().CamlockSettings.Enabled = val
        if not val then
            Target = nil
            locking = false
        end
    end
})

CamlockGroup:AddInput('PredictionInput', {
    Text = 'Prediction',
    Default = tostring(getgenv().CamlockSettings.Prediction),
    Numeric = true,
    Finished = true,
    Callback = function(val)
        local num = tonumber(val)
        if num then getgenv().CamlockSettings.Prediction = num end
    end
})

CamlockGroup:AddSlider('HorizontalPrediction', {
    Text = 'Horizontal Prediction',
    Default = getgenv().CamlockSettings.HorizontalPrediction,
    Min = 0,
    Max = 1,
    Rounding = 3,
    Callback = function(val)
        getgenv().CamlockSettings.HorizontalPrediction = val
    end
})

CamlockGroup:AddSlider('VerticalPrediction', {
    Text = 'Vertical Prediction',
    Default = getgenv().CamlockSettings.VerticalPrediction,
    Min = 0,
    Max = 1,
    Rounding = 3,
    Callback = function(val)
        getgenv().CamlockSettings.VerticalPrediction = val
    end
})

CamlockGroup:AddSlider('HorizontalStrength', {
    Text = 'Horizontal Strength',
    Default = getgenv().CamlockSettings.HorizontalStrength,
    Min = 0,
    Max = 5,
    Rounding = 2,
    Callback = function(val)
        getgenv().CamlockSettings.HorizontalStrength = val
    end
})

CamlockGroup:AddSlider('VerticalStrength', {
    Text = 'Vertical Strength',
    Default = getgenv().CamlockSettings.VerticalStrength,
    Min = 0,
    Max = 5,
    Rounding = 2,
    Callback = function(val)
        getgenv().CamlockSettings.VerticalStrength = val
    end
})

CamlockGroup:AddSlider('CameraSmoothness', {
    Text = 'Camera Smoothness',
    Default = getgenv().CamlockSettings.CameraSmoothness,
    Min = 0.01,
    Max = 1,
    Rounding = 3,
    Callback = function(val)
        getgenv().CamlockSettings.CameraSmoothness = val
    end
})

CamlockGroup:AddSlider('CameraShake', {
    Text = 'Camera Shake',
    Default = getgenv().CamlockSettings.CameraShake,
    Min = 0,
    Max = 1,
    Rounding = 3,
    Callback = function(val)
        getgenv().CamlockSettings.CameraShake = val
    end
})

CamlockGroup:AddSlider('JumpOffset', {
    Text = 'Jump Offset',
    Default = getgenv().CamlockSettings.JumpOffset,
    Min = -5,
    Max = 5,
    Rounding = 2,
    Callback = function(val)
        getgenv().CamlockSettings.JumpOffset = val
    end
})

CamlockGroup:AddButton({
    Text = 'Create Button',
    Func = function()
        local oldGui = game.CoreGui:FindFirstChild("ExtortGUI")
        if oldGui then oldGui:Destroy() end

        local gui = Instance.new("ScreenGui")
        lock_button = Instance.new("ImageButton")
        local button_corner = Instance.new("UICorner")

        gui.Name = "ExtortGUI"
        gui.Parent = game.CoreGui
        gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        gui.ResetOnSpawn = false

        lock_button.Name = "LockButton"
        lock_button.Parent = gui
        lock_button.Active = true
        lock_button.Draggable = true
        lock_button.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        lock_button.BackgroundTransparency = 0.35
        lock_button.Size = UDim2.new(0, 90, 0, 90)
        lock_button.Image = "rbxassetid://72196083151319"
        lock_button.Position = UDim2.new(0.5, -25, 0.5, -25)

        button_corner.CornerRadius = UDim.new(0.2, 0)
        button_corner.Parent = lock_button

        lock_button.MouseButton1Click:Connect(function()
            locking = not locking
            if locking then
                Target = getClosest()
                lock_button.Image = "rbxassetid://15178561786"
                Library:Notify('Target locked!', 3)
            else
                Target = nil
                lock_button.Image = "rbxassetid://72196083151319"
                Library:Notify('Target unlocked!', 3)
            end
        end)

        Library:Notify('Lock button created!', 5)
    end
})

-- Camlock Logic
RunService.RenderStepped:Connect(function()
    if getgenv().CamlockSettings.Enabled and locking and Target and Target.Character and Target.Character:FindFirstChild(getgenv().CamlockSettings.HitPart) then
        local Part = Target.Character[getgenv().CamlockSettings.HitPart]
        local Velocity = Part.AssemblyLinearVelocity

        local PredictedPos = Part.Position
            + Vector3.new(
                Velocity.X * getgenv().CamlockSettings.HorizontalPrediction * getgenv().CamlockSettings.HorizontalStrength,
                Velocity.Y * getgenv().CamlockSettings.VerticalPrediction * getgenv().CamlockSettings.VerticalStrength + getgenv().CamlockSettings.JumpOffset,
                Velocity.Z * getgenv().CamlockSettings.HorizontalPrediction * getgenv().CamlockSettings.HorizontalStrength
            )

        local Shake = Vector3.new(
            math.random(-100,100)/100 * getgenv().CamlockSettings.CameraShake,
            math.random(-100,100)/100 * getgenv().CamlockSettings.CameraShake,
            0
        )

        local CFrameTarget = CFrame.new(Camera.CFrame.Position, PredictedPos + Shake)
        Camera.CFrame = Camera.CFrame:Lerp(CFrameTarget, getgenv().CamlockSettings.CameraSmoothness)
    end
end)

-- === HITBOX EXPANDER (LEGITBOT) ===
local HitboxSettings = {
    Enabled = false,
    Size = 10,
    Transparency = 0.5,
    Visualizer = true
}

local HitboxCache = {}

local function RestoreHitboxes()
    for part, data in pairs(HitboxCache) do
        if part and part.Parent then
            part.Size = data.Size
            part.Transparency = data.Transparency
            part.Material = data.Material
            part.Color = data.Color
            part.CanCollide = false
        end
    end
    table.clear(HitboxCache)
end

RunService.Heartbeat:Connect(function()
    if not HitboxSettings.Enabled then return end

    for part in pairs(HitboxCache) do
        if not part or not part.Parent then
            HitboxCache[part] = nil
        end
    end

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character then
            local hum = plr.Character:FindFirstChildOfClass("Humanoid")
            local hrp = plr.Character:FindFirstChild("HumanoidRootPart")

            if hum and hrp and hum.Health > 0 then
                if not HitboxCache[hrp] then
                    HitboxCache[hrp] = {
                        Size = hrp.Size,
                        Transparency = hrp.Transparency,
                        Material = hrp.Material,
                        Color = hrp.Color
                    }
                end

                hrp.CanCollide = false
                hrp.Size = Vector3.new(
                    HitboxSettings.Size,
                    HitboxSettings.Size,
                    HitboxSettings.Size
                )

                if HitboxSettings.Visualizer then
                    hrp.Material = Enum.Material.ForceField
                    hrp.Color = Color3.fromRGB(255, 0, 0)
                    hrp.Transparency = HitboxSettings.Transparency
                else
                    hrp.Transparency = 1
                end
            end
        end
    end
end)

-- === UI (LEGITBOT → RIGHT GROUPBOX) ===
HitboxGroup:AddToggle('HitboxEnabled', {
    Text = 'Enable Hitbox Expander',
    Default = false,
    Callback = function(val)
        HitboxSettings.Enabled = val
        if not val then
            RestoreHitboxes()
        end
    end
})

HitboxGroup:AddSlider('HitboxSize', {
    Text = 'Hitbox Size',
    Default = 10,
    Min = 2,
    Max = 30,
    Rounding = 0,
    Callback = function(val)
        HitboxSettings.Size = val
    end
})

HitboxGroup:AddToggle('HitboxVisualizer', {
    Text = 'Show Visualizer',
    Default = true,
    Callback = function(val)
        HitboxSettings.Visualizer = val
    end
})

HitboxGroup:AddSlider('HitboxTransparency', {
    Text = 'Visualizer Transparency',
    Default = 50,
    Min = 0,
    Max = 100,
    Rounding = 0,
    Callback = function(val)
        HitboxSettings.Transparency = val / 100
    end
})

-- === RAGEBOT TAB ===
-- Target Strafe Settings
getgenv().TargetStrafeSettings = {
    Enabled = false,
    Speed = 4,
    Distance = 10,
    Height = 3,
    Mode = 'Orbit'
}

-- Enable/Disable
StrafeGroup:AddToggle('TargetStrafeEnabled', {
    Text = 'Enable Target Strafe',
    Default = false,
    Callback = function(val)
        getgenv().TargetStrafeSettings.Enabled = val
    end
})

-- Speed slider
StrafeGroup:AddSlider('TargetStrafeSpeed', {
    Text = 'Speed',
    Default = 4,
    Min = 1,
    Max = 20,
    Rounding = 1,
    Callback = function(val)
        getgenv().TargetStrafeSettings.Speed = val
    end
})

-- Distance slider
StrafeGroup:AddSlider('TargetStrafeDistance', {
    Text = 'Distance',
    Default = 10,
    Min = 1,
    Max = 50,
    Rounding = 1,
    Callback = function(val)
        getgenv().TargetStrafeSettings.Distance = val
    end
})

-- Height slider
StrafeGroup:AddSlider('TargetStrafeHeight', {
    Text = 'Height',
    Default = 3,
    Min = 0,
    Max = 20,
    Rounding = 1,
    Callback = function(val)
        getgenv().TargetStrafeSettings.Height = val
    end
})

-- Mode dropdown
StrafeGroup:AddDropdown('TargetStrafeMode', {
    Values = {'Orbit', 'Random'},
    Default = 'Orbit',
    Text = 'Mode',
    Callback = function(val)
        getgenv().TargetStrafeSettings.Mode = val
    end
})

-- === TARGET STRAFE LOGIC ===
local strafeAngle = 0

RunService.RenderStepped:Connect(function(dt)
    local target = Plr
    if getgenv().TargetStrafeSettings.Enabled and target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        local targetRoot = target.Character.HumanoidRootPart
        if root then
            local dist = getgenv().TargetStrafeSettings.Distance
            local speed = getgenv().TargetStrafeSettings.Speed
            local height = getgenv().TargetStrafeSettings.Height

            if getgenv().TargetStrafeSettings.Mode == 'Orbit' then
                strafeAngle = strafeAngle + speed * dt
                local offset = Vector3.new(math.cos(strafeAngle) * dist, height, math.sin(strafeAngle) * dist)
                root.CFrame = CFrame.new(targetRoot.Position + offset, targetRoot.Position)
            elseif getgenv().TargetStrafeSettings.Mode == 'Random' then
                strafeAngle = math.random() * math.pi * 2
                local offset = Vector3.new(math.cos(strafeAngle) * dist, height, math.sin(strafeAngle) * dist)
                root.CFrame = CFrame.new(targetRoot.Position + offset, targetRoot.Position)
            end
        end
    end
end)

-- Replication Freeze
local note = ReplicationGroup:AddLabel('(resets you can be bugged)')

local enabled = false
local strength = 0

ReplicationGroup:AddToggle('RepFreezeToggle', {
    Text = 'Enable Replication Freeze',
    Default = false,
    Tooltip = 'Fake your position, will reset you'
}):OnChanged(function(value)
    enabled = value
    if enabled then
        coroutine.wrap(function()
            local Players = game:GetService("Players")
            local LocalPlayer = Players.LocalPlayer

            if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.CharacterAdded:Wait()
            end

            local Character = LocalPlayer.Character
            local Humanoid = Character:WaitForChild("Humanoid")
            local LastPivot = Character:GetPivot()

            setfflag("NextGenReplicatorEnabledWrite4", "true")
            Character:BreakJoints()
            Humanoid.Health = 0
            Humanoid:TakeDamage(Humanoid.MaxHealth)
            Humanoid:ChangeState("Dead")

            LocalPlayer.CharacterAdded:Wait()
            task.wait(0.2)
            if LocalPlayer.Character then
                LocalPlayer.Character:PivotTo(LastPivot)
            end
        end)()
    end
end)

ReplicationGroup:AddSlider('RepFreezeStrength', {
    Text = 'Strength',
    Default = 0,
    Min = 0,
    Max = 100,
    Rounding = 1,
    Suffix = ''
}):OnChanged(function(value)
    strength = value
end)

-- === VISUALS TAB - COLOR CORRECTION ===
local colorCorrection = Instance.new('ColorCorrectionEffect')
colorCorrection.Name = 'CustomColorCorrection'
colorCorrection.Parent = Lighting

local ColorCorrectionGroup = VisualTab:AddRightGroupbox("Color Correction")

ColorCorrectionGroup:AddToggle('CCEnabled', {
    Text = 'Enabled',
    Default = false,
    Callback = function(val)
        colorCorrection.Enabled = val
    end,
})

ColorCorrectionGroup:AddSlider('Saturation', {
    Text = 'Saturation',
    Min = -1,
    Max = 1,
    Default = 0,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Saturation = val
    end,
})

ColorCorrectionGroup:AddSlider('Contrast', {
    Text = 'Contrast',
    Min = 0,
    Max = 2,
    Default = 1,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Contrast = val
    end,
})

ColorCorrectionGroup:AddSlider('BrightnessCC', {
    Text = 'Brightness',
    Min = 0,
    Max = 2,
    Default = 0,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Brightness = val
    end,
})

ColorCorrectionGroup:AddSlider('Exposure', {
    Text = 'Exposure',
    Min = -5,
    Max = 5,
    Default = 0,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.TintColor = Color3.new(val, val, val)
    end,
})

-- Initialize
colorCorrection.Enabled = false
colorCorrection.Saturation = 0
colorCorrection.Contrast = 1
colorCorrection.Brightness = 0

-- Strength slider (optional)
ReplicationGroup:AddSlider('RepFreezeStrength', {
    Text = 'Strength',
    Default = 0,
    Min = 0,
    Max = 100,
    Rounding = 1,
    Suffix = '',
}):OnChanged(function(value)
    strength = value
    -- Can later modify fake position offset using this value
end)

-- Add this inside your RageTab
local AutoArmorGroup = RageTab:AddRightGroupbox('Auto Armor')

-- Toggle
local autoArmorEnabled = false
AutoArmorGroup:AddToggle('AutoArmorToggle', {
    Text = 'Enable Auto Armor',
    Default = false,
    Tooltip = 'Automatically buys armor when close',
}):OnChanged(function(value)
    autoArmorEnabled = value
end)

-- Distance slider
local armorDistance = 100
AutoArmorGroup:AddSlider('AutoArmorDistance', {
    Text = 'Distance',
    Default = 100,
    Min = 10,
    Max = 300,
    Rounding = 1,
    Suffix = ' studs',
}):OnChanged(function(value)
    armorDistance = value
end)

-- Speed slider
local armorSpeed = 0.1
AutoArmorGroup:AddSlider('AutoArmorSpeed', {
    Text = 'Speed',
    Default = 0.1,
    Min = 0.01,
    Max = 1,
    Rounding = 2,
    Suffix = ' sec',
}):OnChanged(function(value)
    armorSpeed = value
end)

-- Main Auto Armor logic
local cloneref = getgenv().cloneref or function(...) return ... end
local fireclickdetector = getgenv().fireclickdetector or function(...) return nil end
local GameReference = cloneref(Game)

if not GameReference:IsLoaded() then Game.Loaded:Wait() end

local Workspace = cloneref(GameReference:GetService("Workspace"))
local Players = cloneref(GameReference:GetService("Players"))

local LocalPlayer = Players.LocalPlayer
local LocalCharacter = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 1e9)
local LocalRootPart = LocalHumanoid and LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 1e9)

-- Update character on respawn
LocalPlayer.CharacterAdded:Connect(function(Character)
    LocalCharacter = Character
    LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 1e9)
    LocalRootPart = LocalHumanoid and LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 1e9)
end)

-- Auto Armor loop
RunService.PostSimulation:Connect(function()
    if not autoArmorEnabled then return end
    if not LocalCharacter or not LocalHumanoid or not LocalRootPart then return end

    local Ignored = Workspace:FindFirstChild("Ignored") or Workspace:FindFirstChild("MAP") or Workspace:FindFirstChild("Blacklisted")
    if Ignored then
        local Shop = Ignored:FindFirstChild("Shop") or Ignored:FindFirstChild("Shops") or Ignored:FindFirstChild("Pads") or Ignored:FindFirstChild("BuyPads") or Ignored:FindFirstChild("Bought")
        if Shop then
            for _, Child in ipairs(Shop:GetChildren()) do
                if Child.Name:lower():find("armor") and Child:IsA("Model") and Child:FindFirstChildOfClass("ClickDetector") then
                    local Head = Child:FindFirstChild("Head") or Child:FindFirstChild("Part")
                    if Head and Head:IsA("BasePart") and (Head.Position - LocalRootPart.Position).Magnitude <= armorDistance then
                        fireclickdetector(Child:FindFirstChildOfClass("ClickDetector"))
                        task.wait(armorSpeed)
                    end
                end
            end
        end
    end
end)

-- RageTab CFrame Speed Group
local CFrameGroup = RageTab:AddRightGroupbox('CFrame Speed')

-- Toggle
local cframeEnabled = false
local cframeButtonInjected = false
CFrameGroup:AddToggle('CFrameToggle', {
    Text = 'Enable CFrame Speed',
    Default = false,
    Tooltip = 'Move faster using CFrame manipulation',
}):OnChanged(function(value)
    cframeEnabled = value
end)

-- Speed slider
local cframeSpeed = 50
CFrameGroup:AddSlider('CFrameSpeed', {
    Text = 'Speed',
    Default = 50,
    Min = 10,
    Max = 500,
    Rounding = 1,
    Suffix = ' studs/sec',
}):OnChanged(function(value)
    cframeSpeed = value
end)

-- Create CFrame Button
CFrameGroup:AddButton('Create CFrame Button', function()
    if cframeButtonInjected then return end
    cframeButtonInjected = true

    local gui = Instance.new("ScreenGui")
    gui.Name = "CFrameButtonGUI"
    gui.Parent = game:GetService("CoreGui")

    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 140, 0, 50)
    btn.Position = UDim2.new(0.5, -70, 0.5, -25)
    btn.Text = "CFrame: OFF"
    btn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Active = true
    btn.Draggable = true
    btn.Parent = gui

    local active = false
    btn.MouseButton1Click:Connect(function()
        active = not active
        cframeEnabled = active
        btn.Text = active and "CFrame: ON" or "CFrame: OFF"
        btn.BackgroundColor3 = active and Color3.fromRGB(150, 50, 50) or Color3.fromRGB(50, 150, 50)
    end)
end)

-- Optimized CFrame Speed
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local character = LocalPlayer.Character
local root = character and character:FindFirstChild("HumanoidRootPart")
local humanoid = character and character:FindFirstChildOfClass("Humanoid")

-- Update references if character respawns
LocalPlayer.CharacterAdded:Connect(function(char)
    character = char
    root = character:WaitForChild("HumanoidRootPart")
    humanoid = character:WaitForChild("Humanoid")
end)

RunService.RenderStepped:Connect(function(delta)
    if not cframeEnabled or not root or not humanoid or humanoid.Health <= 0 then return end

    local moveDir = humanoid.MoveDirection
    if moveDir.Magnitude > 0 then
        -- Smooth movement
        local displacement = moveDir.Unit * cframeSpeed * delta
        root.CFrame = root.CFrame + displacement
    end
end)

--// Da Hood RageBot Gun Mods \\--
local FullAutoGroup = RageTab:AddRightGroupbox('Gun Mods')

-- Rapid Fire (Auto Patch)
FullAutoGroup:AddToggle('AutoPatchToggle', {
    Text = 'Rapid Fire',
    Default = false,
    Tooltip = 'Automatically patches your tool for rapid fire',
}):OnChanged(function(value)
    getgenv().autoPatchEnabled = value
    if value then
        local tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass('Tool')
        if tool then
            PatchTool(tool)
        end
    else
        -- revert patches
        for connection, patches in pairs(patchedConnections or {}) do
            for _, patch in ipairs(patches) do
                pcall(function()
                    debug.setupvalue(connection.Function, patch.i, patch.val)
                end)
            end
        end
        table.clear(patchedConnections or {})
    end
end)

-- Bullet Manipulation
FullAutoGroup:AddToggle('IgnoreObjectsToggle', {
    Text = 'Bullet Manipulation',
    Default = false,
    Tooltip = 'Ignores objects like walls for bullets',
}):OnChanged(function(state)
    if state then
        Module.Ignored = defaultIgnored
    else
        Module.Ignored = {} -- stop ignoring objects
    end
end)

-- Magic Bullet
FullAutoGroup:AddToggle('MagicBulletToggle', {
    Text = 'Magic Bullet',
    Default = false,
    Tooltip = 'Makes bullets go through certain objects',
}):OnChanged(function(state)
    if state then
        local Ignored = workspace:FindFirstChild("Ignored")
        if Ignored then
            for _, folderName in ipairs({'Vehicles','MAP'}) do
                local folder = workspace:FindFirstChild(folderName)
                if folder then
                    folder.Parent = Ignored
                end
            end
        end
    else
        -- optional: reset folders if needed
        -- workspace.Ignored.Vehicles.Parent = workspace
        -- workspace.Ignored.MAP.Parent = workspace
    end
end)

-- Spread Modification
getgenv().SpreadMod = {
    BulletSpread = {
        Enabled = false,
        Amount = 100,
    },
}

-- Hook math.random for spread
local oldRandom
oldRandom = hookfunction(math.random, function(...)
    local args = {...}
    if checkcaller() then return oldRandom(...) end

    if (#args == 0) or (args[1] == -0.05 and args[2] == 0.05) or (args[1] == -0.1) or (args[1] == -0.05) then
        if getgenv().SpreadMod.BulletSpread.Enabled then
            local spread = getgenv().SpreadMod.BulletSpread.Amount
            return oldRandom(...) * (spread / 100)
        else
            return oldRandom(...)
        end
    end

    return oldRandom(...)
end)

-- Toggle for Spread Modification
FullAutoGroup:AddToggle('SpreadToggle', {
    Text = 'Spread Modification',
    Default = false,
    Tooltip = 'Changes shotgun/pellet spread, lower = higher damage',
}):OnChanged(function(value)
    getgenv().SpreadMod.BulletSpread.Enabled = value
end)

-- Slider for Spread Amount
FullAutoGroup:AddSlider('SpreadAmount', {
    Text = 'Spread Amount',
    Default = 100,
    Min = 0,
    Max = 100,
    Compact = true,
    Rounding = 1,
    Suffix = '%',
}):OnChanged(function(value)
    getgenv().SpreadMod.BulletSpread.Amount = value
end)


--// Left Groupbox: Auto Shoot \\--
local AutoShootGroup = RageTab:AddLeftGroupbox("Auto Shoot")

-- Toggle Auto Shoot
local autoShootToggle = AutoShootGroup:AddToggle("AutoShootToggle", {
    Text = "Enable Auto Shoot",
    Default = false,
    Tooltip = "Automatically fires your gun",
}):OnChanged(function(value)
    getgenv().AutoShootEnabled = value
end)

-- Slider for delay
local autoShootDelay = AutoShootGroup:AddSlider("AutoShootDelay", {
    Text = "Delay (ms)",
    Default = 100,
    Min = 10,
    Max = 1000,
    Compact = true,
    Rounding = 0,
    Suffix = "ms",
}):OnChanged(function(value)
    getgenv().AutoShootDelay = value / 1000 -- convert ms to seconds
end)

-- Button to create draggable toggle button
AutoShootGroup:AddButton("Create Auto Shoot Button", function()
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer

    --// ScreenGui
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "AutoShootGUI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = game.CoreGui

    --// Toggle Button
    local ToggleButton = Instance.new("TextButton")
    ToggleButton.Size = UDim2.new(0, 120, 0, 50)
    ToggleButton.Position = UDim2.new(0.4, 0, 0.8, 0)
    ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    ToggleButton.Text = "Auto Shoot: OFF"
    ToggleButton.Font = Enum.Font.SourceSansBold
    ToggleButton.TextSize = 20
    ToggleButton.Active = true
    ToggleButton.Draggable = true
    ToggleButton.Parent = ScreenGui

    --// Variables
    getgenv().AutoShootEnabled = false
    getgenv().AutoShootDelay = 0.1

    --// Function to find equipped gun tool
    local function GetGun()
        local char = LocalPlayer.Character
        if not char then return end
        for _, v in pairs(char:GetChildren()) do
            if v:IsA("Tool") and (v:FindFirstChild("Ammo") or v:FindFirstChild("FireEvent") or v:FindFirstChild("RemoteEvent")) then
                return v
            end
        end
    end

    --// Shooting Loop
    RunService.Heartbeat:Connect(function()
        if getgenv().AutoShootEnabled then
            local Tool = GetGun()
            if Tool then
                pcall(function()
                    if Tool:FindFirstChild("RemoteEvent") then
                        Tool.RemoteEvent:FireServer()
                    elseif Tool:FindFirstChild("FireEvent") then
                        Tool.FireEvent:FireServer()
                    else
                        Tool:Activate()
                    end
                end)
                task.wait(getgenv().AutoShootDelay)
            end
        end
    end)

    --// Toggle Button
    ToggleButton.MouseButton1Click:Connect(function()
        getgenv().AutoShootEnabled = not getgenv().AutoShootEnabled
        if getgenv().AutoShootEnabled then
            ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 255, 50)
            ToggleButton.Text = "Auto Shoot: ON"
        else
            ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            ToggleButton.Text = "Auto Shoot: OFF"
        end
    end)
end)

--// Left Groupbox: Network Desync \\--
local NetworkDesyncGroup = RageTab:AddLeftGroupbox("Network Desync")

-- Toggle Network Desync
local networkDesyncToggle = NetworkDesyncGroup:AddToggle("NetworkDesyncToggle", {
    Text = "Enable Network Desync",
    Default = false,
    Tooltip = "Toggles network sleeping to desync your position",
}):OnChanged(function(value)
    getgenv().NetworkDesyncEnabled = value
end)

-- Slider for strength (toggle speed)
local networkDesyncStrength = NetworkDesyncGroup:AddSlider("NetworkDesyncStrength", {
    Text = "Strength",
    Default = 0.1,
    Min = 0.01,
    Max = 1,
    Compact = true,
    Rounding = 2,
    Suffix = "s",
}):OnChanged(function(value)
    getgenv().NetworkDesyncStrength = value
end)

--// Setup
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local LocalCharacter = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 60)
local LocalRootPart = LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 60)

local Sleeping = false

--// Main Functionality
RunService.PostSimulation:Connect(function()
    if not getgenv().NetworkDesyncEnabled then return end
    if LocalCharacter and LocalHumanoid and LocalRootPart then
        sethiddenproperty(LocalRootPart, "NetworkIsSleeping", Sleeping)
        Sleeping = not Sleeping
        task.wait(getgenv().NetworkDesyncStrength) -- toggle speed controlled by slider
    end
end)

--// Respawn Handler
LocalPlayer.CharacterAdded:Connect(function(Character)
    LocalCharacter = Character
    LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 60)
    LocalRootPart = LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 60)
end)

--// Character Removing Handler
LocalPlayer.CharacterRemoving:Connect(function(Character)
    LocalCharacter = nil
    LocalHumanoid = nil
    LocalRootPart = nil
end)

--// Left Groupbox: Velocity Desync \\--
local VelocityDesyncGroup = RageTab:AddLeftGroupbox("Velocity Desync")

-- Toggle Velocity Desync
local velocityDesyncToggle = VelocityDesyncGroup:AddToggle("VelocityDesyncToggle", {
    Text = "Enable Velocity Desync",
    Default = false,
    Tooltip = "Randomizes your velocity to desync hit registration",
}):OnChanged(function(value)
    getgenv().VelocityDesyncEnabled = value
end)

-- Slider for Max Velocity
local velocityDesyncSlider = VelocityDesyncGroup:AddSlider("VelocityDesyncSlider", {
    Text = "Max Velocity",
    Default = 16384,
    Min = 1000,
    Max = 50000,
    Compact = true,
    Rounding = 0,
    Suffix = " studs/s",
}):OnChanged(function(value)
    getgenv().VelocityDesyncMax = value
end)

--// Setup
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local LocalCharacter = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 60)
local LocalRootPart = LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 60)

local Velocities = {
    Velocity = Vector3.new(0,0,0),
    AssemblyLinear = Vector3.new(0,0,0),
    Rot = Vector3.new(0,0,0)
}

-- Random seed updater
task.spawn(function()
    while true do
        math.randomseed(tick())
        task.wait(1)
    end
end)

-- Main Velocity Desync Loop
RunService.Heartbeat:Connect(function()
    if not getgenv().VelocityDesyncEnabled then return end
    if LocalCharacter and LocalHumanoid and LocalRootPart then
        local MaxVelocity = getgenv().VelocityDesyncMax or 16384

        -- Save original velocities
        Velocities.Velocity = LocalRootPart.Velocity
        Velocities.Rot = LocalRootPart.RotVelocity
        Velocities.AssemblyLinear = LocalRootPart.AssemblyLinearVelocity

        -- Apply randomized velocities
        LocalRootPart.Velocity = Vector3.new(
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity)
        )
        LocalRootPart.RotVelocity = Vector3.new(
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity)
        )
        LocalRootPart.AssemblyLinearVelocity = Vector3.new(
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity),
            math.random(-MaxVelocity, MaxVelocity)
        )

        -- Restore original velocities next frame for smoothness
        RunService.PreRender:Wait()
        if LocalCharacter and LocalHumanoid and LocalRootPart then
            LocalRootPart.Velocity = Velocities.Velocity
            LocalRootPart.RotVelocity = Velocities.Rot
            LocalRootPart.AssemblyLinearVelocity = Velocities.AssemblyLinear
        end
    end
end)

-- Respawn handler
LocalPlayer.CharacterAdded:Connect(function(Character)
    LocalCharacter = Character
    LocalHumanoid = LocalCharacter:FindFirstChildOfClass("Humanoid") or LocalCharacter:WaitForChild("Humanoid", 60)
    LocalRootPart = LocalHumanoid.RootPart or LocalCharacter:WaitForChild("HumanoidRootPart", 60)
end)

-- Character removing handler
LocalPlayer.CharacterRemoving:Connect(function(Character)
    LocalCharacter = nil
    LocalHumanoid = nil
    LocalRootPart = nil
end)

--// SERVICES
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local Camera = workspace.CurrentCamera

local LocalPlayer = Players.LocalPlayer

--// DRAWING (UI BASED)
local Drawing = {}
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NebulaESP"
ScreenGui.IgnoreGuiInset = true
ScreenGui.DisplayOrder = 999999
ScreenGui.Parent = CoreGui

function Drawing.new(Type)
    if Type == "Square" then
        local frame = Instance.new("Frame")
        frame.BackgroundTransparency = 1
        frame.BorderSizePixel = 0
        frame.Visible = false
        frame.Parent = ScreenGui

        local stroke = Instance.new("UIStroke")
        stroke.Thickness = 1
        stroke.Parent = frame

        return {
            Set = function(_, props)
                if props.Visible ~= nil then frame.Visible = props.Visible end
                if props.Position then frame.Position = UDim2.fromOffset(props.Position.X, props.Position.Y) end
                if props.Size then frame.Size = UDim2.fromOffset(props.Size.X, props.Size.Y) end
                if props.Color then stroke.Color = props.Color end
            end,
            Reset = function()
                frame.Visible = false
                frame.Position = UDim2.fromOffset(0,0)
                frame.Size = UDim2.fromOffset(0,0)
            end,
            Remove = function()
                frame:Destroy()
            end
        }
    elseif Type == "Text" then
        local label = Instance.new("TextLabel")
        label.BackgroundTransparency = 1
        label.Font = Enum.Font.Code
        label.TextSize = 13
        label.TextStrokeTransparency = 0
        label.Visible = false
        label.Parent = ScreenGui

        return {
            Set = function(_, props)
                if props.Visible ~= nil then label.Visible = props.Visible end
                if props.Position then label.Position = UDim2.fromOffset(props.Position.X, props.Position.Y) end
                if props.Text then label.Text = props.Text end
                if props.Color then label.TextColor3 = props.Color end
                if props.Size then label.TextSize = props.Size end
            end,
            Reset = function()
                label.Visible = false
                label.Position = UDim2.fromOffset(0,0)
                label.Text = ""
            end,
            Remove = function()
                label:Destroy()
            end
        }
    end
end

--// CONFIG
getgenv().Config = {
    Box = {
        Enable = true,
        Color = Color3.fromRGB(255,255,255),

        -- SLIDERS
        WidthScale = 0.6,
        HeightScale = 1.0
    },
    Text = {
        Name = {Enable = true, Color = Color3.fromRGB(255,255,255), Teamcheck = false},
        Studs = {Enable = true, Color = Color3.fromRGB(255,255,255)},
        Tool = {Enable = true, Color = Color3.fromRGB(255,255,255)},
        Health = {Enable = true, Color = Color3.fromRGB(255,255,255)}
    },
    Bars = {
        Health = {Enable = true, Color = Color3.fromRGB(117,182,105)}
    }
}

--// CACHE
local Cache = {}

local function CreateESP(player)
    if player == LocalPlayer then return end

    Cache[player] = {
        Box = {
            Main = Drawing.new("Square"),
            Outline = Drawing.new("Square"),
            Inline = Drawing.new("Square")
        },
        Text = {
            Name = Drawing.new("Text"),
            Studs = Drawing.new("Text"),
            Tool = Drawing.new("Text"),
            Health = Drawing.new("Text")
        },
        HealthBar = {
            Fill = Drawing.new("Square"),
            Outline = Drawing.new("Square")
        }
    }
end

local function RemoveESP(player)
    if Cache[player] then
        for _,group in pairs(Cache[player]) do
            for _,obj in pairs(group) do
                obj:Remove()
            end
        end
        Cache[player] = nil
    end
end

--// RENDER LOOP (HARD FIXED)
RunService.RenderStepped:Connect(function()
    for player,esp in pairs(Cache) do
        local char = player.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local root = char and char:FindFirstChild("HumanoidRootPart")
        local head = char and char:FindFirstChild("Head")

        if not char or not hum or not root or hum.Health <= 0 then
            for _,g in pairs(esp) do for _,o in pairs(g) do o:Reset() end end
            continue
        end

        if Config.Text.Name.Teamcheck and player.Team == LocalPlayer.Team then
            for _,g in pairs(esp) do for _,o in pairs(g) do o:Reset() end end
            continue
        end

        local top3D, topOn = Camera:WorldToViewportPoint(head.Position + Vector3.new(0,0.5,0))
        local bot3D, botOn = Camera:WorldToViewportPoint(root.Position - Vector3.new(0,3,0))

        if not (topOn or botOn) or top3D.Z <= 0 or bot3D.Z <= 0 then
            for _,g in pairs(esp) do for _,o in pairs(g) do o:Reset() end end
            continue
        end

        local height = math.abs(top3D.Y - bot3D.Y)
        if height < 8 then continue end

        -- BOX SIZE (SLIDER CONTROLLED)
        local scaledHeight = height * Config.Box.HeightScale
        local scaledWidth  = height * Config.Box.WidthScale

        local boxPos = Vector2.new(top3D.X - scaledWidth/2, top3D.Y)
        local boxSize = Vector2.new(scaledWidth, scaledHeight)

        -- BOX
        if Config.Box.Enable then
            esp.Box.Main:Set({Visible=true, Position=boxPos, Size=boxSize, Color=Config.Box.Color})
            esp.Box.Outline:Set({Visible=true, Position=boxPos-Vector2.new(1,1), Size=boxSize+Vector2.new(2,2), Color=Color3.new(0,0,0)})
            esp.Box.Inline:Set({Visible=true, Position=boxPos+Vector2.new(1,1), Size=boxSize-Vector2.new(2,2), Color=Color3.new(0,0,0)})
        else
            esp.Box.Main:Reset(); esp.Box.Outline:Reset(); esp.Box.Inline:Reset()
        end

        -- NAME
        if Config.Text.Name.Enable then
            esp.Text.Name:Set({
                Visible=true,
                Text=player.DisplayName,
                Position=Vector2.new(top3D.X, boxPos.Y - 18),
                Color=Config.Text.Name.Color
            })
        else esp.Text.Name:Reset() end

        -- DISTANCE
        if Config.Text.Studs.Enable and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local dist = math.floor((root.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)
            esp.Text.Studs:Set({
                Visible=true,
                Text=dist.." studs",
                Position=Vector2.new(top3D.X, bot3D.Y + 4),
                Color=Config.Text.Studs.Color
            })
        else esp.Text.Studs:Reset() end

        -- TOOL
        if Config.Text.Tool.Enable then
            local tool = char:FindFirstChildOfClass("Tool")
            esp.Text.Tool:Set({
                Visible=true,
                Text=tool and tool.Name or "None",
                Position=Vector2.new(top3D.X, bot3D.Y + 18),
                Color=Config.Text.Tool.Color
            })
        else esp.Text.Tool:Reset() end

        -- HEALTH TEXT
        if Config.Text.Health.Enable then
            esp.Text.Health:Set({
                Visible=true,
                Text=math.floor(hum.Health).. "%",
                Position=Vector2.new(boxPos.X - 30, boxPos.Y + (1 - hum.Health/hum.MaxHealth) * scaledHeight),
                Color=Config.Text.Health.Color
            })
        else esp.Text.Health:Reset() end

        -- HEALTH BAR (NO CORNER BUG)
        if Config.Bars.Health.Enable then
            local pct = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
            local bx = boxPos.X - 8

            esp.HealthBar.Outline:Set({
                Visible=true,
                Position=Vector2.new(bx - 1, boxPos.Y - 1),
                Size=Vector2.new(6, scaledHeight + 2),
                Color=Color3.new(0,0,0)
            })

            esp.HealthBar.Fill:Set({
                Visible=true,
                Position=Vector2.new(bx, boxPos.Y + (1 - pct) * scaledHeight),
                Size=Vector2.new(4, pct * scaledHeight),
                Color=Config.Bars.Health.Color
            })
        else
            esp.HealthBar.Fill:Reset()
            esp.HealthBar.Outline:Reset()
        end
    end
end)

--// PLAYER HANDLING
for _,p in ipairs(Players:GetPlayers()) do CreateESP(p) end
Players.PlayerAdded:Connect(CreateESP)
Players.PlayerRemoving:Connect(RemoveESP)

--// VISUALS TAB
local ESPGroup = VisualTab:AddLeftGroupbox("Player ESP")

ESPGroup:AddToggle("BoxESP",{Text="Box",Default=true,Callback=function(v) Config.Box.Enable=v end})
ESPGroup:AddToggle("NameESP",{Text="Name",Default=true,Callback=function(v) Config.Text.Name.Enable=v end})
ESPGroup:AddToggle("DistanceESP",{Text="Distance",Default=true,Callback=function(v) Config.Text.Studs.Enable=v end})
ESPGroup:AddToggle("ToolESP",{Text="Tool",Default=true,Callback=function(v) Config.Text.Tool.Enable=v end})
ESPGroup:AddToggle("HealthTextESP",{Text="Health Text",Default=true,Callback=function(v) Config.Text.Health.Enable=v end})
ESPGroup:AddToggle("HealthBarESP",{Text="Health Bar",Default=true,Callback=function(v) Config.Bars.Health.Enable=v end})
ESPGroup:AddToggle("TeamCheckESP",{Text="Team Check",Default=false,Callback=function(v) Config.Text.Name.Teamcheck=v end})

-- SLIDERS
ESPGroup:AddSlider("BoxWidth",{
    Text="Box Width",
    Min=0.3,
    Max=1.2,
    Default=0.6,
    Rounding=2,
    Callback=function(v) Config.Box.WidthScale=v end
})

ESPGroup:AddSlider("BoxHeight",{
    Text="Box Height",
    Min=0.6,
    Max=1.5,
    Default=1.0,
    Rounding=2,
    Callback=function(v) Config.Box.HeightScale=v end
})

--// =========================
--// PURPLE CHAMS (HIGHLIGHT)
--// UI SAFE – PUT TOGETHER
--// =========================

-- LOCAL CONFIG (IMPORTANT: NOT getgenv)
local ChamsConfig = {
    Enabled = false,
    FillTransparency = 0.5,
    OutlineTransparency = 0
}

-- SERVICES
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer
local Highlights = {}

local PURPLE = Color3.fromRGB(170, 0, 255)

-- CREATE HIGHLIGHT
local function CreateHighlight(player)
    if player == LocalPlayer then return end

    local hl = Instance.new("Highlight")
    hl.Name = "NebulaChams"
    hl.FillColor = PURPLE
    hl.OutlineColor = PURPLE
    hl.Enabled = false
    hl.Parent = CoreGui

    Highlights[player] = hl
end

-- REMOVE HIGHLIGHT
local function RemoveHighlight(player)
    if Highlights[player] then
        Highlights[player]:Destroy()
        Highlights[player] = nil
    end
end

-- INIT
for _, player in ipairs(Players:GetPlayers()) do
    CreateHighlight(player)
end

Players.PlayerAdded:Connect(CreateHighlight)
Players.PlayerRemoving:Connect(RemoveHighlight)

-- UPDATE LOOP
RunService.RenderStepped:Connect(function()
    for player, hl in pairs(Highlights) do
        local char = player.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")

        if not ChamsConfig.Enabled
        or not char
        or not hum
        or hum.Health <= 0 then
            hl.Enabled = false
            continue
        end

        hl.Adornee = char
        hl.Enabled = true
        hl.FillTransparency = ChamsConfig.FillTransparency
        hl.OutlineTransparency = ChamsConfig.OutlineTransparency
    end
end)

--// =========================
--// VISUALS → PLAYER ESP UI
--// =========================

ESPGroup:AddToggle("Chams_Enable_Toggle", {
    Text = "Chams (Highlight)",
    Default = false
}):OnChanged(function(v)
    ChamsConfig.Enabled = v
end)

ESPGroup:AddSlider("Chams_Fill_Transparency", {
    Text = "Chams Transparency",
    Min = 0,
    Max = 1,
    Default = 0.5,
    Rounding = 2
}):OnChanged(function(v)
    ChamsConfig.FillTransparency = v
end)

ESPGroup:AddSlider("Chams_Outline_Transparency", {
    Text = "Chams Outline Transparency",
    Min = 0,
    Max = 1,
    Default = 0,
    Rounding = 2
}):OnChanged(function(v)
    ChamsConfig.OutlineTransparency = v
end)

-- === RAINBOW TRAIL (SELF) - MINIMIZED LOCALS ===
local TrailConfig = {
    Enabled = true,
    Thickness = 0.2,
    FadeTime = 3,
    HueSpeed = 0.01
}

local trailRunning = false
local trailConn = nil

local function createTrailPart(startPos, endPos, color)
    local dist = (startPos - endPos).Magnitude
    if dist < 0.1 then return end

    local part = Instance.new("Part")
    part.Anchored = true
    part.CanCollide = false
    part.Material = Enum.Material.Neon
    part.Color = color
    part.Transparency = 0
    part.Size = Vector3.new(TrailConfig.Thickness, TrailConfig.Thickness, dist)
    part.CFrame = CFrame.lookAt(startPos, endPos) * CFrame.new(0, 0, -dist / 2)
    part.Parent = Workspace

    game:GetService("TweenService"):Create(part, TweenInfo.new(TrailConfig.FadeTime), {Transparency = 1}):Play()
    game.Debris:AddItem(part, TrailConfig.FadeTime + 0.1)
end

local TrailGroup = VisualTab:AddLeftGroupbox('Rainbow Trail')

TrailGroup:AddToggle('TrailEnabled', {
    Text = 'Enable Rainbow Trail',
    Default = true,
    Callback = function(v)
        TrailConfig.Enabled = v
        if v and not trailRunning then
            trailRunning = true
            local root = Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if not root then return end

            local lastPos = root.Position
            local hue = 0

            trailConn = RunService.Heartbeat:Connect(function()
                if not TrailConfig.Enabled or not trailRunning then return end
                local char = Players.LocalPlayer.Character
                if not char or not char:FindFirstChild("HumanoidRootPart") then return end

                local currentPos = char.HumanoidRootPart.Position
                local color = Color3.fromHSV(hue, 1, 1)
                createTrailPart(lastPos, currentPos, color)

                lastPos = currentPos
                hue = (hue + TrailConfig.HueSpeed) % 1
            end)
        elseif not v and trailConn then
            trailConn:Disconnect()
            trailConn = nil
            trailRunning = false
        end
    end
})

TrailGroup:AddSlider('TrailThickness', {
    Text = 'Thickness',
    Min = 0.1,
    Max = 1,
    Default = 0.2,
    Rounding = 2,
    Callback = function(v) TrailConfig.Thickness = v end
})

TrailGroup:AddSlider('TrailFade', {
    Text = 'Fade Time',
    Min = 0.5,
    Max = 10,
    Default = 3,
    Rounding = 1,
    Callback = function(v) TrailConfig.FadeTime = v end
})

TrailGroup:AddSlider('TrailSpeed', {
    Text = 'Rainbow Speed',
    Min = 0.001,
    Max = 0.05,
    Default = 0.01,
    Rounding = 3,
    Callback = function(v) TrailConfig.HueSpeed = v end
})

-- Auto-start if enabled
if TrailConfig.Enabled then
    TrailGroup.Options.TrailEnabled:SetValue(true)
end