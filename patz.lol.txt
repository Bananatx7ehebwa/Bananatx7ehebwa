
if game.PlaceId == 9825515356 then

local a = game:GetService("ReplicatedStorage"):WaitForChild("MainEvent")
local b = {"IS_MOBILE"}
local c
if a and a:IsA("RemoteEvent") then
    c = hookmetamethod(game, "__namecall", function(self, ...)
        local method = getnamecallmethod()
        local args = {...}
        if method == "FireServer" and self == a and table.find(b, args[1]) then
            return
        end
        return c(self, ...)
    end)
end
end

for Key, Object in pairs(getgc(true)) do
            if type(Object) == "table" then
                setreadonly(Object, false)
                local indexInstance = rawget(Object, "indexInstance")
                if type(indexInstance) == "table" and indexInstance[1] == "kick" then
                    setreadonly(indexInstance, false)
                    rawset(Object, "Table", {"kick", function() coroutine.yield() end})
                break
           end
      end
end

local bullet_trailin = false;

local Players           = game:GetService("Players");
local RunService        = game:GetService("RunService");
local UserInputService  = game:GetService("UserInputService");
local ReplicatedStorage = game:GetService("ReplicatedStorage");
local Workspace         = game:GetService("Workspace");
local TweenService      = game:GetService("TweenService");
local Configurations    = getgenv().Configurations -- you can just do Configurations but i defined it since i dont want that yellow indicator on my roblox lsp
local Debris            = game:GetService('Debris');
local Lighting       = game:GetService("Lighting");
local utility = {};

local LocalPlayer       = Players.LocalPlayer
local Camera = Workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()
local crosshair_position = "Middle";
local clone_chams_tick = tick();
local connections = {}
if not LPH_OBFUSCATED then
    LPH_JIT = function(...) return ... end
    LPH_JIT_MAX = function(...) return ... end
    LPH_JIT_ULTRA = function(...) return ... end
    LPH_NO_VIRTUALIZE = function(...) return ... end
    LPH_NO_UPVALUES = function(f) return function(...) return f(...) end end
    LPH_ENCSTR = function(...) return ... end
    LPH_STRENC = function(...) return ... end
    LPH_HOOK_FIX = function(...) return ... end
    LPH_CRASH = function() return print(debug.traceback()) end
end

local wrap = LPH_NO_VIRTUALIZE(function(f)
    coroutine.resume(coroutine.create(f))
end)

local function GetBullet()
    if workspace:FindFirstChild("Ignored") and workspace.Ignored:FindFirstChild("Siren") and workspace.Ignored.Siren:FindFirstChild("Radius") then
        return {
            BulletPath = workspace.Ignored.Siren.Radius,
            BulletName = "BULLET_RAYS",
            BulletBeamName = "GunBeam"
        }
    elseif workspace:FindFirstChild("Ignored") then
        return {
            BulletPath = workspace.Ignored,
            BulletName = "BULLET_RAYS",
            BulletBeamName = "GunBeam"
        }
    elseif workspace then
        return {
            BulletPath = workspace,
            BulletName = "Part",
            BulletBeamName = "gb"
        }
    end
    return nil
end

local support = GetBullet()
local bullet_beam_name = support.BulletBeamName
local bullet_name = support.BulletName
local bullet_path = support.BulletPath

local Library = loadstring(game:HttpGet("https://gist.githubusercontent.com/public-account-7/4098a248a7fcd0ad67e45c75671b409e/raw/50e0d671122a0541276a92f4bf31221dff2f11f9/gistfile1.txt",true))()

local MainColor = Library.Accent

Configurations = {
    Visuals = {
        Bullet_Trails = {
            Enabled = false,
            Width = 1.7,
            Duration = 5,
            Fade = false,
            FadeDuration = 5,
            Color = MainColor,
            Texture = "Normal" -- 12781803086
        }
    }
}


local utility = {}; do
    utility.world_to_screen = LPH_NO_VIRTUALIZE(function(position)
        local screen_position, on_screen = Camera:WorldToViewportPoint(position)
        return {position = Vector2.new(screen_position.X, screen_position.Y), on_screen = on_screen}
    end)
utility.instance_new = function(type, properties)
        local instance = Instance.new(type);

        for property, value in properties do
            instance[property] = value;
        end;

        return instance;
    end;

    utility.has_character = LPH_NO_VIRTUALIZE(function(player)
        return (player and player.Character and player.Character:FindFirstChild("Humanoid")) and true or false
    end)

    utility.new_connection = function(type, callback) --// by all means do NOT virtualize this
        local connection = type:Connect(callback)
        table.insert(connections, connection)
        return connection
    end

    utility.create_beam = LPH_NO_VIRTUALIZE(function(from, to, color_1, color_2, duration, fade_enabled, fade_duration)
        local tween
        local total_time = 0

        local main_part = utility.instance_new("Part", {
            Parent = workspace,
            Size = Vector3.new(0, 0, 0),
            Massless = true,
            Transparency = 1,
            CanCollide = false,
            Position = from,
            Anchored = true
        })

        local part0 = utility.instance_new("Part", {
            Parent = main_part,
            Size = Vector3.new(0, 0, 0),
            Massless = true,
            Transparency = 1,
            CanCollide = false,
            Position = from,
            Anchored = true
        })

        local part1 = utility.instance_new("Part", {
            Parent = main_part,
            Size = Vector3.new(0, 0, 0),
            Massless = true,
            Transparency = 1,
            CanCollide = false,
            Position = to,
            Anchored = true
        })

        local attachment0 = utility.instance_new("Attachment", {Parent = part0})
        local attachment1 = utility.instance_new("Attachment", {Parent = part1})

        local beam = utility.instance_new("Beam", {
            Texture = "rbxassetid://446111271",
            TextureMode = Enum.TextureMode.Wrap,
            TextureLength = 10,
            LightEmission = 1,
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
            Attachment0 = attachment0,
            Attachment1 = attachment1,
            Enabled = true,
            Parent = main_part
        })

        if fade_enabled then
            tween = utility.new_connection(RunService.Heartbeat, function(delta_time) --// credits to Xander
                total_time += delta_time
                beam.Transparency = NumberSequence.new(
                    TweenService:GetValue((total_time / fade_duration), Enum.EasingStyle.Quad, Enum.EasingDirection.In)
                )
            end)
        end

        task.delay(duration, function()
            main_part:Destroy()
            if tween then
                tween:Disconnect()
            end
        end)
    end)
end

local Notifications = {}
local Utility = {}
local Desync = {}

local function SirenAdded(Obj)
        local Character = LocalPlayer.Character
        local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")

        local function VerifyBullet(obj)
            return (obj.Name == bullet_name or obj:FindFirstChild("Attachment") or obj:FindFirstChild(bullet_beam_name)) and obj
        end

        local PlayerChecks = {PlayerGun = false}
        local BulletRay = VerifyBullet(Obj)
    
        if BulletRay and RootPart then
            local Mag = (RootPart.Position - BulletRay.Position).Magnitude
            if Mag <= 13 then PlayerChecks.PlayerGun = true end

            if PlayerChecks.PlayerGun then
                local GunBeam = BulletRay:WaitForChild(bullet_beam_name)
                
                local Attachment0 = GunBeam.Attachment0 -- closest to player
                local Attachment1 = GunBeam.Attachment1 -- mouse position


            if Configurations.Visuals.Bullet_Trails.Enabled then
                GunBeam:Destroy()
                utility.create_beam(
                    BulletRay.Position,
                    Attachment1.WorldCFrame.Position,
                    Configurations.Visuals.Bullet_Trails.Color,
                    Configurations.Visuals.Bullet_Trails.Color,
                    Configurations.Visuals.Bullet_Trails.Duration,
                    Configurations.Visuals.Bullet_Trails.Fade,
                    Configurations.Visuals.Bullet_Trails.FadeDuration
                )
            end
        end
    end
end

if bullet_path then
    bullet_path.ChildAdded:Connect(SirenAdded)
end



if not LPH_OBFUSCATED then
    LPH_JIT = function(...) return ... end
    LPH_JIT_MAX = function(...) return ... end
    LPH_JIT_ULTRA = function(...) return ... end
    LPH_NO_VIRTUALIZE = function(...) return ... end
    LPH_NO_UPVALUES = function(f) return function(...) return f(...) end end
    LPH_ENCSTR = function(...) return ... end
    LPH_STRENC = function(...) return ... end
    LPH_HOOK_FIX = function(...) return ... end
    LPH_CRASH = function() return print(debug.traceback()) end
end

local Script = {
        Functions = {},
        Folders = {},
        Parts = {},
        Locals = {
            Target = nil,
            Targeting = false,
            Resolver = {
                OldTick = tick(),
                OldPos = Vector3.new(0, 0, 0),
                ResolvedVelocity = Vector3.new(0, 0, 0)
            },
            AutoSelectTick = tick(),
            AntiAimViewer = {
                MouseRemoteFound = false,
                MouseRemote = nil,
                MouseRemoteArgs = nil,
                MouseRemotePositionIndex = nil
            },
           GunTP = { 
                Enabled = false,
                Anchor = false,
                Offset = {0,-1,0},
           },
           Aura = { 
                Enabled = true,
                Color = Color3.fromRGB(0,0,67),
           },
           RocketTP = {
                Enabled = false,
           },
           GrenadeTP = {
                Enabled = false,
           },
           KnifeAbilityTest = {
               TargetPart = "HumanoidRootPart",
               Radius = 90,
               Visible = false
            },
            HitEffect = {
                ["Nova Impact"] = nil,
                ["Crescent Slash"] = nil,
                ["Crescent Slash"] = nil,
                ["Coom"] = nil,
                ["Cosmic Explosion"] = nil,
                ["Slash"] = nil,
                ["Atomic Slash"] = nil,
            },
            Gun = {
                PreviousGun = nil,
                PreviousAmmo = 999,
                Shotguns = {"[Double-Barrel SG]", "[TacticalShotgun]", "[Shotgun]"}
            },
            PlayerHealth = {},
            JumpOffset = 0,
            BulletPath = {
                [4312377180] = Workspace:FindFirstChild("MAP") and Workspace.MAP:FindFirstChild("Ignored") or nil,
                [1008451066] = Workspace:FindFirstChild("Ignored") and Workspace.Ignored:FindFirstChild("Siren") and Workspace.Ignored.Siren:FindFirstChild("Radius") or nil,
                [3985694250] = Workspace and Workspace:FindFirstChild("Ignored") or nil,
                [5106782457] = Workspace and Workspace:FindFirstChild("Ignored") or nil,
                [4937639028] = Workspace and Workspace:FindFirstChild("Ignored") or nil,
                [1958807588] = Workspace and Workspace:FindFirstChild("Ignored") or nil
            },
            SavedCFrame = nil,
            NetworkPreviousTick = tick(),
            NetworkShouldSleep = false,
            FFlags = {
      }
            ,OriginalVelocity = {},
            RotationAngle = 0
        },
        Utility = {
            Drawings = {},
            EspCache = {}
        },
        Connections = {
            GunConnections = {}
        },
        AuraIgnoreFolder = Instance.new("Folder", game:GetService("Workspace"))
    }

    local Settings = {
        Combat = {
            Enabled = false,
            Skibidi = true,
            Spectate = true,
            AimPart = "HumanoidRootPart",
            ESP = true,
            Silent = false,
            BetaAirshot = false,
            TriggerBot = {
                Enabled = false,
                Delay = 0,
                TargeyOnly = false,
                FOV = {
                    Show = true,
                    Size = 80
                }
            },
            TargetInfo = false,
            Camera = false,
            EasingStyle = "Sine",
            EasingDirection = "Out",
            Alerts = true,
            LookAt = false,
            Spectate = false,
            PingBased = false,
            UseIndex = false,
            AntiAimViewer = false,
            AutoSelect = {
                Enabled = false,
                Cooldown = {
                    Enabled = false,
                    Amount = 0.5
                }
            },
            Checks = {
                Enabled = false,
                Knocked = false,
                Crew = false,
                Wall = false,
                Grabbed = false,
                Vehicle = false
            },
            Smoothing = {
                Horizontal = 1,
                Vertical = 1
            },
            Prediction = {
                Horizontal = 0.134,
                Vertical = 0.134
            },
            Resolver = {
                Enabled = false,
                RefreshRate = 190
            },
            Fov = {
                Visualize = {
                    Enabled = false,
                    Color = Color3.new(1, 1, 1)
                },
                Radius = 80
            },
            Visuals = {
                Enabled = true,
                Tracer = {
                    Enabled = false,
                    Color = Color3.new(1, 1, 1),
                    Thickness = 2
                },
                Dot = {
                    Enabled = false,
                    Color = Color3.new(1, 1, 1),
                    Filled = true,
                    Size = 6
                },
                Chams = {
                    Enabled = false,
                    Fill = {
                        Color = Color3.fromRGB(255,209,220),
                        Transparency = 0.5
                    },
                    Outline = {
                        Color = Color3.new(255,255,255),
                        Transparency = 0
                    }
                }
            },
            Air = {
                Enabled = true,
                AirAimPart = {
                    Enabled = false,
                    HitPart = "LowerTorso"
                },
                JumpOffset = {
                    Enabled = false,
                    Offset = 0
                }
            }
        },
        Visuals = {
            Backtrack = {
                Enabled = true,
                Color = Color3.fromRGB(255,255,255),
                Method = "Folllow",
                Transparency = 0.5,
                Material = "Plastic",
            },
            BulletTracers = {
                Enabled = false,
                Color = {
                    Gradient1 = Color3.new(1, 1, 1),
                    Gradient2 = Color3.new(0, 0, 0)
                },
                Duration = 1,
                Fade = {
                    Enabled = false,
                    Duration = 0.5
                }
            },
            BulletImpacts = {
                Enabled = false,
                Color = Color3.new(1, 1, 1),
                Duration = 1,
                Size = 1,
                Material = "SmoothPlastic",
                Fade = {
                    Enabled = false,
                    Duration = 0.5
                }
            },
            OnHit = {
                Enabled = false,
                Effect = {
                    Enabled = false,
                    Color = Color3.new(1, 1, 1)
                },
                Sound = {
                    Enabled = false,
                    Volume = 5,
                    Value = "hentai4.wav"
                },
                Chams = {
                    Enabled = false,
                    Color = Color3.fromRGB(255,209,220),
                    Material = "ForceField",
                    Duration = 1
                }
            },
            World = {
                Enabled = false,
                Fog = {
                    Enabled = false,
                    Color = Color3.fromRGB(255,209,220),
                    End = 1000,
                    Start = 10000
                },
                Ambient = {
                    Enabled = false,
                    Color = Color3.fromRGB(255,209,220)
                },
                Brightness = {
                    Enabled = false,
                    Value = 0
                },
                ClockTime = {
                    Enabled = false,
                    Value = 24
                },
                WorldExposure = {
                    Enabled = false,
                    Value = -0.1
                }
            },
            Crosshair = {
                Enabled = false,
                StickToTarget = false,
                Color = Color3.new(1, 1, 1),
                Size = 10,
                Gap = 2,
                Rotation = {
                    Enabled = false,
                    Speed = 1
                }
            }
        },
        AntiAim = {
            DaCoolBoyDesync = false,
            DaCoolBoyDesync2 = false,
            DaCoolBoyDesync3 = false,
            VelocitySpoofer = {
                Enabled = false,
                Visualize = {
                    Enabled = false,
                    Color = Color3.fromRGB(255,209,220),
                    Prediction = 0.134
                },
                Type = "Underground",
                Roll = 0,
                Pitch = 0,
                Yaw = 0
            },
            CSync = {
                Enabled = false,
                Spoof = false,
                Type = "Target Strafe",
                Visualize = {
                    Enabled = false,
                    Color = Color3.fromRGB(255,209,220)
                },
                RandomDistance = 10,
                Custom = {
                    X = 0,
                    Y = 0,
                    Z = 0
                },
                TargetStrafe = {
                    Speed = 1,
                    Distance = 1,
                    Height = 1
                }
            },
            Network = {
                Enabled = false,
                WalkingCheck = false,
                Amount = 0.05
            },
            VelocityDesync = {
                Enabled = false,
                Range = 1
            },
            FFlagDesync = {
                Enabled = false,
                SetNew = false,
                FFlags = {"S2PhysicsSenderRate"}, 
                SetNewAmount = 1,
                Amount = 1
            },
        },
        Misc = {
            Movement = {
                Macro = {
                    Enabled = false,
                    Speed = 0.1,
                },
                Speed = {
                    Enabled = false,
                    Amount = 1
                },
            },
            Exploits = {
                Enabled = false,
                NoRecoil = false,
                NoJumpCooldown = false,
                NoSlowDown = false
            }
        }
    }



