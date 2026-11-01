local players = game:GetService('Players')
local uis = game:GetService('UserInputService')
local RunService: RunService = cloneref(game:GetService('RunService'))
local tweenservice = game:GetService('TweenService')
local marketplaceservice = game:GetService('MarketplaceService')
local textservice = game:GetService('TextService')
local coregui = game:GetService('CoreGui')
local httpservice = game:GetService('HttpService')

local player = players.LocalPlayer
local mouse = player:GetMouse()
local camera = game.Workspace.CurrentCamera
local AZURE_TmKALRSX = player
local Position
local Rawtable = getrawmetatable(game)
setreadonly(Rawtable, false)
local Indexx = Rawtable.__index

local Resolvers = {
    Options = {
        Velocity = false,
        HumanoidRedirection = false,
        VelocityRecalculation = false, -- done
    },
}

local target_aim = {
    Aiming = {
        Target = {
            Enabled = true,
            Key = Enum.KeyCode.Q,
            Prediction = 0.1433,
            AutoPred = false,
            Radius = 35,
            TargetPart = 'HumanoidRootPart',
            JumpOffset = nil,
            LookAt = nil,
            TargetStats = nil,
            AntiGroundShot = nil,
            ForceFieldCheck = nil,
            Tracer = nil,
            HitSkeleton = nil,
            HitPart = nil,
        },
    },
}

local TargetAimbot = {
    Enabled = true,
    Keybind = Enum.KeyCode.Q,
    Autoselect = false,
    Prediction = 0.145,
    RealPrediction = 0.145,
    Resolver = false,
    ResolverType = 'Recalculate',
    JumpOffset = 0.06,
    RealJumpOffset = 0.09,
    HitParts = { 'HumanoidRootPart' },
    RealHitPart = 'HumanoidRootPart',
    KoCheck = false,
    LookAt = false,
    CSync = {
        Enabled = false,
        Type = 'Orbit',
        Distance = 10,
        Height = 2,
        Speed = 10,
        RandomAmount = 10,
        Color = Color3.fromRGB(255, 255, 255),
        Saved = nil,
        Visualize = false,
    },
    ViewAt = false,
    Tracer = false,
    Highlight = true,
    HighlightColor1 = Color3.fromRGB(255, 255, 255),
    HighlightColor2 = Color3.fromRGB(255, 255, 255),
    Stats = false,
    UseFov = false,
    HitEffect = false,
    HitEffectType = 'Coom', --  {{ Nova, Crescent Slash, Coom, Cosmic Explosion, Slash, Atomic Slash, Aura Burst }}
    HitEffectColor = Color3.fromRGB(255, 255, 255),
    HitSounds = false,
    HitSound = 'Bameware',
    HitChams = false,
    HitChamsMaterial = Enum.Material.Neon,
    HitChamsDuration = 2,
    HitChamsColor = Color3.fromRGB(255, 0, 0),
    HitChamColorEnabled = false,
    HitChamsTransparency = 0,
    HitChamsAcc = false,
    SkeleColor = Color3.fromRGB(155, 0, 155),
}

-- drawing library and esp handlers
local YunDrawingApi = loadstring(
    game:HttpGet(
        'https://raw.githubusercontent.com/caIIed/Librarys/main/Yun%20Api.lua',
        true
    )
)()
loadstring(
    game:HttpGet(
        'https://raw.githubusercontent.com/Ziheim51000/test/refs/heads/main/DrawingLibrary',
        true
    )
)()
loadstring(
    game:HttpGet(
        'https://raw.githubusercontent.com/Ziheim51000/test/refs/heads/main/ESP%20FINAL',
        true
    )
)()

local Rain = loadstring(
    game:HttpGet(
        'https://raw.githubusercontent.com/laagginq/obese.vip/refs/heads/main/rainmodule.lua'
    )
)()

local repo = 'https://raw.githubusercontent.com/deividcomsono/Obsidian/main/'
local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager =
    loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

local Options = Library.Options
local Toggles = Library.Toggles

Library.ForceCheckbox = false -- Forces AddToggle to AddCheckbox
Library.ShowToggleFrameInKeybinds = true -- Make toggle keybinds work inside the keybinds UI (aka adds a toggle to the UI). Good for mobile users (Default value = true)

local Window = Library:CreateWindow({
    Title = 'nebula v2',
    Footer = 'version: 0.01',
    Icon = 72196083151319,
    NotifySide = 'Right',
    ShowCustomCursor = false,
})

local Tabs = {
    Rage = Window:AddTab('Aiming', 'zap'),
    AntiAim = Window:AddTab('Anti-Aim', 'rotate-ccw'),
    Visuals = Window:AddTab('Visuals', 'eye'),
    World = Window:AddTab('World', 'earth'),
    Misc = Window:AddTab('Misc', 'settings'),
    ['UI Settings'] = Window:AddTab('Configuration', 'save'),
}

local Tab1 = Tabs.Rage:AddLeftGroupbox('Targetting', 'zap')

local Visualization = Tabs.Rage:AddRightGroupbox('Indicators', 'eye')

Tab1:AddToggle('MyToggle', {
    Text = 'enabled',
    Default = true,
    Callback = function(Value)
        target_aim.Aiming.Target.Enabled = Value
    end,
}):AddKeyPicker('KeyPicker', {
    Default = 'Q',
    SyncToggleState = true,
    Mode = 'Toggle',
    Text = 'Targetting',
    NoUI = false,
    Callback = function(Value)
        TargetAimEnabled = Value
        if TargetAimEnabled then
            AZURE_TmKALRSX = targetchosen()
            if AZURE_TmKALRSX ~= nil then
                local HEALTH = AZURE_TmKALRSX.Character.Humanoid.Health
                conn1 = AZURE_TmKALRSX.Character.Humanoid.HealthChanged:Connect(
                    function(NEWhealth)
                        local changedHealth = NEWhealth - HEALTH
                        local fixedHealth = string.gsub(changedHealth, '-', '')
                        if NEWhealth < HEALTH then
                            local STRING =
                                '%s was hit at distance %s with %s for %s in %s'
                            local DISTANCE = (
                                AZURE_TmKALRSX.Character.HumanoidRootPart.Position
                                - player.Character.HumanoidRootPart.Position
                            ).Magnitude
                            for i, v in pairs(player.Character:GetChildren()) do
                                if v:IsA('Tool') then
                                    TOOLNAME = v.Name
                                end
                            end
                            Library:Notify(
                                string.format(
                                    STRING,
                                    AZURE_TmKALRSX.Character.Humanoid.DisplayName,
                                    math.round(DISTANCE / 1),
                                    TOOLNAME,
                                    math.round(fixedHealth / 1),
                                    target_aim.Aiming.Target.TargetPart
                                )
                            )
                        end
                        HEALTH = NEWhealth
                    end
                )
            end
        else
            AZURE_TmKALRSX = nil
            if conn1 then
                conn1:Disconnect()
            end
        end
    end,
})

if
    target_aim.Aiming.Target.Enabled
    and AZURE_TmKALRSX
    and AZURE_TmKALRSX.Character
    and target_aim.Aiming.Target.ForceFieldCheck
    and AZURE_TmKALRSX.Character:FindFirstChildOfClass('ForceField')
then
    AZURE_TmKALRSX = nil
end

Tab1:AddToggle('TargetVisibilityToggle', {
    Text = 'visible check',
    Default = false,
    Callback = function(value) end,
})

Tab1:AddToggle('TargetVisibilityToggle', {
    Text = 'forcefield check',
    Default = false,
    Callback = function(value)
        target_aim.Aiming.Target.ForceFieldCheck = value
    end,
})

Tab1:AddToggle('TargetVisibilityToggle', {
    Text = 'team check',
    Default = false,
    Callback = function(value)
        target_aim.Aiming.Target.ForceFieldCheck = value
    end,
})

local MDResolver = false -- if u remove this its fucked
Tab1:AddToggle('MyToggle', {
    Text = 'resolver',
    Default = false,
    Callback = function(Value)
        MDResolver = Value
    end,
})

-- logic here

Tab1:AddToggle('AutoClickToggle', {
    Text = 'anti-aim-viewer (beta)',
    Default = false,
    Callback = function(value)
        manipulationEnabled = value
    end,
})

Tab1:AddToggle('MyToggle', {
    Text = 'ping based prediction',
    Default = false,
    Callback = function(Value)
        target_aim.Aiming.Target.AutoPred = Value
    end,
})

Tab1:AddToggle('MyToggle', {
    Text = 'no ground shots',
    Default = false,
    Callback = function(Value)
        target_aim.Aiming.Target.AntiGroundShot = Value
    end,
})

Tab1:AddDropdown('MyDropdown', {
    Values = { 'HumanoidRootPart', 'UpperTorso', 'Head', 'LowerTorso' },
    Default = 1,
    Multi = false,
    Text = 'target part', -- Information shown when you hover over the dropdown
    Callback = function(Value)
        target_aim.Aiming.Target.TargetPart = Value
    end,
})

Tab1:AddInput('MyTextbox', {
    Default = '0.1322',
    Numeric = true,
    Finished = false,
    Text = 'horizontal prediction',
    Placeholder = '0.1322',
    Callback = function(Value)
        target_aim.Aiming.Target.Prediction = Value
    end,
})

Tab1:AddInput('MyTextbox', {
    Default = '0.14',
    Numeric = true,
    Finished = false,
    Text = 'vertical prediction',
    Placeholder = '0.14',
    Callback = function(Value) end,
})

Tab1:AddInput('MyTextbox', {
    Default = '0.04',
    Numeric = true,
    Finished = false,
    Text = 'offset',
    Placeholder = '0.04',
    Callback = function(Value)
        target_aim.Aiming.Target.JumpOffset = Value
    end,
})

local Visualization2 = Tabs.Rage:AddLeftGroupbox('Target', 'crosshair')

local stompTargetEnabled = false -- random var im not moving this tho
local lastPosition = nil
local ReplicatedStorage = game:GetService('ReplicatedStorage')
local Players = game:GetService('Players')

