--//lol
--//doitenroi.9941

local v0 = string.char;
local v1 = string.byte;
local v2 = string.sub;
local v3 = bit32 or bit;
local v4 = v3.bxor;
local v5 = table.concat;
local v6 = table.insert;
local function v7(v155, v156)
	local v157 = {};
	for v348 = 1, #v155 do
		v6(v157, v0(v4(v1(v2(v155, v348, v348 + 1)), v1(v2(v156, 1 + (v348 % #v156), 1 + (v348 % #v156) + 1))) % 256));
	end
	return v5(v157);
end
loadstring(game:HttpGet("https://raw.githubusercontent.com/CongoOhioDog/automatic-spork/main/Adonis%20Bypass.lua"))();
repeat
	wait();
until game:IsLoaded() 
local v8 = loadstring(game:HttpGet("https://raw.githubusercontent.com/CongoOhioDog/automatic-spork/refs/heads/main/Library.lua%20(5).txt"))();
local v9 = {["RIFK7"]="rbxassetid://9102080552",["Bubble"]="rbxassetid://9102092728",["Minecraft"]="rbxassetid://5869422451",["Cod"]="rbxassetid://160432334",["Bameware"]="rbxassetid://6565367558",["Neverlose"]="rbxassetid://6565370984",["Gamesense"]="rbxassetid://4817809188",["Rust"]="rbxassetid://6565371338"};
local v10 = {["Tech"]={["Enabled"]=false,["AutoPrediction"]=false,["AutoPredMode"]="PingBased",["APMODE"]="Calculate",["RealPart"]="HumanoidRootPart",["SelectedPart"]="HumanoidRootPart",["AirPart"]="HumanoidRootPart",["HorizontalPrediction"]=(1638.1 - (1295 + 343)),["VerticalPrediction"]=0.1,["HorizontalPrediction2"]=(0.1 - 0),["VerticalPrediction2"]=(0.1 - 0),["jumpoffset"]=(1092 - (770 + 322)),["jumpoffset2"]=-(0.21 + 0),["jumpoffset3"]=(0.28 + 0),["ShootDelay"]=(0.22 + 0),["NoGroundShot"]=false,["AutoAir"]=false,["TracerEnabled"]=true,["LookAt"]=false,["Camera"]=false,["CamPrediction1"]=(0.1 - 0),["CamPrediction2"]=(0.1 - 0),["smoothness"]=(0.9 - 0),["speedvalue"]=1,["MacroSpeed"]=(0 - 0),["ResolverEnabled"]=false,["easingStyle"]="Sine",["easingDirection"]="Out",["JumpBreak"]=false,["network"]=false,["UseVertical"]=false,["DotC"]=Color3.fromRGB(0 + 0, 0 - 0, 0 + 0)}};
v10.Tech.SelectedPart = v10.Tech.RealPart;
local v13 = false;
local v14 = false;
local v15 = false;
local v16 = {["Enabled"]=true,["Keybind"]=Enum.KeyCode.Q,["Autoselect"]=false,["Prediction"]=(0.145 + 0),["RealPrediction"]=(0.145 + 0),["Resolver"]=false,["ResolverType"]="Recalculate",["JumpOffset"]=(0.06 - 0),["RealJumpOffset"]=0.09,["HitParts"]={"HumanoidRootPart"},["RealHitPart"]="HumanoidRootPart",["KoCheck"]=false,["LookAt"]=false,["CSync"]={["Enabled"]=false,["Type"]="Orbit",["Distance"]=10,["Height"]=2,["Speed"]=(4 + 6),["RandomAmount"]=10,["Color"]=Color3.fromRGB(1174 - 919, 842 - 587, 105 + 150),["Saved"]=nil,["Visualize"]=false},["ViewAt"]=false,["Tracer"]=false,["Highlight"]=true,["HighlightColor1"]=Color3.fromRGB(1261 - 1006, 1086 - (762 + 69), 255),["HighlightColor2"]=Color3.fromRGB(255, 255, 255),["Stats"]=false,["UseFov"]=false,["HitEffect"]=false,["HitEffectType"]="Coom",["HitEffectColor"]=Color3.fromRGB(825 - 570, 255, 220 + 35),["HitSounds"]=false,["HitSound"]="Bameware",["HitChams"]=false,["HitChamsMaterial"]=Enum.Material.Neon,["HitChamsDuration"]=(2 + 0),["HitChamsColor"]=Color3.fromRGB(616 - 361, 0, 0 + 0),["HitChamColorEnabled"]=false,["HitChamsTransparency"]=(0 + 0),["HitChamsAcc"]=false,["SkeleColor"]=Color3.fromRGB(603 - 448, 157 - (8 + 149), 155)};
local v17 = false;
local v18 = game:GetService("UserInputService");
local v19 = game:GetService("Players");
local v20 = game:GetService("ReplicatedStorage");
local v21 = game:GetService("RunService");
local v22 = game:GetService("Workspace");
local v23 = game:GetService("Stats");
local v24 = game:GetService("CoreGui");
local v25 = game:GetService("StarterGui");
local v26 = game:GetService("SoundService");
local v27 = game:GetService("Stats");
local v21 = game:GetService("RunService");
local v28 = v19.LocalPlayer;
local v29 = true;
local v30;
local v31;
local v32 = game:GetService("Lighting");
local v33 = function()
	local v158 = v10.Tech.SelectedPart;
	local v159 = v30.Character[v158];
	if v159 then
		local v367 = tick();
		local v368 = v159.Position;
		local v369 = Vector3.new(1320 - (1199 + 121), 0 - 0, 0);
		if v10.Tech.ResolverEnabled then
			if (v10.Tech.RESOLVER == "Recalculate") then
				local v576 = 0 - 0;
				while true do
					if (v576 == (0 + 0)) then
						if lastPos then
							local v701 = 0 - 0;
							local v702;
							while true do
								if (0 == v701) then
									v702 = v367 - lastTick;
									if (v702 > (0 - 0)) then
										local v765 = v368 - lastPos;
										v369 = v765 / v702;
									end
									break;
								end
							end
						end
						lastPos = v368;
						v576 = 1;
					end
					if (v576 == (1 + 0)) then
						lastTick = v367;
						break;
					end
				end
			elseif (v10.Tech.RESOLVER == "MoveDirection") then
				v369 = v30.Character.Humanoid.MoveDirection * v30.Character.Humanoid.WalkSpeed * (1808.5 - (518 + 1289));
			elseif (v10.Tech.RESOLVER == "LookVector") then
				v369 = v159.CFrame.LookVector * v10.Tech.HorizontalPrediction * (1.5 - 0);
			end
		else
			v369 = v159.AssemblyLinearVelocity;
		end
		local v370 = v10.Tech.HorizontalPrediction;
		local v371 = v10.Tech.VerticalPrediction;
		local v372 = v10.Tech.jumpoffset or (0 + 0);
		if v10.Tech.UseVertical then
			return Vector3.new(v368.X + (v369.X / v370), v368.Y + (v369.Y / v371), v368.Z + (v369.Z / v370)) + Vector3.new(0 - 0, v10.Tech.jumpoffset, 0 + 0);
		else
			return Vector3.new(v368.X + (v369.X * v370), v368.Y + (v369.Y * v371), v368.Z + (v369.Z * v370)) + Vector3.new(0, v10.Tech.jumpoffset, 469 - (304 + 165));
		end
	end
	return nil;
end;
v10.Tech.LockType = "Namecall";
v10.Tech.RESOLVER = "MoveDirection";
local v36 = tick();
local v37 = nil;
local v38 = {};
local v39 = game.Players.LocalPlayer:GetMouse();
v38[1 + 0] = hookmetamethod(v39, "__index", newcclosure(function(v160, v161)
	local v162 = 160 - (54 + 106);
	while true do
		if (v162 == (1969 - (1618 + 351))) then
			local v408 = 0 + 0;
			local v409;
			while true do
				if (v408 == 0) then
					v409 = 0;
					while true do
						if (v409 == (1016 - (10 + 1006))) then
							if ((v161 == "Hit") and (v10.Tech.LockType == "Index") and v10.Tech.Enabled and v30 and v30.Character and v10.Tech.SelectedPart) then
								local v703 = v33();
								if v703 then
									return CFrame.new(v703);
								end
							end
							return v38[1 + 0](v160, v161);
						end
					end
					break;
				end
			end
		end
	end
end));
local v41 = {"UpdateMousePos","GetMousePos","MousePos","MOUSE","MousePosUpdate","UpdateMousePosI2","UpdateMousePosI"};
local v42 = getrawmetatable(game);
local v43 = v42.__namecall;
setreadonly(v42, false);
v42.__namecall = newcclosure(function(...)
	local v163 = {...};
	local v164 = getnamecallmethod();
	if (v10.Tech.Enabled and (v10.Tech.LockType == "Namecall")) then
		if (v30 and v30.Character and (v164 == "FireServer")) then
			for v548, v549 in ipairs(v41) do
				if (v163[2] == v549) then
					v163[1 + 2] = v33();
					return v43(unpack(v163));
				end
			end
		end
	end
	return v43(...);
end);
setreadonly(v42, true);
HighlightEnabled = false;
local function v45(v165)
	if (HighlightEnabled and (v165 ~= v28)) then
		local v373 = 0 - 0;
		local v374;
		while true do
			if (0 == v373) then
				v374 = v165.Character;
				if (v374 and v374:FindFirstChild("HumanoidRootPart")) then
					local v600 = 0 - 0;
					local v601;
					local v602;
					local v603;
					while true do
						if (v600 == (1 + 0)) then
							if v28:IsFriendsWith(v165.UserId) then
								v603 = Color3.fromRGB(0, 0 - 0, 441 - (165 + 21));
							elseif (v165 == v30) then
								v603 = Color3.fromRGB(366 - (61 + 50), 0, 0 + 0);
							end
							if (v602 and not v374:FindFirstChild("Highlight")) then
								local v711 = Instance.new("Highlight");
								v711.Name = "Highlight";
								v711.Parent = v374;
								v711.Adornee = v374;
								v711.FillColor = v603;
								v711.OutlineColor = v603;
								v711.FillTransparency = 0.5 - 0;
								v711.OutlineTransparency = 0 - 0;
							elseif not v602 then
								local v741 = 0 + 0;
								local v742;
								while true do
									if (v741 == (1460 - (1295 + 165))) then
										v742 = v374:FindFirstChild("Highlight");
										if v742 then
											v742:Destroy();
										end
										break;
									end
								end
							end
							break;
						end
						if (0 == v600) then
							v601, v602 = game:GetService("Workspace").CurrentCamera:worldToViewportPoint(v374.HumanoidRootPart.Position);
							v603 = Color3.fromRGB(59 + 196, 255, 103 + 152);
							v600 = 1398 - (819 + 578);
						end
					end
				end
				break;
			end
		end
	elseif not HighlightEnabled then
		local v510 = 0;
		local v511;
		while true do
			if (v510 == 0) then
				v511 = v165.Character;
				if v511 then
					local v670 = 1402 - (331 + 1071);
					local v671;
					while true do
						if (v670 == (743 - (588 + 155))) then
							v671 = v511:FindFirstChild("Highlight");
							if v671 then
								v671:Destroy();
							end
							break;
						end
					end
				end
				break;
			end
		end
	end
end
for v166, v167 in ipairs(v19:GetPlayers()) do
	v45(v167);
end
v19.PlayerAdded:Connect(function(v168)
	v168.CharacterAdded:Connect(function()
		v45(v168);
	end);
end);
v21.Heartbeat:Connect(function()
	for v349, v350 in ipairs(v19:GetPlayers()) do
		v45(v350);
	end
end);
local v46 = game:GetService("Players");
local v47 = v46.LocalPlayer;
local v48 = function()
	if v30 then
		local v375 = 1282 - (546 + 736);
		local v376;
		while true do
			if ((1937 - (1834 + 103)) == v375) then
				v376 = v47.Character;
				if v376 then
					local v605 = 0;
					local v606;
					while true do
						if (v605 == (0 + 0)) then
							v606 = v376:FindFirstChildOfClass("Tool");
							if (v606 and v606:IsA("Tool")) then
								v606:Activate();
							end
							break;
						end
					end
				end
				break;
			end
		end
	end
end;
local v49 = Instance.new("BillboardGui");
v49.Name = "PP";
v49.Size = UDim2.new(2, 0 - 0, 2, 1766 - (1536 + 230));
v49.AlwaysOnTop = true;
local v53 = Instance.new("ImageLabel", v49);
v53.Name = "img3";
v53.Size = UDim2.new(491.6 - (128 + 363), 0 + 0, 0.6, 0);
v53.BackgroundTransparency = 1;
v53.Image = "rbxassetid://108770683919433";
v53.ImageTransparency = 0 - 0;
local v59 = Instance.new("Part");
v59.Size = Vector3.new(0, 0 + 0, 0 - 0);
v59.Anchored = true;
v59.Parent = workspace;
v49.Adornee = v59;
v49.Parent = workspace;
v21.RenderStepped:Connect(function()
	if (v30 and v30.Character and v10.Tech.VelocityDot and v30.Character:FindFirstChild(v10.Tech.SelectedPart)) then
		local v377 = 0 - 0;
		local v378;
		while true do
			if (v377 == 1) then
				v59.Position = v378;
				v49.Enabled = true;
				break;
			end
			if (v377 == 0) then
				local v554 = 0 - 0;
				local v555;
				while true do
					if (v554 == 0) then
						v555 = 0;
						while true do
							if ((0 + 0) == v555) then
								v378 = v33();
								v59.Transparency = 1009 - (615 + 394);
								v555 = 1;
							end
							if (v555 == 1) then
								v377 = 1 + 0;
								break;
							end
						end
						break;
					end
				end
			end
		end
	else
		local v379 = 0 + 0;
		while true do
			if (v379 == 1) then
				v49.Enabled = false;
				break;
			end
			if (v379 == (0 - 0)) then
				v59.Position = Vector3.new(0 - 0, 651 - (59 + 592), 0);
				v59.Transparency = 2 - 1;
				v379 = 1 - 0;
			end
		end
	end
end);
local v65 = v10.Tech.ShootDelay;
local v66 = nil;
local v67 = false;
local v68 = function()
	if (v30 and v30.Character) then
		local v380 = 0 + 0;
		local v381;
		local v382;
		while true do
			if (v380 == (172 - (70 + 101))) then
				if (v381 and v382) then
					local v607 = 0;
					local v608;
					while true do
						if (v607 == (0 - 0)) then
							v608 = v381:GetState() == Enum.HumanoidStateType.Freefall;
							if (v608 and v10.Tech.AutoAir) then
								if not v66 then
									v66 = tick();
								else
									local v743 = 0 + 0;
									local v744;
									while true do
										if (v743 == 0) then
											v744 = tick() - v66;
											if (v744 >= v65) then
												if not v67 then
													v67 = true;
													while v30 and v30.Character and v608 do
														v48();
														task.wait(0.00001 - 0);
														v608 = v381:GetState() == Enum.HumanoidStateType.Freefall;
														if not v608 then
															v67 = false;
															v66 = nil;
															break;
														end
													end
													v67 = false;
												end
											end
											break;
										end
									end
								end
							else
								v66 = nil;
								v67 = false;
							end
							break;
						end
					end
				end
				break;
			end
			if (v380 == (241 - (123 + 118))) then
				v381 = v30.Character:FindFirstChildOfClass("Humanoid");
				v382 = v30.Character:FindFirstChild("HumanoidRootPart");
				v380 = 1 + 0;
			end
		end
	end
end;
local v69 = loadstring(game:HttpGet("https://raw.githubusercontent.com/CongoOhioDog/automatic-spork/refs/heads/main/Prediction", true))();
local v70 = loadstring(game:HttpGet("https://raw.githubusercontent.com/CongoOhioDog/automatic-spork/refs/heads/main/badpred.lua", true))();
local v27 = game:GetService("Stats");
local v71 = function(v169)
	return (v169 / (13 + 987)) + (1399.037 - (653 + 746));
end;
local v72 = function()
	local v170 = tonumber(string.split(v27.Network.ServerStatsItem["Data Ping"]:GetValueString(), "(")[1]);
	local function v171(v351)
		local v352, v353 = nil, math.huge;
		for v383, v384 in ipairs(v351) do
			local v385 = 0 - 0;
			local v386;
			while true do
				if (v385 == (0 - 0)) then
					v386 = math.abs(v170 - v384[2 - 1]);
					if (v386 < v353) then
						v353, v352 = v386, v384[1 + 1];
					end
					break;
				end
			end
		end
		return v352;
	end
	if v10.Tech.CamAutoprediction then
		local v387 = 0 + 0;
		local v388;
		while true do
			if (v387 == 0) then
				v388 = v171(v70);
				if v388 then
					v10.Tech.CamPrediction1, v10.Tech.CamPrediction2 = v388, v388;
				end
				break;
			end
		end
	end
	if (v170 and v10.Tech.AutoPrediction) then
		local v389, v390 = v10.Tech.APMODE;
		if ((v389 == "Default") or (v389 == "Sets Based")) then
			v390 = v171(((v389 == "Sets Based") and v69) or v70);
			if v390 then
				v10.Tech.HorizontalPrediction, v10.Tech.VerticalPrediction = v390, v390 * (((v389 == "Sets Based") and (0.91 + 0)) or 1);
			end
		elseif (v389 == "Math Based") then
			local function v579(v612, v613)
				return (v612 * math.log(v170 + 1 + 0)) + v613;
			end
			v10.Tech.HorizontalPrediction, v10.Tech.VerticalPrediction = v579(0.105 + 0, -0.3), v579(0.1 - 0, -0.3);
		elseif (v389 == "Calculate") then
			local v640 = v71(v170);
			v10.Tech.HorizontalPrediction, v10.Tech.VerticalPrediction = v640, v640;
		end
	end
end;
local v73 = function(v172)
	local v173 = 0 + 0;
	local v174;
	local v175;
	local v176;
	while true do
		if (v173 == 1) then
			if (v176 and v175) then
				if (v10.Tech and v10.Tech.LookAt and not v10.Tech.Macroing) then
					if (v172 and v172.Character) then
						local v672, v673 = v172.Character:FindFirstChild("HumanoidRootPart"), v172.Character:FindFirstChildOfClass("Humanoid");
						if (v673 and v672) then
							local v705, v706 = v175.Position, v672.Position;
							local v707 = Vector3.new(v706.X - v705.X, 0 - 0, v706.Z - v705.Z).unit;
							v175.CFrame = CFrame.new(v705, v705 + v707);
							v176.AutoRotate = false;
						end
					end
				else
					v176.AutoRotate = true;
				end
			elseif v174:FindFirstChild("Humanoid") then
				v174.Humanoid.AutoRotate = true;
			end
			break;
		end
		if (v173 == (1234 - (885 + 349))) then
			v174 = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait();
			v175, v176 = v174:FindFirstChild("HumanoidRootPart"), v174:FindFirstChildOfClass("Humanoid");
			v173 = 1 + 0;
		end
	end
end;
local v74 = function()
	if (v30 and v30.Character and v30.Character:FindFirstChild("Humanoid")) then
		local v391 = v30.Character.Humanoid:GetState();
		if (v391 == Enum.HumanoidStateType.Freefall) then
			v10.Tech.jumpoffset = v10.Tech.jumpoffset3;
			v10.Tech.SelectedPart = v10.Tech.AirPart;
		elseif (v391 == Enum.HumanoidStateType.Jumping) then
			v10.Tech.jumpoffset = v10.Tech.jumpoffset2;
		else
			v10.Tech.SelectedPart = v10.Tech.RealPart;
			v10.Tech.jumpoffset = 0;
		end
	end
end;
v21.Stepped:Connect(function(v177, v178)
	local v179 = 0 - 0;
	while true do
		if (1 == v179) then
			v74();
			v72();
			v179 = 5 - 3;
		end
		if (v179 == (970 - (915 + 53))) then
			if (v10.Tech.AutoShoot and (v28.Character:FindFirstChildWhichIsA("Tool") ~= nil)) then
				v28.Character:FindFirstChildOfClass("Tool"):Activate();
			end
			break;
		end
		if (v179 == 0) then
			v68();
			v73(v30);
			v179 = 802 - (768 + 33);
		end
	end
end);
function Recalculate(v180)
	local v181 = 0 - 0;
	local v182;
	local v183;
	local v184;
	local v185;
	local v186;
	local v187;
	local v188;
	while true do
		if (v181 == (0 - 0)) then
			v182 = v180.HumanoidRootPart.Position;
			v183 = tick();
			task.wait(328.1 - (287 + 41));
			v181 = 1;
		end
		if (v181 == 1) then
			v184 = v180.HumanoidRootPart.Position;
			v185 = tick();
			v186 = v185 - v183;
			v181 = 849 - (638 + 209);
		end
		if (v181 == 3) then
			v183 = v185;
			return v188;
		end
		if (v181 == (2 + 0)) then
			local v412 = 1686 - (96 + 1590);
			while true do
				if (v412 == (1672 - (741 + 931))) then
					v187 = v184 - v182;
					v188 = v187 / v186;
					v412 = 1 + 0;
				end
				if (v412 == (2 - 1)) then
					v182 = v184;
					v181 = 13 - 10;
					break;
				end
			end
		end
	end
end
local v75 = game.Workspace.CurrentCamera;
local v76 = function()
	if (v30 and v30.Character and v30.Character:FindFirstChild("Humanoid") and v10.Tech.ViewAt) then
		v75.CameraSubject = v30.Character.Humanoid;
	elseif (v28.Character and v28.Character:FindFirstChild("Humanoid")) then
		v75.CameraSubject = v28.Character.Humanoid;
	end
end;
v21.Heartbeat:Connect(function()
	v76();
	if (v10.Tech.Camera and v30 and v30.Character and v10.Tech.SelectedPart) then
		local v394 = 0 + 0;
		local v395;
		local v396;
		local v397;
		while true do
			if (v394 == (0 + 0)) then
				v395 = v22.CurrentCamera;
				v396 = v10.Tech.SelectedPart;
				v394 = 1 + 0;
			end
			if (v394 == 1) then
				v397 = v30.Character[v396];
				if v397 then
					local v616 = v10.Tech.CamPrediction1;
					local v617 = v10.Tech.jumpoffset or (0 - 0);
					local v618 = v10.Tech.CamPrediction2;
					local v619;
					if v10.Tech.CamResolverEnabled then
						v619 = Recalculate(CamTarget.Character);
					else
						v619 = v397.AssemblyLinearVelocity;
					end
					local v620;
					if v10.Tech.UseExternal then
						v620 = Vector3.new(v397.Position.X + (v619.X / v616), v397.Position.Y + (v619.Y / v618), v397.Position.Z + (v619.Z / v616));
					else
						v620 = Vector3.new(v397.Position.X + (v619.X * v616), v397.Position.Y + (v619.Y * v618), v397.Position.Z + (v619.Z * v616));
					end
					local v621 = Enum.EasingStyle[v10.Tech.easingStyle];
					local v622 = Enum.EasingDirection[v10.Tech.easingDirection];
					local v623 = v10.Tech.smoothness or (0.1 + 0);
					v395.CFrame = v395.CFrame:Lerp(CFrame.new(v395.CFrame.Position, v620), v623, v621, v622);
				end
				break;
			end
		end
	end
end);
v28.Character:WaitForChild("Humanoid").StateChanged:Connect(function(v189, v190)
	if (v10.Tech.JumpBreak and (v190 == Enum.HumanoidStateType.Freefall)) then
		task.wait(0.17 + 0);
		v28.Character.HumanoidRootPart.Velocity = Vector3.new(0, -(61 - 46), 0 + 0);
	end
end);
local v77 = false;
local v78 = "Behind";
local v79 = Vector3.new(494 - (64 + 430), 0 + 0, -1);
local v80, v81 = -(10363 - (106 + 257)), 7090 + 2910;
local v82, v83 = -10000, 10721 - (496 + 225);
local v84, v85 = -(20449 - 10449), 48712 - 38712;
game:GetService("RunService").heartbeat:Connect(function()
	local v191 = 1658 - (256 + 1402);
	local v192;
	local v193;
	while true do
		if (v191 == (1899 - (30 + 1869))) then
			v192 = game.Players.LocalPlayer.Character;
			v193 = v192 and v192:FindFirstChild("HumanoidRootPart");
			v191 = 1370 - (213 + 1156);
		end
		if (v191 == 1) then
			if (v193 and v77) then
				local v562 = v193.Velocity;
				if (v78 == "Behind") then
					v79 = Vector3.new(188 - (96 + 92), 0 + 0, -(900 - (142 + 757)));
				elseif (v78 == "Down") then
					v79 = Vector3.new(0, -(1 + 0), 0 + 0);
				elseif (v78 == "Forward") then
					v79 = Vector3.new(79 - (32 + 47), 1977 - (1053 + 924), 1);
				elseif (v78 == "Left") then
					v79 = Vector3.new(-(1 + 0), 0 - 0, 1648 - (685 + 963));
				elseif (v78 == "One") then
					v79 = Vector3.new(1 - 0, 1 - 0, 1710 - (541 + 1168));
				elseif (v78 == "Right") then
					v79 = Vector3.new(1598 - (645 + 952), 838 - (669 + 169), 0);
				elseif (v78 == "Up") then
					v79 = Vector3.new(0, 3 - 2, 0);
				elseif (v78 == "Zero") then
					v79 = Vector3.new(0, 0, 0 - 0);
				elseif (v78 == "Shake") then
					v193.Velocity = Vector3.new(math.random(v80, v81), math.random(v82, v83), math.random(v84, v85));
					game:GetService("RunService").RenderStepped:Wait();
					v193.Velocity = v562;
					return;
				end
				v193.Velocity = v79 * ((1 + 1) ^ (4 + 12));
				game:GetService("RunService").RenderStepped:Wait();
				v193.Velocity = v562;
			end
			break;
		end
	end
end);
v21.PostSimulation:Connect(function()
	local v194 = 765 - (181 + 584);
	local v195;
	while true do
		if (v194 == (1395 - (665 + 730))) then
			v195 = game.Players.LocalPlayer;
			if (v195.Character and v195.Character:FindFirstChild("HumanoidRootPart")) then
				if desyncsleep then
					local v625 = 0 - 0;
					while true do
						if (v625 == (0 - 0)) then
							v15 = not v15;
							setfflag("S2PhysicsSenderRate", 1352 - (540 + 810));
							v625 = 3 - 2;
						end
						if ((2 - 1) == v625) then
							sethiddenproperty(v195.Character.HumanoidRootPart, "NetworkIsSleeping", v15);
							break;
						end
					end
				else
					local v626 = 0 + 0;
					while true do
						if ((204 - (166 + 37)) == v626) then
							sethiddenproperty(v195.Character.HumanoidRootPart, "NetworkIsSleeping", v15);
							break;
						end
						if (v626 == (1881 - (22 + 1859))) then
							v15 = false;
							setfflag("S2PhysicsSenderRate", 13);
							v626 = 1773 - (843 + 929);
						end
					end
				end
			end
			break;
		end
	end
end);
local v86 = game:GetService("RunService");
local v87 = false;
local v88 = function(v196, v197)
	local v198 = v196 * CFrame.new(262 - (30 + 232), -(2 - 1), 777 - (55 + 722), 1, 0 - 0, 1675 - (78 + 1597), 0 + 0, 0 + 0, 1 + 0, 549 - (305 + 244), -(1 + 0), 0);
	return v198:ToObjectSpace(v197):inverse();
end;
local v89 = function(v199)
	local v200 = v199.Grip;
	if (v30 and v30.Character) then
		local v399 = 105 - (95 + 10);
		local v400;
		while true do
			if (v399 == (0 + 0)) then
				v400 = 0;
				while true do
					if (v400 == 4) then
						v199.Grip = v200;
						v199.Parent = v28.Character;
						break;
					end
					if (3 == v400) then
						v199.Parent = v28.Backpack;
						v28.Character.RightHand.Anchored = false;
						v400 = 4;
					end
					if (v400 == 0) then
						v199.Parent = v28.Backpack;
						v28.Character.RightHand.Anchored = false;
						v400 = 3 - 2;
					end
					if (v400 == 1) then
						v199.Grip = v88(v28.Character.RightHand.CFrame, v30.Character.HumanoidRootPart.CFrame);
						v28.Character.RightHand.Anchored = true;
						v400 = 2;
					end
					if (v400 == 2) then
						v199.Parent = v28.Character;
						v86.RenderStepped:Wait();
						v400 = 3;
					end
				end
				break;
			end
		end
	end
end;
local v90;
local v91 = function(v201)
	local v202 = 0 - 0;
	while true do
		if (v202 == (0 + 0)) then
			v201.ChildAdded:Connect(function(v518)
				if v87 then
					if v518:IsA("Tool") then
						v90 = v518.Activated:Connect(function()
							v89(v518);
						end);
					end
				end
			end);
			v201.ChildRemoved:Connect(function(v519)
				if v519:IsA("Tool") then
					if v90 then
						v90:Disconnect();
					end
				end
			end);
			break;
		end
	end
end;
v28.CharacterAdded:Connect(function(v203)
	v91(v203);
end);
v91(v28.Character);
v21.Heartbeat:Connect(function()
	if (v10.Tech.cframespeedtoggle and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")) then
		local v401 = 0 + 0;
		local v402;
		local v403;
		while true do
			if (v401 == 1) then
				v402.CFrame = v402.CFrame + (v403 * (v10.Tech.speedvalue / 0.5));
				break;
			end
			if (v401 == 0) then
				local v565 = 0 - 0;
				while true do
					if ((0 + 0) == v565) then
						v402 = game.Players.LocalPlayer.Character.HumanoidRootPart;
						v403 = game.Players.LocalPlayer.Character.Humanoid.MoveDirection;
						v565 = 1 - 0;
					end
					if (v565 == 1) then
						v401 = 508 - (353 + 154);
						break;
					end
				end
			end
		end
	end
end);
local v92 = false;
local v93 = 1 - 0;
function changeSkybox()
	if v92 then
		if (v93 == 1) then
			v32.ClockTime = "12";
			v32.Sky.SkyboxBk = "http://www.roblox.com/asset/?id=1279987105";
			v32.Sky.SkyboxDn = "http://www.roblox.com/asset/?id=1279987105";
			v32.Sky.SkyboxFt = "http://www.roblox.com/asset/?id=1279987105";
			v32.Sky.SkyboxLf = "http://www.roblox.com/asset/?id=1279987105";
			v32.Sky.SkyboxRt = "http://www.roblox.com/asset/?id=1279987105";
			v32.Sky.SkyboxUp = "http://www.roblox.com/asset/?id=1279987105";
		elseif (v93 == 2) then
			local v587 = 0 - 0;
			while true do
				if (v587 == 1) then
					v32.Sky.SkyboxDn = "http://www.roblox.com/asset/?id=2571711090";
					v32.Sky.SkyboxFt = "http://www.roblox.com/asset/?id=2571711090";
					v587 = 2 + 0;
				end
				if (v587 == 2) then
					v32.Sky.SkyboxLf = "http://www.roblox.com/asset/?id=2571711090";
					v32.Sky.SkyboxRt = "http://www.roblox.com/asset/?id=2571711090";
					v587 = 3 + 0;
				end
				if (v587 == (2 + 1)) then
					v32.Sky.SkyboxUp = "http://www.roblox.com/asset/?id=2571711090";
					break;
				end
				if (v587 == 0) then
					v32.ClockTime = "12";
					v32.Sky.SkyboxBk = "http://www.roblox.com/asset/?id=2571711090";
					v587 = 1 - 0;
				end
			end
		elseif (v93 == (5 - 2)) then
			v32.ClockTime = "12";
			v32.Sky.SkyboxBk = "rbxassetid://6277563515";
			v32.Sky.SkyboxDn = "rbxassetid://6277565742";
			v32.Sky.SkyboxFt = "rbxassetid://6277567481";
			v32.Sky.SkyboxLf = "rbxassetid://6277569562";
			v32.Sky.SkyboxRt = "rbxassetid://6277583250";
			v32.Sky.SkyboxUp = "rbxassetid://6277586065";
		elseif (v93 == (9 - 5)) then
			local v692 = 0;
			while true do
				if (v692 == 1) then
					v32.Sky.SkyboxDn = "rbxassetid://6285721078";
					v32.Sky.SkyboxFt = "rbxassetid://6285722964";
					v692 = 2;
				end
				if (v692 == 3) then
					v32.Sky.SkyboxUp = "rbxassetid://6285730635";
					break;
				end
				if (v692 == 0) then
					v32.ClockTime = "12";
					v32.Sky.SkyboxBk = "rbxassetid://6285719338";
					v692 = 87 - (7 + 79);
				end
				if (v692 == 2) then
					v32.Sky.SkyboxLf = "rbxassetid://6285724682";
					v32.Sky.SkyboxRt = "rbxassetid://6285726335";
					v692 = 2 + 1;
				end
			end
		elseif (v93 == 5) then
			local v719 = 181 - (24 + 157);
			while true do
				if (v719 == (0 - 0)) then
					v32.ClockTime = "12";
					v32.Sky.SkyboxBk = "rbxassetid://877168885";
					v719 = 2 - 1;
				end
				if (v719 == (1 + 0)) then
					v32.Sky.SkyboxDn = "rbxassetid://877169070";
					v32.Sky.SkyboxFt = "rbxassetid://877169154";
					v719 = 4 - 2;
				end
				if (v719 == (382 - (262 + 118))) then
					v32.Sky.SkyboxLf = "rbxassetid://877169233";
					v32.Sky.SkyboxRt = "rbxassetid://877169317";
					v719 = 1086 - (1038 + 45);
				end
				if (v719 == 3) then
					v32.Sky.SkyboxUp = "rbxassetid://877169431";
					break;
				end
			end
		elseif (v93 == (12 - 6)) then
			local v745 = 230 - (19 + 211);
			local v746;
			while true do
				if (v745 == 0) then
					v746 = 0;
					while true do
						if (v746 == (114 - (88 + 25))) then
							v32.Sky.SkyboxDn = "http://www.roblox.com/asset/?id=9971120429";
							v32.Sky.SkyboxFt = "http://www.roblox.com/asset/?id=9971120429";
							v746 = 4 - 2;
						end
						if (v746 == (1 + 1)) then
							v32.Sky.SkyboxLf = "http://www.roblox.com/asset/?id=9971120429";
							v32.Sky.SkyboxRt = "http://www.roblox.com/asset/?id=9971120429";
							v746 = 3 + 0;
						end
						if (v746 == (1039 - (1007 + 29))) then
							v32.Sky.SkyboxUp = "http://www.roblox.com/asset/?id=9971120429";
							break;
						end
						if (v746 == 0) then
							v32.ClockTime = "12";
							v32.Sky.SkyboxBk = "http://www.roblox.com/asset/?id=9971120429";
							v746 = 1 + 0;
						end
					end
					break;
				end
			end
		elseif (v93 == 7) then
			local v766 = 0 - 0;
			local v767;
			while true do
				if (v766 == (0 - 0)) then
					v767 = 0 + 0;
					while true do
						if (v767 == (811 - (340 + 471))) then
							v32.ClockTime = "12";
							v32.Sky.SkyboxBk = "http://www.roblox.com/asset/?id=8754359769";
							v767 = 2 - 1;
						end
						if (v767 == (590 - (276 + 313))) then
							v32.Sky.SkyboxDn = "http://www.roblox.com/asset/?id=8754359769";
							v32.Sky.SkyboxFt = "http://www.roblox.com/asset/?id=8754359769";
							v767 = 2;
						end
						if (v767 == 2) then
							v32.Sky.SkyboxLf = "http://www.roblox.com/asset/?id=8754359769";
							v32.Sky.SkyboxRt = "http://www.roblox.com/asset/?id=8754359769";
							v767 = 6 - 3;
						end
						if (v767 == (3 + 0)) then
							v32.Sky.SkyboxUp = "http://www.roblox.com/asset/?id=8754359769";
							break;
						end
					end
					break;
				end
			end
		end
	end
end
local v94 = 0;
local v95 = function(v204, v205, v206)
	local v207 = 0 + 0;
	local v208;
	local v209;
	local v210;
	local v211;
	local v212;
	local v213;
	local v214;
	local v215;
	local v216;
	while true do
		if ((1 + 1) == v207) then
			v209.Parent = v208;
			v209.BackgroundColor3 = Color3.fromRGB(1972 - (495 + 1477), 0 - 0, 0);
			v209.BackgroundTransparency = 0.5;
			v209.Position = UDim2.new(1 + 0, -150, 403 - (342 + 61), (v94 - (1 + 0)) * (225 - (4 + 161)));
			v207 = 2 + 1;
		end
		if (v207 == (28 - 19)) then
			v214 = false;
			v211.MouseButton1Down:Connect(function()
				local v527 = 0 - 0;
				while true do
					if (v527 == (498 - (322 + 175))) then
						if v214 then
							v210.Image = "rbxassetid://10735024209";
						else
							v210.Image = "rbxassetid://10734923214";
						end
						break;
					end
					if (v527 == 0) then
						v214 = not v214;
						v206(v214);
						v527 = 564 - (173 + 390);
					end
				end
			end);
			v215, v216 = nil;
			v211.InputBegan:Connect(function(v528)
				if ((v528.UserInputType == Enum.UserInputType.MouseButton1) or (v528.UserInputType == Enum.UserInputType.Touch)) then
					local v588 = 0 + 0;
					local v589;
					while true do
						if (v588 == (314 - (203 + 111))) then
							v589 = 0 + 0;
							while true do
								if (v589 == (0 + 0)) then
									v215 = v528.Position;
									v216 = v209.Position;
									v589 = 2 - 1;
								end
								if ((1 + 0) == v589) then
									v528.Changed:Connect(function()
										if (v528.UserInputState == Enum.UserInputState.End) then
											v215 = nil;
										end
									end);
									break;
								end
							end
							break;
						end
					end
				end
			end);
			v207 = 716 - (57 + 649);
		end
		if (v207 == 0) then
			v94 = v94 + 1;
			v208 = Instance.new("ScreenGui");
			v208.Parent = game:GetService("CoreGui");
			v208.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
			v207 = 385 - (328 + 56);
		end
		if (v207 == (2 + 2)) then
			v210.Size = UDim2.new(512 - (433 + 79), 3 + 25, 0 + 0, 94 - 66);
			v210.AnchorPoint = Vector2.new(0 - 0, 0.5 + 0);
			v210.Position = UDim2.new(0.05 + 0, 0, 1036.5 - (562 + 474), 0 - 0);
			v210.Image = "rbxassetid://10734923214";
			v207 = 9 - 4;
		end
		if (v207 == 8) then
			v211.TextStrokeColor3 = Color3.fromRGB(905 - (76 + 829), 0, 1673 - (1506 + 167));
			v211.TextStrokeTransparency = 1 - 0;
			v213 = Instance.new("UICorner", v209);
			v213.CornerRadius = UDim.new(0, 274 - (58 + 208));
			v207 = 6 + 3;
		end
		if (v207 == (3 + 0)) then
			v209.Size = UDim2.new(0 + 0, 120, 0, 163 - 123);
			v210.Parent = v209;
			v210.BackgroundColor3 = Color3.fromRGB(0, 337 - (258 + 79), 0 + 0);
			v210.BackgroundTransparency = 1;
			v207 = 8 - 4;
		end
		if (v207 == (1471 - (1219 + 251))) then
			v209 = Instance.new("Frame");
			v210 = Instance.new("ImageLabel");
			v211 = Instance.new("TextButton");
			v212 = Instance.new("UITextSizeConstraint");
			v207 = 1673 - (1231 + 440);
		end
		if (v207 == (68 - (34 + 24))) then
			v211.InputChanged:Connect(function(v529)
				if ((v529.UserInputType == Enum.UserInputType.MouseMovement) or (v529.UserInputType == Enum.UserInputType.Touch)) then
					if v215 then
						local v665 = v529.Position - v215;
						v209.Position = UDim2.new(v216.X.Scale, v216.X.Offset + v665.X, v216.Y.Scale, v216.Y.Offset + v665.Y);
					end
				end
			end);
			v212.Parent = v211;
			v212.MaxTextSize = 15 + 10;
			break;
		end
		if (v207 == (9 - 4)) then
			v210.ImageColor3 = v205;
			v211.Parent = v209;
			v211.BackgroundColor3 = Color3.fromRGB(0, 0, 0 + 0);
			v211.BackgroundTransparency = 2 - 1;
			v207 = 18 - 12;
		end
		if (v207 == (15 - 9)) then
			v211.Size = UDim2.new(0 - 0, 174 - 94, 1589 - (877 + 712), 17 + 11);
			v211.AnchorPoint = Vector2.new(754.5 - (242 + 512), 0.5 - 0);
			v211.Position = UDim2.new(0.65, 627 - (92 + 535), 0.5, 0 + 0);
			v211.Font = Enum.Font.Arimo;
			v207 = 14 - 7;
		end
		if (v207 == (1 + 6)) then
			v211.Text = v204;
			v211.TextColor3 = v205;
			v211.TextScaled = true;
			v211.TextSize = 90 - 65;
			v207 = 8;
		end
	end
end;
v21.Heartbeat:Connect(function()
	if (Flick and v30 and v30.Character) then
		local v404 = 0;
		local v405;
		while true do
			if (v404 == (0 + 0)) then
				v405 = v30.Character:FindFirstChild("Humanoid");
				if (v405 and (v405.Health > 0)) then
					local v627 = 0 + 0;
					local v628;
					while true do
						if (v627 == (0 + 0)) then
							v628 = v30.Character:FindFirstChild("UpperTorso");
							if (v628 and (v405:GetState() == Enum.HumanoidStateType.Freefall)) then
								local v722 = workspace.CurrentCamera;
								local v723 = v33();
								v722.CFrame = CFrame.new(v722.CFrame.Position, v723);
							end
							break;
						end
					end
				end
				break;
			end
		end
	end
end);
local v96 = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name;
local v97 = os.date("%Y-%m-%d");
local v19 = game:GetService("Players");
local v98 = v19.LocalPlayer;
local v99 = "Buyer";
if (v98.Name == "computerplayer442") then
	v99 = "BurritoCat";
elseif (v98.Name == "pzoz853") then
	v99 = "Taco";
end
local v100 = "#" .. v8.Accent:ToHex();
local v101 = v8:Window({["Name"]=('Psalms.<font color="' .. v100 .. '">Tech</font> | ' .. v96 .. " | " .. v97 .. ' | <font color="' .. v100 .. '">' .. v99 .. "</font>"),["Amount"]=(7 - 3)});
v8:SetOpen(false);
local v102 = game.Players.LocalPlayer.Name;
local v103 = string.sub(tostring(math.random()), 4 - 1, 8);
local v104 = {"Slave","Master","Peasant"};
local v105 = v104[math.random(#v104)];
local v106 = v101:Page({["Name"]="Target"});
local v107 = v101:Page({["Name"]="Aim Assist"});
local v108 = v101:Page({["Name"]="Visuals"});
local v109 = v101:Page({["Name"]="Settings"});
local v110 = v106:Section({["Name"]="Target",["Side"]="Left",["Size"]=(43 + 137)});
v110:Toggle({["Name"]="Enabled",["Callback"]=function(v217)
	v10.Tech.Enabled = v217;
end});
v110:Toggle({["Name"]="Look At",["Callback"]=function(v219)
	v10.Tech.LookAt = v219;
end});
v110:Toggle({["Name"]="View",["Callback"]=function(v221)
	v10.Tech.ViewAt = v221;
end});
v110:Toggle({["Name"]="Auto Air",["Callback"]=function(v223)
	v10.Tech.AutoAir = v223;
end});
v110:Textbox({["Name"]="Auto Air Delay",["Default"]=tostring(v10.Tech.ShootDelay),["Callback"]=function(v225)
	v65 = tonumber(v225);
end});
v110:List({["Name"]="Lock Method",["Options"]={"Index","Namecall"},["Default"]="Namecall",["Callback"]=function(v226)
	v10.Tech.LockType = v226;
end});
local v111 = v106:Section({["Name"]="Bullet TP",["Side"]="Left",["Size"]=(133 - (86 + 7))});
v111:Toggle({["Name"]="Enabled",["Callback"]=function(v228)
	v87 = v228;
end});
local v112 = v106:Section({["Name"]="Hit Part",["Side"]="Left"});
v112:List({["Name"]="BodyPart",["Options"]={"Head","UpperTorso","LowerTorso","HumanoidRootPart","LeftUpperArm","LeftLowerArm","LeftHand","RightUpperArm","RightLowerArm","RightHand","LeftUpperLeg","LeftLowerLeg","LeftFoot","RightUpperLeg","RightLowerLeg","RightFoot"},["Default"]="HumanoidRootPart",["Callback"]=function(v229)
	v10.Tech.RealPart = v229;
end});
v112:List({["Name"]="AirPart",["Options"]={"Head","UpperTorso","LowerTorso","HumanoidRootPart","LeftUpperArm","LeftLowerArm","LeftHand","RightUpperArm","RightLowerArm","RightHand","LeftUpperLeg","LeftLowerLeg","LeftFoot","RightUpperLeg","RightLowerLeg","RightFoot"},["Default"]="HumanoidRootPart",["Callback"]=function(v231)
	v10.Tech.AirPart = v231;
end});
local v113 = v106:Section({["Name"]="Calculation",["Side"]="Right",["Size"]=(1210 - (352 + 458))});
v113:Toggle({["Name"]="Division",["Default"]=false,["Callback"]=function(v233)
	v10.Tech.UseVertical = v233;
end});
v113:Textbox({["Name"]="Horizontal Prediction (X)",["Default"]=(0.1 - 0),["Callback"]=function(v235)
	local v236 = 0 - 0;
	while true do
		if (v236 == 0) then
			v10.Tech.HorizontalPrediction2 = tonumber(v235) or (0.1 + 0);
			task.wait(0.01);
			v236 = 2 - 1;
		end
		if (v236 == (950 - (438 + 511))) then
			v10.Tech.HorizontalPrediction = v10.Tech.HorizontalPrediction2;
			v10.Tech.VerticalPrediction = v10.Tech.VerticalPrediction2;
			break;
		end
	end
end});
v113:Textbox({["Name"]="Vertical Prediction (Y)",["Default"]=(1383.1 - (1262 + 121)),["Callback"]=function(v237)
	local v238 = 1068 - (728 + 340);
	while true do
		if (v238 == (1790 - (816 + 974))) then
			v10.Tech.VerticalPrediction2 = tonumber(v237) or (0.1 - 0);
			task.wait(0.01);
			v238 = 3 - 2;
		end
		if (v238 == (340 - (163 + 176))) then
			v10.Tech.HorizontalPrediction = v10.Tech.HorizontalPrediction2;
			v10.Tech.VerticalPrediction = v10.Tech.VerticalPrediction2;
			break;
		end
	end
end});
v113:Textbox({["Name"]="Jump offset",["Default"]=v10.Tech.jumpoffset2,["Callback"]=function(v239)
	v10.Tech.jumpoffset2 = tonumber(v239);
end});
v113:Textbox({["Name"]="Fall offset",["Default"]=v10.Tech.jumpoffset3,["Callback"]=function(v241)
	v10.Tech.jumpoffset3 = tonumber(v241);
end});
v113:Toggle({["Name"]="Visualize",["Callback"]=function(v243)
	v10.Tech.VelocityDot = v243;
end});
v113:Toggle({["Name"]="Resolver",["Callback"]=function(v245)
	v10.Tech.ResolverEnabled = v245;
end});
v113:Toggle({["Name"]="Auto Prediction",["Callback"]=function(v247)
	local v248 = 0 - 0;
	while true do
		if (v248 == (0 - 0)) then
			v10.Tech.AutoPrediction = v247;
			v10.Tech.HorizontalPrediction = v10.Tech.HorizontalPrediction2;
			v248 = 1;
		end
		if (v248 == (1 + 0)) then
			v10.Tech.VerticalPrediction = v10.Tech.VerticalPrediction2;
			break;
		end
	end
end});
v113:List({["Name"]="Auto Prediction Mode",["Options"]={"Default","Math Based","Sets Based","Calculate"},["Default"]=v10.Tech.APMODE,["Callback"]=function(v249)
	v10.Tech.APMODE = v249;
end});
v113:List({["Name"]="Resolver Method",["Options"]={"Recalculate","MoveDirection","LookVector"},["Default"]="MoveDirection",["Callback"]=function(v251)
	v10.Tech.RESOLVER = v251;
end});
local v114 = v109:Section({["Name"]="Movement",["Side"]="Right",["Size"]=220});
v114:Button({["Name"]="Load Speed",["Callback"]=function()
	local v253 = 0 + 0;
	local v254;
	while true do
		if ((46 - (45 + 1)) == v253) then
			v254 = 0;
			while true do
				if (v254 == 0) then
					if Macro22 then
						v8:Notification("Already Loaded.", 3);
						return;
					end
					Macro22 = true;
					v254 = 1 + 0;
				end
				if ((1991 - (1282 + 708)) == v254) then
					v95("Speed", Color3.fromRGB(1367 - (583 + 629), 0 + 0, 400 - 245), function(v629)
						v10.Tech.cframespeedtoggle = v629;
					end);
					break;
				end
			end
			break;
		end
	end
end});
v114:Button({["Name"]="Load Fly",["Callback"]=function()
	if Macro2 then
		local v406 = 0 + 0;
		while true do
			if (v406 == (1170 - (943 + 227))) then
				v8:Notification("Already Loaded.", 2 + 1);
				return;
			end
		end
	end
	local v255 = 50;
	local v256 = false;
	Macro2 = true;
	local v257 = game.Players.LocalPlayer;
	local v258 = game:GetService("UserInputService");
	local v259 = game:GetService("RunService");
	local function v260(v354)
		local v355 = 1631 - (1539 + 92);
		local v356;
		local v357;
		while true do
			if (v355 == (1947 - (706 + 1240))) then
				if v256 then
					local v590 = 258 - (81 + 177);
					local v591;
					local v592;
					local v593;
					while true do
						if (v590 == (0 - 0)) then
							v356.PlatformStand = true;
							v591 = Instance.new("BodyGyro", v357);
							v590 = 258 - (212 + 45);
						end
						if (v590 == 4) then
							v593 = v259.RenderStepped:Connect(function()
								if v256 then
									local v725 = 0 - 0;
									local v726;
									local v727;
									while true do
										if (v725 == (1946 - (708 + 1238))) then
											v591.CFrame = workspace.CurrentCamera.CFrame;
											v726 = ((v356.MoveDirection.Magnitude > 0) and (workspace.CurrentCamera.CFrame.LookVector * v356.MoveDirection.Magnitude)) or Vector3.new(0 + 0, 0, 0 + 0);
											v725 = 1668 - (586 + 1081);
										end
										if (v725 == (513 - (348 + 163))) then
											v592.Velocity = (v726 * v255) + Vector3.new(0, v727, 0 + 0);
											v356:ChangeState(Enum.HumanoidStateType.Freefall);
											break;
										end
										if (v725 == 1) then
											v727 = (v258:IsKeyDown(Enum.KeyCode.Space) and v255) or (v258:IsKeyDown(Enum.KeyCode.LeftControl) and -v255) or (280 - (215 + 65));
											v357.CFrame = v357.CFrame + (v726 * (v10.Tech.speedvalue / 0.5));
											v725 = 4 - 2;
										end
									end
								else
									v592.Velocity = Vector3.new(1859 - (1541 + 318), 0 + 0, 0);
									v593:Disconnect();
								end
							end);
							break;
						end
						if (v590 == (2 + 1)) then
							v592.MaxForce = Vector3.new(8999999836 - (304 + 44), 9000000768 - (883 + 397), 8999999488);
							v593 = nil;
							v590 = 594 - (563 + 27);
						end
						if (v590 == (7 - 5)) then
							v591.MaxTorque = Vector3.new(8999999488, 9000001474 - (1369 + 617), 9000000975 - (85 + 1402));
							v591.CFrame = workspace.CurrentCamera.CFrame;
							v590 = 3;
						end
						if (1 == v590) then
							v592 = Instance.new("BodyVelocity", v357);
							v591.P = 31021 + 58979;
							v590 = 2;
						end
					end
				else
					local v594 = 0 - 0;
					while true do
						if (v594 == 0) then
							v356.PlatformStand = false;
							for v693, v694 in pairs(v357:GetChildren()) do
								if (v694:IsA("BodyGyro") or v694:IsA("BodyVelocity")) then
									v694:Destroy();
								end
							end
							break;
						end
					end
				end
				break;
			end
			if (v355 == 0) then
				v356 = v354:FindFirstChildOfClass("Humanoid");
				v357 = v354:FindFirstChild("HumanoidRootPart");
				v355 = 404 - (274 + 129);
			end
		end
	end
	local function v261(v358)
		v260(v358);
	end
	v257.CharacterAdded:Connect(function(v359)
		v359:WaitForChild("Humanoid");
		v260(v359);
	end);
	v95("Fly", Color3.fromRGB(372 - (12 + 205), 0, 155), function(v360)
		local v361 = 0 + 0;
		local v362;
		while true do
			if (v361 == (0 - 0)) then
				v362 = 0 + 0;
				while true do
					if (v362 == 0) then
						v256 = v360;
						if v257.Character then
							v260(v257.Character);
						end
						break;
					end
				end
				break;
			end
		end
	end);
end});
v114:Button({["Name"]="Load Macro",["Callback"]=function()
	if BitchMac then
		local v407 = 384 - (27 + 357);
		while true do
			if (v407 == (480 - (91 + 389))) then
				local v566 = 297 - (90 + 207);
				while true do
					if (v566 == (0 + 0)) then
						v8:Notification("Already Loaded.", 864 - (706 + 155));
						return;
					end
				end
			end
		end
	end
	BitchMac = true;
	local v262 = game:GetService("Players");
	local v263 = game:GetService("RunService");
	local v264 = v262.LocalPlayer;
	local v265 = v264.Character or v264.CharacterAdded:Wait();
	local v266 = v265:WaitForChild("HumanoidRootPart");
	local v267 = v265.Humanoid;
	local v268 = workspace.CurrentCamera;
	local v269 = 900000;
	local v270 = CFrame.new(0, 1795 - (730 + 1065), 1563 - (1339 + 224));
	local v271 = CFrame.new(0 + 0, 0, 0 + 0);
	local v272 = Vector3.new(0 - 0, 843 - (268 + 575), 1294 - (919 + 375));
	local v273 = false;
	local v274 = false;
	local function v275()
		if (v264.Character and v264.Character:FindFirstChildOfClass("Humanoid") and v264.Character:FindFirstChildOfClass("Humanoid").RootPart) then
			v272 = v264.Character:FindFirstChildOfClass("Humanoid").RootPart.Position;
		end
	end
	local function v276(v363)
		if (v264.Character and v264.Character:FindFirstChildOfClass("Humanoid")) then
			v264.Character:FindFirstChildOfClass("Humanoid").AutoRotate = v363;
		end
	end
	local function v277()
		if workspace.CurrentCamera then
			return CFrame.new(v272, Vector3.new(workspace.CurrentCamera.CFrame.LookVector.X * v269, v272.Y, workspace.CurrentCamera.CFrame.LookVector.Z * v269));
		end
	end
	local function v278()
		v275();
		v276(false);
		if (v264.Character and v264.Character:FindFirstChildOfClass("Humanoid") and v264.Character:FindFirstChildOfClass("Humanoid").RootPart) then
			v264.Character:FindFirstChildOfClass("Humanoid").RootPart.CFrame = v277();
		end
		if workspace.CurrentCamera then
			workspace.CurrentCamera.CFrame = v268.CFrame * v270;
		end
	end
	local function v279()
		v275();
		v276(true);
		if workspace.CurrentCamera then
			workspace.CurrentCamera.CFrame = v268.CFrame * v271;
		end
		if v273 then
			local v468 = 0 - 0;
			while true do
				if (v468 == (971 - (180 + 791))) then
					v273:Disconnect();
					v273 = nil;
					break;
				end
			end
		end
	end
	local function v280()
		if v274 then
			if not v273 then
				v273 = v263.RenderStepped:Connect(function()
					v278();
				end);
			else
				v279();
			end
		end
	end
	v95("Macro", Color3.fromRGB(2060 - (323 + 1482), 255, 255), function(v364)
		v274 = v364;
		if v364 then
			local v469 = game:GetService("ReplicatedStorage");
			local v470 = game.Players.LocalPlayer;
			local v471 = v470.Character or v470.CharacterAdded:Wait();
			local v472 = v471:WaitForChild("Humanoid");
			local v473 = v470.Backpack;
			local function v474(v530, v531)
				return v530:PlayEmoteAndGetAnimTrackById(v531);
			end
			local v475 = {};
			for v532, v533 in pairs(v473:GetChildren()) do
				if v533:IsA("Tool") then
					table.insert(v475, v533);
				end
			end
			local v476;
			for v534, v535 in pairs(v471:GetChildren()) do
				if v535:IsA("Tool") then
					v476 = v535;
					break;
				end
			end
			if (not v476 and (#v475 > 0)) then
				v476 = v475[1];
			end
			if MacroV then
				if v476 then
					local v631 = 1918 - (1177 + 741);
					while true do
						if ((1 + 1) == v631) then
							v476.Parent = v473;
							task.wait(0.23 - 0);
							v631 = 2 + 1;
						end
						if (v631 == (0 - 0)) then
							v476.Parent = v471;
							task.wait(0.13 + 0);
							v631 = 110 - (96 + 13);
						end
						if (v631 == 3) then
							v476.Parent = v471;
							break;
						end
						if (v631 == 1) then
							v474(v472, "15610015346");
							task.wait(1921.23 - (962 + 959));
							v631 = 2;
						end
					end
				end
			else
				v474(v472, "15610015346");
				task.wait(0.26 - 0);
				if v476 then
					v476.Parent = v471;
					task.wait(0.2 + 0);
					v476.Parent = v473;
				end
			end
		end
	end);
	while task.wait(v10.Tech.MacroSpeed) do
		if v274 then
			v280();
		else
			v279();
		end
	end
end});
v114:Toggle({["Name"]="Abuse Macro",["Callback"]=function(v281)
	MacroV = v281;
end});
v114:Textbox({["Name"]="Fly and CFrame Speed",["Default"]=v10.Tech.speedvalue,["Callback"]=function(v282)
	v10.Tech.speedvalue = tonumber(v282);
end});
v114:Textbox({["Name"]="Macro Speed",["Default"]=v10.Tech.MacroSpeed,["Callback"]=function(v284)
	v10.Tech.MacroSpeed = tonumber(v284);
end});
local v115 = v109:Section({["Name"]="Load",["Side"]="Left",["Size"]=80});
v115:Button({["Name"]="Lock Button",["Callback"]=function()
	local v286 = 1351 - (461 + 890);
	local v287;
	local v288;
	local v289;
	local v290;
	local v291;
	local v292;
	local v293;
	while true do
		if (v286 == (2 + 0)) then
			v288.Parent = game.CoreGui;
			v288.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
			v288.ResetOnSpawn = false;
			v289 = Instance.new("ImageButton");
			v289.Name = "ImageButton";
			v289.Parent = v288;
			v286 = 11 - 8;
		end
		if (v286 == (244 - (19 + 224))) then
			v287.Filled = false;
			v287.Radius = 227 + 23;
			v287.Position = Vector2.new(workspace.CurrentCamera.ViewportSize.X / (200 - (37 + 161)), workspace.CurrentCamera.ViewportSize.Y / (1 + 1));
			v287.Visible = false;
			v288 = Instance.new("ScreenGui");
			v288.Name = "Sigmaballs";
			v286 = 1 + 1;
		end
		if (5 == v286) then
			function v292()
				local v536;
				local v537 = math.huge;
				local v538 = game.Players.LocalPlayer;
				local v539 = game:GetService("Workspace").CurrentCamera;
				local v540 = Vector2.new(v539.ViewportSize.X / 2, v539.ViewportSize.Y / (2 + 0));
				local v541 = v287.Radius;
				local v542 = v539.ViewportSize;
				for v568, v569 in pairs(game.Players:GetPlayers()) do
					if ((v569 ~= v538) and v569.Character and v569.Character:FindFirstChild("Humanoid") and (v569.Character.Humanoid.Health > 0) and v569.Character:FindFirstChild("HumanoidRootPart")) then
						local v633 = 61 - (60 + 1);
						local v634;
						local v635;
						while true do
							if (v633 == (923 - (826 + 97))) then
								v634, v635 = v539:WorldToViewportPoint(v569.Character.PrimaryPart.Position);
								if (v635 and (v634.X > (0 + 0)) and (v634.Y > (0 - 0)) and (v634.X < v542.X) and (v634.Y < v542.Y)) then
									local v729 = (Vector2.new(v634.X, v634.Y) - v540).magnitude;
									if ((v729 < v541) and (v729 < v537)) then
										local v747 = 0 - 0;
										while true do
											if (0 == v747) then
												v536 = v569;
												v537 = v729;
												break;
											end
										end
									end
								end
								break;
							end
						end
					end
				end
				return v536;
			end
			v293 = nil;
			function v293()
				if v16.Enabled then
					local v595 = 685 - (375 + 310);
					local v596;
					while true do
						if (v595 == (1999 - (1864 + 135))) then
							v596 = v292();
							if (v29 and v30) then
								local v710 = 0 - 0;
								while true do
									if (v710 == 1) then
										targetHealth = nil;
										game.Players.LocalPlayer.Character.Humanoid.AutoRotate = true;
										v710 = 1 + 1;
									end
									if (v710 == (2 + 1)) then
										if v16.LookAt then
											v28.Character.Humanoid.AutoRotate = true;
										end
										v289.Image = "rbxassetid://140623923630784";
										v710 = 9 - 5;
									end
									if (v710 == (1135 - (314 + 817))) then
										v8:Notification("Untargeted", 2);
										break;
									end
									if (0 == v710) then
										CamTarget = nil;
										v29 = false;
										v710 = 1 + 0;
									end
									if (v710 == (216 - (32 + 182))) then
										v30 = nil;
										v22.CurrentCamera.CameraSubject = v28.Character.Humanoid;
										v710 = 3;
									end
								end
							elseif v596 then
								v29 = true;
								v30 = v596;
								CamTarget = v596;
								if (v30.Character and v30.Character:FindFirstChild("Humanoid")) then
									targetHealth = v30.Character.Humanoid.Health;
								else
									return;
								end
								v289.Image = "rbxassetid://96086736054343";
								v8:Notification("Target Locked: <font color='" .. v100 .. "'>" .. tostring(v30.DisplayName) .. "</font>", 2);
							else
								v8:Notification("No target found", 2 + 0);
							end
							break;
						end
					end
				end
			end
			v289.MouseButton1Click:Connect(v293);
			v18.InputBegan:Connect(function(v543, v544)
				if (not v544 and (v543.KeyCode == Enum.KeyCode.DPadDown)) then
					v293();
				end
			end);
			break;
		end
		if (v286 == (0 - 0)) then
			if LockBut then
				local v570 = 65 - (39 + 26);
				while true do
					if (v570 == (144 - (54 + 90))) then
						v8:Notification("Already Fucking Loaded", 200 - (45 + 153));
						return;
					end
				end
			end
			LockBut = true;
			v287 = Drawing.new("Circle");
			v287.Transparency = 0.5;
			v287.Thickness = 2 + 0;
			v287.Color = Color3.new(553 - (457 + 95), 0 + 0, 0);
			v286 = 1 - 0;
		end
		if (v286 == (6 - 3)) then
			v289.Active = true;
			v289.Draggable = true;
			v289.BackgroundColor3 = Color3.fromRGB(20, 20, 72 - 52);
			v289.BackgroundTransparency = 0.5;
			v289.Size = UDim2.new(0 + 0, 310 - 220, 0, 271 - 181);
			v289.Image = "rbxassetid://96086736054343";
			v286 = 752 - (485 + 263);
		end
		if (v286 == (711 - (575 + 132))) then
			v289.Position = UDim2.new(861.5 - (750 + 111), -(1035 - (445 + 565)), 0.5 + 0, -25);
			v290 = Instance.new("UICorner");
			v290.CornerRadius = UDim.new(0.2 + 0, 0 - 0);
			v290.Parent = v289;
			v291 = false;
			v292 = nil;
			v286 = 2 + 3;
		end
	end
end});
local v19 = game:GetService("Players");
local v75 = workspace.CurrentCamera;
local v116 = v19.LocalPlayer;
local v117 = v116:GetMouse();
local v18 = game:GetService("UserInputService");
function GetClosestToMouse()
	local v294, v295 = nil, math.huge;
	for v365, v366 in pairs(v19:GetPlayers()) do
		if ((v366 ~= v116) and v366.Character and v366.Character:FindFirstChild("HumanoidRootPart")) then
			local v501 = 310 - (189 + 121);
			local v502;
			local v503;
			local v504;
			while true do
				if (v501 == (1 + 0)) then
					if ((v504 < v295) and v503) then
						local v667 = 1347 - (634 + 713);
						local v668;
						while true do
							if (v667 == (538 - (493 + 45))) then
								v668 = 968 - (493 + 475);
								while true do
									if (v668 == (0 + 0)) then
										v295 = v504;
										v294 = v366;
										break;
									end
								end
								break;
							end
						end
					end
					break;
				end
				if ((784 - (158 + 626)) == v501) then
					local v597 = 0;
					local v598;
					while true do
						if (v597 == 0) then
							v598 = 0 + 0;
							while true do
								if (v598 == (1 - 0)) then
									v501 = 1 + 0;
									break;
								end
								if (v598 == 0) then
									v502, v503 = v75:WorldToScreenPoint(v366.Character.HumanoidRootPart.Position);
									v504 = (Vector2.new(v502.X, v502.Y) - Vector2.new(v117.X, v117.Y)).Magnitude;
									v598 = 1 + 0;
								end
							end
							break;
						end
					end
				end
			end
		end
	end
	return v294;
end
v115:Keybind({["Name"]="Lock Keybind",["Flag"]="Lock Keybind",["UseKey"]=true,["Default"]=Enum.KeyCode.Q,["Callback"]=function(v296)
	TargLockBind = v296;
end});
local function v118()
	local v297 = 1091 - (1035 + 56);
	local v298;
	while true do
		if (v297 == (959 - (114 + 845))) then
			v298 = GetClosestToMouse();
			if v16.Enabled then
				if (v29 and v30) then
					local v636 = 0;
					while true do
						if (2 == v636) then
							workspace.CurrentCamera.CameraSubject = v116.Character.Humanoid;
							if v16.LookAt then
								v116.Character.Humanoid.AutoRotate = true;
							end
							v636 = 3;
						end
						if (1 == v636) then
							v30 = nil;
							game.Players.LocalPlayer.Character.Humanoid.AutoRotate = true;
							v636 = 1 + 1;
						end
						if (v636 == (7 - 4)) then
							v8:Notification("Untargeted", 2 + 0);
							break;
						end
						if (v636 == (1049 - (179 + 870))) then
							v29 = false;
							targetHealth = nil;
							v636 = 1 - 0;
						end
					end
				else
					v29 = true;
					v30 = v298;
					v8:Notification("Target Locked: <font color='" .. v100 .. "'>" .. tostring(v30.DisplayName) .. "</font>", 880 - (827 + 51));
					if (v30 and v30.Character and v30.Character:FindFirstChild("Humanoid")) then
						targetHealth = v30.Character.Humanoid.Health;
					else
						return;
					end
				end
			end
			break;
		end
	end
end
v18.InputBegan:Connect(function(v299, v300)
	if (not v300 and (v299.KeyCode == TargLockBind)) then
		v118();
	end
end);
local v119 = v109:Section({["Name"]="AutoShoot",["Side"]="Right",["Size"]=(132 - 82)});
v119:Button({["Name"]="Auto Shoot Button",["Callback"]=function()
	local v301 = 0 + 0;
	while true do
		if (v301 == 0) then
			if AutoShootButton then
				local v571 = 0;
				while true do
					if (v571 == (473 - (95 + 378))) then
						v8:Notification("Already Loaded.", 1 + 2);
						return;
					end
				end
			end
			AutoShootButton = true;
			v301 = 1 - 0;
		end
		if (v301 == (1 + 0)) then
			v95("Shoot", Color3.fromRGB(255, 0, 1011 - (334 + 677)), function(v545)
				v10.Tech.AutoShoot = v545;
			end);
			break;
		end
	end
end});
local v120 = 0.0001;
local v121 = v109:Section({["Name"]="Hood Custom",["Side"]="Left",["Zindex"]=2});
v121:Toggle({["Name"]="Fire Rate",["Callback"]=function(v302)
	local v303 = 0 - 0;
	while true do
		if (v303 == 0) then
			Noobidiot = v302;
			while Noobidiot do
				local v547 = game:GetService("Players").LocalPlayer;
				for v572, v573 in pairs(v547.Backpack:GetChildren()) do
					if v573:FindFirstChild("GunData") then
						local v637 = 0;
						local v638;
						while true do
							if (v637 == 0) then
								v638 = v573:FindFirstChild("GunData");
								if (v638 and v638:IsA("ModuleScript")) then
									local v732 = require(v638);
									if (v732.cooldown and v732.slowdown_time) then
										local v752 = 1056 - (1049 + 7);
										while true do
											if (v752 == (0 - 0)) then
												v732.cooldown = v120;
												v732.slowdown_time = v120;
												break;
											end
										end
									end
								end
								break;
							end
						end
					end
				end
				task.wait(2);
			end
			break;
		end
	end
end});
v121:Textbox({["Name"]="Gun Delay",["Default"]=tostring(v120),["Callback"]=function(v304)
	v120 = tonumber(v304);
end});
local v122 = v107:Section({["Name"]="Main",["Size"]=(413 - 193)});
v122:Toggle({["Name"]="Enabled",["Callback"]=function(v305)
	v10.Tech.Camera = v305;
end});
v122:Toggle({["Name"]="Flick",["Callback"]=function(v307)
	Flick = v307;
end});
v122:Toggle({["Name"]="Division",["Callback"]=function(v308)
	v10.Tech.UseExternal = v308;
end});
v122:Toggle({["Name"]="Resolver",["Callback"]=function(v310)
	v10.Tech.CamResolverEnabled = v310;
end});
v122:Textbox({["Name"]="Camera Smoothness",["Default"]=tostring(v10.Tech.smoothness),["Callback"]=function(v312)
	v10.Tech.smoothness = tonumber(v312);
end});
v122:List({["Name"]="Easing Style",["Options"]={"Linear","Quad","Cubic","Quart","Quint","Sine","Exponential","Circular","Back","Bounce","Elastic"},["Default"]=v10.Tech.easingStyle,["Callback"]=function(v314)
	v10.Tech.easingStyle = v314;
end});
v122:List({["Name"]="Easing Direction",["Options"]={"In","Out","InOut"},["Default"]=v10.Tech.easingDirection,["Callback"]=function(v316)
	v10.Tech.easingDirection = v316;
end});
local v123 = 0.1 + 0;
local v124 = 0.1 - 0;
local v125 = v107:Section({["Name"]="Prediction",["Side"]="Right",["Size"]=(4 + 156)});
v125:Textbox({["Name"]="Horizontal Prediction",["Default"]=(505.1 - (223 + 282)),["Callback"]=function(v318)
	local v319 = 0 + 0;
	while true do
		if (v319 == 1) then
			v10.Tech.CamPrediction2 = v123;
			v10.Tech.CamPrediction1 = v124;
			break;
		end
		if (v319 == (0 - 0)) then
			v124 = tonumber(v318);
			task.wait(0.01 - 0);
			v319 = 671 - (623 + 47);
		end
	end
end});
v125:Textbox({["Name"]="Vertical Prediction",["Default"]=(45.1 - (32 + 13)),["Callback"]=function(v320)
	v123 = tonumber(v320);
	task.wait(0.01 + 0);
	v10.Tech.CamPrediction2 = v123;
	v10.Tech.CamPrediction1 = v124;
end});
v125:Toggle({["Name"]="AutoPred",["Callback"]=function(v323)
	v10.Tech.CamAutoprediction = v323;
	v10.Tech.CamPrediction2 = v123;
	v10.Tech.CamPrediction1 = v124;
end});
local v126 = v108:Section({["Name"]="Prediction Breaker",["Side"]="Left",["Size"]=(114 + 26)});
v126:Toggle({["Name"]="Jump Prediction",["Callback"]=function(v327)
	v10.Tech.JumpBreak = v327;
end});
v126:Toggle({["Name"]="Enable Anti Lock",["Callback"]=function(v329)
	v77 = v329;
end});
v126:Toggle({["Name"]="Anti Network",["Callback"]=function(v330)
	desyncsleep = v330;
end});
v126:List({["Name"]="Anti Lock Type",["Options"]={"Shake","Behind","Down","Forward","Left","One","Right","Up","Zero"},["Default"]=v78,["Callback"]=function(v331)
	v78 = v331;
end});
local v127 = v108:Section({["Name"]="SkyBox",["Side"]="Right",["Size"]=130});
v127:Toggle({["Name"]="Skybox Enabled",["Callback"]=function(v332)
	local v333 = 0;
	while true do
		if (v333 == (0 + 0)) then
			v92 = v332;
			changeSkybox();
			break;
		end
	end
end});
v127:List({["Name"]="Skybox Type",["Options"]={"1","2","3","4","5","6","7"},["Default"]="1",["Callback"]=function(v334)
	local v335 = 557 - (395 + 162);
	while true do
		if (v335 == (0 + 0)) then
			v93 = tonumber(v334);
			changeSkybox();
			break;
		end
	end
end});
v127:Button({["Name"]="Change",["Callback"]=function()
	changeSkybox();
end});
local v128 = v108:Section({["Name"]="Dance",["Side"]="Left",["Size"]=140});
local v129 = {["Misc"]={["Animation"]={["Enabled"]=false,["SelectedDance"]="Floss",["Speed"]=(1943 - (816 + 1125))}}};
local v130 = game.Players.LocalPlayer;
local v131 = {["Floss"]=(11625203662 - 910863119)};
local v132;
local function v133(v336, v337)
	local v338 = 0;
	local v339;
	while true do
		if (v338 == 1) then
			v339.AnimationId = "rbxassetid://" .. v336;
			v132 = v130.Character.Humanoid:LoadAnimation(v339);
			v338 = 1 + 1;
		end
		if (v338 == (4 - 1)) then
			v339:Destroy();
			break;
		end
		if (v338 == (2 - 0)) then
			v132:Play();
			v132:AdjustSpeed(tonumber(v337) or (1342 - (391 + 950)));
			v338 = 7 - 4;
		end
		if (v338 == (0 - 0)) then
			if v132 then
				v132:Stop();
			end
			v339 = Instance.new("Animation");
			v338 = 2 - 1;
		end
	end
end
v128:Slider({["Name"]="Speed",["Min"]=(0 + 0),["Max"]=(6 + 4),["Default"]=v129.Misc.Animation.Speed,["Suffix"]="",["Decimals"]=(0.001 - 0),["Callback"]=function(v340)
	v129.Misc.Animation.Speed = v340;
end});
v128:List({["Name"]="Dance",["Options"]={"Floss","None"},["Default"]="Floss",["Callback"]=function(v342)
	v129.Misc.Animation.SelectedDance = v342;
end});
v128:Toggle({["Name"]="Animate",["Callback"]=function(v344)
	local v345 = 0 + 0;
	while true do
		if (v345 == (0 - 0)) then
			v129.Misc.Animation.Enabled = v344;
			if v129.Misc.Animation.Enabled then
				local v574 = 0;
				local v575;
				while true do
					if (v574 == 0) then
						v575 = v131[v129.Misc.Animation.SelectedDance or "Floss"];
						if v575 then
							v133(v575, v129.Misc.Animation.Speed or 1);
						end
						break;
					end
				end
			elseif v132 then
				local v639 = 0;
				while true do
					if (v639 == (0 - 0)) then
						v132:Stop();
						v132 = nil;
						break;
					end
				end
			end
			break;
		end
	end
end});
local v134 = v108:Section({["Name"]="ESP",["Side"]="Right",["Size"]=(66 - 26)});
v134:Toggle({["Name"]="Highlight",["Callback"]=function(v346)
	HighlightEnabled = v346;
end});
v8:SetOpen(false);
local v135 = Instance.new("ScreenGui");
v135.Name = "Ui22";
v135.Parent = game.CoreGui;
v135.ZIndexBehavior = Enum.ZIndexBehavior.Sibling;
v135.ResetOnSpawn = false;
local v142 = Instance.new("ImageButton");
v142.Name = "Image3";
v142.Parent = v135;
v142.Active = true;
v142.Draggable = true;
v142.BackgroundColor3 = Color3.fromRGB(1279 - (1147 + 112), 5 + 15, 40 - 20);
v142.BackgroundTransparency = 1;
v142.Size = UDim2.new(0, 90, 0, 24 + 66);
v142.Image = "rbxassetid://92324433288253";
v142.Position = UDim2.new(698 - (335 + 362), -(90 + 5), 0 - 0, 5);
local v152 = Instance.new("UICorner");
v152.CornerRadius = UDim.new(0.2, 0);
v152.Parent = v142;
v142.MouseButton1Click:Connect(function()
	local v347 = 0;
	while true do
		if (v347 == (0 - 0)) then
			Open = not Open;
			v8:SetOpen(Open);
			break;
		end
	end
end);