getgenv().Sentinel = {
    Enabled = true,
    HorizontalPrediction = 0.1,
    VerticalPrediction = 0.1,
    jumpoffset2 = -1,
    jumpoffset = 0,
    ResolverEnabled = false,
    SelectedPart = "HumanoidRootPart",
    CamPredHori = 0.1,
    CamPredVert = 0.4,
    AutoPrediction = true,
    Sway = false,
    AutoPredMode = "PingBased", 
    ShootDelay = 0.22,
    NoGroundShot = true,
    AutoAir = true,
    ForceFieldCheck = true,
    LookAt = true,
    smoothness = 0.3,
    TracerEnabled = true,
    NearestPart = false,
    speedvalue = 1,
    MacroSpeed = 0.1,
    Camera = false,
    easingStyle = "Sine",
    easingDirection = "Out",
    JumpBreak = false,
    network = false
}




local GrenadeTP = false
local RocketTP = false
getgenv().Desync = false
getgenv().AntiLockType = "Behind"
getgenv().Direction = Vector3.new(0, 0, -1)

    local Debris = game:GetService('Debris')
    local EtherealParts = Instance.new('Folder', workspace)
    EtherealParts.Name  = 'EtherealParts'
    local UserInputService = game:GetService("UserInputService")
    local Workspace = game:GetService("Workspace")
    local Lighting = game:GetService("Lighting")
    local RunService = game:GetService("RunService")
    local TweenService = game:GetService("TweenService")
    local ReplicatedStorage = game:GetService("ReplicatedStorage")


Script.Functions.VisualizeMovement = function()
                if Settings.Combat.Skibidi then
                  local Character = game.Players.LocalPlayer and (game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait())
                  local RootPart = Character and Character.HumanoidRootPart
                  local Ball = Instance.new('Part') do
                      Ball.Anchored = true
                      Ball.Size = Vector3.new(0.5, 0.5, 0.5)
                      Ball.Transparency = -0.5
                      Ball.Shape = Enum.PartType.Ball
                      Ball.Color = Color3.fromRGB(255,209,220)
                      Ball.Material = Enum.Material.ForceField
                      Ball.Parent = Workspace
                      Ball.CFrame = RootPart.CFrame
                      Ball.CanCollide = false
                      local highlight = Instance.new("Highlight")
                      highlight.Adornee = Ball
                      highlight.FillColor = Color3.fromRGB(255,209,220)
                      highlight.OutlineColor = Color3.fromRGB(255,255,255)
                      highlight.Parent = Ball
                  end;
                  Debris:AddItem(Ball, 2)
                end
            end

Script.Functions.VisualizeMovement()






    print("OpenAc Flagged & Bypassed!, Loading Script!")
       


local Psalms = {
    Tech = {
        speedvalue = 1
    }
}







 


        
    



--[[ http://www.roblox.com/asset/?id=17677463033
Ak47 : rbxassetid://88836741434398

REV : rbxassetid://106397886212631

SG : 118698023926650

Snipah : 104037274406938

138737595020251

113531571097970

]]


local hitsounds = {
    ["RIFK7"] = "rbxassetid://9102080552",
    ["Bubble"] = "rbxassetid://9102092728",
    ["Minecraft"] = "rbxassetid://5869422451",
    ["Cod"] = "rbxassetid://160432334",
    ["Bameware"] = "rbxassetid://6565367558",
    ["Neverlose"] = "rbxassetid://6565370984",
    ["Gamesense"] = "rbxassetid://4817809188",
    ["Rust"] = "rbxassetid://6565371338",
    ["BlackPencil"] = "https://github.com/Shatapmatehabibi/Hitsounds/raw/main/bananapencil.mp3.mp3",
    ["Fein"] = "https://github.com/CongoOhioDog/SoundS/blob/main/psalms%20highly%20defined%20fein.mp3?raw=true",
    ["Unicorn"] = "https://github.com/CongoOhioDog/SoundS/blob/main/shiny%20unicorn%20for%20dh%20_%20psalms.mp3?raw=true",
    ["Kitty"] = "https://github.com/CongoOhioDog/SoundS/blob/main/Kitty.mp3?raw=true",
    ["Bird"] = "https://github.com/CongoOhioDog/SoundS/blob/main/bird%20chirping%20for%20DH%20_%20psalms%20audio.mp3?raw=true",
    ["BirthdayCake"] = "https://github.com/CongoOhioDog/SoundS/blob/main/Birthday%20cake%20for%20dh%20_%20psalms.mp3?raw=true", 
    ["KenCarson"] =  "https://github.com/CongoOhioDog/SoundS/blob/main/ken_carson_-_jennifer_s_body_offici(2).mp3?raw=true"
}



local TargetAimbot = {
    Enabled = true, 
    Keybind = Enum.KeyCode.Q,
    Autoselect = false,
    Prediction = 0.145, 
    RealPrediction = 0.145, 
    Resolver = false, 
    ResolverType = "Recalculate", 
    JumpOffset = 0.06, 
    RealJumpOffset = 0.09, 
    HitParts = {"HumanoidRootPart"}, 
    RealHitPart = "HumanoidRootPart", 
    KoCheck = false, 
    LookAt = false, --
    CSync = {
        Enabled = false,
        Type = "Orbit",
        Distance = 10,
        Height = 2,
        Speed = 10,
        Circle = true,
        VisualizeMovement = false,
        RandomAmount = 10,
        Color = Color3.fromRGB(255, 255, 255),
        Spectate = false,
    },
    ViewAt = false,
    Tracer = true,
    Highlight = true,
    HighlightColor1 =Color3.fromRGB(255, 255, 255),
    HighlightColor2 =Color3.fromRGB(255, 255, 255),
    Stats = false, 
    UseFov = false,
    HitEffect = true,
    HitEffectType = "Coom", -- Nova, Crescent Slash, Coom, Cosmic Explosion, Slash, Atomic Slash
    HitEffectColor = Color3.fromRGB(255, 255, 255),
    HitSounds = true,
    HitSound = "Bameware",
    HitChams = true,
    HitSkele = true,
    SkeleColor = Library.Accent,
    HitChamsMaterial = Enum.Material.Neon,
    HitChamsDuration = 1,
    HitChamsColor = Color3.fromRGB(173, 216, 230)
}

local  Highlight = false

local ESP = {
    Enabled = false,
    TeamCheck = true,
    MaxDistance = 200,
    FontSize = 11,
    TargetOnly = false,
    FadeOut = {
        OnDistance = true,
        OnDeath = false,
        OnLeave = false,
    },
    Options = { 
        Teamcheck = false, TeamcheckRGB = Color3.fromRGB(0, 255, 0),
        Friendcheck = true, FriendcheckRGB = Color3.fromRGB(0, 255, 0),
        Highlight = false, HighlightRGB = Color3.fromRGB(255, 0, 0),
    },
    Drawing = {
        Chams = {
            Enabled  = true,
            Thermal = true,
            FillRGB = Color3.fromRGB(173, 216, 230),
            Fill_Transparency = 100,
            OutlineRGB = Color3.fromRGB(173, 216, 230),
            Outline_Transparency = 100,
            VisibleCheck = true,
        },
        Names = {
            Enabled = true,
            RGB = Color3.fromRGB(255, 255, 255),
        },
        Flags = {
            Enabled = true,
        },
        Distances = {
            Enabled = true, 
            Position = "Text",
            RGB = Color3.fromRGB(255, 255, 255),
        },
        Weapons = {
            Enabled = true, WeaponTextRGB = Color3.fromRGB(119, 120, 255),
            Outlined = false,
            Gradient = false,
            GradientRGB1 = Color3.fromRGB(255, 255, 255), GradientRGB2 = Color3.fromRGB(119, 120, 255),
        },
        Healthbar = {
            Enabled = true,  
            HealthText = true, Lerp = false, HealthTextRGB = Color3.fromRGB(119, 120, 255),
            Width = 2.5,
            Gradient = true, GradientRGB1 = Color3.fromRGB(p, 0, 0), GradientRGB2 = Color3.fromRGB(60, 60, 125), GradientRGB3 = Color3.fromRGB(173, 216, 230), 
        },
        Boxes = {
            Animate = true,
            RotationSpeed = 300,
            Gradient = false, GradientRGB1 = Color3.fromRGB(173, 216, 230), GradientRGB2 = Color3.fromRGB(0, 0, 0), 
            GradientFill = true, GradientFillRGB1 = Color3.fromRGB(119, 120, 255), GradientFillRGB2 = Color3.fromRGB(0, 0, 0), 
            Filled = {
                Enabled = true,
                Transparency = 0.75,
                RGB = Color3.fromRGB(0, 0, 0),
            },
            Full = {
                Enabled = true,
                RGB = Color3.fromRGB(255, 255, 255),
            },
            Corner = {
                Enabled = true,
                RGB = Color3.fromRGB(255, 255, 255),
            },
        };
    };
    Connections = {
        RunService = RunService;
    };
    Fonts = {};
}




local player = game:GetService("Players").LocalPlayer
local display = player.DisplayName
local Window = Library:Window({Name = "Patz.lol | đŸ„ Da Hood | " .. display})

--
local plyaha = game.Players.LocalPlayer.Name
local dick2 = string.sub(tostring(math.random()), 3, 8) 
local what = {"Santa", "The Grinch", "Ozzy"}
local christmasCharacter = what[math.random(#what)]
--
local Watermark = Library:Watermark({Name = string.format("$$ osamason $$ | %s | %s | %s | Christmas Cheer | Jingle Bells | Snowflakes | Holiday Vibes | gyatt", plyaha, dick2, christmasCharacter)})
--
local Indicator = Library:Indicator({Name = "Target Indicator"})
local TextIndicator = Indicator:NewValue({Name = "Target:", Value = "No Target"})
local BarIndicator = Indicator:NewBar({Name = "Health", Value = 0, Min = 0, Max = 100})
--


local Page = Window:Page({Name = "Main", Weapons = true})
local Visuals = Window:Page({Name = "Visuals", Weapons = true})
local Sat = Window:Page({Name = "Rage", Weapons = true})
local playahlis = Window:Page({Name = "Playerlist", Weapons = true})
local Settings = Window:Page({Name = "Settings"})
--
local SubPage1 = Page:Weapon({Icon = "rbxassetid://78117064682304"})
local SubPage2 = Page:Weapon({Icon = "rbxassetid://120304994615919"})

local SubPage3 = Page:Weapon({Icon = "rbxassetid://77469221937135"})
local SubPage4 = Page:Weapon({Icon = "rbxassetid://128385756502234"})

local SubPage5 = playahlis:Weapon({Icon = "rbxassetid://114736089502081"})

local SubPage6 = Sat:Weapon({Icon = "rbxassetid://8547258459"})

local SubPage7 = Visuals:Weapon({})
local SubPage8 = Visuals:Weapon({Icon = "rbxassetid://16997762295"})

-- // TargetPlayers:PlayerList({})

SubPage5:PlayerList({Flag = "CurrentPlayer"})
local PlayerlistONE = SubPage5:Section({Name = ">_<", Zindex = 2, Side = "Right"})
PlayerlistONE:Button({Name = "Goto", Callback = function()
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = Library.Flags.CurrentPlayer.Character.HumanoidRootPart.CFrame 
end})

local VisSectiom = 
SubPage8:Section({Name = "Hit Detection"})

local Others = SubPage8:Section({Name = "Others"})

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local gradient = Instance.new("UIGradient")
local UICorner = Instance.new("UICorner")
local UIStroke = Instance.new("UIStroke")

ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local sama_sama_sama = true
local whattheskibidi = 0.6
local mode = "Center"
local coluhhh = Color3.fromRGB(255, 255, 255)
local coluhhh2 = Color3.fromRGB(0,54,0)
local HuhDrawing = 200

Frame.Parent = ScreenGui
Frame.Visible = sama_sama_sama
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BackgroundTransparency = whattheskibidi
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0

UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = Frame

UIStroke.Thickness = 1
UIStroke.Color = Color3.fromRGB(255, 255, 255)
UIStroke.Parent = Frame

gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, coluhhh),
    ColorSequenceKeypoint.new(1, coluhhh2)
}
gradient.Parent = Frame

local function ChangeFOVFrickinSizees(HuhDrawing)
    Frame.Size = UDim2.new(0, HuhDrawing, 0, HuhDrawing)
    Frame.Position = UDim2.new(0.5, -Frame.Size.X.Offset / 2, 0.5, -Frame.Size.Y.Offset / 2)
end

ChangeFOVFrickinSizees(200)

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local camera = game.Workspace.CurrentCamera

local stick_to_target = false


mouse.Move:Connect(function()
    if mouse and mode == "Mouse" then
        Frame.Position = UDim2.new(0, mouse.X - Frame.Size.X.Offset / 2, 0, mouse.Y - Frame.Size.Y.Offset / 2)
    elseif stick_to_target == false then
            Frame.Position = UDim2.new(0.5, -Frame.Size.X.Offset / 2, 0.5, -Frame.Size.Y.Offset / 2)
        end
end)

local RotateThisDick = 3

spawn(function()
    while true do
        gradient.Rotation = gradient.Rotation + RotateThisDick
        wait(0.01)
    end
end)

Others:Toggle({Name = "Gradient FOV", Default = sama_sama_sama, Callback = function(a) Frame.Visible = a end})

local bullet_tracers = SubPage8:Section({Name = "Bullet Tracers", Side = "Right"}) do 
    bullet_tracers:Toggle({Name = "Enabled", Flag = "Bullet Tracers", Callback = function(a) Configurations.Visuals.Bullet_Trails.Enabled = a end}):Colorpicker({Default = Library.Accent, Flag = "Bullet Tracers Color", Callback = function(a) Configurations.Visuals.Bullet_Trails.Color = a end})
    bullet_tracers:List({Name = "Type", Flag = "Bullet Tracers Type", Options = {"Drawing", "Beam"}, Default = "Beam"})
    bullet_tracers:Slider({Name = "Thickness", Flag = 'Bullet Tracers Thickness', Min = 0, Max = 5, Default = 2, Decimals = 0.1, Callback = function(a) Configurations.Visuals.Bullet_Trails.Width = a end})
    bullet_tracers:Slider({Name = "Duration", Flag = 'Tracers Life Time', Min = 0, Max = 10, Default = 2, Decimals = 0.1, Callback = function(a) Configurations.Visuals.Bullet_Trails.Duration = a end})
    bullet_tracers:Toggle({Name = "Fade", Flag = "Bullet Fade", Callback = function(a) Configurations.Visuals.Bullet_Trails.Fade = a end})
    bullet_tracers:Slider({Name = "Fade Duration", Flag = 'Tracers Fade Life Time', Min = 0, Max = 10, Default = 2, Decimals = 0.1, Callback = function(a) Configurations.Visuals.Bullet_Trails.FadeDuration = a end})