task.spawn(function()
    while true do
        if
            stompTargetEnabled
            and AZURE_TmKALRSX
            and AZURE_TmKALRSX ~= LocalPlayer
        then
            local character = AZURE_TmKALRSX.Character
            if character then
                local bodyEffects = character:FindFirstChild('BodyEffects')
                local isKO = bodyEffects
                    and bodyEffects:FindFirstChild('K.O')
                    and bodyEffects['K.O'].Value
                local isSDeath = bodyEffects
                    and bodyEffects:FindFirstChild('SDeath')
                    and bodyEffects['SDeath'].Value

                if isKO and not isSDeath then
                    local upperTorso = character:FindFirstChild('UpperTorso')
                    if upperTorso then
                        local humanoidRootPart =
                            LocalPlayer.Character:WaitForChild(
                                'HumanoidRootPart'
                            )
                        if not lastPosition then
                            lastPosition = humanoidRootPart.Position
                        end
                        humanoidRootPart.CFrame = CFrame.new(
                            upperTorso.Position + Vector3.new(0, 3, 0)
                        )
                        RunService.RenderStepped:Wait()
                    end
                elseif isSDeath and lastPosition then
                    if killSayEnabled then
                        local message =
                            killSayMessages[math.random(1, #killSayMessages)]
                        game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(
                            message,
                            'All'
                        )
                    end
                    local humanoidRootPart =
                        LocalPlayer.Character:WaitForChild('HumanoidRootPart')
                    while
                        (humanoidRootPart.Position - lastPosition).Magnitude
                        > 5
                    do
                        humanoidRootPart.CFrame = CFrame.new(lastPosition)
                        task.wait()
                    end
                    lastPosition = nil
                end
            else
                if lastPosition then
                    local humanoidRootPart =
                        LocalPlayer.Character:WaitForChild('HumanoidRootPart')
                    while
                        (humanoidRootPart.Position - lastPosition).Magnitude
                        > 5
                    do
                        humanoidRootPart.CFrame = CFrame.new(lastPosition)
                        task.wait()
                    end
                    lastPosition = nil
                end
            end
            ReplicatedStorage.MainEvent:FireServer('Stomp')
        end
        task.wait(0)
    end
end)

Visualization2:AddToggle('MyToggle', {
    Text = 'face target',
    Default = false,
    Callback = function(Value)
        target_aim.Aiming.Target.LookAt = Value
    end,
})

Visualization2:AddToggle('MyToggle', {
    Text = 'spectate target',
    Default = false,
    Callback = function(Value)
        viewatxd = Value
    end,
})

Visualization2:AddToggle('StompTarget', {
    Text = 'stomp target',
    Default = false,
    Callback = function(Value)
        stompTargetEnabled = Value
    end,
})

getgenv().keytoclick = 'Q'

local Players = game:GetService('Players')
local LocalPlayer = Players.LocalPlayer
local vim = game:GetService('VirtualInputManager')

local function giveTool()
    local tool = Instance.new('Tool')
    tool.RequiresHandle = false
    tool.Name = 'Q'
    tool.Activated:Connect(function()
        vim:SendKeyEvent(true, keytoclick, false, game)
    end)
    tool.Parent = LocalPlayer.Backpack
end

giveTool()

LocalPlayer.CharacterAdded:Connect(function()
    repeat
        task.wait()
    until LocalPlayer:FindFirstChild('Backpack')
    task.wait(0.2)
    giveTool()
end)

local Stats = game:GetService('Stats')

RunService.RenderStepped:Connect(function()
    if
        target_aim.Aiming.Target.Enabled and target_aim.Aiming.Target.AutoPred
    then
        local Ping =
            math.round(Stats.Network.ServerStatsItem['Data Ping']:GetValue())

        if Ping < 10 then
            target_aim.Aiming.Target.Prediction = 0.1099
        elseif Ping < 20 then
            target_aim.Aiming.Target.Prediction = 0.10294
        elseif Ping < 30 then
            target_aim.Aiming.Target.Prediction = 0.12
        elseif Ping < 40 then
            target_aim.Aiming.Target.Prediction = 0.125
        elseif Ping < 50 then
            target_aim.Aiming.Target.Prediction = 0.1322
        elseif Ping < 60 then
            target_aim.Aiming.Target.Prediction = 0.1325
        elseif Ping < 70 then
            target_aim.Aiming.Target.Prediction = 0.141123
        elseif Ping < 80 then
            target_aim.Aiming.Target.Prediction = 0.141
        elseif Ping < 90 then
            target_aim.Aiming.Target.Prediction = 0.142
        elseif Ping < 100 then
            target_aim.Aiming.Target.Prediction = 0.1378
        elseif Ping < 110 then
            target_aim.Aiming.Target.Prediction = 0.1459
        elseif Ping < 120 then
            target_aim.Aiming.Target.Prediction = 0.14683943
        elseif Ping < 130 then
            target_aim.Aiming.Target.Prediction = 0.15175864
        elseif Ping < 140 then
            target_aim.Aiming.Target.Prediction = 0.15873582
        elseif Ping < 150 then
            target_aim.Aiming.Target.Prediction = 0.15873582 -- not updating past 150 fuck you
        elseif Ping < 160 then
            target_aim.Aiming.Target.Prediction = 0.161
        elseif Ping < 170 then
            target_aim.Aiming.Target.Prediction = 0.162
        elseif Ping < 180 then
            target_aim.Aiming.Target.Prediction = 0.165
        elseif Ping < 190 then
            target_aim.Aiming.Target.Prediction = 0.172
        elseif Ping < 200 then
            target_aim.Aiming.Target.Prediction = 0.173
        elseif Ping < 210 then
            target_aim.Aiming.Target.Prediction = 0.175
        elseif Ping < 220 then
            target_aim.Aiming.Target.Prediction = 0.175232432
        elseif Ping < 230 then
            target_aim.Aiming.Target.Prediction = 0.1876
        elseif Ping < 240 then
            target_aim.Aiming.Target.Prediction = 0.23
        elseif Ping < 250 then
            target_aim.Aiming.Target.Prediction = 0.22
        elseif Ping < 260 then
            target_aim.Aiming.Target.Prediction = 0.24
        elseif Ping < 270 then
            target_aim.Aiming.Target.Prediction = 0.21
        elseif Ping < 280 then
            target_aim.Aiming.Target.Prediction = 0.2
        elseif Ping < 290 then
            target_aim.Aiming.Target.Prediction = 0.2
        elseif Ping < 300 then
            target_aim.Aiming.Target.Prediction = 0.26
        else
            target_aim.Aiming.Target.Prediction = 0.13
        end
    end
end)

spawn(function()
    RunService.Stepped:Connect(function()
        if
            target_aim.Aiming.Target.Enabled
            and viewatxd
            and TargetAimEnabled
        then
            workspace.CurrentCamera.CameraSubject =
                AZURE_TmKALRSX.Character.Humanoid
            spawn(function()
                if viewatxd == false then
                    workspace.CurrentCamera.CameraSubject =
                        player.Character.Humanoid
                end
            end)
        else
            workspace.CurrentCamera.CameraSubject = player.Character.Humanoid
        end
    end)
end)

local highlight_instance = Instance.new('Highlight', game.CoreGui)

spawn(function()
    RunService.Stepped:Connect(function()
        if
            target_aim.Aiming.Target.Enabled
            and hightlightendalbed
            and TargetAimEnabled
        then
            highlight_instance.Parent = AZURE_TmKALRSX.Character
            highlight_instance.FillColor = fillcolorxd
            highlight_instance.OutlineColor = Color3.new(0, 0, 0)
            spawn(function()
                if hightlightendalbed == false then
                    highlight_instance.Parent = game.CoreGui
                end
            end)
        else
            highlight_instance.Parent = game.CoreGui
        end
    end)
end)

-- Tracer lines
local TargTracerOutlineLeft = Drawing.new('Line')
local TargTracerOutlineRight = Drawing.new('Line')
local TargTracerLine = Drawing.new('Line')

-- Main tracer (center color)
TargTracerLine.Thickness = 1
TargTracerLine.ZIndex = 0.5
TargTracerLine.Visible = false

-- Outline (left)
TargTracerOutlineLeft.Thickness = 1
TargTracerOutlineLeft.Color = Color3.new(0, 0, 0)
TargTracerOutlineLeft.ZIndex = 1
TargTracerOutlineLeft.Visible = false

-- Outline (right)
TargTracerOutlineRight.Thickness = 1
TargTracerOutlineRight.Color = Color3.new(0, 0, 0)
TargTracerOutlineRight.ZIndex = 1
TargTracerOutlineRight.Visible = false

spawn(function()
    RunService.Stepped:Connect(function()
        if
            tracerenabledlolol
            and target_aim.Aiming.Target.Enabled
            and AZURE_TmKALRSX
            and AZURE_TmKALRSX.Character
        then
            local char = AZURE_TmKALRSX.Character
            local targetPart =
                char:FindFirstChild(target_aim.Aiming.Target.TargetPart)

            if targetPart then
                local predictedPos = targetPart.Position
                    + Vector3.new(0, target_aim.Aiming.Target.JumpOffset, 0)
                    + (
                        targetPart.Velocity
                        * target_aim.Aiming.Target.Prediction
                    )

                local screenPos, onScreen =
                    camera:WorldToViewportPoint(predictedPos)

                if onScreen then
                    local mousePos = Vector2.new(
                        mouse.X,
                        mouse.Y + game:GetService('GuiService'):GetGuiInset().Y
                    )
                    local toPos = Vector2.new(screenPos.X, screenPos.Y)
                    local dir = (toPos - mousePos).Unit
                    local perp = Vector2.new(-dir.Y, dir.X) * 1.5 -- Perpendicular offset

                    -- Left outline (offset left)
                    TargTracerOutlineLeft.Visible = true
                    TargTracerOutlineLeft.From = mousePos - perp
                    TargTracerOutlineLeft.To = toPos - perp

                    -- Right outline (offset right)
                    TargTracerOutlineRight.Visible = true
                    TargTracerOutlineRight.From = mousePos + perp
                    TargTracerOutlineRight.To = toPos + perp

                    -- Center colored tracer
                    TargTracerLine.Visible = true
                    TargTracerLine.From = mousePos
                    TargTracerLine.To = toPos
                    TargTracerLine.Color = tracercolorlolol
                else
                    TargTracerLine.Visible = false
                    TargTracerOutlineLeft.Visible = false
                    TargTracerOutlineRight.Visible = false
                end
            else
                TargTracerLine.Visible = false
                TargTracerOutlineLeft.Visible = false
                TargTracerOutlineRight.Visible = false
            end
        else
            TargTracerLine.Visible = false
            TargTracerOutlineLeft.Visible = false
            TargTracerOutlineRight.Visible = false
        end
    end)
end)

local target = AZURE_TmKALRSX

if not getgenv().lastHealth then
    getgenv().lastHealth = {}
end

RunService.Heartbeat:Connect(function()
    if not getgenv().hitsoundEnabled then
        return
    end

    -- Check for valid target through your target_aim system
    if
        target_aim
        and target_aim.Aiming.Target.Enabled
        and AZURE_TmKALRSX
        and AZURE_TmKALRSX.Character
    then
        local target = AZURE_TmKALRSX
        local humanoid = target.Character:FindFirstChild('Humanoid')

        if humanoid then
            local name = target.Name

            if not getgenv().lastHealth[name] then
                getgenv().lastHealth[name] = humanoid.Health
            end

            if humanoid.Health < getgenv().lastHealth[name] then
                playHitsound()
            end

            getgenv().lastHealth[name] = humanoid.Health
        end
    end
end)

spawn(function()
    RunService.RenderStepped:Connect(function()
        if
            TargetAimEnabled
            and target_aim.Aiming.Target.Enabled
            and AZURE_TmKALRSX.Character:FindFirstChild('UpperTorso')
        then
            if target_aim.Aiming.Target.LookAt then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(
                    player.Character.HumanoidRootPart.CFrame.Position,
                    Vector3.new(
                        AZURE_TmKALRSX.Character.HumanoidRootPart.CFrame.X,
                        player.Character.HumanoidRootPart.CFrame.Position.Y,
                        AZURE_TmKALRSX.Character.HumanoidRootPart.CFrame.Z
                    )
                )
                player.Character.Humanoid.AutoRotate = false
                spawn(function()
                    if target_aim.Aiming.Target.LookAt == false then
                        player.Character.Humanoid.AutoRotate = true
                    end
                end)
            end
        else
            spawn(function()
                player.Character.Humanoid.AutoRotate = true
            end)
        end
    end)
end)

local JumpOffsetValue
spawn(function()
    if target_aim.Aiming.Target.AntiGroundShot == true and AZURE_TmKALRSX then
        if
            AZURE_TmKALRSX.Character.Humanoid.Jump == true
            and AZURE_TmKALRSX.Character.Humanoid.FloorMaterial
                == Enum.Material.Air
        then
            JumpOffsetValue = target_aim.Aiming.Target.JumpOffset
        else
            JumpOffsetValue = -0
        end
    end
end)

-- || Mouse Hit Index || --
Rawtable.__index = function(self, Index)
    if
        not checkcaller()
        and self == mouse
        and target_aim.Aiming.Target.TargetPart
    then
        if AZURE_TmKALRSX and AZURE_TmKALRSX.Character then
            if MDResolver then
                Position = AZURE_TmKALRSX.Character[target_aim.Aiming.Target.TargetPart].Position
                    + (
                        Vector3.new(
                            AZURE_TmKALRSX.Character.Humanoid.MoveDirection.X,
                            JumpOffsetValue,
                            AZURE_TmKALRSX.Character.Humanoid.MoveDirection.Z
                        )
                        * target_aim.Aiming.Target.Prediction
                        * 18
                    )
            else
                Position = AZURE_TmKALRSX.Character[target_aim.Aiming.Target.TargetPart].Position
                    + Vector3.new(0.01, JumpOffsetValue, 0.01)
                    + (
                        AZURE_TmKALRSX.Character[target_aim.Aiming.Target.TargetPart].Velocity
                        * target_aim.Aiming.Target.Prediction
                    )
            end
            if Index == 'Hit' then
                return CFrame.new(Position)
            end
        end
    end
    return Indexx(self, Index)
end

function targetchosen()
    local distance = math.huge
    local zclosest
    for i, v in pairs(game.Players:GetPlayers()) do
        if
            v ~= LocalPlayer
            and v.Character
            and v.Character:FindFirstChild('Humanoid')
            and v.Character.Humanoid.Health ~= 0
            and v.Character:FindFirstChild('HumanoidRootPart')
        then
            local pos = workspace.CurrentCamera:WorldToViewportPoint(
                v.Character.PrimaryPart.Position
            )
            local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(
                LocalPlayer:GetMouse().X,
                LocalPlayer:GetMouse().Y
            )).magnitude
            if magnitude < distance then
                zclosest = v
                distance = magnitude
            end
        end
    end
    return zclosest
end

local crosshair_position = 'Middle'

local Cursor = loadstring(
    game:HttpGet(
        'https://raw.githubusercontent.com/Ziheim51000/test/refs/heads/main/Drawing%20Crosshair',
        true
    )
)()
getgenv().crosshair.color = Color3.new(168, 189, 149)
getgenv().crosshair.mode = 'Middle'
getgenv().crosshair.sticky = false
getgenv().crosshair.enabled = false
getgenv().crosshair.spin = false

local Camera = workspace.CurrentCamera

RunService.RenderStepped:Connect(function()
    -- Make sure everything needed exists
    local isSticky = getgenv().crosshair.sticky
    local isTargetLockEnabled = target_aim.Aiming.Target.Enabled
    local currentTarget = target_aim.Aiming.Target.Part -- Assuming this is the actual TargetPart

    if
        isSticky
        and isTargetLockEnabled
        and currentTarget
        and currentTarget:IsDescendantOf(workspace)
    then
        local Position, onScreen =
            Camera:WorldToViewportPoint(currentTarget.Position)
        if onScreen then
            getgenv().crosshair.mode = 'custom'
            getgenv().crosshair.position = Vector2.new(Position.X, Position.Y)
        end
    else
        getgenv().crosshair.mode = crosshair_position -- fallback to whatever static mode you set
        getgenv().crosshair.position = nil
    end
end)

local UserInputService = game:GetService('UserInputService')

local player = Players.LocalPlayer
local clicking = false
local autoClickEnabled = false
local clickInterval = 0.1

local function getEquippedTool()
    local character = player.Character
    if character then
        for _, tool in ipairs(character:GetChildren()) do
            if tool:IsA('Tool') then
                return tool
            end
        end
    end
    return nil
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if
        input.UserInputType == Enum.UserInputType.MouseButton1
        and not gameProcessed
    then
        clicking = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        clicking = false
    end
end)