end

local world_visuals = Visuals:Section({Name = "World", Side = "Right"}) do
        world_visuals:Toggle({Name = "Fog", Flag = "visuals_world_fog"}):Colorpicker({Name = "Color", Flag = "visuals_world_fog_color", Default = Library.Accent})
        world_visuals:Slider({Name = "Start", Flag = "visuals_world_fog_start", Default = 50, Minimum = 1, Maximum = 1000, Decimals = 1, Ending = "%"})
        world_visuals:Slider({Name = "End", Flag = "visuals_world_fog_end", Default = 50, Minimum = 1, Maximum = 1000, Decimals = 1, Ending = "%"})
        
        world_visuals:Toggle({Name = "Ambient", Flag = "visuals_world_ambient"}):Colorpicker({Name = "Color", Flag = "visuals_world_ambient_color", Default = Library.Accent})
        
        world_visuals:Toggle({Name = "Clock Time", Flag = "visuals_world_clock_time"})
        world_visuals:Slider({Name = "Time", Flag = "visuals_world_clock_time_time", Default = 14, Minimum = 0, Maximum = 24, Decimals = 1})
        
        
        world_visuals:Toggle({Name = "Color Shift Top", Flag = "visuals_world_color_shift_top"}):Colorpicker({Name = "Color", Flag = "visuals_world_color_shift_top_color", Default = Library.Accent})
        world_visuals:Toggle({Name = "Color Shift Bottom", Flag = "visuals_world_color_shift_bottom"}):Colorpicker({Name = "Color", Flag = "visuals_world_color_shift_bottom_color", Default = Library.Accent})
    end
local HitEffectModule = {
    Locals = {
        Type = {
            ["Nova"] = nil,
            ["Crescent Slash"] = nil,
            ["Coom"] = nil,
            ["Cosmic Explosion"] = nil,
            ["Slash"] = nil,
            ["Atomic Slash"] = nil,
        },
    },
    Functions = {},
    Settings = {HitEffect = {Color = TargetAimbot.HitEffectColor}}
}


VisSectiom:Toggle({
    Name = "Enabled",
    Callback = function(a)
  TargetAimbot.HitEffect = a
    end
})

VisSectiom:Toggle({
    Name = "Hit Skeleton",
    Callback = function(a)
  TargetAimbot.HitSkele = a
    end
})

VisSectiom:Toggle({
    Name = "Hit Chams",
    Callback = function(a)
     TargetAimbot.HitChams = a
    end
})



local HitEffectXD = {"Nova", "Crescent Slash", "Coom", "Cosmic Explosion", "Slash", "Atomic Slash"}

VisSectiom:List({
    Name = "Effect Type",
    Options = HitEffectXD,
    Default = TargetAimbot.HitEffectType,
    Callback = function(a)
        TargetAimbot.HitEffectType = a
    end
})

VisSectiom:Toggle({
    Name = "Hit Sounds",
    Callback = function(a)
   TargetAimbot.HitSounds = a
    end
})


local hitdoundz = {
    "RIFK7",
    "Bubble",
    "Minecraft",
    "Cod",
    "Bameware",
    "Neverlose",
    "Gamesense",
    "Rust",
    "BlackPencil",
    "Fein",
    "Unicorn",
    "Bird",
    "KenCarson"
}

VisSectiom:List({
    Name = "Sound Type",
    Options = hitdoundz,
    Default = TargetAimbot.HitSound,
    Callback = function(a)
        TargetAimbot.HitSound = a
    end
})

local TargetAimSection = SubPage1:Section({Name = "Silent/Target", Side = "Left"})

local CamlockSection = SubPage2:Section({Name = "Aim Assist", Side = "Left"})

local RoboticShit = SubPage2:Section({Name = "Robotic Movement", Side = "Right"})

local CamHitPart = SubPage2:Section({Name = "Hit Part", Side = "Left"})


CamlockSection:Toggle({
    Name = "Enabled",
    Default = getgenv().Sentinel.Camera,
    Callback = function(a)
  getgenv().Sentinel.Camera = a
    end
})
CamlockSection:Toggle({
   Name = "Sway -- Left Or Right",
   Callback = function(a)
     Sentinel.Sway = a
   end
})

CamlockSection:Textbox({
   Name = "Prediction",
   Default = "0.134",
   Callback = function(a)
     Sentinel.CamPredHori = a
   end
})


CamlockSection:Textbox({
   Name = "Vertical Prediction",
   Default = "0.134",
   Callback = function(a)
     Sentinel.CamPredVert = a
   end
})


CamHitPart:List({
   Name = "Part",
   Options = {
        "Head", "UpperTorso", "LowerTorso", "HumanoidRootPart", 
        "LeftUpperArm", "LeftLowerArm", "LeftHand", 
        "RightUpperArm", "RightLowerArm", "RightHand", 
        "LeftUpperLeg", "LeftLowerLeg", "LeftFoot", 
        "RightUpperLeg", "RightLowerLeg", "RightFoot"
   },
   Default = "Head"
})

RoboticShit:Toggle({
    Name = "Enabled",
    Default = getgenv().Sentinel.Camera
})
RoboticShit:Textbox({
    Name = "Amount",
    Default = "1",
    Callback = function(a)
      Sentinel.smoothness = a
   end
})

RoboticShit:List({
   Name = "Easing Style",
   Options = 
   {"Linear",
  "Sine","Cubical"},
   Default = "Linear",
   Callback = function(a)
      easingstyle = a 
   end
})

RoboticShit:List({
   Name = "Easing Direction",
   Options = {"InOut","In","Out"},
   Default = "InOut",
   Callback = function(a)
      easingDirection = a
   end
})


     

TargetAimSection:Toggle({
    Name = "Enabled",
    Callback = function(a)
  getgenv().Sentinel.Enabled = a
    end
})

TargetAimSection:Toggle({
    Name = "Auto Air",
    Callback = function(a)
  getgenv().Sentinel.AutoAir = a
    end
})

TargetAimSection:Toggle({
    Name = "Look At",
    Callback = function(a)
  getgenv().Sentinel.LookAt = a
    end
})



TargetAimSection:Toggle({
    Name = "Spectate",
    Callback = function(a)
  TargetAimbot.CSync.Spectate = a
    end
})

TargetAimSection:Toggle({
    Name = "Highlight",
    Callback = function(a)
  Highlight = a
    end
})

local TapY = SubPage1:Section({Name = "Bullet Tp ", Side = "Left"})
TapY:Toggle({
    Name = "Enable",
    Callback = function(a)
  Script.Locals.GunTP.Enabled = a
    end
})

TapY:Toggle({
    Name = "Anchor",
    Callback = function(a)
  Script.Locals.GunTP.Anchor = a
    end
})

TapY:Toggle({
    Name = "Rocket TP",
    Callback = function(a)
  Script.Locals.RocketTP.Enabled = a
    end
})

TapY:Toggle({
    Name = "Grenade TP",
    Callback = function(a)
  Script.Locals.GrenadeTP.Enabled = a
    end
})

TapY:List({
   Name = "Method",
   Options = {"GP","Shh"},
   Default = "GP",
   Callback = function(a)
 
   end
})

local clucluxclan = SubPage1:Section({Name = "Toggle", Side = "Left"})

clucluxclan:Toggle({
   Name = "Hold Mode -- PC",
   Callback = function()
   end
})

clucluxclan:List({
   Name = "Target Mode",
   Options = {"Button", "Key"}, 
   Default = "Button",
   Callback = function()
   end
})

local nnigga = SubPage1:Section({Name = "HitPart", Side = "Left"})

nnigga:Toggle({
    Name = "Randomized",
    Callback = function(a)
        getgenv().Sentinel.NearestPart = a
    end
})

nnigga:List({
    Name = "BodyPart",
    Options = {
        "Head", "UpperTorso", "LowerTorso", "HumanoidRootPart", 
        "LeftUpperArm", "LeftLowerArm", "LeftHand", 
        "RightUpperArm", "RightLowerArm", "RightHand", 
        "LeftUpperLeg", "LeftLowerLeg", "LeftFoot", 
        "RightUpperLeg", "RightLowerLeg", "RightFoot"
    },
    Default = "HumanoidRootPart",
    Callback = function(a)
        getgenv().Sentinel.SelectedPart = a
    end
})

local idkquit = SubPage1:Section({Name = "Calculation", Side = "Right"})
idkquit:Divider({Name = "Prediction"})
idkquit:Toggle({
    Name = "Auto Prediction",
    Callback = function(a)
  getgenv().Sentinel.AutoPrediction = a
    end
})

idkquit:List({
    Name = "Lock Type",
    Options = {
        "Namecall", "Index"
    },
    Default = "Namecall",
    Callback = function(a)
        getgenv().Sentinel.LockType = a
    end
})

idkquit:Textbox({
    Name = "Horizontal Prediction",
    Default = "0.164",
    Callback = function(a)
        getgenv().Sentinel.HorizontalPrediction = a
    end
})

idkquit:Divider({Name = "Resolver"})

idkquit:Toggle({
    Name = "Enable Resolver",
    Default = getgenv().Sentinel,
    Callback = function(a)
 getgenv().Sentinel.ResolverEnabled = a
    end
})

idkquit:List({
    Name = "Mode",
    Options = {"LookVector", "Diddy", "MoveDirection"},
    Callback = function(a)
    getgenv().Sentinel.RESOLVER = a
    end
})
    

local nomorees = SubPage1:Section({Name = "Checks", Side = "Right"})

nomorees:Toggle({
    Name = "WallCheck",
    Callback = function(a)
  
    end
})

nomorees:Toggle({
    Name = "ForceField Check",
    Callback = function(a)
  Sentinel.ForceFieldCheck = a
    end
})

nomorees:Toggle({
    Name = "Knocked",
    Callback = function(a)
  
    end
})

nomorees:Toggle({
    Name = "Crew Check",
    Callback = function(a)
  
    end
})

nomorees:Toggle({
    Name = "Anti Curve",
    Callback = function(a)
  
    end
})

nomorees:Toggle({
    Name = "Anti Ground Shots",
    Default = Sentinel.NoGroundShots,
    Callback = function(a)
      Sentinel.NoGroundShots = a
    end
})

local buyarmorandfooddelay = 0.5
local foodbuy
local bro
local map = workspace:FindFirstChild("MAP")

if map then
    local shops = map:FindFirstChild("Shops")
    local pads = map:FindFirstChild("Pads")

    if shops and shops:FindFirstChild("[Medium Armor]") then
        bro = shops["[Medium Armor]"]
        foodbuy = shops["[Taco]"]
    elseif pads and pads:FindFirstChild("[Medium Armor]") then
        bro = pads["[Medium Armor]"]
        foodbuy = pads["[Pizza]"]
    else
        bro = nil
        foodbuy = nil
    end
else
    bro = nil
    foodbuy = nil
end

    local VoidFall = SubPage6:Section({Name = "AutoBuy -- Voidfall Games", Side = "Right", Zindex = 2})

    VoidFall:Button({
        Name = "Buy",
        Callback = function()
            if bro:FindFirstChild("ClickDetector") then
                fireclickdetector(bro.ClickDetector)
fireclickdetector(foodbuy.ClickDetector)
            end
        end
    })

    VoidFall:Toggle({
        Name = "AutoBuy",
        Callback = function(a)
   AutoArmor = a
            while AutoArmor do
                if bro:FindFirstChild("ClickDetector") then
                    fireclickdetector(bro.ClickDetector)
fireclickdetector(foodbuy.ClickDetector)
                else
                    break
                end
                wait(buyarmorandfooddelay)
            end
        end
    })

VoidFall:Slider({
    Name = "Delay",
    Min = 0,
    Max = 10,
    Default = buyarmorandfooddelay,
    Suffix = "",
    Decimals = 0.001,
    Callback = function(a)
        buyarmorandfooddelay = a
    end
})

local flySpeed = 50
local flying = false
local player = game.Players.LocalPlayer
local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")

local function toggleFlying(character, state)
    flying = state
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    
    if flying and humanoid and humanoidRootPart then
        humanoid.PlatformStand = false
        local bodyGyro = Instance.new("BodyGyro", humanoidRootPart)
        local bodyVelocity = Instance.new("BodyVelocity", humanoidRootPart)
        bodyGyro.P = 9e4
        bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
        bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)

        local connection
        connection = runService.RenderStepped:Connect(function()
            if flying then
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
                local moveDirection = workspace.CurrentCamera.CFrame.LookVector
                local verticalVelocity = userInputService:IsKeyDown(Enum.KeyCode.Space) and flySpeed or userInputService:IsKeyDown(Enum.KeyCode.LeftControl) and -flySpeed or 0
                
                bodyVelocity.Velocity = moveDirection * flySpeed + Vector3.new(0, verticalVelocity, 0)
                humanoid:ChangeState(Enum.HumanoidStateType.Freefall)
            else
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
                connection:Disconnect()
            end
        end)
    elseif humanoid and humanoidRootPart then
        for _, v in pairs(humanoidRootPart:GetChildren()) do
            if v:IsA("BodyGyro") or v:IsA("BodyVelocity") then
                v:Destroy()
            end
        end
    end
end

local function onCharacterAdded(character)
    toggleFlying(character, false)
end

player.CharacterAdded:Connect(function(character)
    character:WaitForChild("Humanoid")
    onCharacterAdded(character)
end)

local KillBotSection = SubPage6:Section({Name = "KillBot -- Uses Silent/Target", Side = "Right", Zindex = 2})

local CFrameShit = SubPage6:Section({Name = "CFrame Speed", Side = "Left", Zindex = 2})

local VelFly = SubPage6:Section({Name = "Velocity Fly", Side = "Left", Zindex = 2})


local AutoBuyingShit = SubPage6:Section({Name = "Auto Buy/Armor", Side = "Left", Zindex = 2})

local atlas = {
   Misc = {
    AutoBuy = {
        Enabled = false,
        ReturnBackDelay = 0.023,
        Gun = "LMG",
        Foods = "Chicken",
        Misc = "Surgeon Mask",
        Armor = "Medium Armor",
        AmmoAmount = 3,
        AutoArmor = {
            Enabled = false,
            BuyOn = 80
        },
        AutoEat = {
            Enabled = false,
            BuyOn = 80
         }
     }
  }
}

local Client = Players.LocalPlayer

function TeleportBuy(Target)
    if not Target or Target == "" then
        return
    end

    local targetItem = workspace.Ignored.Shop:FindFirstChild(Target)
    if not targetItem then
        return
    end

    local playerCharacter = Client and Client.Character
    if not playerCharacter or not playerCharacter:FindFirstChild("HumanoidRootPart") then
        return
    end

    local oldCFrame = playerCharacter.HumanoidRootPart.CFrame
    local returnDelay = atlas.Misc.AutoBuy.ReturnBackDelay

    playerCharacter.HumanoidRootPart.CFrame = targetItem.Head.CFrame * CFrame.new(0, 1, 0)
    wait(0.2)

    local clickDetector = targetItem:FindFirstChild("ClickDetector")
    if not clickDetector then
        return
    end

    if atlas.Misc.AutoBuy.Enabled then
        for i = 1, 3 do
            fireclickdetector(clickDetector)
            wait(0.05)
        end
    end

    wait(returnDelay)
    playerCharacter.HumanoidRootPart.CFrame = oldCFrame
end

function ToolName(Name)
    for _, item in ipairs(workspace.Ignored.Shop:GetChildren()) do
        if item.Name:match("%[" .. Name .. "%] %- %$%d+") then
            return item.Name
        end
    end
end

function ToolAmmo(Name)
    for _, item in ipairs(workspace.Ignored.Shop:GetChildren()) do
        if item.Name:match("%d+ %[" .. Name .. " Ammo%] %- %$%d+") then
            return item.Name
        end
    end
end

 AutoBuyingShit:Toggle({
        Name = 'Enable',
        Default = atlas.Misc.AutoBuy.Enabled, 
        Callback = function(bool)
    atlas.Misc.AutoBuy.Enabled = bool
end
})

AutoBuyingShit:List({
        Options = {"Glock","SMG","Silencer","TacticalShotgun","P90","AUG","Shotgun","RPG","AR","Double-Barrel SG","Flamethrower","Revolver","LMG","AK47","DrumGun","Silencer","GrenadeLauncher","Taser","SilencerAR","Grenade"}, 
        Default = atlas.Misc.AutoBuy.Gun,
        Callback = function(bool)
            atlas.Misc.AutoBuy.Gun = bool
        end
})

AutoBuyingShit:Button({Name = "Purchase Gun", Callback = function() 
        TeleportBuy(ToolName(atlas.Misc.AutoBuy.Gun))
    end})



AutoBuyingShit:List({
        Options = {"Cranberry","Donut","Pizza","Chicken","Popcorn","Hamburger","Taco","Starblox Latte","Lettuce","Lemonade"}, 
        Default = atlas.Misc.AutoBuy.Foods,
        Callback = function(bool)
            atlas.Misc.AutoBuy.Foods = bool
        end
    })

AutoBuyingShit:Button({Name = "Purchase Foods", Callback = function() 
        TeleportBuy(ToolName(atlas.Misc.AutoBuy.Foods))
    end})


AutoBuyingShit:List({
        Options = {"Surgeon Mask","Knife"}, 
        Default = atlas.Misc.AutoBuy.Misc,
        Callback = function(bool)
            atlas.Misc.AutoBuy.Misc = bool
        end
    })


--// Ratz

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")


local AutoArmor = true
local BuyOn = 75
local AutoBuyShit = true

local selectedArmor = "[High-Medium Armor] - $2440" 
    
    local function AutoBuyArmor()
        task.spawn(function()
            while RunService.RenderStepped:Wait() do
                if AutoArmor and AutoBuyShit then
                    local Character = Client.Character
                    if Character and Character:FindFirstChild("BodyEffects") and Character.BodyEffects:FindFirstChild("Armor") then
                        if Character.BodyEffects.Armor.Value < BuyOn then
                            local OldPosition = Character.HumanoidRootPart.CFrame

                            local armorItem = workspace.Ignored.Shop:FindFirstChild(selectedArmor)
                            if armorItem and armorItem:FindFirstChild("Head") and armorItem:FindFirstChild("ClickDetector") then
                                Character.HumanoidRootPart.CFrame = armorItem.Head.CFrame
                                task.wait(0.1)
                                fireclickdetector(armorItem.ClickDetector)
                                task.wait(0.1)
                                
                                Character.HumanoidRootPart.CFrame = OldPosition
                            end
                        end
                    end
                end
            end
        end)
    end

AutoBuyArmor()

AutoBuyingShit:Toggle({
    Name = "Enabled",
    Default = false,
    Callback = function(state)
        AutoArmor = state
    end
})

AutoBuyingShit:Slider({
    Name = "Buy on %",
    Default = 40,
    Min = 0,
    Max = 100,
    Suffix = "",
    Callback = function(state)
        BuyOn = state
    end
})

AutoBuyingShit:List({
        Name = "Armor",
        Options = { "High-Medium Armor", "Medium Armor" }, 
        Default = "High-Medium Atmor",
        Callback = function(selected)
            if selected == "High-Medium Armor" then
                selectedArmor = "[High-Medium Armor] - $2440"
            elseif selected == "Medium Armor" then
                selectedArmor = "[Medium Armor] - $1061"
            end
        end
})

VelFly:Toggle({
    Name = "Enabled",
    Default = false,
    Callback = function(state)
        if player.Character then
            toggleFlying(player.Character, state)
        end
    end
})

VelFly:Slider({
    Name = "Speed",
    Min = 10,
    Max = 100,
    Default = 50,
    Callback = function(a)
        flySpeed = a
    end
})


CFrameShit:Toggle({
    Name = "Enabled",
    Callback = function(a)
     getgenv().Sentinel.cframespeedtoggle = a 
  end
})

CFrameShit:Slider({
    Name = "Speed",
    Min = 0,
    Max = 20,
    Default = 1.24,
    Decimal = 0.01,
    Suffix = "",
     Callback = function(a)
        getgenv().Sentinel.speedvalue = a 
  end
})

KillBotSection:Toggle({
    Name = "Enable",
    Default = false,
    Callback = function(a)
    TargetAimbot.CSync.Enabled = a 
 end
}):Colorpicker({Default = Library.Accent, Flag = "Bullet Tracers Color"})

KillBotSection:Toggle({
    Name = "Prevent Client TP",
    Default = false
})


local line = false




local circleclakaa = TargetAimbot.CSync.Circle




KillBotSection:Toggle({
    Name = "Visualize Circle",
    Default = true,
    Callback = function(a)
        TargetAimbot.CSync.Circle = a
    end
}):Colorpicker({
    Default = Library.Accent,
    Flag = "Bullet Tracers Color",
    Callback = function(a)
        TargetAimbot.CSync.Color = a
    end
})


KillBotSection:Toggle({
    Name = "Position Visualizer",
    Default = false,
    Callback = function(a) 
    TargetAimbot.CSync.VisualizeMovement = a
  end
}):Colorpicker({Default = Library.Accent, Flag = "Bullet Tracers Color", Callback = function(a) TargetAimbot.CSync.Color = a end})

KillBotSection:Toggle({
    Name = "Always Visualize Position",
    Default = false
})

KillBotSection:List({
    Name = "Killbot Method",
    Options = {"Random", "Orbit"},
    Default = "Orbit",
    Callback = function(a)
     TargetAimbot.CSync.Type = a
    end
})

KillBotSection:List({
    Name = "Circle Face Direction",
    Options = {"Client", "Target"},
    Default = "Client"
})

KillBotSection:Slider({
    Name = "Distance",
    Min = 0,
    Max = 20,
    Suffix = "",
    Default = 10,
    Callback = function(a)
    TargetAimbot.CSync.Distance = a
  end
})

KillBotSection:Slider({
    Name = "Speed",
    Default = 2,
    Min = 0,
    Suffix = "",
    Max = 10,
    Callback = function(a)
    TargetAimbot.CSync.Speed = a
  end
})

KillBotSection:Slider({
    Name = "Height",
    Default = 0,
    Min = 0,
    Suffix = "",
    Max = 10,
    Callback = function(a)
    TargetAimbot.CSync.Height = a
  end
})

KillBotSection:Divider({Name = "Randomize Settings"})

KillBotSection:Slider({
    Name = "Amount",
    Min = 0,
    Max = 20,
    Suffix = "",
    Default = 10,
    Callback = function(a)
    TargetAimbot.CSync.RandomAmount = a
  end
})









do
    local CFG = Settings:Section({Name = "Config", Zindex = 2})




    local ConfigList = CFG:List({Name = "Config", Flag = "SettingConfigurationList", Options = {}})
    CFG:Textbox({Flag = "SettingsConfigurationName", Name = "Config Name"})
    
    local CurrentList = {}

    local function EnsureConfigsDirectoryExists()
        if not isfolder("Ratz") then
            makefolder("Ratz")
        end
    end

    local function UpdateConfigList()
        EnsureConfigsDirectoryExists()
        local List = {}
        for _, file in ipairs(listfiles("Ratz")) do
            local FileName = file:gsub("Ratz/", ""):gsub("%.cfg$", "")
            table.insert(List, FileName)
        end

        if #List ~= #CurrentList or table.concat(List) ~= table.concat(CurrentList) then
            CurrentList = List
            -- Update the options directly if Refresh does not exist
            ConfigList.Options = CurrentList
            if ConfigList.Refresh then
                ConfigList:Refresh(CurrentList)
            end
        end
    end

    CFG:Button({
        Name = "Create",
        Callback = function()
            EnsureConfigsDirectoryExists()
            local ConfigName = Flags.SettingsConfigurationName
            if ConfigName and ConfigName ~= "" then
                local filePath = "Ratz/" .. ConfigName .. ".cfg"
                if not isfile(filePath) then
                    writefile(filePath, "")
                    UpdateConfigList()
                else
                    Library:Notification("Config file already exists: " .. filePath)
                end
            else
                Library:Notification("Config name cannot be empty.")
            end
        end
    })

    CFG:Button({
        Name = "Save",
        Callback = function()
            local SelectedConfig = Flags.SettingConfigurationList
            if SelectedConfig then
                local filePath = "Ratz/" .. SelectedConfig .. ".cfg"
                writefile(filePath, Library:GetConfig())
            end
        end
    })

    CFG:Button({
        Name = "Load",
        Callback = function()
            local SelectedConfig = Flags.SettingConfigurationList
            if SelectedConfig then
                local filePath = "Ratz/" .. SelectedConfig .. ".cfg"
                if isfile(filePath) then
                    local success, err = pcall(function()
                        Library:LoadConfig(readfile(filePath))
                    end)
                    if not success then
                        print("Error loading config: " .. err)
                    end
                else
                    print("Error: Config file does not exist.")
                end
            end
        end
    })

    CFG:Button({
        Name = "Delete",
        Callback = function()
            local SelectedConfig = Flags.SettingConfigurationList
            if SelectedConfig then
                local filePath = "Ratz/" .. SelectedConfig .. ".cfg"
                if isfile(filePath) then
                    delfile(filePath)
                    UpdateConfigList()
                else
                    print("Error: Config file does not exist.")
                end
            end
        end
    })

    CFG:Button({
        Name = "Refresh",
        Callback = UpdateConfigList
    })

    CFG:Keybind({
        Name = "Menu Key",
        Flag = "MenuKey",
        UseKey = true,
        Default = Enum.KeyCode.End,
        Callback = function(State)
            Library.UIKey = State
        end
    })

    CFG:Colorpicker({
        Name = "Menu Accent",
        Flag = "MenuAccent",
        Default = Library.Accent,
        Callback = function(State)
            Library:ChangeAccent(State)
        end
    })

    CFG:Toggle({
        Name = "Show Watermark",
        Flag = "Watermark",
        Callback = function(State)
            Watermark:SetVisible(State)
        end
    })

    CFG:Textbox({
        Flag = "WatermarkText",
        Name = "Watermark Text",
        State = string.format("$$ osamason $$ | %s | %s | %s | Christmas Cheer | Jingle Bells | Snowflakes | Holiday Vibes | gyatt", plyaha, dick2, christmasCharacter),
        Callback = function(State)
            Watermark:UpdateText(State)
        end
    })
--
local idiotdelay = 0.0001
local HoodC = Settings:Section({Name = "Hood Custom", Side = "Right", Zindex = 2})

HoodC:Toggle({
    Name = "Fire Rate",
    Callback = function(a)
        Noobidiot = a
        while Noobidiot do
            local player = game:GetService("Players").LocalPlayer

            for _, tool in pairs(player.Backpack:GetChildren()) do
                if tool:FindFirstChild("GunData") then
                    local gunData = tool:FindFirstChild("GunData")

                    if gunData and gunData:IsA("ModuleScript") then
                        local gunDataContent = require(gunData)

                        if gunDataContent.cooldown and gunDataContent.slowdown_time then

local gun_find = tool:FindFirstChild("GunData")

local Gun_Data = require(gun_find)

Gun_Data.is_shotgun = a
                            gunDataContent.cooldown = idiotdelay
                            gunDataContent.slowdown_time = idiotdelay
                        end
                    end
                end
            end
            task.wait(1)
        end
    end
})
HoodC:Textbox({
    Name = "Gun Delay",
  Default = tostring(idiotdelay),
    Callback = function(a)
        idiotdelay  = tonumber(a)
    end
})




end



local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Stats = game:GetService("Stats")
local CoreGui = game:GetService("CoreGui")
local StarterGui = game:GetService("StarterGui")
local SoundService = game:GetService("SoundService")
local Stas = game:GetService("Stats")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local TargBindEnabled = true
local TargetPlr
local TargResolvePos

local hitsounds = {
    ["RIFK7"] = "rbxassetid://9102080552",
    ["Bubble"] = "rbxassetid://9102092728",
    ["Minecraft"] = "rbxassetid://5869422451",
    ["Cod"] = "rbxassetid://160432334",
    ["Bameware"] = "rbxassetid://6565367558",
    ["Neverlose"] = "rbxassetid://6565370984",
    ["Gamesense"] = "rbxassetid://4817809188",
    ["Rust"] = "rbxassetid://6565371338",
    ["BlackPencil"] = "https://github.com/Shatapmatehabibi/Hitsounds/raw/main/bananapencil.mp3.mp3",
    ["Fein"] = "https://github.com/CongoOhioDog/SoundS/blob/main/psalms%20highly%20defined%20fein.mp3?raw=true",
    ["Unicorn"] = "https://github.com/CongoOhioDog/SoundS/blob/main/shiny%20unicorn%20for%20dh%20_%20psalms.mp3?raw=true",
    ["Kitty"] = "https://github.com/CongoOhioDog/SoundS/blob/main/Kitty.mp3?raw=true",
    ["Bird"] = "https://github.com/CongoOhioDog/SoundS/blob/main/bird%20chirping%20for%20DH%20_%20psalms%20audio.mp3?raw=true",
    ["BirthdayCake"] = "https://github.com/CongoOhioDog/SoundS/blob/main/Birthday%20cake%20for%20dh%20_%20psalms.mp3?raw=true", 
    ["KenCarson"] =  "https://github.com/CongoOhioDog/SoundS/blob/main/ken_carson_-_jennifer_s_body_offici(2).mp3?raw=true"
}



local TargHighlight = Instance.new("Highlight")
TargHighlight.Parent = CoreGui
TargHighlight.FillColor = TargetAimbot.HighlightColor1
TargHighlight.OutlineColor = TargetAimbot.HighlightColor2
TargHighlight.FillTransparency = 0.5
TargHighlight.OutlineTransparency = 0
TargHighlight.Enabled = false

local Tracer = Drawing.new("Line")
Tracer.Visible = false
Tracer.Color = Color3.fromRGB(154, 7, 250)
Tracer.Thickness = 1
Tracer.Transparency = 1

local HitChamsFolder = Instance.new("Folder")
HitChamsFolder.Name = "HitChamsFolder"
HitChamsFolder.Parent = Workspace

--// Crescent Slash


game:GetService("RunService").Heartbeat:Connect(function()
    if getgenv().Sentinel.cframespeedtoggle  then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame =
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame +
            game.Players.LocalPlayer.Character.Humanoid.MoveDirection * getgenv().Sentinel.speedvalue / 0.5
    end


end)

do
local Insane = Instance.new("Part")
Insane.Parent = ReplicatedStorage

local Attachment = Instance.new("Attachment")
Attachment.Name = "Attachment"
Attachment.Parent = Insane

HitEffectModule.Locals.Type["Crescent Slash"] = Attachment

local Glow = Instance.new("ParticleEmitter")
Glow.Name = "Glow"
Glow.Lifetime = NumberRange.new(0.16, 0.16)
Glow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1421725, 0.6182796), NumberSequenceKeypoint.new(1, 1)})
Glow.Color = ColorSequence.new(Color3.fromRGB(91, 177, 252))
Glow.Speed = NumberRange.new(0, 0)
Glow.Brightness = 5
Glow.Size = NumberSequence.new(9.1873131, 16.5032349)
Glow.Enabled = false
Glow.ZOffset = -0.0565939
Glow.Rate = 50
Glow.Texture = "rbxassetid://8708637750"