RunService.RenderStepped:Connect(function()
    if autoClickEnabled and clicking then
        local tool = getEquippedTool()
        if tool and tool:IsA('Tool') then
            tool:Activate()
        end
        task.wait(clickInterval)
    end
end)

Visualization:AddToggle('MyToggle', {
    Text = 'tracer',
    Default = false,
    Callback = function(Value)
        tracerenabledlolol = Value
    end,
}):AddColorPicker('ColorPicker', {
    Default = Color3.new(98, 33, 180), -- Bright green
    Title = 'tracer color', -- Optional. Allows you to have a custom color picker title (when you open it)
    Transparency = nil, -- Optional. Enables transparency changing for this color picker (leave as nil to disable)

    Callback = function(Value)
        tracercolorlolol = Value
    end,
})

Visualization:AddToggle('MyToggle', {
    Text = 'highlight',
    Default = false,
    Callback = function(Value)
        hightlightendalbed = Value
    end,
}):AddColorPicker('ColorPicker', {
    Default = Color3.new(255, 119, 175), -- Bright green
    Title = 'Highlight inLine', -- Optional. Allows you to have a custom color picker title (when you open it)
    Transparency = nil, -- Optional. Enables transparency changing for this color picker (leave as nil to disable)

    Callback = function(Value)
        fillcolorxd = Value
    end,
})

local Strafe = Tabs.Rage:AddRightGroupbox('Orbit', 'orbit')

local strafeEnabled = false
local strafeMode = 'Orbit'
local strafeSpeed = 5
local strafeXOffset = 5
local predictMovementEnabled = false
local PredicTvalue = 0.3
local targetPart = true
local isGrabbed = false

function predictPosition(targetRoot, prediction)
    local velocity = targetRoot.Velocity
    return targetRoot.Position + (velocity * prediction)
end

RunService.Heartbeat:Connect(function()
    if strafeEnabled and targetPart and not isGrabbed then
        if not AZURE_TmKALRSX or not AZURE_TmKALRSX.Character then
            return
        end

        local targetRoot =
            AZURE_TmKALRSX.Character:FindFirstChild('HumanoidRootPart')
        if not targetRoot then
            return
        end

        local targetPosition = targetRoot.Position
        if predictMovementEnabled then
            targetPosition = predictPosition(targetRoot, PredicTvalue)
        end

        local offset
        if strafeMode == 'Orbit' then
            local angle = tick() * strafeSpeed
            offset = Vector3.new(
                math.cos(angle) * strafeXOffset,
                -0.1,
                math.sin(angle) * strafeXOffset
            )
            LocalPlayer.Character.HumanoidRootPart.CFrame =
                CFrame.new(targetPosition + offset, targetPosition)
        elseif strafeMode == 'Random' then
            offset = Vector3.new(
                math.random(-20, 20),
                math.random(-10, 10),
                math.random(-20, 20)
            )
            local randomRotation = CFrame.Angles(
                math.rad(math.random(0, 360)),
                math.rad(math.random(0, 360)),
                math.rad(math.random(0, 360))
            )
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(
                targetPosition + offset
            ) * randomRotation
        end
    end
end)

-- Just a toggle now, no keybind
Strafe:AddToggle('StrafeToggle', {
    Text = 'orbit target',
    Default = false,
    Callback = function(Value)
        strafeEnabled = Value
        if not Value then
            if Core then
                Core:Destroy()
                Core = nil
            end
            if BodyVelocity then
                BodyVelocity:Destroy()
                BodyVelocity = nil
            end
            if oldPosition then
                LocalPlayer.Character.HumanoidRootPart.CFrame = oldPosition
                oldPosition = nil
            end
            workspace.CurrentCamera.CameraSubject =
                LocalPlayer.Character:FindFirstChild('Humanoid')
        end
    end,
})

Strafe:AddDropdown('StrafeMode', {
    Text = 'mode',
    Values = { 'Orbit', 'Random' },
    Default = 'Orbit',
    Callback = function(Value)
        strafeMode = Value
    end,
})

Strafe:AddSlider('StrafeSpeed', {
    Text = 'speed',
    Default = 5,
    Min = 1,
    Max = 20,
    Rounding = 1,
    Callback = function(Value)
        strafeSpeed = Value
    end,
})

Strafe:AddSlider('StrafeXOffset', {
    Text = 'distance',
    Default = 5,
    Min = 1,
    Max = 20,
    Rounding = 1,
    Callback = function(Value)
        strafeXOffset = Value
    end,
})

Strafe:AddToggle('PredictMovement', {
    Text = 'predict',
    Default = false,
    Callback = function(Value)
        predictMovementEnabled = Value
    end,
})

Strafe:AddSlider('StrafePredictionDistance', {
    Text = 'movement prediction',
    Default = 0.3,
    Min = 0.1,
    Max = 10,
    Rounding = 1,
    Callback = function(Value)
        PredicTvalue = Value
    end,
})

local GunShiet = Tabs.Rage:AddRightGroupbox('Gun', 'bow-arrow')

GunShiet:AddToggle('AutoClickToggle', {
    Text = 'fully automatic',
    Default = false,
    Tooltip = 'Makes all weapons automatic',
    Callback = function(value)
        autoClickEnabled = value
    end,
})

getgenv().autoPatchEnabled = false
local patchedConnections = {}

function PatchTool(tool)
    if not tool or not tool:FindFirstChild('GunScript') then
        return
    end
    for _, connection in ipairs(getconnections(tool.Activated)) do
        if not patchedConnections[connection] then
            local info = debug.getinfo(connection.Function)
            for i = 1, info.nups do
                local upval, name = debug.getupvalue(connection.Function, i)
                if type(upval) == 'number' and upval > 0.01 then
                    patchedConnections[connection] = patchedConnections[connection]
                        or {}
                    table.insert(
                        patchedConnections[connection],
                        { i = i, val = upval }
                    )
                    debug.setupvalue(connection.Function, i, 0.0000000000001)
                end
            end
        end
    end
end

LocalPlayer.Character.ChildAdded:Connect(function(child)
    if getgenv().autoPatchEnabled and child:IsA('Tool') then
        PatchTool(child)
    end
end)

GunShiet:AddToggle('AutoPatchToggle', {
    Text = 'rapid fire',
    Default = false,
    Callback = function(value)
        getgenv().autoPatchEnabled = value
        if value then
            tool = LocalPlayer.Character
                and LocalPlayer.Character:FindFirstChildOfClass('Tool')
            PatchTool(tool)
        else
            -- revert patches
            for connection, patches in pairs(patchedConnections) do
                for _, patch in ipairs(patches) do
                    pcall(function()
                        debug.setupvalue(
                            connection.Function,
                            patch.i,
                            patch.val
                        )
                    end)
                end
            end
            table.clear(patchedConnections)
        end
    end,
})

getgenv().SpreadMod = {
    BulletSpread = {
        Enabled = false,
        Amount = 100,
    },
}

local old
old = hookfunction(math.random, function(...)
    local args = { ... }

    if checkcaller() then
        return old(...)
    end

    if
        (#args == 0)
        or (args[1] == -0.05 and args[2] == 0.05)
        or (args[1] == -0.1)
        or (args[1] == -0.05)
    then
        if SpreadMod.BulletSpread.Enabled then
            local spread = SpreadMod.BulletSpread.Amount
            return old(...) * (spread / 100)
        else
            return old(...)
        end
    end

    return old(...)
end)

GunShiet:AddToggle('SpreadToggle', {
    Text = 'spread modification',
    Default = false,
    Tooltip = 'Changes the spread of pellets on shotgun, lower values = higher damage',
    Callback = function(value)
        SpreadMod.BulletSpread.Enabled = value
    end,
})

GunShiet:AddSlider('SpreadAmount', {
    Text = 'amount',
    Default = 100,
    Min = 0,
    Max = 100,
    Compact = true,
    suffix = '%',
    Rounding = 1,
    Callback = function(value)
        SpreadMod.BulletSpread.Amount = value
    end,
})

local VelocitySpoofBro = Tabs.AntiAim:AddLeftGroupbox('safety', 'shield-check')

VelocitySpoofBro
    :AddToggle('FakePosToggle', {
        Text = 'freeze position',
        Default = false,
        Tooltip = 'freezes your position for all other players, but allows u to move freely',
        Callback = function(v)
            Enabled = v
            if Enabled then
                task.spawn(function()
                    while
                        Enabled
                        and Client.Character
                        and Client.Character:FindFirstChild('HumanoidRootPart')
                    do
                        sethiddenproperty(
                            Client.Character.HumanoidRootPart,
                            'NetworkIsSleeping',
                            true
                        )
                        task.wait()
                        sethiddenproperty(
                            Client.Character.HumanoidRootPart,
                            'NetworkIsSleeping',
                            false
                        )
                        task.wait()
                    end
                end)
                setfflag('S2PhysicsSenderRate', 1)
                setfpscap(1)
                task.wait(1)
                setfflag('S2PhysicsSenderRate', 38760)
                task.wait(1)
                setfpscap(240)
                notify('Fake position active')
            else
                setfflag('S2PhysicsSenderRate', 13)
                if
                    Client.Character
                    and Client.Character:FindFirstChild('HumanoidRootPart')
                then
                    sethiddenproperty(
                        Client.Character.HumanoidRootPart,
                        'NetworkIsSleeping',
                        false
                    )
                end
                notify('Fake position off')
            end
        end,
    })
    :AddKeyPicker('FakePosKeybind', {
        Default = 'V',
        SyncToggleState = false,
        Mode = 'Toggle',
        Text = 'Freeze Position',
        NoUI = false,
        Callback = function(Value)
            currentKey = Value
        end,
    })

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed or UserInputService:GetFocusedTextBox() then
        return
    end
    if input.KeyCode == currentKey then
        Toggles.FakePosToggle:Set(not Toggles.FakePosToggle.Value)
    end
end)

VelocitySpoofBro:AddToggle('AntiStomp', {
    Text = 'safe mode',
    Default = false,
    Callback = function(state) end,
})

VelocitySpoofBro:AddToggle('AntiStomp', {
    Text = 'always on',
    Default = false,
    Callback = function(state) end,
})

local VelocitySpoofBrother =
    Tabs.AntiAim:AddRightGroupbox('legit antiaim', 'bell-off')

local LocalPlayer = Players.LocalPlayer
local LocalCharacter = LocalPlayer.Character
    or LocalPlayer.CharacterAdded:Wait()
local LocalHumanoid = LocalCharacter:FindFirstChildOfClass('Humanoid')
    or LocalCharacter:WaitForChild('Humanoid', 60)
local LocalRootPart = LocalHumanoid.RootPart
    or LocalCharacter:WaitForChild('HumanoidRootPart', 60)

-- Update LocalCharacter references when respawning
LocalPlayer.CharacterAdded:Connect(function(Character)
    LocalCharacter = Character
    LocalHumanoid = LocalCharacter:FindFirstChildOfClass('Humanoid')
        or LocalCharacter:WaitForChild('Humanoid', 60)
    LocalRootPart = LocalHumanoid.RootPart
        or LocalCharacter:WaitForChild('HumanoidRootPart', 60)
end)

LocalPlayer.CharacterRemoving:Connect(function()
    LocalCharacter = nil
    LocalHumanoid = nil
    LocalRootPart = nil
end)

local AntiStompEnabled = false

task.spawn(function()
    local Sleeping = false
    while true do
        RunService.Heartbeat:Wait()
        if
            AntiStompEnabled
            and LocalCharacter
            and LocalHumanoid
            and LocalRootPart
        then
            sethiddenproperty(LocalRootPart, 'NetworkIsSleeping', Sleeping)
            Sleeping = not Sleeping
            RunService.PostSimulation:Wait()
        end
    end
end)

-- Add toggle
VelocitySpoofBrother:AddToggle('AntiStomp', {
    Text = 'enabled',
    Default = false,
    Tooltip = 'Makes lockers and legit players miss silently',
    Callback = function(state)
        AntiStompEnabled = state
    end,
}):AddKeyPicker('FakePosKeybind', {
    Default = 'V',
    SyncToggleState = false,
    Mode = 'Toggle',
    Text = 'Freeze Position',
    NoUI = false,
    Callback = function()
        currentKey = Value
    end,
})

VelocitySpoofBrother:AddDropdown('AuraType', {
    Text = 'type',
    Values = { 'Legit', 'Blatant' },
    Default = 'Legit',
    Callback = function(selected)
        aamodex = selected
    end,
})

local desync_setback = Instance.new('Part')
desync_setback.Name = 'Desync Setback'
desync_setback.Parent = workspace
desync_setback.Size = Vector3.new(2, 2, 1)
desync_setback.CanCollide = false
desync_setback.Anchored = true
desync_setback.Transparency = 1

local desync = {
    enabled = false,
    mode = 'Void',
    teleportPosition = Vector3.new(0, 0, 0),
    old_position = nil,
    voidSpamActive = false,
    toggleEnabled = false,
}

local Offsetposx = 10
local OffsetposY = 10
local Offsetposz = 10

local function resetCamera()
    if LocalPlayer.Character then
        local humanoid = LocalPlayer.Character:FindFirstChild('Humanoid')
        if humanoid then
            workspace.CurrentCamera.CameraSubject = humanoid
        end
    end
end

local function toggleDesync(state)
    desync.enabled = state
    if desync.enabled then
        workspace.CurrentCamera.CameraSubject = desync_setback
        Library:Notify(desync.mode .. ' Enabled | nebula', 2)
    else
        resetCamera()
        Library:Notify(desync.mode .. ' Disabled | nebula', 2)
    end
end

local function setDesyncMode(mode)
    desync.mode = mode
end

local VelocitySpoofBrothers =
    Tabs.AntiAim:AddLeftGroupbox('velocity spoofer', 'ban')

VelocitySpoofBrothers:AddToggle('AntiStomp', {
    Text = 'enabled',
    Default = false,
    Callback = function(state)
        desync.toggleEnabled = state
        if not desync.toggleEnabled then
            toggleDesync(false)
        end
    end,
}):AddKeyPicker('FakePosKeybind', {
    Default = 'V',
    SyncToggleState = false,
    Mode = 'Toggle',
    Text = 'Velocity Spoofer',
    NoUI = false,
    Callback = function()
        if not desync.toggleEnabled or UserInputService:GetFocusedTextBox() then
            return
        end
        toggleDesync(not desync.enabled)
    end,
})

VelocitySpoofBrothers:AddDropdown('AuraType', {
    Text = 'type',
    Values = {
        'Destroy Cheaters',
        'Rotation',
        'Custom',
        'Underground',
        'Void Spam',
        'Spin',
        'Raining',
        'Teleport Maze',
        'Void',
        'UnderGroundV2',
    },
    Default = 'Void Spam',
    Callback = function(selected)
        setDesyncMode(selected)
    end,
})

-- Main Desync Logic
RunService.Heartbeat:Connect(function()
    if desync.enabled and LocalPlayer.Character then
        local rootPart =
            LocalPlayer.Character:FindFirstChild('HumanoidRootPart')
        if rootPart then
            desync.old_position = rootPart.CFrame

            if desync.mode == 'Destroy Cheaters' then
                desync.teleportPosition =
                    Vector3.new(11223344556677889900, 1, 1)
            elseif desync.mode == 'Underground' then
                desync.teleportPosition = rootPart.Position
                    - Vector3.new(0, 9, 0)
            elseif desync.mode == 'UnderGroundV2' then
                desync.teleportPosition = rootPart.Position
                    - Vector3.new(0, 11, 0)
            elseif desync.mode == 'Custom' then
                desync.teleportPosition = rootPart.Position
                    - Vector3.new(Offsetposx, OffsetposY, Offsetposz)
            elseif desync.mode == 'Void Spam' then
                desync.teleportPosition = math.random(1, 2) == 1
                        and desync.old_position.Position
                    or Vector3.new(
                        math.random(10000, 50000),
                        math.random(10000, 50000),
                        math.random(10000, 50000)
                    )
            elseif desync.mode == 'Void' then
                desync.teleportPosition = Vector3.new(
                    rootPart.Position.X + math.random(-444444, 444444),
                    rootPart.Position.Y + math.random(-444444, 444444),
                    rootPart.Position.Z + math.random(-44444, 44444)
                )
            elseif desync.mode == 'Spin' then
                desync.teleportPosition = rootPart.Position
                    + Vector3.new(0, math.sin(tick() * 2) * 10, 0)
            elseif desync.mode == 'Raining' then
                desync.teleportPosition = Vector3.new(
                    rootPart.Position.X + math.random(-10, 10),
                    rootPart.Position.Y + math.random(2, 5),
                    rootPart.Position.Z + math.random(-10, 10)
                )
            elseif desync.mode == 'Teleport Maze' then
                desync.teleportPosition = Vector3.new(
                    math.random(-100, 100),
                    math.random(5, 50),
                    math.random(-100, 100)
                )
            end

            local visualizer = workspace:FindFirstChild('DesyncVisualizer')
            if not visualizer then
                visualizer = Instance.new('Part')
                visualizer.Name = 'DesyncVisualizer'
                visualizer.Size = Vector3.new(1, 1, 1)
                visualizer.Anchored = true
                visualizer.CanCollide = false
                visualizer.BrickColor = BrickColor.new('Bright blue')
                visualizer.Parent = workspace
            end

            visualizer.Position = desync.teleportPosition
            visualizer.Transparency = 1

            if desync.mode ~= 'Rotation' then
                rootPart.CFrame = CFrame.new(desync.teleportPosition)
                workspace.CurrentCamera.CameraSubject = desync_setback

                RunService.RenderStepped:Wait()

                desync_setback.CFrame = desync.old_position
                    * CFrame.new(0, rootPart.Size.Y / 2 + 0.5, 0)
                rootPart.CFrame = desync.old_position
            end
        end
    end
end)

local layerESP = Tabs.Visuals:AddLeftGroupbox('esp', 'user')

layerESP
    :AddToggle('MyToggle', {
        Text = 'box',
        Default = false,
        Callback = function(val)
            getgenv().Config.Box.Enable = val
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(98, 117, 180),
        Title = 'box color',
        Callback = function(color)
            getgenv().Config.Box.Color = color
        end,
    })

layerESP
    :AddToggle('MyToggle', {
        Text = 'filled',
        Default = false,
        Callback = function(val)
            getgenv().Config.Box.Filled.Enable = val
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(98, 117, 180),
        Title = 'start',
        Callback = function(color)
            getgenv().Config.Box.Filled.Gradient.Color.Start = color
        end,
    })
    :AddColorPicker('dsdsdad', {
        Default = Color3.fromRGB(98, 117, 180),
        Title = 'end',
        Callback = function(color)
            getgenv().Config.Box.Filled.Gradient.Color.End = color
        end,
    })

layerESP:AddToggle('MyToggle', {
    Text = 'text',
    Default = false,
    Callback = function(val)
        getgenv().Config.Text.Enable = val
    end,
})

layerESP
    :AddToggle('MyToggle', {
        Text = 'healthbar',
        Default = false,
        Callback = function(val)
            getgenv().Config.Bars.Health.Enable = val
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(98, 117, 180),
        Callback = function(color)
            getgenv().Config.Bars.Health.Color1 = color
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(255, 255, 0),
        Callback = function(color)
            getgenv().Config.Bars.Health.Color2 = color
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(255, 0, 0),
        Callback = function(color)
            getgenv().Config.Bars.Health.Color3 = color
        end,
    })

layerESP
    :AddToggle('MyToggle', {
        Text = 'armor-bar',
        Default = false,
        Callback = function(val)
            getgenv().Config.Bars.Armor.Enable = val
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(0, 0, 255),
        Callback = function(color)
            getgenv().Config.Bars.Armor.Color1 = color
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(135, 206, 235),
        Callback = function(color)
            getgenv().Config.Bars.Armor.Color2 = color
        end,
    })
    :AddColorPicker('FOVColor', {
        Default = Color3.fromRGB(1, 0, 0),
        Callback = function(color)
            getgenv().Config.Bars.Armor.Color3 = color
        end,
    })

getgenv().HighlightESP = getgenv().HighlightESP
    or {
        Enabled = false,
        FillColor = Color3.fromRGB(98, 117, 180),
        OutlineColor = Color3.fromRGB(0, 0, 0),
        FadeEffect = false, -- added in 1.05
    }

Highlights = {}

function CreateHighlight(player)
    if player.Character and not Highlights[player] then
        local highlight = Instance.new('Highlight')
        highlight.FillColor = getgenv().HighlightESP.FillColor
        highlight.OutlineColor = getgenv().HighlightESP.OutlineColor
        highlight.FillTransparency = 0
        highlight.OutlineTransparency = 0
        highlight.Adornee = player.Character
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        highlight.Parent = game:GetService('CoreGui')

        Highlights[player] = highlight
    end
end

function RemoveHighlight(player)
    if Highlights[player] then
        Highlights[player]:Destroy()
        Highlights[player] = nil
    end
end