local Gradient1 = Instance.new("ParticleEmitter")
Gradient1.Name = "Gradient1"
Gradient1.Lifetime = NumberRange.new(0.3, 0.3)
Gradient1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.15, 0.3), NumberSequenceKeypoint.new(1, 1)})
Gradient1.Color = ColorSequence.new(Color3.fromRGB(115, 201, 255))
Gradient1.Speed = NumberRange.new(0, 0)
Gradient1.Brightness = 6
Gradient1.Size = NumberSequence.new(0, 11.6261358)
Gradient1.Enabled = false
Gradient1.ZOffset = 0.9187313
Gradient1.Rate = 50
Gradient1.Texture = "rbxassetid://8196169974"
Gradient1.Parent = Attachment

local Shards = Instance.new("ParticleEmitter")
Shards.Name = "Shards"
Shards.Lifetime = NumberRange.new(0.19, 0.7)
Shards.SpreadAngle = Vector2.new(-90, 90)
Shards.Color = ColorSequence.new(Color3.fromRGB(108, 184, 255))
Shards.Drag = 10
Shards.VelocitySpread = -90
Shards.Squash = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5705521, 0.4125001), NumberSequenceKeypoint.new(1, -0.9375)})
Shards.Speed = NumberRange.new(97.7530136, 146.9970093)
Shards.Brightness = 4
Shards.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.284774, 1.2389833, 0.1534118), NumberSequenceKeypoint.new(1, 0)})
Shards.Enabled = false
Shards.Acceleration = Vector3.new(0, -56.961341857910156, 0)
Shards.ZOffset = 0.5705321
Shards.Rate = 50
Shards.Texture = "rbxassetid://8030734851"
Shards.Rotation = NumberRange.new(90, 90)
Shards.Orientation = Enum.ParticleOrientation.VelocityParallel
Shards.Parent = Attachment

local ShardsDark = Instance.new("ParticleEmitter")
ShardsDark.Name = "ShardsDark"
ShardsDark.Lifetime = NumberRange.new(0.19, 0.35)
ShardsDark.SpreadAngle = Vector2.new(-90, 90)
ShardsDark.Color = ColorSequence.new(Color3.fromRGB(108, 184, 255))
ShardsDark.Drag = 10
ShardsDark.VelocitySpread = -90
ShardsDark.Squash = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5705521, 0.4125001), NumberSequenceKeypoint.new(1, -0.9375)})
ShardsDark.Speed = NumberRange.new(97.7530136, 146.9970093)
ShardsDark.Brightness = 4
ShardsDark.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.290774, 0.6734411, 0.1534118), NumberSequenceKeypoint.new(1, 0)})
ShardsDark.Enabled = false
ShardsDark.ZOffset = 0.5705321
ShardsDark.Rate = 50
ShardsDark.Texture = "rbxassetid://8030734851"
ShardsDark.Rotation = NumberRange.new(90, 90)
ShardsDark.Orientation = Enum.ParticleOrientation.VelocityParallel
ShardsDark.Parent = Attachment

local Specs = Instance.new("ParticleEmitter")
Specs.Name = "Specs"
Specs.Lifetime = NumberRange.new(0.33, 1.4)
Specs.SpreadAngle = Vector2.new(360, -1000)
Specs.Color = ColorSequence.new(Color3.fromRGB(98, 174, 255))
Specs.Drag = 10
Specs.VelocitySpread = 360
Specs.Speed = NumberRange.new(36.7492523, 146.9970093)
Specs.Brightness = 7
Specs.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.200774, 2.0311937, 0.4363973), NumberSequenceKeypoint.new(1, 0)})
Specs.Enabled = false
Specs.Acceleration = Vector3.new(0, 36.74925231933594, 0)
Specs.Rate = 50
Specs.Texture = "rbxassetid://8030760338"
Specs.EmissionDirection = Enum.NormalId.Right
Specs.Parent = Attachment

local Specs1 = Instance.new("ParticleEmitter")
Specs1.Name = "Specs"
Specs1.Lifetime = NumberRange.new(0.33, 1.75)
Specs1.SpreadAngle = Vector2.new(90, -90)
Specs1.Color = ColorSequence.new(Color3.fromRGB(106, 171, 255))
Specs1.Drag = 9
Specs1.VelocitySpread = 90
Specs1.Speed = NumberRange.new(42.2616425, 73.4985046)
Specs1.Brightness = 6
Specs1.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.210774, 0.3978962, 0.1855686), NumberSequenceKeypoint.new(1, 0)})
Specs1.Enabled = false
Specs1.Acceleration = Vector3.new(0, -20.21208953857422, 0)
Specs1.ZOffset = 0.5144895
Specs1.Rate = 50
Specs1.Texture = "rbxassetid://8030760338"
Specs1.Parent = Attachment

local Specs2 = Instance.new("ParticleEmitter")
Specs2.Name = "Specs"
Specs2.Lifetime = NumberRange.new(0.19, 1.2)
Specs2.SpreadAngle = Vector2.new(360, -1000)
Specs2.Color = ColorSequence.new(Color3.fromRGB(98, 174, 255))
Specs2.Drag = 10
Specs2.VelocitySpread = 360
Specs2.Speed = NumberRange.new(36.7492523, 146.9970093)
Specs2.Brightness = 7
Specs2.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.200774, 2.0311937, 0.4363973), NumberSequenceKeypoint.new(1, 0)})
Specs2.Enabled = false
Specs2.Acceleration = Vector3.new(0, 36.74925231933594, 0)
Specs2.Rate = 50
Specs2.Texture = "rbxassetid://8030760338"
Specs2.EmissionDirection = Enum.NormalId.Right
Specs2.Parent = Attachment

local Specs21 = Instance.new("ParticleEmitter")
Specs21.Name = "Specs2"
Specs21.Lifetime = NumberRange.new(0.19, 1.35)
Specs21.SpreadAngle = Vector2.new(90, -90)
Specs21.Color = ColorSequence.new(Color3.fromRGB(106, 171, 255))
Specs21.Drag = 12
Specs21.VelocitySpread = 90
Specs21.Speed = NumberRange.new(42.2616425, 73.4985046)
Specs21.Brightness = 6
Specs21.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.216774, 0.5721694, 0.1855686), NumberSequenceKeypoint.new(1, 0)})
Specs21.Enabled = false
Specs21.Acceleration = Vector3.new(0, -20.21208953857422, 0)
Specs21.ZOffset = 0.5144895
Specs21.Rate = 50
Specs21.Texture = "rbxassetid://8030760338"
Specs21.Parent = Attachment

local ddddddddddddddddddd = Instance.new("ParticleEmitter")
ddddddddddddddddddd.Name = "ddddddddddddddddddd"
ddddddddddddddddddd.Lifetime = NumberRange.new(0.19, 0.37)
ddddddddddddddddddd.SpreadAngle = Vector2.new(90, -90)
ddddddddddddddddddd.LockedToPart = true
ddddddddddddddddddd.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.6429392, 0), NumberSequenceKeypoint.new(1, 0)})
ddddddddddddddddddd.LightEmission = 1
ddddddddddddddddddd.Color = ColorSequence.new(Color3.fromRGB(90, 184, 255), Color3.fromRGB(165, 251, 255))
ddddddddddddddddddd.Drag = 6
ddddddddddddddddddd.TimeScale = 0.7
ddddddddddddddddddd.VelocitySpread = 90
ddddddddddddddddddd.Speed = NumberRange.new(81.5833435, 110.2477646)
ddddddddddddddddddd.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.410774, 0.6711507, 0.3356177), NumberSequenceKeypoint.new(1, 0)})
ddddddddddddddddddd.Enabled = false
ddddddddddddddddddd.Acceleration = Vector3.new(0, -81.58334350585938, 0)
ddddddddddddddddddd.ZOffset = 0.8345273
ddddddddddddddddddd.Rate = 50
ddddddddddddddddddd.Texture = "rbxassetid://1053546634"
ddddddddddddddddddd.RotSpeed = NumberRange.new(-444, 166)
ddddddddddddddddddd.Rotation = NumberRange.new(-360, 360)
ddddddddddddddddddd.Parent = Attachment

local large_shard = Instance.new("ParticleEmitter")
large_shard.Name = "large_shard"
large_shard.Lifetime = NumberRange.new(0.19, 0.28)
large_shard.SpreadAngle = Vector2.new(-90, 90)
large_shard.Color = ColorSequence.new(Color3.fromRGB(108, 184, 255))
large_shard.Drag = 10
large_shard.VelocitySpread = -90
large_shard.Squash = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5705521, 0.4125001), NumberSequenceKeypoint.new(1, -0.9375)})
large_shard.Speed = NumberRange.new(97.7530136, 146.9970093)
large_shard.Brightness = 4
large_shard.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.260774, 3.515605, 0.1534118), NumberSequenceKeypoint.new(1, 0)})
large_shard.Enabled = false
large_shard.ZOffset = 0.5705321
large_shard.Rate = 50
large_shard.Texture = "rbxassetid://8030734851"
large_shard.Rotation = NumberRange.new(90, 90)
large_shard.Orientation = Enum.ParticleOrientation.VelocityParallel
large_shard.Parent = Attachment

local out_Specs = Instance.new("ParticleEmitter")
out_Specs.Name = "out_Specs"
out_Specs.Lifetime = NumberRange.new(0.19, 1)
out_Specs.SpreadAngle = Vector2.new(44, -1000)
out_Specs.Color = ColorSequence.new(Color3.fromRGB(98, 174, 255))
out_Specs.Drag = 10
out_Specs.VelocitySpread = 44
out_Specs.Speed = NumberRange.new(36.7492523, 146.9970093)
out_Specs.Brightness = 7
out_Specs.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.244774, 0.5469525, 0.1433053), NumberSequenceKeypoint.new(1, 0)})
out_Specs.Enabled = false
out_Specs.Acceleration = Vector3.new(0, -3.215559720993042, 0)
out_Specs.Rate = 50
out_Specs.Texture = "rbxassetid://8030760338"
out_Specs.EmissionDirection = Enum.NormalId.Right
out_Specs.Parent = Attachment

local Effect = Instance.new("ParticleEmitter")
Effect.Name = "Effect"
Effect.Lifetime = NumberRange.new(0.4, 0.7)
Effect.FlipbookLayout = Enum.ParticleFlipbookLayout.Grid4x4
Effect.SpreadAngle = Vector2.new(360, -360)
Effect.LockedToPart = true
Effect.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1070999, 0.19375), NumberSequenceKeypoint.new(0.7761194, 0.88125), NumberSequenceKeypoint.new(1, 1)})
Effect.LightEmission = 1
Effect.Color = ColorSequence.new(Color3.fromRGB(92, 161, 252))
Effect.Drag = 1
Effect.VelocitySpread = 360
Effect.Speed = NumberRange.new(0.0036749, 0.0036749)
Effect.Brightness = 2.0999999
Effect.Size = NumberSequence.new(6.9680691, 9.9213123)
Effect.Enabled = false
Effect.ZOffset = 0.4777403
Effect.Rate = 50
Effect.Texture = "rbxassetid://9484012464"
Effect.RotSpeed = NumberRange.new(-150, -150)
Effect.FlipbookMode = Enum.ParticleFlipbookMode.OneShot
Effect.Rotation = NumberRange.new(50, 50)
Effect.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Effect.Parent = Attachment

local Crescents = Instance.new("ParticleEmitter")
Crescents.Name = "Crescents"
Crescents.Lifetime = NumberRange.new(0.19, 0.38)
Crescents.SpreadAngle = Vector2.new(-360, 360)
Crescents.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1932907, 0), NumberSequenceKeypoint.new(0.778754, 0), NumberSequenceKeypoint.new(1, 1)})
Crescents.LightEmission = 1
Crescents.Color = ColorSequence.new(Color3.fromRGB(92, 161, 252))
Crescents.VelocitySpread = -360
Crescents.Speed = NumberRange.new(0.0826858, 0.0826858)
Crescents.Brightness = 20
Crescents.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.398774, 8.8026266, 2.2834616), NumberSequenceKeypoint.new(1, 11.477972, 1.860431)})
Crescents.Enabled = false
Crescents.ZOffset = 0.4542207
Crescents.Rate = 50
Crescents.Texture = "rbxassetid://12509373457"
Crescents.RotSpeed = NumberRange.new(800, 1000)
Crescents.Rotation = NumberRange.new(-360, 360)
Crescents.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Crescents.Parent = Attachment

Insane.Parent = workspace
end

do --// Cosmic Explosion

local Part = Instance.new("Part")

Part.Parent = ReplicatedStorage

local Attachment = Instance.new("Attachment")
Attachment.Name = "Attachment"
Attachment.Parent = Part

HitEffectModule.Locals.Type["Cosmic Explosion"] = Attachment

local Glow = Instance.new("ParticleEmitter")
Glow.Name = "Glow"
Glow.Lifetime = NumberRange.new(0.16, 0.16)
Glow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1421725, 0.6182796), NumberSequenceKeypoint.new(1, 1)})
Glow.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Glow.Speed = NumberRange.new(0, 0)
Glow.Brightness = 5
Glow.Size = NumberSequence.new(9.1873131, 16.5032349)
Glow.Enabled = false
Glow.ZOffset = -0.0565939
Glow.Rate = 50
Glow.Texture = "rbxassetid://8708637750"
Glow.Parent = Attachment

local Effect = Instance.new("ParticleEmitter")
Effect.Name = "Effect"
Effect.Lifetime = NumberRange.new(0.4, 0.7)
Effect.FlipbookLayout = Enum.ParticleFlipbookLayout.Grid4x4
Effect.SpreadAngle = Vector2.new(360, -360)
Effect.LockedToPart = true
Effect.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1070999, 0.19375), NumberSequenceKeypoint.new(0.7761194, 0.88125), NumberSequenceKeypoint.new(1, 1)})
Effect.LightEmission = 1
Effect.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Effect.Drag = 1
Effect.VelocitySpread = 360
Effect.Speed = NumberRange.new(0.0036749, 0.0036749)
Effect.Brightness = 2.0999999
Effect.Size = NumberSequence.new(6.9680691, 9.9213123)
Effect.Enabled = false
Effect.ZOffset = 0.4777403
Effect.Rate = 50
Effect.Texture = "rbxassetid://9484012464"
Effect.RotSpeed = NumberRange.new(-150, -150)
Effect.FlipbookMode = Enum.ParticleFlipbookMode.OneShot
Effect.Rotation = NumberRange.new(50, 50)
Effect.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Effect.Parent = Attachment

local Gradient1 = Instance.new("ParticleEmitter")
Gradient1.Name = "Gradient1"
Gradient1.Lifetime = NumberRange.new(0.3, 0.3)
Gradient1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.15, 0.3), NumberSequenceKeypoint.new(1, 1)})
Gradient1.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Gradient1.Speed = NumberRange.new(0, 0)
Gradient1.Brightness = 6
Gradient1.Size = NumberSequence.new(0, 11.6261358)
Gradient1.Enabled = false
Gradient1.ZOffset = 0.9187313
Gradient1.Rate = 50
Gradient1.Texture = "rbxassetid://8196169974"
Gradient1.Parent = Attachment

local Shards = Instance.new("ParticleEmitter")
Shards.Name = "Shards"
Shards.Lifetime = NumberRange.new(0.19, 0.7)
Shards.SpreadAngle = Vector2.new(-90, 90)
Shards.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Shards.Drag = 10
Shards.VelocitySpread = -90
Shards.Squash = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5705521, 0.4125001), NumberSequenceKeypoint.new(1, -0.9375)})
Shards.Speed = NumberRange.new(97.7530136, 146.9970093)
Shards.Brightness = 4
Shards.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.284774, 1.2389833, 0.1534118), NumberSequenceKeypoint.new(1, 0)})
Shards.Enabled = false
Shards.Acceleration = Vector3.new(0, -56.961341857910156, 0)
Shards.ZOffset = 0.5705321
Shards.Rate = 50
Shards.Texture = "rbxassetid://8030734851"
Shards.Rotation = NumberRange.new(90, 90)
Shards.Orientation = Enum.ParticleOrientation.VelocityParallel
Shards.Parent = Attachment

local Crescents = Instance.new("ParticleEmitter")
Crescents.Name = "Crescents"
Crescents.Lifetime = NumberRange.new(0.19, 0.38)
Crescents.SpreadAngle = Vector2.new(-360, 360)
Crescents.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1932907, 0), NumberSequenceKeypoint.new(0.778754, 0), NumberSequenceKeypoint.new(1, 1)})
Crescents.LightEmission = 10
Crescents.Color = ColorSequence.new(Color3.fromRGB(160, 96, 255))
Crescents.VelocitySpread = -360
Crescents.Speed = NumberRange.new(0.0826858, 0.0826858)
Crescents.Brightness = 4
Crescents.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.398774, 8.8026266, 2.2834616), NumberSequenceKeypoint.new(1, 11.477972, 1.860431)})
Crescents.Enabled = false
Crescents.ZOffset = 0.4542207
Crescents.Rate = 50
Crescents.Texture = "rbxassetid://12509373457"
Crescents.RotSpeed = NumberRange.new(800, 1000)
Crescents.Rotation = NumberRange.new(-360, 360)
Crescents.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Crescents.Parent = Attachment

local ParticleEmitter2 = Instance.new("ParticleEmitter")
ParticleEmitter2.Name = "ParticleEmitter2"
ParticleEmitter2.FlipbookFramerate = NumberRange.new(20, 20)
ParticleEmitter2.Lifetime = NumberRange.new(0.19, 0.38)
ParticleEmitter2.FlipbookLayout = Enum.ParticleFlipbookLayout.Grid4x4
ParticleEmitter2.SpreadAngle = Vector2.new(360, 360)
ParticleEmitter2.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.209842, 0.5), NumberSequenceKeypoint.new(0.503842, 0.263333), NumberSequenceKeypoint.new(0.799842, 0.5), NumberSequenceKeypoint.new(1, 1)})
ParticleEmitter2.LightEmission = 1
ParticleEmitter2.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
ParticleEmitter2.VelocitySpread = 360
ParticleEmitter2.Speed = NumberRange.new(0.0161231, 0.0161231)
ParticleEmitter2.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 4.3125), NumberSequenceKeypoint.new(0.3985056, 7.9375), NumberSequenceKeypoint.new(1, 10)})
ParticleEmitter2.Enabled = false
ParticleEmitter2.ZOffset = 0.15
ParticleEmitter2.Rate = 100
ParticleEmitter2.Texture = "http://www.roblox.com/asset/?id=12394566430"
ParticleEmitter2.FlipbookMode = Enum.ParticleFlipbookMode.OneShot
ParticleEmitter2.Rotation = NumberRange.new(39, 999)
ParticleEmitter2.Orientation = Enum.ParticleOrientation.VelocityParallel
ParticleEmitter2.Parent = Attachment

Part.Parent = workspace
end

do --// Coom

local Part = Instance.new("Part")

Part.Parent = ReplicatedStorage

local Attachment = Instance.new("Attachment")
Attachment.Parent = Part

HitEffectModule.Locals.Type["Coom"] = Attachment

local Foam = Instance.new("ParticleEmitter")
Foam.Name = "Foam"
Foam.LightInfluence = 0.5
Foam.Lifetime = NumberRange.new(1, 1)
Foam.SpreadAngle = Vector2.new(360, -360)
Foam.VelocitySpread = 360
Foam.Squash = NumberSequence.new(1)
Foam.Speed = NumberRange.new(20, 20)
Foam.Brightness = 2.5
Foam.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.1016692, 0.6508875, 0.6508875), NumberSequenceKeypoint.new(0.6494689, 1.4201183, 0.4127519), NumberSequenceKeypoint.new(1, 0)})
Foam.Enabled = false
Foam.Acceleration = Vector3.new(0, -66.04029846191406, 0)
Foam.Rate = 100
Foam.Texture = "rbxassetid://8297030850"
Foam.Rotation = NumberRange.new(-90, -90)
Foam.Orientation = Enum.ParticleOrientation.VelocityParallel
Foam.Parent = Attachment

Part.Parent = workspace
end

do --// Slash
local Part = Instance.new("Part")
Part.Parent = ReplicatedStorage

local Attachment = Instance.new("Attachment")
Attachment.Parent = Part

HitEffectModule.Locals.Type["Slash"] = Attachment

local Crescents = Instance.new("ParticleEmitter")
Crescents.Name = "Crescents"
Crescents.Lifetime = NumberRange.new(0.19, 0.38)
Crescents.SpreadAngle = Vector2.new(-360, 360)
Crescents.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1932907, 0), NumberSequenceKeypoint.new(0.778754, 0), NumberSequenceKeypoint.new(1, 1)})
Crescents.LightEmission = 10
Crescents.Color = ColorSequence.new({ColorSequenceKeypoint.new(0, Color3.fromRGB(160, 96, 255)), ColorSequenceKeypoint.new(0.3160622, Color3.fromRGB(160, 96, 255)), ColorSequenceKeypoint.new(0.5146805, Color3.fromRGB(154, 82, 255)), ColorSequenceKeypoint.new(1, Color3.fromRGB(160, 96, 255))})
Crescents.VelocitySpread = -360
Crescents.Speed = NumberRange.new(0.0826858, 0.0826858)
Crescents.Brightness = 4
Crescents.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.398774, 8.8026266, 2.2834616), NumberSequenceKeypoint.new(1, 11.477972, 1.860431)})
Crescents.Enabled = false
Crescents.ZOffset = 0.4542207
Crescents.Rate = 50
Crescents.Texture = "rbxassetid://12509373457"
Crescents.RotSpeed = NumberRange.new(800, 1000)
Crescents.Rotation = NumberRange.new(-360, 360)
Crescents.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Crescents.Parent = Attachment

Part.Parent = workspace
end

do --// Atomic Slash

local Part = Instance.new("Part")

Part.Parent = ReplicatedStorage

local Attachment = Instance.new("Attachment")
Attachment.Parent = Part

HitEffectModule.Locals.Type["Atomic Slash"] = Attachment

local Crescents = Instance.new("ParticleEmitter")
Crescents.Name = "Crescents"
Crescents.Lifetime = NumberRange.new(0.19, 0.38)
Crescents.SpreadAngle = Vector2.new(-360, 360)
Crescents.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1932907, 0), NumberSequenceKeypoint.new(0.778754, 0), NumberSequenceKeypoint.new(1, 1)})
Crescents.LightEmission = 10
Crescents.Color = ColorSequence.new(Color3.fromRGB(160, 96, 255))
Crescents.VelocitySpread = -360
Crescents.Speed = NumberRange.new(0.0826858, 0.0826858)
Crescents.Brightness = 4
Crescents.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.398774, 8.8026266, 2.2834616), NumberSequenceKeypoint.new(1, 11.477972, 1.860431)})
Crescents.Enabled = false
Crescents.ZOffset = 0.4542207
Crescents.Rate = 50
Crescents.Texture = "rbxassetid://12509373457"
Crescents.RotSpeed = NumberRange.new(800, 1000)
Crescents.Rotation = NumberRange.new(-360, 360)
Crescents.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Crescents.Parent = Attachment

local Glow = Instance.new("ParticleEmitter")
Glow.Name = "Glow"
Glow.Lifetime = NumberRange.new(0.16, 0.16)
Glow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1421725, 0.6182796), NumberSequenceKeypoint.new(1, 1)})
Glow.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Glow.Speed = NumberRange.new(0, 0)
Glow.Brightness = 5
Glow.Size = NumberSequence.new(9.1873131, 16.5032349)
Glow.Enabled = false
Glow.ZOffset = -0.0565939
Glow.Rate = 50
Glow.Texture = "rbxassetid://8708637750"
Glow.Parent = Attachment

local Effect = Instance.new("ParticleEmitter")
Effect.Name = "Effect"
Effect.Lifetime = NumberRange.new(0.4, 0.7)
Effect.FlipbookLayout = Enum.ParticleFlipbookLayout.Grid4x4
Effect.SpreadAngle = Vector2.new(360, -360)
Effect.LockedToPart = true
Effect.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.1070999, 0.19375), NumberSequenceKeypoint.new(0.7761194, 0.88125), NumberSequenceKeypoint.new(1, 1)})
Effect.LightEmission = 1
Effect.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Effect.Drag = 1
Effect.VelocitySpread = 360
Effect.Speed = NumberRange.new(0.0036749, 0.0036749)
Effect.Brightness = 2.0999999
Effect.Size = NumberSequence.new(6.9680691, 9.9213123)
Effect.Enabled = false
Effect.ZOffset = 0.4777403
Effect.Rate = 50
Effect.Texture = "rbxassetid://9484012464"
Effect.RotSpeed = NumberRange.new(-150, -150)
Effect.FlipbookMode = Enum.ParticleFlipbookMode.OneShot
Effect.Rotation = NumberRange.new(50, 50)
Effect.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
Effect.Parent = Attachment

local Gradient1 = Instance.new("ParticleEmitter")
Gradient1.Name = "Gradient1"
Gradient1.Lifetime = NumberRange.new(0.3, 0.3)
Gradient1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.15, 0.3), NumberSequenceKeypoint.new(1, 1)})
Gradient1.Color = ColorSequence.new(Color3.fromRGB(173, 82, 252))
Gradient1.Speed = NumberRange.new(0, 0)
Gradient1.Brightness = 6
Gradient1.Size = NumberSequence.new(0, 11.6261358)
Gradient1.Enabled = false
Gradient1.ZOffset = 0.9187313
Gradient1.Rate = 50
Gradient1.Texture = "rbxassetid://8196169974"
Gradient1.Parent = Attachment

local Shards = Instance.new("ParticleEmitter")
Shards.Name = "Shards"
Shards.Lifetime = NumberRange.new(0.19, 0.7)
Shards.SpreadAngle = Vector2.new(-90, 90)
Shards.Color = ColorSequence.new(Color3.fromRGB(179, 145, 253))
Shards.Drag = 10
Shards.VelocitySpread = -90
Shards.Squash = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.5705521, 0.4125001), NumberSequenceKeypoint.new(1, -0.9375)})
Shards.Speed = NumberRange.new(97.7530136, 146.9970093)
Shards.Brightness = 4
Shards.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.284774, 1.2389833, 0.1534118), NumberSequenceKeypoint.new(1, 0)})
Shards.Enabled = false
Shards.Acceleration = Vector3.new(0, -56.961341857910156, 0)
Shards.ZOffset = 0.5705321
Shards.Rate = 50
Shards.Texture = "rbxassetid://8030734851"
Shards.Rotation = NumberRange.new(90, 90)
Shards.Orientation = Enum.ParticleOrientation.VelocityParallel
Shards.Parent = Attachment

Part.Parent = workspace
end

do
    local part = Instance.new("Part")
    part.Parent = ReplicatedStorage
    local attachment = Instance.new("Attachment")
    attachment.Name = "Attachment"
    attachment.Parent = part
    HitEffectModule.Locals.Type["Nova"] = attachment

    local function createParticleEmitter(acceleration)
        local emitter = Instance.new("ParticleEmitter")
        emitter.Name = "ParticleEmitter"
        emitter.Acceleration = acceleration
        emitter.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 0, 0)),
            ColorSequenceKeypoint.new(0.495, HitEffectModule.Settings.HitEffect.Color),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
        })
        emitter.Lifetime = NumberRange.new(0.5, 0.5)
        emitter.LightEmission = 1
        emitter.LockedToPart = true
        emitter.Rate = 1
        emitter.Rotation = NumberRange.new(0, 360)
        emitter.Size = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 1),
            NumberSequenceKeypoint.new(1, 10),
            NumberSequenceKeypoint.new(1, 1)
        })
        emitter.Speed = NumberRange.new(0, 0)
        emitter.Texture = "rbxassetid://1084991215"
        emitter.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0),
            NumberSequenceKeypoint.new(0, 0.1),
            NumberSequenceKeypoint.new(0.534, 0.25),
            NumberSequenceKeypoint.new(1, 0.5),
            NumberSequenceKeypoint.new(1, 0)
        })
        emitter.ZOffset = 1
        emitter.Parent = attachment
        return emitter
    end

    createParticleEmitter(Vector3.new(0, 0, 1))
    local perpendicularEmitter = createParticleEmitter(Vector3.new(0, 1, -0.001))
    perpendicularEmitter.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
end

HitEffectModule.Functions.Effect = function(character, color)
    if not character then return end
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end

    local effectAttachment = HitEffectModule.Locals.Type[TargetAimbot.HitEffectType]:Clone()
    effectAttachment.Parent = humanoidRootPart

    for _, emitter in pairs(effectAttachment:GetChildren()) do
        if emitter:IsA("ParticleEmitter") then
            emitter.Color = ColorSequence.new({
                ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 0, 0)),
                ColorSequenceKeypoint.new(0.495, TargetAimbot.HitEffectColor),
                ColorSequenceKeypoint.new(1, TargetAimbot.HitEffectColor)
            })
            
            if TargetAimbot.HitEffect then
                emitter:Emit()
            end
        end
    end

    task.delay(2, function()
        effectAttachment:Destroy()
    end)
end

local function PlayHitSound()
    if TargetAimbot.HitSounds and hitsounds[TargetAimbot.HitSound] then
        local sound = Instance.new("Sound")
        sound.SoundId = hitsounds[TargetAimbot.HitSound]
        sound.Parent = SoundService
        sound:Play()
        sound.Ended:Connect(function()
            sound:Destroy()
        end)
    end
end

local HitChamsSkeleton = LPH_NO_VIRTUALIZE(function(Player)
    if not TargetAimbot.HitSkele then return end

    if Player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
        local bones = {
            {"Head", "UpperTorso"},
            {"UpperTorso", "LowerTorso"},
            {"UpperTorso", "RightUpperArm"},
            {"RightUpperArm", "RightLowerArm"},
            {"RightLowerArm", "RightHand"},
            {"UpperTorso", "LeftUpperArm"},
            {"LeftUpperArm", "LeftLowerArm"},
            {"LeftLowerArm", "LeftHand"},
            {"LowerTorso", "RightUpperLeg"},
            {"RightUpperLeg", "RightLowerLeg"},
            {"RightLowerLeg", "RightFoot"},
            {"LowerTorso", "LeftUpperLeg"},
            {"LeftUpperLeg", "LeftLowerLeg"},
            {"LeftLowerLeg", "LeftFoot"}
        }

        local lines = {}

        for _, bonePair in ipairs(bones) do
            local parentBone = Player.Character:FindFirstChild(bonePair[1])
            local childBone = Player.Character:FindFirstChild(bonePair[2])

            if parentBone and childBone then
                local line = Instance.new("Part")
                line.Size = Vector3.new(0.02, 0.02, (parentBone.Position - childBone.Position).Magnitude)
                line.CFrame = CFrame.new(parentBone.Position, childBone.Position) * CFrame.new(0, 0, -line.Size.Z / 2)
                line.Anchored = true
                line.CanCollide = false
                line.Transparency = TargetAimbot.HitChamsTransparency
                line.Color = TargetAimbot.SkeleColor
                line.Material = Enum.Material.Neon
                line.Parent = workspace

                table.insert(lines, line)
            end
        end

        task.delay(TargetAimbot.HitChamsDuration, function()
            for _, line in ipairs(lines) do
                if line and line.Parent then
                    line:Destroy()
                end
            end
        end)
    end
end)

local TweenService = game:GetService("TweenService")