function UpdateHighlightColors()
    for _, highlight in pairs(Highlights) do
        if highlight and highlight:IsA('Highlight') then
            highlight.FillColor = getgenv().HighlightESP.FillColor
            highlight.OutlineColor = getgenv().HighlightESP.OutlineColor
        end
    end
end

getgenv().Fading = false

task.spawn(function()
    local fadeDirection = 1 -- 1 = fade out, -1 = fade in
    local transparency = 0

    RunService.RenderStepped:Connect(function(dt)
        if
            getgenv().HighlightESP.FadeEffect
            and getgenv().HighlightESP.Enabled
        then
            transparency = transparency + (dt * 0.5 * fadeDirection)
            if transparency >= 1 then
                transparency = 1
                fadeDirection = -1
            elseif transparency <= 0 then
                transparency = 0
                fadeDirection = 1
            end

            for _, highlight in pairs(Highlights) do
                if highlight and highlight:IsA('Highlight') then
                    highlight.FillTransparency = transparency
                    highlight.OutlineTransparency = transparency
                end
            end
        end
    end)
end)

layerESP
    :AddToggle('HighlightESP_Toggle', {
        Text = 'chams',
        Default = false,
        Callback = function(val)
            getgenv().HighlightESP.Enabled = val

            if val then
                for _, player in ipairs(game.Players:GetPlayers()) do
                    if player ~= game.Players.LocalPlayer then
                        CreateHighlight(player)
                    end
                end

                game.Players.PlayerAdded:Connect(function(player)
                    player.CharacterAdded:Connect(function()
                        if getgenv().HighlightESP.Enabled then
                            task.wait(0.5)
                            CreateHighlight(player)
                        end
                    end)
                end)
            else
                for player, _ in pairs(Highlights) do
                    RemoveHighlight(player)
                end
            end
        end,
    })
    :AddColorPicker('HighlightESP_FillColor', {
        Text = 'fill color',
        Default = getgenv().HighlightESP.FillColor,
        Callback = function(color)
            getgenv().HighlightESP.FillColor = color
            for _, highlight in pairs(Highlights) do
                if highlight and highlight:IsA('Highlight') then
                    highlight.FillColor = color
                end
            end
        end,
    })
    :AddColorPicker('HighlightESP_OutlineColor', {
        Text = 'outline color',
        Default = getgenv().HighlightESP.OutlineColor,
        Callback = function(color)
            getgenv().HighlightESP.OutlineColor = color
            for _, highlight in pairs(Highlights) do
                if highlight and highlight:IsA('Highlight') then
                    highlight.OutlineColor = color
                end
            end
        end,
    })

layerESP:AddToggle('HighlightFadeEffect', {
    Text = 'animate chams',
    Default = false,
    Callback = function(val)
        getgenv().HighlightESP.FadeEffect = val
        if not val then
            for _, highlight in pairs(Highlights) do
                if highlight and highlight:IsA('Highlight') then -- shitty way to do it but its 4 am at the time of coding this
                    highlight.FillTransparency = 0
                    highlight.OutlineTransparency = 0
                end
            end
        end
    end,
})

layerESP:AddToggle('MyToggle', {
    Text = 'team check',
    Default = false,
    Callback = function(val)
        getgenv().Config.Text.Name.Teamcheck = val
    end,
})

local TabPlayer = Tabs.Visuals:AddRightGroupbox('aura', 'flame')

local HitEffectModule = {
    Locals = {
        HitEffect = {
            Type = {},
        },
    },
}

local Settings = {
    Visuals = {
        SelfESP = {
            Trail = {
                Color = Color3.fromRGB(255, 110, 0),
                Color2 = Color3.fromRGB(255, 0, 0), -- Second color for gradient
                LifeTime = 1.6,
                Width = 0.1,
            },
            Aura = {
                Color = Color3.fromRGB(152, 0, 252),
            },
        },
    },
} -- make sure all tables are properly closed
local Attachment = Instance.new('Attachment')
HitEffectModule.Locals.HitEffect.Type['ForceField'] = Attachment
local swirl = Instance.new('ParticleEmitter', Attachment)
swirl.Name = 'swirl'
swirl.Lifetime = NumberRange.new(2)
swirl.SpreadAngle = Vector2.new(-360, 360)
swirl.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 1),
    NumberSequenceKeypoint.new(0.5, 0.5),
    NumberSequenceKeypoint.new(1, 1),
})
swirl.LightEmission = 10
swirl.Color = ColorSequence.new(Settings.Visuals.SelfESP.Aura.Color)
swirl.VelocitySpread = -360
swirl.Squash = NumberSequence.new(0)
swirl.Speed = NumberRange.new(0.01)
swirl.Size = NumberSequence.new(7)
swirl.ZOffset = -1
swirl.ShapeInOut = Enum.ParticleEmitterShapeInOut.InAndOut
swirl.Rate = 40
swirl.LockedToPart = true
swirl.Texture = 'rbxassetid://10558425570'
swirl.RotSpeed = NumberRange.new(200)
swirl.Orientation = Enum.ParticleOrientation.VelocityPerpendicular

local Bolts = Instance.new('ParticleEmitter', Attachment)
Bolts.Name = 'Bolts'
Bolts.Lifetime = NumberRange.new(0.333)
Bolts.LockedToPart = true
Bolts.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 0.88),
    NumberSequenceKeypoint.new(0.055, 0.98),
    NumberSequenceKeypoint.new(0.111, 0.17),
    NumberSequenceKeypoint.new(0.166, 0.39),
    NumberSequenceKeypoint.new(0.222, 0.12),
    NumberSequenceKeypoint.new(0.277, 0.92),
    NumberSequenceKeypoint.new(0.333, 0.41),
    NumberSequenceKeypoint.new(0.388, 0.21),
    NumberSequenceKeypoint.new(0.444, 0.78),
    NumberSequenceKeypoint.new(0.499, 0.23),
    NumberSequenceKeypoint.new(0.555, 0.78),
    NumberSequenceKeypoint.new(0.610, 0.81),
    NumberSequenceKeypoint.new(0.666, 0.91),
    NumberSequenceKeypoint.new(0.721, 0.87),
    NumberSequenceKeypoint.new(0.777, 0.41),
    NumberSequenceKeypoint.new(0.832, 0.30),
    NumberSequenceKeypoint.new(0.888, 0.16),
    NumberSequenceKeypoint.new(0.943, 0.39),
    NumberSequenceKeypoint.new(0.999, 0.70),
    NumberSequenceKeypoint.new(1, 1),
})
Bolts.LightEmission = 1
Bolts.Color = ColorSequence.new(Settings.Visuals.SelfESP.Aura.Color)
Bolts.Speed = NumberRange.new(0)
Bolts.Size = NumberSequence.new(4.8)
Bolts.Rate = 12
Bolts.Texture = 'rbxassetid://1084955012'
Bolts.Rotation = NumberRange.new(-180, 180)

local Bubble = Instance.new('ParticleEmitter', Attachment)
Bubble.Name = 'Bubble'
Bubble.Lifetime = NumberRange.new(1)
Bubble.LockedToPart = true
Bubble.Transparency = NumberSequence.new({
    NumberSequenceKeypoint.new(0, 1),
    NumberSequenceKeypoint.new(0.5, 0.7),
    NumberSequenceKeypoint.new(1, 1),
})
Bubble.LightEmission = 1
Bubble.Color = ColorSequence.new(Settings.Visuals.SelfESP.Aura.Color)
Bubble.Speed = NumberRange.new(0)
Bubble.Size = NumberSequence.new(4)
Bubble.Rate = 6
Bubble.Texture = 'rbxassetid://1084955488'
Bubble.Rotation = NumberRange.new(-180, 180)

local function applyAura(auraName)
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild('HumanoidRootPart')

    Attachment.Parent = humanoidRootPart

    if getgenv().auraEnabled then
        swirl.Enabled = auraName == 'ForceField'
        Bolts.Enabled = auraName == 'Bolts'
        Bubble.Enabled = auraName == 'Bubble'
        humanoidRootPart.Material = Enum.Material.Neon
    else
        swirl.Enabled = false
        Bolts.Enabled = false
        Bubble.Enabled = false
    end
end

local function onCharacterAdded(character)
    if getgenv().auraEnabled then
        applyAura(getgenv().selectedAura or 'ForceField')
    end
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end

TabPlayer:AddToggle('AuraToggle', {
    Text = 'aura',
    Default = false,
    Callback = function(state)
        getgenv().auraEnabled = state
        applyAura(getgenv().selectedAura or 'ForceField')
    end,
}):AddColorPicker('AuraColor', {
    Text = 'color',
    Default = Settings.Visuals.SelfESP.Aura.Color,
    Callback = function(color)
        Settings.Visuals.SelfESP.Aura.Color = color
        swirl.Color = ColorSequence.new(color)
        Bolts.Color = ColorSequence.new(color)
        Bubble.Color = ColorSequence.new(color)
        if getgenv().auraEnabled then
            applyAura(getgenv().selectedAura or 'ForceField')
        end
    end,
})

TabPlayer:AddDropdown('AuraType', {
    Text = 'type',
    Values = { 'ForceField', 'Bolts', 'Bubble' },
    Default = 'Bubble',
    Callback = function(selected)
        getgenv().selectedAura = selected
        if getgenv().auraEnabled then
            applyAura(selected)
        end
    end,
})

local TabCrosshair = Tabs.Visuals:AddLeftGroupbox('crosshair', 'plus')

local SPACING = 5
local OUTLINE_THICK = 5
local outlineColor = Color3.new(0, 0, 0) -- black outline

-- Default values
local LENGTH = 100
local THICK = 2
local ROT_SPEED = 350
local crosshairColor = Color3.fromRGB(255, 255, 255)

local crosshairEnabled = false
local attachToTarget = false
local rotateCrosshair = false
local positionMode = 'mouse' -- default: "mouse", other option: "middle"

local angle = 0
local lastPos = nil

local crosshairLines = {}
local outlineLines = {}

for i = 1, 4 do
    local outlineLine = Drawing.new('Line')
    outlineLine.Thickness = OUTLINE_THICK
    outlineLine.Color = outlineColor
    outlineLine.Visible = false
    table.insert(outlineLines, outlineLine)

    local mainLine = Drawing.new('Line')
    mainLine.Thickness = THICK
    mainLine.Color = crosshairColor
    mainLine.Visible = false
    table.insert(crosshairLines, mainLine)
end

local function rot(x, y, rad)
    local cosA, sinA = math.cos(rad), math.sin(rad)
    return Vector2.new(cosA * x - sinA * y, sinA * x + cosA * y)
end

local function updateCrosshair(dt)
    if not crosshairEnabled then
        for i = 1, 4 do
            crosshairLines[i].Visible = false
            outlineLines[i].Visible = false
        end
        lastPos = nil
        return
    end

    if rotateCrosshair then
        angle = angle + ROT_SPEED * dt
    end

    local rad = math.rad(angle)
    local targetPos2D

    if positionMode == 'middle' then
        targetPos2D =
            Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    elseif positionMode == 'mouse' then
        targetPos2D = UserInputService:GetMouseLocation()
    else
        -- fallback to mouse position if invalid mode
        targetPos2D = UserInputService:GetMouseLocation()
    end

    -- If attachToTarget enabled, override targetPos2D with target's screen pos
    if
        attachToTarget
        and Target
        and Configurations.Target
        and Configurations.Target.Enabled
    then
        if Target.Character then
            local targetHRP =
                Target.Character:FindFirstChild('HumanoidRootPart')
            if targetHRP then
                local screenPos, onScreen =
                    Camera:WorldToViewportPoint(targetHRP.Position)
                if onScreen then
                    targetPos2D = Vector2.new(screenPos.X, screenPos.Y)
                end
            end
        end
    end

    if not lastPos then
        lastPos = targetPos2D
    end

    local smoothingSpeed = 12
    local smoothPos =
        lastPos:Lerp(targetPos2D, math.clamp(smoothingSpeed * dt, 0, 1))
    lastPos = smoothPos

    local points = {
        { Vector2.new(0, -LENGTH / 2 - SPACING), Vector2.new(0, -SPACING) },
        { Vector2.new(0, SPACING), Vector2.new(0, LENGTH / 2 + SPACING) },
        { Vector2.new(-LENGTH / 2 - SPACING, 0), Vector2.new(-SPACING, 0) },
        { Vector2.new(SPACING, 0), Vector2.new(LENGTH / 2 + SPACING, 0) },
    }

    for i = 1, 4 do
        local startPoint = rot(points[i][1].X, points[i][1].Y, rad)
        local endPoint = rot(points[i][2].X, points[i][2].Y, rad)

        outlineLines[i].From = smoothPos + startPoint
        outlineLines[i].To = smoothPos + endPoint
        outlineLines[i].Visible = true
        outlineLines[i].Color = outlineColor
        outlineLines[i].Thickness = OUTLINE_THICK

        crosshairLines[i].From = smoothPos + startPoint
        crosshairLines[i].To = smoothPos + endPoint
        crosshairLines[i].Visible = true
        crosshairLines[i].Color = crosshairColor
        crosshairLines[i].Thickness = THICK
    end