local function HitChams(Player)
    if not TargetAimbot.HitChams then return end

    if Player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
        Player.Character.Archivable = true
        local Cloned = Player.Character:Clone()
        Cloned.Name = "Player Clone"

        local BodyParts = {
            "Head", "UpperTorso", "LowerTorso",
            "LeftUpperArm", "LeftLowerArm", "LeftHand",
            "RightUpperArm", "RightLowerArm", "RightHand",
            "LeftUpperLeg", "LeftLowerLeg", "LeftFoot",
            "RightUpperLeg", "RightLowerLeg", "RightFoot"
        }

        for _, Part in ipairs(Cloned:GetChildren()) do
            if Part:IsA("BasePart") then
                local PartValid = false
                for _, validPart in ipairs(BodyParts) do
                    if Part.Name == validPart then
                        PartValid = true
                        break
                    end
                end
                
                if not PartValid then
                    Part:Destroy()
                end
            elseif Part:IsA("Accessory") or Part:IsA("Tool") or Part.Name == "face" or Part:IsA("Shirt") or Part:IsA("Pants") or Part:IsA("Hat") then
                Part:Destroy()
            end
        end

        if Cloned:FindFirstChild("Humanoid") then
            Cloned.Humanoid:Destroy()
        end

        for _, BodyPart in ipairs(Cloned:GetChildren()) do
            if BodyPart:IsA("BasePart") then
                BodyPart.CanCollide = false
                BodyPart.Anchored = true
                BodyPart.Transparency = TargetAimbot.HitChamsTransparency
                BodyPart.Color = TargetAimbot.HitChamsColor
                BodyPart.Material = TargetAimbot.HitChamsMaterial
            end
        end

        if Cloned:FindFirstChild("Head") then
            local Head = Cloned.Head
            Head.Transparency = TargetAimbot.HitChamsTransparency
            Head.Color = TargetAimbot.HitChamsColor
            Head.Material = TargetAimbot.HitChamsMaterial

            if Head:FindFirstChild("face") then
                Head.face:Destroy()
            end
        end

        Cloned.Parent = game.Workspace

        local tweenInfo = TweenInfo.new(
            TargetAimbot.HitChamsDuration,
            Enum.EasingStyle.Sine,
            Enum.EasingDirection.InOut,
            0,
            true
        )

        for _, BodyPart in ipairs(Cloned:GetChildren()) do
            if BodyPart:IsA("BasePart") then
                local tween = TweenService:Create(BodyPart, tweenInfo, { Transparency = 1 })
                tween:Play()
            end
        end

        task.delay(TargetAimbot.HitChamsDuration, function()
            if Cloned and Cloned.Parent then
                Cloned:Destroy()
            end
        end)
    end
end
local target_health = nil

local function updatetarget_health()
    if TargBindEnabled and TargetPlr and TargetPlr.Character then
        local humanoid = TargetPlr.Character:FindFirstChild("Humanoid")
        if humanoid then
            local currentHealth = humanoid.Health
            if currentHealth < target_health then
                if Hitnotify then
                    Library:Notification('Garbage<font color="#ADD8E6">.hook</font>  >  ' .. '+1 Hit | ' .. tostring(getgenv().Sentinel.SelectedPart) .. ' | Target : ' .. TargetPlr.DisplayName, 1.5)
                end
                
                PlayHitSound()
HitChamsSkeleton(TargetPlr)
                HitEffectModule.Functions.Effect(TargetPlr.Character)
                HitChams(TargetPlr)
            end
            target_health = currentHealth
        end
    end
end


RunService.RenderStepped:Connect(function()
    if TargetAimbot.Enabled and TargBindEnabled and TargetAimbot.Highlight and TargetPlr and TargetPlr.Character and Highlight then
        TargHighlight.FillColor = TargetAimbot.HighlightColor1
TargHighlight.OutlineColor = TargetAimbot.HighlightColor2
TargHighlight.Adornee = TargetPlr.Character
        TargHighlight.Enabled = true
    else
        TargHighlight.Adornee = nil
        TargHighlight.Enabled = false
    end
end)




local client = game.Players.LocalPlayer
local camera = workspace.CurrentCamera
local visuals = {}
local ignore_folder = Instance.new("Folder", game:GetService("Workspace"))

visuals["visualize_model"] = game:GetObjects("rbxassetid://9474737816")[1]
visuals["visualize_model"].Head.Face:Destroy()
for _, v in pairs(visuals["visualize_model"]:GetChildren()) do
    v.Transparency = v.Name == "HumanoidRootPart" and 1 or 0.7
    v.Material = "Neon"
    v.Color = Color3.fromRGB(153, 0, 153)
    v.CanCollide = false
end

game:GetService("RunService").Heartbeat:Connect(function()
    if TargetAimbot.CSync.Enabled and TargetPlr then
        if TargBindEnabled and TargetAimbot.CSync.Type == "Random" then
            client.Character.HumanoidRootPart.CFrame = CFrame.new(TargetPlr.Character.HumanoidRootPart.Position + Vector3.new(
                math.random(-TargetAimbot.CSync.RandomAmount, TargetAimbot.CSync.RandomAmount),
                math.random(0, TargetAimbot.CSync.RandomAmount),
                math.random(-TargetAimbot.CSync.RandomAmount, TargetAimbot.CSync.RandomAmount)
            )) * CFrame.Angles(
                math.rad(math.random(0, 360)),
                math.rad(math.random(0, 360)),
                math.rad(math.random(0, 360))
            )
        elseif TargBindEnabled and TargetAimbot.CSync.Type == "Orbit" then
            local current_time = tick()
            client.Character.HumanoidRootPart.CFrame = CFrame.new(TargetPlr.Character.HumanoidRootPart.Position) *
                CFrame.Angles(0, 2 * math.pi * current_time * TargetAimbot.CSync.Speed % (2 * math.pi), 0) *
                CFrame.new(0, TargetAimbot.CSync.Height, TargetAimbot.CSync.Distance)
        end

        if TargetAimbot.CSync.VisualizeMovement then
            visuals["visualize_model"].Parent = ignore_folder
            visuals["visualize_model"]:SetPrimaryPartCFrame(client.Character.HumanoidRootPart.CFrame)

            for _, part in pairs(visuals["visualize_model"]:GetChildren()) do
                part.Color = TargetAimbot.CSync.Color
                part.Transparency = 0
            end
        else
            visuals["visualize_model"].Parent = nil
        end

        camera.CameraSubject = client.Character.Humanoid
    else
        visuals["visualize_model"].Parent = nil
    end
end)




local FOV43 = Drawing.new("Circle")
FOV43.Transparency = 0.5
FOV43.Thickness = 2
FOV43.Color = Color3.new(1, 0, 0)
FOV43.Filled = false
FOV43.Radius = 250
FOV43.Position = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2)
FOV43.Visible = false


local Sigmaballs = Instance.new("ScreenGui")
Sigmaballs.Name = "Sigmaballs"
Sigmaballs.Parent = game.CoreGui
Sigmaballs.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
Sigmaballs.ResetOnSpawn = false

local Button = Instance.new("TextButton")
Button.Name = "FlyButton"
Button.Parent = Sigmaballs
Button.Active = true
Button.Draggable = true
Button.BackgroundColor3 = Color3.fromRGB(28, 28, 48)
Button.BackgroundTransparency = 0
Button.BorderSizePixel = 0
Button.Size = UDim2.new(0, 150, 0, 50)
Button.Position = UDim2.new(0.5, -75, 0.5, -25)
Button.Font = Enum.Font.ArialBold
Button.Text = "Lock: " .. "<font color='rgb(255, 0, 0)'>OFF</font>"
Button.TextColor3 = Color3.fromRGB(255, 255, 255)
Button.TextSize = 25
Button.RichText = true
Button.TextStrokeTransparency = 0.5

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = Button

local UIStroke = Instance.new("UIStroke")
UIStroke.Parent = Button
UIStroke.Thickness = 2
UIStroke.Color = Color3.fromRGB(16, 16, 32)


local stk = Instance.new("UIStroke")
stk.Parent = Button
stk.Thickness = 3
stk.Color = Color3.fromRGB(16, 16, 32)
stk.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    
local bottun = Instance.new("TextButton")
bottun.Name = "FlyButton"
bottun.Parent = Sigmaballs
bottun.Active = true
bottun.Draggable = true
bottun.BackgroundColor3 = Color3.fromRGB(28, 28, 48)
bottun.BackgroundTransparency = 0
bottun.BorderSizePixel = 0
bottun.Size = UDim2.new(0, 150, 0, 50)
bottun.Position = UDim2.new(0.5, -75, 0.5, -25)
bottun.Font = Enum.Font.ArialBold
bottun.Text = "UI: " .. "<font color='rgb(255, 0, 0)'>OFF</font>"
bottun.TextColor3 = Color3.fromRGB(255, 255, 255)
bottun.TextSize = 25
bottun.RichText = true
bottun.TextStrokeTransparency = 0.5

local uic = Instance.new("UICorner")
uic.CornerRadius = UDim.new(0, 8)
uic.Parent = bottun

local ustrk = Instance.new("UIStroke")
ustrk.Parent = bottun
ustrk.Thickness = 2
ustrk.Color = Color3.fromRGB(16, 16, 32)


local stc = Instance.new("UIStroke")
stc.Parent = bottun
stc.Thickness = 3
stc.Color = Color3.fromRGB(16, 16, 32)
stc.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local Open = false

bottun.MouseButton1Click:Connect(function()
    Open = not Open
    Library:SetOpen(Open)
    
    if Open == false then
        bottun.Text = "UI: " .. "<font color='rgb(255, 0, 0)'>OFF</font>"
    else
        bottun.Text = "UI: " .. "<font color='rgb(0, 255, 0)'>ON</font>"
    end
end)


function SigmaOhioPlayer()
    local closestPlayer
    local shortestDistance = math.huge
    local player = game.Players.LocalPlayer
    local CC = game:GetService("Workspace").CurrentCamera
    local screenCenter = Vector2.new(CC.ViewportSize.X / 2, CC.ViewportSize.Y / 2)
    local fovRadius = FOV43.Radius
    local viewportSize = CC.ViewportSize

    for i, v in pairs(game.Players:GetPlayers()) do
        if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") 
           and v.Character.Humanoid.Health > 0 and v.Character:FindFirstChild("HumanoidRootPart") then
            local pos, onScreen = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            
            if onScreen and pos.X > 0 and pos.Y > 0 
               and pos.X < viewportSize.X and pos.Y < viewportSize.Y then
                local magnitude = (Vector2.new(pos.X, pos.Y) - screenCenter).magnitude
                if magnitude < fovRadius and magnitude < shortestDistance then
                    closestPlayer = v
                    shortestDistance = magnitude
                end
            end
        end
    end
    
    return closestPlayer
end



local enabled = false


toggle_lock = function()
    if TargetAimbot.Enabled then
        local closest = SigmaOhioPlayer()
        if TargBindEnabled and TargetPlr then
            TargBindEnabled = false
            target_health = nil
            TargetPlr = nil
            Workspace.CurrentCamera.CameraSubject = LocalPlayer.Character.Humanoid
            if TargetAimbot.LookAt then
                LocalPlayer.Character.Humanoid.AutoRotate = true
            end
            Button.Text = "Lock: " .. "<font color='rgb(255, 0, 0)'>OFF</font>"
            Library:Notification("Untargeted", 2)
        else
            TargBindEnabled = true
            TargetPlr = closest
            if TargetPlr.Character and TargetPlr.Character:FindFirstChild("Humanoid") then
                target_health = TargetPlr.Character.Humanoid.Health
            else
                return
            end
            Button.Text = "Lock: " .. "<font color='rgb(0, 255, 0)'>ON</font>"
            Library:Notification("Target Locked: " .. tostring(TargetPlr.DisplayName), 2)
        end
    end
end

Button.MouseButton1Click:Connect(toggle_lock)

local RunService = game:GetService("RunService")
local Tracer = Drawing.new("Line")
Tracer.Visible = false
Tracer.Color = Library.Accent
Tracer.Thickness = 2
Tracer.Transparency = 1
RunService.RenderStepped:Connect(function()
    if TargetPlr and line and TargetPlr.Character and TargetPlr.Character:FindFirstChild("HumanoidRootPart") then
        local characterPos = TargetPlr.Character.HumanoidRootPart.Position
        local camera = workspace.CurrentCamera
        local screenPos, onScreen = camera:WorldToScreenPoint(characterPos)

        if onScreen then
            Tracer.Visible = true
            Tracer.From = Vector2.new(LocalPlayer:GetMouse().X, LocalPlayer:GetMouse().Y + 36)
            Tracer.To = Vector2.new(screenPos.X, screenPos.Y)
        else
            Tracer.Visible = false
        end
    else
        Tracer.Visible = false
    end
end)

game:GetService("RunService").RenderStepped:Connect(
    function()
        if TargetAimbot.CSync.Spectate and TargetPlr and TargetPlr.Character then
game.Workspace.CurrentCamera.CameraSubject = TargetPlr.Character
   else
game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
        end
   end
)


local YunDrawingApi = loadstring(game:HttpGet('https://raw.githubusercontent.com/caIIed/Librarys/main/Yun%20Api.lua', true))()

local TargetCircle = YunDrawingApi:New3DCircle() 
TargetCircle.Visible = false
TargetCircle.ZIndex = 1
TargetCircle.Transparency = 1
TargetCircle.Color = TargetAimbot.CSync.Color
TargetCircle.Thickness = 1
TargetCircle.Radius = 2
TargetCircle.Rotation = Vector2.new(2, 0)

RunService.RenderStepped:Connect(function()
    if TargetAimbot.CSync.Circle and TargetPlr then
        TargetCircle.Visible = true
        TargetCircle.Position = TargetPlr.Character.HumanoidRootPart.Position
    else
        TargetCircle.Visible = false
    end;
end)



UserInputService.InputBegan:Connect(function(input, processed)
    if not processed and input.KeyCode == Enum.KeyCode.DPadDown then
        toggle_lock()
    end
end)



getgenv().Sentinel.LockType = "Namecall"
getgenv().Sentinel.RESOLVER = "MoveDirection"
getgenv().Sentinel.PredType = "Position"

local game_support = loadstring(game:HttpGet("https://raw.githubusercontent.com/LuarmorIsGay5/automatic-spork/refs/heads/main/Argument.txt",true))()

local function getRemoteInfo()
    local placeId = game.PlaceId
    return game_support[placeId] or { Remote = "MainEvent", Argument = "UpdateMousePos" }
end



local remoteInfo = getRemoteInfo()

local RunService = game:GetService("RunService")

local function predictedposition()
    local selectedPart = getgenv().Sentinel.SelectedPart
    local targetPart = TargetPlr.Character[selectedPart]

    if targetPart then
        local velocity
        if not getgenv().Sentinel.ResolverEnabled then
            velocity = targetPart.Velocity
        else
            if getgenv().Sentinel.RESOLVER == "MoveDirection" then
                velocity = TargetPlr.Character.Humanoid.MoveDirection * TargetPlr.Character.Humanoid.WalkSpeed
            elseif getgenv().Sentinel.RESOLVER == "LookVector" then
                velocity = targetPart.CFrame.LookVector * getgenv().Sentinel.HorizontalPrediction * 1.2
            else
                velocity = targetPart.Velocity
            end
        end

        local horizontalPrediction = getgenv().Sentinel.HorizontalPrediction

        local predictedPosition = Vector3.new(
            targetPart.Position.X + (velocity.X * horizontalPrediction),
            targetPart.Position.Y,
            targetPart.Position.Z + (velocity.Z * horizontalPrediction)
        )

        return predictedPosition
    end
end