end

RunService.RenderStepped:Connect(updateCrosshair)

TabCrosshair:AddToggle('EnableCrosshair', {
    Text = 'enabled',
    Default = false,
    Callback = function(value)
        crosshairEnabled = value
        if not value then
            for _, line in pairs(crosshairLines) do
                line.Visible = false
            end
            for _, outline in pairs(outlineLines) do
                outline.Visible = false
            end
        end
    end,
}):AddColorPicker('CrosshairColor', {
    Text = 'Crosshair Color',
    Default = crosshairColor,
    Callback = function(color)
        crosshairColor = color
        for _, line in pairs(crosshairLines) do
            line.Color = crosshairColor
        end
    end,
})

TabCrosshair:AddToggle('RotateCrosshair', {
    Text = 'spin',
    Default = false,
    Callback = function(value)
        rotateCrosshair = value
    end,
})

TabCrosshair:AddSlider('RotationSpeed', {
    Text = 'rotation speed',
    Default = ROT_SPEED,
    Min = 0,
    Max = 1000,
    Rounding = 1,
    Callback = function(value)
        ROT_SPEED = value
    end,
})

TabCrosshair:AddSlider('LineThickness', {
    Text = 'thickness',
    Default = THICK,
    Min = 1,
    Max = 10,
    Rounding = 1,
    Callback = function(value)
        THICK = value
        for i = 1, 4 do
            crosshairLines[i].Thickness = THICK
        end
    end,
})

TabCrosshair:AddSlider('LineLength', {
    Text = 'length',
    Default = LENGTH,
    Min = 10,
    Max = 300,
    Rounding = 1,
    Callback = function(value)
        LENGTH = value
    end,
})

TabCrosshair:AddDropdown('AuraType', {
    Text = 'position',
    Values = { 'middle', 'mouse' },
    Default = 'Bubble',
    Callback = function(selected)
        positionMode = selected
    end,
})

local HitSound = Tabs.Visuals:AddRightGroupbox('hit sounds', 'volume-2')

getgenv().hitsounds = {
    Bameware = 'rbxassetid://3124331820',
    Tf2 = 'rbxassetid://137392628136734',
    Bubble = 'rbxassetid://6534947588',
    Fortnite = 'rbxassetid://2513174484',
    Tralalero = 'rbxassetid://105044304109159',
    Rust = 'rbxassetid://1255040462',
    Oof = 'rbxassetid://79348298352567',
    HitMarker = 'rbxassetid://7242037470',
    Sparkle = 'rbxassetid://78601008552434',
    Vine = 'rbxassetid://5332680810',
    Bruh = 'rbxassetid://4578740568',
    Skeet = 'rbxassetid://5633695679',
    Neverlose = 'rbxassetid://6534948092',
    Fatality = 'rbxassetid://6534947869',
    Bonk = 'rbxassetid://5766898159',
    Minecraft = 'rbxassetid://4018616850',
}

getgenv().selectedHitsound = 'Fortnite'
getgenv().hitsoundEnabled = false
getgenv().hitsoundVolume = 1

function playHitsound()
    if getgenv().hitsoundEnabled then
        local sound = Instance.new('Sound')
        sound.SoundId = getgenv().hitsounds[getgenv().selectedHitsound]
        sound.Volume = getgenv().hitsoundVolume
        sound.Parent = workspace
        sound:Play()
        sound.Ended:Connect(function()
            sound:Destroy()
        end)
    end
end

HitSound:AddToggle('hstoggle', {
    Text = 'Hit Sound',
    Default = false,
    Callback = function(state)
        getgenv().hitsoundEnabled = state
    end,
})

-- Build dropdown values dynamically from hitsounds table
local soundNames = {}
for name, _ in pairs(getgenv().hitsounds) do
    table.insert(soundNames, name)
end
table.sort(soundNames) -- optional: sort alphabetically

HitSound:AddDropdown('hs', {
    Text = 'Sound to Play',
    Values = soundNames,
    Default = 'Fortnite',
    Callback = function(value)
        getgenv().selectedHitsound = value
    end,
})

local DMGNumber = Tabs.Visuals:AddRightGroupbox('damage number', 'heart-minus')

local lastHealth = nil
local lastTarget = nil

local function createDamageNumber(amount, position)
    local dmgText = Drawing.new('Text')
    dmgText.Text = '-' .. tostring(math.floor(amount))
    dmgText.Size = 20 -- bigger size
    dmgText.Center = true
    dmgText.Outline = true
    dmgText.Color = Color3.new(1, 0.3, 0.3)
    dmgText.OutlineColor = Color3.new(0, 0, 0) -- black outline
    dmgText.Visible = true

    local screenPos, onScreen = Camera:WorldToViewportPoint(position)
    if not onScreen then
        dmgText:Remove()
        return
    end
    dmgText.Position = Vector2.new(screenPos.X, screenPos.Y)

    local startTime = tick()
    local duration = 1.5 -- seconds

    task.spawn(function()
        while tick() - startTime < duration do
            local elapsed = tick() - startTime
            local progress = elapsed / duration

            -- move up smoothly over time
            local yOffset = -(progress * 30) -- 30px total over 1.5s
            dmgText.Position = Vector2.new(screenPos.X, screenPos.Y + yOffset)

            -- fade out smoothly
            dmgText.Transparency = 1 - (1 - progress)

            RunService.RenderStepped:Wait()
        end
        dmgText:Remove()
    end)
end

-- Main loop to check damage
RunService.Heartbeat:Connect(function()
    if not showDamageNumbers then
        return
    end

    if
        target_aim
        and target_aim.Aiming.Target
        and target_aim.Aiming.Target.Enabled
    then
        local targetPlayer = AZURE_TmKALRSX
        if
            targetPlayer
            and targetPlayer.Character
            and targetPlayer.Character:FindFirstChild('Humanoid')
        then
            local humanoid = targetPlayer.Character.Humanoid
            local currentHealth = humanoid.Health

            if lastTarget ~= targetPlayer then
                lastHealth = currentHealth
                lastTarget = targetPlayer
                return
            end

            if lastHealth and currentHealth < lastHealth then
                local damage = lastHealth - currentHealth
                local head = targetPlayer.Character:FindFirstChild('Head')
                    or targetPlayer.Character:FindFirstChild('HumanoidRootPart')
                if head then
                    createDamageNumber(
                        damage,
                        head.Position + Vector3.new(0, 2, 0)
                    )
                end
            end

            lastHealth = currentHealth
        end
    else
        lastHealth = nil
        lastTarget = nil
    end
end)

DMGNumber:AddToggle('ShowDamage', {
    Text = 'damage number',
    Default = false,
    Callback = function(Value)
        showDamageNumbers = Value
    end,
})

local function GetBullet()
    if
        workspace:FindFirstChild('Ignored')
        and workspace.Ignored:FindFirstChild('Siren')
        and workspace.Ignored.Siren:FindFirstChild('Radius')
    then
        return {
            BulletPath = workspace.Ignored.Siren.Radius,
            BulletName = 'BULLET_RAYS',
            BulletBeamName = 'GunBeam',
        }
    elseif workspace:FindFirstChild('Ignored') then
        return {
            BulletPath = workspace.Ignored,
            BulletName = 'BULLET_RAYS',
            BulletBeamName = 'GunBeam',
        }
    elseif workspace then
        return {
            BulletPath = workspace,
            BulletName = 'Part',
            BulletBeamName = 'gb',
        }
    end
    return nil
end

local support = GetBullet()
local bullet_beam_name = support.BulletBeamName
local bullet_name = support.BulletName
local bullet_path = support.BulletPath

Configurations = {
    Visuals = {
        Bullet_Trails = {
            Enabled = false,
            Width = 1.0,
            Duration = 3,
            Fade = false,
            FadeDuration = 3,
            Color = Library.Accent,
            Texture = 'Electro', -- 12781803086
        },
    },
}

local BulletTexture = {
    Electro = 'rbxassetid://139193109954329',
    Cool = 'rbxassetid://116848240236550',
    Cum = 'rbxassetid://88263664141635',
}

local utility = {}
do
    utility.instance_new = function(type, properties)
        local instance = Instance.new(type)

        for property, value in properties do
            instance[property] = value
        end

        return instance
    end

    utility.new_connection = function(type, callback) --// by all means do NOT virtualize this
        local connection = type:Connect(callback)
        table.insert(connections, connection)
        return connection
    end

    utility.create_beam = LPH_NO_VIRTUALIZE(
        function(
            BulletTex,
            wid,
            from,
            to,
            color_1,
            color_2,
            duration,
            fade_enabled,
            fade_duration
        )
            local tween
            local total_time = 0

            local main_part = utility.instance_new('Part', {
                Parent = workspace,
                Size = Vector3.new(0, 0, 0),
                Massless = true,
                Transparency = 1,
                CanCollide = false,
                Position = from,
                Anchored = true,
            })

            local part0 = utility.instance_new('Part', {
                Parent = main_part,
                Size = Vector3.new(0, 0, 0),
                Massless = true,
                Transparency = 1,
                CanCollide = false,
                Position = from,
                Anchored = true,
            })

            local part1 = utility.instance_new('Part', {
                Parent = main_part,
                Size = Vector3.new(0, 0, 0),
                Massless = true,
                Transparency = 1,
                CanCollide = false,
                Position = to,
                Anchored = true,
            })

            local attachment0 =
                utility.instance_new('Attachment', { Parent = part0 })
            local attachment1 =
                utility.instance_new('Attachment', { Parent = part1 })

            local beam = utility.instance_new('Beam', {
                Texture = BulletTex,
                TextureMode = Enum.TextureMode.Wrap,
                TextureLength = 10,
                LightEmission = 10,
                LightInfluence = 1,
                FaceCamera = true,
                ZOffset = -1,
                Transparency = NumberSequence.new({
                    NumberSequenceKeypoint.new(0, 0),
                    NumberSequenceKeypoint.new(1, 1),
                }),
                Color = ColorSequence.new({
                    ColorSequenceKeypoint.new(0, color_1),
                    ColorSequenceKeypoint.new(1, color_2),
                }),
                Width0 = wid,
                Width1 = wid,
                Attachment0 = attachment0,
                Attachment1 = attachment1,
                Enabled = true,
                Parent = main_part,
            })

            if fade_enabled then
                tween = utility.new_connection(
                    RunService.Heartbeat,
                    function(delta_time) --// credits to Xander
                        total_time += delta_time
                        beam.Transparency = NumberSequence.new(
                            TweenService:GetValue(
                                (total_time / fade_duration),
                                Enum.EasingStyle.Quad,
                                Enum.EasingDirection.In
                            )
                        )
                    end
                )
            end

            task.delay(duration, function()
                main_part:Destroy()
                if tween then
                    tween:Disconnect()
                end
            end)
        end
    )
end

local Utility = {}

local function SirenAdded(Obj)
    local Character = LocalPlayer.Character
    local RootPart = Character and Character:FindFirstChild('HumanoidRootPart')

    local function VerifyBullet(obj)
        return (
            obj.Name == bullet_name
            or obj:FindFirstChild('Attachment')
            or obj:FindFirstChild(bullet_beam_name)
        ) and obj
    end

    local PlayerChecks = { PlayerGun = false }
    local BulletRay = VerifyBullet(Obj)

    if BulletRay and RootPart then
        local Mag = (RootPart.Position - BulletRay.Position).Magnitude
        if Mag <= 13 then
            PlayerChecks.PlayerGun = true
        end

        if PlayerChecks.PlayerGun then
            local GunBeam = BulletRay:WaitForChild(bullet_beam_name)

            local Attachment0 = GunBeam.Attachment0 -- closest to player
            local Attachment1 = GunBeam.Attachment1 -- mouse position

            if Configurations.Visuals.Bullet_Trails.Enabled then
                GunBeam:Destroy()
                utility.create_beam(
                    BulletTexture[Configurations.Visuals.Bullet_Trails.Texture],
                    Configurations.Visuals.Bullet_Trails.Width,
                    BulletRay.Position,
                    Attachment1.WorldCFrame.Position,
                    Configurations.Visuals.Bullet_Trails.Color,
                    Configurations.Visuals.Bullet_Trails.Color,
                    Configurations.Visuals.Bullet_Trails.Duration,
                    Configurations.Visuals.Bullet_Trails.Fade,
                    Configurations.Visuals.Bullet_Trails.Duration
                )
            end
        end
    end
end

if bullet_path then
    bullet_path.ChildAdded:Connect(SirenAdded)
end

local BTCs = Tabs.Visuals:AddRightGroupbox('bullet tracers', 'activity')

BTCs:AddToggle('ShowDamage', {
    Text = 'bullet tracers',
    Default = false,
    Callback = function(Value)
        Configurations.Visuals.Bullet_Trails.Enabled = Value
    end,
}):AddColorPicker('ColorPicker', {
    Default = Color3.new(255, 0, 0), -- Bright green
    Title = 'bullet tracers color', -- Optional. Allows you to have a custom color picker title (when you open it)
    Transparency = nil, -- Optional. Enables transparency changing for this color picker (leave as nil to disable)

    Callback = function(Value)
        Configurations.Visuals.Bullet_Trails.Color = Value
    end,
})

local WorldWorldWorld = Tabs.World:AddLeftGroupbox('World', 'globe')
local Lighting = game:GetService('Lighting')

local atmosphere = Lighting:FindFirstChildOfClass('Atmosphere')
    or Instance.new('Atmosphere', Lighting)
atmosphere.Parent = nil -- i do this so whenever its executed, you wont have this shit visual on.

WorldWorldWorld:AddToggle('AtmosphereToggle', {
    Text = 'enabled',
    Default = false,
    Callback = function(state)
        atmosphere.Parent = state and Lighting or nil
    end,
})
    :AddColorPicker('ColorPicker', {
        Text = 'atmosphere color',
        Default = atmosphere.Color,
        Callback = function(color)
            atmosphere.Color = color
        end,
    })
    :AddColorPicker('DecayColorPicker', {
        Text = 'decay color',
        Default = atmosphere.Decay,
        Callback = function(color)
            atmosphere.Decay = color
        end,
    })

WorldWorldWorld:AddSlider('DensitySlider', {
    Text = 'density',
    Min = 0,
    Max = 1,
    Default = atmosphere.Density,
    Rounding = 3,
    Compact = true,
    Callback = function(v)
        atmosphere.Density = v
    end,
})

WorldWorldWorld:AddSlider('GlareSlider', {
    Text = 'glare',
    Min = 0,
    Max = 10,
    Default = atmosphere.Glare,
    Rounding = 2,
    Compact = true,
    Callback = function(v)
        atmosphere.Glare = v
    end,
})

WorldWorldWorld:AddSlider('HazeSlider', {
    Text = 'haze',
    Min = 0,
    Max = 10,
    Default = atmosphere.Haze,
    Rounding = 2,
    Compact = true,
    Callback = function(value)
        atmosphere.Haze = value
    end,
})

local colorCorrection = Instance.new('ColorCorrectionEffect')
colorCorrection.Name = 'CustomColorCorrection'
colorCorrection.Parent = Lighting

local ColorCorrectionTab = Tabs.World:AddRightGroupbox("color correction", "brush")

ColorCorrectionTab:AddToggle('CCEnabled', {
    Text = 'enabled',
    Default = false,
    Callback = function(val)
        colorCorrection.Enabled = val
    end,
})

ColorCorrectionTab:AddSlider('Saturation', {
    Text = 'saturation',
    Min = -1,
    Max = 1,
    Default = 0.5,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Saturation = val
    end,
})

ColorCorrectionTab:AddSlider('Contrast', {
    Text = 'contrast',
    Min = 0,
    Max = 2,
    Default = 0.8,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Contrast = val
    end,
})

ColorCorrectionTab:AddSlider('BrightnessCC', {
    Text = 'brightness',
    Min = 0,
    Max = 2,
    Default = 0.5,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Brightness = val
    end,
})

ColorCorrectionTab:AddSlider('Contrast', {
    Text = 'exposure',
    Min = -5,
    Max = 5,
    Default = 0,
    Rounding = 2,
    Compact = true,
    Callback = function(val)
        colorCorrection.Contrast = val
    end,
})

colorCorrection.Enabled = true
colorCorrection.Saturation = 0 -- Default
colorCorrection.Contrast = 0 -- Default
colorCorrection.Brightness = 0 -- Default

local LightingShit = Tabs.World:AddLeftGroupbox("lighting", "moon")

local techToggleEnabled = false
local selectedTech = Lighting.Technology.Name

LightingShit:AddToggle('WorldTintEnabled', {
    Text = 'Ambient',
    Default = false,
    Callback = function(val)
        colorCorrection.TintColor = val and Options.WorldTintColor.Value
            or Color3.new(1, 1, 1)
    end,
}):AddColorPicker('WorldTintColor', {
    Text = 'tint color',
    Default = Color3.new(1, 1, 1), -- white (no tint)
    Callback = function(color)
        if Options.WorldTintEnabled.Value then
            colorCorrection.TintColor = color
        end
    end,
})

LightingShit:AddToggle('ColorShiftTopEnabled', {
    Text = 'ColorShift_Top',
    Default = false,
    Callback = function(val)
        Lighting.ColorShift_Top = val and Options.ColorShiftTop.Value
            or Color3.new(0, 0, 0)
    end,
}):AddColorPicker('ColorShiftTop', {
    Text = 'Top Color',
    Default = Color3.new(0, 0, 0),
    Callback = function(color)
        if Options.ColorShiftTopEnabled.Value then
            Lighting.ColorShift_Top = color
        end
    end,
})

LightingShit:AddToggle('ColorShiftBottomEnabled', {
    Text = 'ColorShift_Bottom',
    Default = false,
    Callback = function(val)
        if val then
            Lighting.ColorShift_Bottom = ColorShiftBottomPicker.Value
        else
            Lighting.ColorShift_Bottom = Color3.new(0, 0, 0)
        end
    end,
}):AddColorPicker('ColorShiftBottom', {
    Text = 'Bottom Color',
    Default = Color3.new(0, 0, 0),
    Callback = function(color)
        Lighting.ColorShift_Bottom = color
    end,
})

LightingShit:AddToggle('ColorShiftBottomEnabled', {
    Text = 'Fog',
    Default = false,
    Callback = function(val) end,
}):AddColorPicker('ColorShiftBottom', {
    Text = 'Fog Color',
    Default = Color3.new(0, 0, 0),
    Callback = function(color) end,
})

LightingShit:AddToggle('ColorShiftBottomEnabled', {
    Text = 'FogStart',
    Default = false,
    Callback = function(val) end,
})

LightingShit:AddSlider('BrightnessCC', {
    Text = 'Start',
    Min = 0,
    Max = 10000,
    Default = 0,
    Rounding = 1,
    Compact = true,
    Callback = function(val) end,
})

LightingShit:AddToggle('ColorShiftBottomEnabled', {
    Text = 'FogEnd',
    Default = false,
    Callback = function(val) end,
})

LightingShit:AddSlider('BrightnessCC', {
    Text = 'End',
    Min = 0,
    Max = 10000,
    Default = 500,
    Rounding = 1,
    Compact = true,
    Callback = function(val) end,
})

LightingShit:AddToggle('LightingTechToggle', {
    Text = 'GlobalShadows',
    Default = techToggleEnabled,
    Callback = function(val)
        techToggleEnabled = val
        if techToggleEnabled then
            Lighting.Technology = Enum.Technology[selectedTech]
        else
        end
    end,
})

local techOptions = {}
for _, tech in ipairs(Enum.Technology:GetEnumItems()) do
    table.insert(techOptions, tech.Name)
end

LightingShit:AddDropdown('LightingTechnology', {
    Text = 'Type',
    Default = selectedTech,
    Values = techOptions,
    Callback = function(val)
        selectedTech = val
        if techToggleEnabled then
            Lighting.Technology = Enum.Technology[selectedTech]
        end
    end,
})

local MOVEMENTLOLBROXD = Tabs.Misc:AddLeftGroupbox("character", "footprints")

getgenv().speedEnabled = false
getgenv().jumpEnabled = false
getgenv().SpeedValue = 50
getgenv().JumpValue = 100
getgenv().SpeedKeybind = nil

MOVEMENTLOLBROXD
    :AddToggle('NoJumpCD', {
        Text = 'flyhack',
        Default = false,
        Callback = function(val)
            if val then
                FlyLoop = RunService.Stepped:Connect(function()
                    pcall(function()
                        local speed = FlySpeed
                        local velocity = Vector3.new(0, 1, 0)

                        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                            velocity += workspace.CurrentCamera.CFrame.LookVector * speed
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                            velocity -= workspace.CurrentCamera.CFrame.LookVector * speed
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                            velocity -= workspace.CurrentCamera.CFrame.RightVector * speed
                        end
                        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                            velocity += workspace.CurrentCamera.CFrame.RightVector * speed
                        end

                        if
                            LocalPlayer.Character
                            and LocalPlayer.Character:FindFirstChild(
                                'HumanoidRootPart'
                            )
                        then
                            LocalPlayer.Character.HumanoidRootPart.Velocity =
                                velocity
                            LocalPlayer.Character.Humanoid:ChangeState(
                                Enum.HumanoidStateType.Freefall
                            )
                        end
                    end)
                end)
            elseif FlyLoop then
                FlyLoop:Disconnect()
                FlyLoop = nil
                if
                    LocalPlayer.Character
                    and LocalPlayer.Character:FindFirstChild('Humanoid')
                then
                    LocalPlayer.Character.Humanoid:ChangeState(
                        Enum.HumanoidStateType.Landed
                    )
                end
            end
        end,
    })
    :AddKeyPicker('KeyPicker', {
        Default = 'X',
        SyncToggleState = true,

        Mode = 'Toggle',

        Text = 'Flight Hack',
        NoUI = false,
        Callback = function(Value)
            FlightBindxdxd = Value
        end,
    })
MOVEMENTLOLBROXD:AddToggle('NoJUmpCD', {
    Text = 'speedhack',
    Default = false,
    Callback = function(val)
        speedenablelolol = val
    end,
}):AddKeyPicker('KeyPicker', {
    Default = 'Z',
    SyncToggleState = true,

    Mode = 'Toggle',

    Text = 'Speed Hack',
    NoUI = false,
    Callback = function(Value)
        SpeedBindxd = Value
    end,
})

MOVEMENTLOLBROXD:AddToggle('SpeedToggle', {
    Text = 'velocity speed',
    Default = false,
    Callback = function(Value)
        getgenv().speedEnabled = Value
        ApplyMovementSettings()
    end,
})

MOVEMENTLOLBROXD:AddToggle('JumpToggle', {
    Text = 'jump power',
    Default = false,
    Callback = function(Value)
        getgenv().jumpEnabled = Value
        ApplyMovementSettings()
    end,
})