RunService.PostSimulation:Connect(function(DeltaTime)
    if Sentinel.Enabled then
        if Sentinel.LockType == "Index" then
            local LocalPlayer = game.Players.LocalPlayer
            local LocalFramework = LocalPlayer.PlayerGui:WaitForChild("Framework", 1e9)

            if LocalFramework then
                local FrameworkEnvironment = getsenv(LocalFramework)

                if FrameworkEnvironment._G and FrameworkEnvironment._G.MOUSE_POSITION then
                    if TargetPlr then
                        FrameworkEnvironment._G.MOUSE_POSITION = predictedposition() 
                    end
                end
            end
        end
    end
end)


local remoteInfo = getRemoteInfo()
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)

local Vect3 = Vector3.new


do -- // Hooking
    __namecall = hookmetamethod(game, "__namecall", newcclosure(function(Self, ...)
        local args, method = {...}, tostring(getnamecallmethod())

        if not checkcaller() and method == "FireServer" then
            for i, arg in pairs(args) do
                if typeof(arg) == "Vector3" then
                    if TargetPlr and getgenv().Sentinel.Enabled and getgenv().Sentinel.LockType == "Namecall" then
                        local selectedPart = getgenv().Sentinel.SelectedPart
                        local targetPart = TargetPlr.Character[selectedPart]

                        if targetPart then
                            local velocity
                            if getgenv().Sentinel.ResolverEnabled then
                                if getgenv().Sentinel.RESOLVER == "MoveDirection" then
                                    velocity = TargetPlr.Character.Humanoid.MoveDirection * TargetPlr.Character.Humanoid.WalkSpeed
                                elseif getgenv().Sentinel.RESOLVER == "LookVector" then
                                    velocity = targetPart.CFrame.LookVector * getgenv().Sentinel.HorizontalPrediction * 1.0
                                else
                                    velocity = targetPart.Velocity
                                end
                            else
                                velocity = targetPart.Velocity
                            end

                            local horizontalPrediction = getgenv().Sentinel.HorizontalPrediction

                            args[i] = targetPart.Position + (targetPart.Velocity * horizontalPrediction)
                        end
                    end
                    return __namecall(Self, unpack(args))
                elseif type(arg) == "table" then
                    for index, element in ipairs(arg) do
                        if typeof(element) == "Vector3" then
                            if TargetPlr and getgenv().Sentinel.Enabled and getgenv().Sentinel.LockType == "Namecall" then
                                local selectedPart = getgenv().Sentinel.SelectedPart
                                local targetPart = TargetPlr.Character[selectedPart]

                                if targetPart then
                                    local velocity
                                    if getgenv().Sentinel.ResolverEnabled then
                                        if getgenv().Sentinel.RESOLVER == "MoveDirection" then
                                            velocity = TargetPlr.Character.Humanoid.MoveDirection * TargetPlr.Character.Humanoid.WalkSpeed
                                        elseif getgenv().Sentinel.RESOLVER == "LookVector" then
                                            velocity = targetPart.CFrame.LookVector * getgenv().Sentinel.HorizontalPrediction * 1.0
                                        else
                                            velocity = targetPart.Velocity
                                        end
                                    else
                                        velocity = targetPart.Velocity
                                    end

                                    local horizontalPrediction = getgenv().Sentinel.HorizontalPrediction

                                    arg[index] = targetPart.Position + (targetPart.Velocity * horizontalPrediction)
                                end
                            end
                        end
                    end
                end
            end
            return __namecall(Self, unpack(args))
        end

        return __namecall(Self, ...)
    end))
end

local players = game:GetService("Players")
local client = players.LocalPlayer
local function AutoShoot()
    if TargetPlr then
        local character = client.Character
        if character then
            if Sentinel.ForceFieldCheck and TargetPlr.Character:FindFirstChildOfClass("ForceField") then
                return
            end
            local tool = character:FindFirstChildOfClass("Tool")
            if tool and tool:IsA("Tool") then
                tool:Activate()
            end
        end
    end
end



local targetSigm99928 = getgenv().Sentinel.ShootDelay 
local targetSigmaPOBALLs = nil
local Shot2ing = false

local function checkTarget()
    if TargetPlr and TargetPlr.Character then
        local humanoid = TargetPlr.Character:FindFirstChildOfClass("Humanoid")
        local humanoidRootPart = TargetPlr.Character:FindFirstChild("HumanoidRootPart")

        if humanoid and humanoidRootPart then
            local SigmaAir = humanoid:GetState() == Enum.HumanoidStateType.Freefall

            if SigmaAir and getgenv().Sentinel.AutoAir then
                if not targetSigmaPOBALLs then
                    targetSigmaPOBALLs = tick()
                else
                    local airDuration = tick() - targetSigmaPOBALLs
                    if airDuration >= targetSigm99928 then
                        if not Shot2ing then
                            Shot2ing = true
                            while TargetPlr and TargetPlr.Character and SigmaAir do
                               AutoShoot()
                                wait(0.001)

                                SigmaAir = humanoid:GetState() == Enum.HumanoidStateType.Freefall

                                if not SigmaAir then
                                    Shot2ing = false
                                    targetSigmaPOBALLs = nil -- Reset the start time
                                    break
                                end
                            end
                            Shot2ing = false
                        end
                    end
                end
            else
                targetSigmaPOBALLs = nil
                Shot2ing = false
            end
        end
    end
end




function LookAtPlayer(Target)
    local localChar = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
    local localHumanoidRootPart = localChar:FindFirstChild("HumanoidRootPart")

    if localHumanoidRootPart then
        if getgenv().Sentinel and getgenv().Sentinel.LookAt and not MCenabled then
            if Target and TargetPlr.Character and TargetPlr.Character:FindFirstChild("HumanoidRootPart") then
                local targetHumanoidRootPart = TargetPlr.Character.HumanoidRootPart
                
                local targetPosition = targetHumanoidRootPart.Position
                local localPosition = localHumanoidRootPart.Position
                
                local horizontalDirection = Vector3.new(targetPosition.X - localPosition.X, 0, targetPosition.Z - localPosition.Z).unit
                
                localHumanoidRootPart.CFrame = CFrame.new(localPosition, localPosition + horizontalDirection)
                localChar.Humanoid.AutoRotate = false
            end
        else
            localChar.Humanoid.AutoRotate = true
        end
    end
    
    if not (Target and TargetPlr.Character and TargetPlr.Character:FindFirstChild("HumanoidRootPart")) then
        localChar.Humanoid.AutoRotate = true
    end
end

local function NearestPart(TargetPlr)
    local BodyParts = {
        "Head", "UpperTorso", "LowerTorso", 
        "LeftUpperArm", "LeftLowerArm", "LeftHand", 
        "RightUpperArm", "RightLowerArm", "RightHand", 
        "LeftUpperLeg", "LeftLowerLeg", "LeftFoot", 
        "RightUpperLeg", "RightLowerLeg", "RightFoot"
    }

    local selectedPartName = getgenv().Sentinel.SelectedPart

    if TargetPlr and TargetPlr.Character then
        if getgenv().Sentinel.NearestPart then
            local minDistance = math.huge
            local nearestPart = nil
            
            for _, partName in pairs(BodyParts) do
                local part = TargetPlr.Character:FindFirstChild(partName)
                if part then
                    local distance = (part.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if distance < minDistance then
                        minDistance = distance
                        nearestPart = part
                    end
                end
            end
            
            if nearestPart then
                getgenv().Sentinel.SelectedPart = nearestPart.Name
            end
        else
            getgenv().Sentinel.SelectedPart = selectedPartName
        end
    end
end


function inAir()
    if TargetPlr and TargetPlr.Character and TargetPlr.Character:FindFirstChild("Humanoid") then
        local SigmaTuah = TargetPlr.Character.Humanoid:GetState() == Enum.HumanoidStateType.Freefall
        if SigmaTuah then
            getgenv().Sentinel.jumpoffset = getgenv().Sentinel.jumpoffset2
        else
            getgenv().Sentinel.jumpoffset = 0
        end
    end
end



RunService.Stepped:Connect(function()
    checkTarget()
    updatetarget_health()
    LookAtPlayer(TargetPlr)
    NearestPart(TargetPlr)
    inAir()
    if not getgenv().Sentinel.AutoPrediction then
        getgenv().Sentinel.HorizontalPrediction2 = getgenv().Sentinel.HorizontalPrediction2
        getgenv().Sentinel.VerticalPrediction = getgenv().Sentinel.HorizontalPrediction2
    end
end)

RunService.Heartbeat:Connect(function()
    if Script.Locals.GrenadeTP.Enabled and TargetPlr and TargetPlr.Character and workspace:FindFirstChild("Ignored") then
        if workspace.Ignored:FindFirstChild("Handle") then
            workspace.Ignored.Handle.Position = TargetPlr.Character[getgenv().Sentinel.SelectedPart].Position + (TargetPlr.Character[getgenv().Sentinel.SelectedPart].Velocity * getgenv().Sentinel.HorizontalPrediction)
        end
    end
end)

if workspace:FindFirstChild("Ignored") then
    workspace.Ignored.ChildAdded:Connect(function(object)
        if Script.Locals.RocketTP.Enabled and 
           TargetPlr and 
           TargetPlr.Character and 
           (object.Name == "Model" or object.Name == "GrenadeLauncherAmmo") then
            
            local SkibidiGrenadeLauncher = object.Name == "GrenadeLauncherAmmo"
            local part = SkibidiGrenadeLauncher and object:WaitForChild("Main") or object:WaitForChild("Launcher")
            
            part.CFrame = CFrame.new(1, 1, 1)
            
            if not SkibidiGrenadeLauncher then
                part.BodyVelocity:Destroy()
                part.TouchInterest:Destroy()
            end
            
            local connection
            connection = RunService.PostSimulation:Connect(function()
                if TargetPlr and TargetPlr.Character then
                    part.CFrame = TargetPlr.Character.HumanoidRootPart.CFrame
                    part.Velocity = Vector3.new(0, 0.001, 0)
                end
            end)
            
            object.Destroying:Connect(function()
                connection:Disconnect()
            end)
        end
    end)
end

RunService.Heartbeat:Connect(function()
    if getgenv().Sentinel.Camera and TargetPlr and TargetPlr.Character and getgenv().Sentinel.SelectedPart then
        local camera = Workspace.CurrentCamera
        local selectedPart = getgenv().Sentinel.SelectedPart
        local targetPart = TargetPlr.Character[selectedPart]

        if targetPart then
            local velocity
            if getgenv().Sentinel.ResolverEnabled then
                if getgenv().Sentinel.RESOLVER == "MoveDirection" then
                    velocity = TargetPlr.Character.Humanoid.MoveDirection * TargetPlr.Character.Humanoid.WalkSpeed
                elseif getgenv().Sentinel.RESOLVER == "LookVector" then
                    velocity = targetPart.CFrame.LookVector * getgenv().Sentinel.HorizontalPrediction * 1.0
                else
                    velocity = targetPart.Velocity
                end
            else
                velocity = targetPart.Velocity
            end

            local jumpOffset = getgenv().Sentinel.jumpoffset or 0
            local fallOffset = getgenv().Sentinel.FallOffset or 0


            local horizontalPrediction = getgenv().Sentinel.CamPredHori
            local verticalPrediction = getgenv().Sentinel.CamPredVert

local zprediction = horizontalPrediction

if Sentinel.Sway then
   zprediction = 1
 else
   zprediction = horizontalPrediction
end

            local targetPosition = Vector3.new(
                targetPart.Position.X + (velocity.X * horizontalPrediction),
                targetPart.Position.Y + (velocity.Y * verticalPrediction),
                targetPart.Position.Z + (velocity.Z * zprediction)
            )

            local smoothness = getgenv().Sentinel.smoothness or 0.1
            local easingStyle = Enum.EasingStyle[getgenv().Sentinel.easingStyle] or Enum.EasingStyle.Quad
            local easingDirection = Enum.EasingDirection[getgenv().Sentinel.easingDirection] or Enum.EasingDirection.In

            camera.CFrame = camera.CFrame:Lerp(CFrame.new(camera.CFrame.Position, targetPosition), smoothness, easingStyle, easingDirection)
        end
    end
end)

local Plr = game.Players.LocalPlayer

Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(function(old, new)
    if getgenv().Sentinel.JumpBreak and new == Enum.HumanoidStateType.Freefall then
        wait(0.27)
        Plr.Character.HumanoidRootPart.Velocity = Vector3.new(0, -15, 0)
    end
end)


game:GetService("RunService").heartbeat:Connect(function()
    if getgenv().Desync == true then
        local abc = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity

        if getgenv().AntiLockType == "Behind" then
            getgenv().Direction = Vector3.new(0, 0, -1)
        elseif getgenv().AntiLockType == "Down" then
            getgenv().Direction = Vector3.new(0, -1, 0)
        elseif getgenv().AntiLockType == "ForWard" then
            getgenv().Direction = Vector3.new(0, 0, 1)
        elseif getgenv().AntiLockType == "Left" then
            getgenv().Direction = Vector3.new(-1, 0, 0)
        elseif getgenv().AntiLockType == "One" then
            getgenv().Direction = Vector3.new(1, 1, 1)
        elseif getgenv().AntiLockType == "Right" then
            getgenv().Direction = Vector3.new(1, 0, 0)
        elseif getgenv().AntiLockType == "Up" then
            getgenv().Direction = Vector3.new(0, 1, 0)
        elseif getgenv().AntiLockType == "Zero" then
            getgenv().Direction = Vector3.new(0, 0, 0)
        end
        
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = getgenv().Direction * (2^16)
        game:GetService("RunService").RenderStepped:Wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = abc
    end
end)

local Client = game.Players.LocalPlayer
local RunService = game:GetService("RunService")
local boolattp = true
local cframe_to_offset = function(origin, target)
    local actual_origin = origin * CFrame.new(Script.Locals.GunTP.Offset[1], Script.Locals.GunTP.Offset[2], Script.Locals.GunTP.Offset[3], 1, 0, 0, 0, 0, 1, 0, -1, 0)
    return actual_origin:ToObjectSpace(target):inverse();
end

local something_tp = function(Tool)
    local old_grip = Tool.Grip
    if TargetPlr and TargetPlr.Character then
        Tool.Parent = Client.Backpack
        Client.Character.RightHand.Anchored = false
        Tool.Grip = cframe_to_offset(Client.Character.RightHand.CFrame, TargetPlr.Character.HumanoidRootPart.CFrame)
        Client.Character.RightHand.Anchored = true
        Tool.Parent = Client.Character
        RunService.RenderStepped:Wait()
        Tool.Parent = Client.Backpack
        Client.Character.RightHand.Anchored = false
        Tool.Grip = old_grip
        Tool.Parent = Client.Character
    end
end

local bullet_teleport = function(Character)
    Character.ChildAdded:Connect(function(Child)
        if Script.Locals.GunTP.Enabled then
            if Child:IsA("Tool") then
                local Connection
                Connection = Child.Activated:Connect(function()
                    something_tp(Child)
                end)

                Character.ChildRemoved:Connect(function(RemovedChild)
                    if RemovedChild == Child then
                        Connection:Disconnect()
                    end
                end)
            end
        end
    end)
end

bullet_teleport(Client.Character)

Client.CharacterAdded:Connect(function()
bullet_teleport(Client.Character)
end)



RunService.Heartbeat:Connect(function()
    if Plr.Character and Plr.Character:FindFirstChild("HumanoidRootPart") then
        if getgenv().Sentinel and getgenv().Sentinel.network then
            sethiddenproperty(Plr.Character.HumanoidRootPart, "NetworkIsSleeping", true)
            task.wait()
            sethiddenproperty(Plr.Character.HumanoidRootPart, "NetworkIsSleeping", false)
            setfflag("S2PhysicsSenderRate", 2)
        else
            setfflag("S2PhysicsSenderRate", 13)
            sethiddenproperty(Plr.Character.HumanoidRootPart, "NetworkIsSleeping", false)
        end
    end
end)