spawn(function()
    RunService.Heartbeat:Connect(function()
        if speedenablelolol == true then
            player.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
                + player.Character.Humanoid.MoveDirection * SpeedValue
        end
    end)
end)

MOVEMENTLOLBROXD:AddSlider('WalkSpeedSlider', {
    Text = 'velocity speed',
    Min = 1,
    Max = 1000,
    Default = 50,
    Rounding = 1,
    Compact = true,
    Callback = function(Value)
        getgenv().SpeedValue = Value
        ApplyMovementSettings()
    end,
})

MOVEMENTLOLBROXD:AddSlider('JumpPowerSlider', {
    Text = 'jump power',
    Min = 1,
    Max = 500,
    Default = 100,
    Rounding = 1,
    Compact = true,
    Callback = function(Value)
        getgenv().JumpValue = Value
        ApplyMovementSettings()
    end,
})

game:GetService('RunService').RenderStepped:Connect(function()
    if
        LocalPlayer.Character
        and LocalPlayer.Character:FindFirstChild('Humanoid')
    then
        local hum = LocalPlayer.Character.Humanoid

        if getgenv().speedEnabled then
            hum.WalkSpeed = getgenv().SpeedValue
        end

        if getgenv().jumpEnabled then
            hum.JumpPower = getgenv().JumpValue
        end
    end
end)



LocalPlayer.CharacterAdded:Connect(function()
    LocalPlayer.Character:WaitForChild('Humanoid')
    ApplyMovementSettings()
end)

MOVEMENTLOLBROXD:AddSlider('MySlider', {
    Text = 'Amount',
    Default = 0,
    Min = 0,
    Max = 5000,
    Rounding = 1,
    Compact = true,
    Callback = function(Value)
        SpeedValue = Value / 1000
    end,
})

MOVEMENTLOLBROXD:AddSlider('MySlider', {
    Text = 'flight speed',
    Default = 0,
    Min = 0,
    Max = 5000,
    Rounding = 1,
    Compact = true,
    Callback = function(Value)
        FlySpeed = Value / 1000 * 50
    end,
})

local PLAYERLOLBRODUDEWHAT = Tabs.Misc:AddRightGroupbox("player", "shield-user")

PLAYERLOLBRODUDEWHAT:AddToggle('NoJUmpCD', {
    Text = 'no jump cooldown',
    Default = false,
    Callback = function(val)
        if val then
            local IsA = game.IsA
            local newindex = nil

            newindex = hookmetamethod(
                game,
                '__newindex',
                function(self, Index, Value)
                    if
                        not checkcaller()
                        and IsA(self, 'Humanoid')
                        and Index == 'JumpPower'
                    then
                        return
                    end
                    return newindex(self, Index, Value)
                end
            )
        end
    end,
})

PLAYERLOLBRODUDEWHAT:AddToggle('NoJUmpCD', {
    Text = 'no slowdown',
    Default = false,
    Callback = function(val)
        Settings.Misc.Random.NoSlow = val
    end,
})

PLAYERLOLBRODUDEWHAT:AddToggle('SpeedToggle', {
    Text = 'anti void kill',
    Default = false,
    Callback = function(Value)
        workspace.FallenPartsDestroyHeight = Value and -50000 or -500
    end,
})

PLAYERLOLBRODUDEWHAT:AddToggle('AutoStompToggle', {
    Text = 'auto stomp',
    Default = false,
    Callback = function(v)
        if v then
            game.ReplicatedStorage.MainEvent:FireServer('Stomp')
        end
    end,
})

PLAYERLOLBRODUDEWHAT:AddToggle('AutoDropToggle', {
    Text = 'auto drop cash',
    Default = false,
    Callback = function(v)
        if v then
            game.ReplicatedStorage.MainEvent:FireServer(
                'DropMoney',
                Settings.Misc.Cash.Amount or 8000
            )
        end
    end,
})

PLAYERLOLBRODUDEWHAT:AddToggle('AutoPickToggle', {
    Text = 'auto pickup cash',
    Default = false,
    Callback = function(v)
        if v then
            pcall(function()
                for _, drop in pairs(Workspace.Ignored.Drop:GetChildren()) do
                    if drop.Name == 'MoneyDrop' then
                        local dist = (
                            drop.Position
                            - Client.Character.HumanoidRootPart.Position
                        ).Magnitude
                        if dist < 25 then
                            fireclickdetector(
                                drop:FindFirstChildOfClass('ClickDetector')
                            )
                        end
                    end
                end
            end)
        end
    end,
})

PLAYERLOLBRODUDEWHAT:AddSlider('AutoDropAmount', {
    Text = 'drop amount',
    Default = 7000,
    Min = 1,
    Max = 10000,
    Rounding = 0,
    Compact = true,
    Callback = function(v)
        Settings.Misc.Cash.Amount = v
    end,
})

Settings = { -- temp table
    Misc = {
        Random = {
            AutoStomp = true,
            AntiBag = true,
            NoJumpCooldown = false,
            NoSlow = false,
            NoRecoil = false,
        },
        Cash = {
            AutoDrop = true,
            Amount = 8000,
            AutoPick = true,
        },
    },
}

local Client = game:GetService('Players').LocalPlayer

local PurchaseShit12345 = Tabs.Misc:AddLeftGroupbox("locations", "pin")

local CFrameValues = {
    ['Admin Base'] = CFrame.new(-874.903992, -32.6492004, -525.215698),
    ['High Medium Armor'] = CFrame.new(
        -934.73651123047,
        -28.492471694946,
        565.99884033203
    ),
    ['Food'] = CFrame.new(-788.39318847656, -39.649200439453, -935.27795410156),
    ['Gas Station'] = CFrame.new(
        608.599426,
        65.3087997,
        -267.643066,
        -0.414288431,
        -1.04483455e-09,
        -0.91014564,
        -1.30518893e-08,
        1,
        4.79309215e-09,
        0.91014564,
        1.38648408e-08,
        -0.41428
    ),
    ['School'] = CFrame.new(
        -581.790283,
        68.4947281,
        331.046448,
        0.220051467,
        -7.56681329e-05,
        0.975498199,
        -3.96428077e-05,
        0.999999583,
        8.65130132e-05,
        -0.975498199,
        -5.77078645e-05,
        0.22005
    ),
    ['Military'] = CFrame.new(
        92.643799,
        122.749977,
        -860.128784,
        0.986730993,
        5.09704545e-09,
        0.162363499,
        -9.24942123e-10,
        1,
        -2.57716568e-08,
        -0.162363499,
        2.52795154e-08,
        0.986730993
    ),
    ['Ufo'] = CFrame.new(
        65.1504517,
        138.999954,
        -691.819031,
        -0.935115993,
        -5.9791418e-08,
        -0.354341775,
        -3.10840989e-08,
        1,
        -8.67077574e-08,
        0.354341775,
        -7.0067415e-08,
        -0.935115993
    ),
    ['Bank'] = CFrame.new(
        -374.538391,
        102.052887,
        -440.20871,
        0.958144963,
        9.24065989e-06,
        -0.286283433,
        -9.98981818e-07,
        1,
        2.89345699e-05,
        0.286283433,
        -2.74375216e-05,
        0.958144963
    ),
    ['Gym Top'] = CFrame.new(
        -76.178093,
        56.6998138,
        -629.940979,
        -0.9998914,
        -1.09370752e-07,
        0.0147391548,
        -1.0945012e-07,
        1,
        -4.57786342e-09,
        -0.0147391548,
        -6.1905685e-09,
        -0.9998914
    ),
    ['Casino'] = CFrame.new(
        -1048.95093,
        110.254997,
        -154.554016,
        0.198458344,
        0.0412604436,
        -0.979240835,
        -4.06676299e-05,
        0.999113858,
        0.0420895219,
        0.98010987,
        -0.00831318926,
        0.198284075
    ),
    ['Uphill'] = CFrame.new(
        485.651947,
        112.5,
        -644.316833,
        -0.998899043,
        1.33881997e-06,
        0.0469136797,
        8.00526664e-07,
        1,
        -1.14929126e-05,
        -0.0469136797,
        -1.14426994e-05,
        -0.998899043
    ),
    ['Revolver'] = CFrame.new(
        -659.053162,
        110.748001,
        -158.224365,
        0.146754071,
        -2.38941595e-08,
        -0.989172995,
        -1.60316838e-09,
        1,
        -2.43935396e-08,
        0.989172995,
        5.16566212e-09,
        0.146754071
    ),
    ['Flank'] = CFrame.new(
        376.730621,
        130.748001,
        -245.620468,
        0.996583343,
        5.90310174e-06,
        -0.0825867951,
        -1.72590728e-06,
        1,
        5.06508768e-05,
        0.0825867951,
        -5.03353003e-05,
        0.996583343
    ),
    ['PlayGround'] = CFrame.new(
        -260.836182,
        126.424866,
        -877.783875,
        -0.977067351,
        -1.56508904e-05,
        -0.212922528,
        9.92513264e-07,
        1,
        -7.80593255e-05,
        0.212922528,
        -7.64806027e-05,
        -0.977067351
    ),
}

local options = {}
for name in pairs(CFrameValues) do
    table.insert(options, name)
end
table.sort(options)

PurchaseShit12345:AddDropdown('TeleportDropdown', {
    Text = 'choose location',
    Values = options,
    Default = options[1],
    Callback = function(selected)
        if
            Client.Character
            and Client.Character:FindFirstChild('HumanoidRootPart')
        then
            local cframe = CFrameValues[selected]
            if cframe then
                Client.Character.HumanoidRootPart.CFrame = cframe
            end
        end
    end,
})

local MenuGroup = Tabs['UI Settings']:AddLeftGroupbox('Menu', 'wrench')

MenuGroup:AddToggle('KeybindMenuOpen', {
    Default = Library.KeybindFrame.Visible,
    Text = 'Open Keybind Menu',
    Callback = function(value)
        Library.KeybindFrame.Visible = value
    end,
})
MenuGroup:AddToggle('ShowCustomCursor', {
    Text = 'Custom Cursor',
    Default = true,
    Callback = function(Value)
        Library.ShowCustomCursor = Value
    end,
})
MenuGroup:AddDropdown('NotificationSide', {
    Values = { 'Left', 'Right' },
    Default = 'Right',

    Text = 'Notification Side',

    Callback = function(Value)
        Library:SetNotifySide(Value)
    end,
})
MenuGroup:AddDropdown('DPIDropdown', {
    Values = { '50%', '75%', '100%', '125%', '150%', '175%', '200%' },
    Default = '100%',

    Text = 'DPI Scale',

    Callback = function(Value)
        Value = Value:gsub('%%', '')
        local DPI = tonumber(Value)

        Library:SetDPIScale(DPI)
    end,
})
MenuGroup:AddDivider()
MenuGroup:AddLabel('Menu bind'):AddKeyPicker(
    'MenuKeybind',
    { Default = 'RightShift', NoUI = true, Text = 'Menu keybind' }
)

MenuGroup:AddButton('Unload', function()
    Library:Unload()
end)

Library.ToggleKeybind = Options.MenuKeybind -- Allows you to have a custom keybind for the menu

-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- ThemeManager (Allows you to have a menu theme system)

-- Hand the library over to our managers
ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- Adds our MenuKeybind to the ignore list
-- (do you want each config to have a different menu key? probably not.)
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
ThemeManager:SetFolder('Sapphire')
SaveManager:SetFolder('Sapphire/Universal')
SaveManager:SetSubFolder('specific-place') -- if the game has multiple places inside of it (for example: DOORS)
-- you can use this to save configs for those places separately
-- The path in this script would be: MyScriptHub/specific-game/settings/specific-place
-- [ This is optional ]

-- Builds our config menu on the right side of our tab
SaveManager:BuildConfigSection(Tabs['UI Settings'])

-- Builds our theme menu (with plenty of built in themes) on the left side
-- NOTE: you can also call ThemeManager:ApplyToGroupbox to add it to a specific groupbox
ThemeManager:ApplyToTab(Tabs['UI Settings'])

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()
