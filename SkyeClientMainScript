local Notification = loadstring(game:HttpGet("https://raw.githubusercontent.com/Jxereas/UI-Libraries/main/notification_gui_library.lua", true))()
local stop = Notification.new("info", "Skye Client", "Waiting for Skye Client to load")
wait(0.5)
local stop2 = Notification.new("info", "Skye Client", "Thank you for using Skye!")
wait(2)

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Skye Client", "Ocean")
local SectionCombat = Tab:NewSection("Combat")
local SectionMovement = Tab:NewSection("Movement")
local SectionPlr = Tab:NewSection("Player")
local SectionWorld = Tab:NewSection("World")
local SectionVis = Tab:NewSection("Visuals")

SectionPlr:NewButton("AntiVoid", "Dont die in the void", function()
    		task.spawn(function()
			task.wait(1)
			Module.AntivoidPart.Touched:Connect(function(hit)
				if hit.Parent.Name == game.Players.LocalPlayer.Character.Name then
					for i = 1, math.round(Dropdowns.Jumps[5]) do
						game.Players.LocalPlayer.Character.Humanoid:ChangeState"Jumping"
						task.wait(0.1)
					end
				end
			end)
		end)
		repeat
			task.wait()
			if Module.AntivoidPart then
				Module.AntivoidPart.Color = getgenv().HUDColor
				Module.AntivoidPart.Transparency = Dropdowns.Transparency[5] / 100
				Module.AntivoidPart.Position = Vector3.new(100, 25 - Dropdowns.LowerY[5], 100)
			else
				local AntiPart = Instance.new("Part", workspace)
				AntiPart.Size = Vector3.new(1.999e3, 1, 1.999e3)
				AntiPart.Anchored = true
				AntiPart.Material = Enum.Material.Neon
				AntiPart.Position = Vector3.new(100, 25 - Dropdowns.LowerY[5], 100)
				Module.AntivoidPart = AntiPart
			end
		until Module.Enabled == false or Uninjected == true
		Module.AntivoidPart:Destroy()
		Module.AntivoidPart = nil
end)

SectionPlr:NewButton("AutoConsume", "Consumes anything that is consumable", function()
    local inventory =  game.Players.LocalPlayer.Character:WaitForChild("InventoryFolder").Value
		local EatConnection = inventory.ChildAdded:Connect(function(NewItem)
			if string.find(string.lower(NewItem.Name),"cone") or string.find(string.lower(NewItem.Name),"pie") then
				Bedwars.Eat:CallServerAsync({["item"] = NewItem})
			end
			if string.find(string.lower(NewItem.Name),"app") then
				task.spawn(function()
					repeat
						task.wait()
					until game.Players.LocalPlayer.Character.Humanoid.Health <= Dropdowns.AutoEatHeal[5]
					Bedwars.Eat:CallServerAsync({["item"] = NewItem})
				end)
			end
		end)
		repeat task.wait(1) until Module.Enabled == false or Uninjected == true
		EatConnection:Disconnect()
	end)
end)
SectionVis:NewButton("Chams", "Creates a FE border around everyone on your screen", function()
    local Chams = {}
		repeat
			task.wait(2.5)
			for i,v in pairs(game.Players:GetPlayers()) do
				if v.Character and Utilities.IsAlive(v.Character) and not v.Character:FindFirstChild("Highlight") then
					local Highlight = Instance.new("Highlight",v.Character)
					table.insert(Chams,Highlight)
					task.spawn(function()
						repeat
							task.wait()
							Highlight.FillTransparency = Dropdowns.FillTransparency[5] / 100
							if Dropdowns.TeamColor[3] == false and Utilities.IsAlive(v.Character) and v.Team and v.Team.TeamColor then
								Highlight.FillColor = HUDColor
							else
								Highlight.FillColor = v.Team.TeamColor.Color
							end
							Highlight.OutlineTransparency = Dropdowns.ChamsOutlineTransparency[5]/100
							if Dropdowns.OutlinesMatchFillColor[3] == true then
								Highlight.OutlineColor = HUDColor
							else
								Highlight.OutlineColor = Color3.fromRGB(255,255,255)
							end
						until not v.Character or not Utilities.IsAlive(v.Character)
					end)
				end
			end
end)
Section:NewButton("BedNuker", "Break beds without you brekaing it yourself", function()
    local raycastParams = RaycastParams.new()
		raycastParams.IgnoreWater = true

		GetBeds = function()
			beds = {}
			for i,v in pairs(game.Workspace:GetChildren()) do
				if v.Name == "bed" and v.Covers.BrickColor ~= game.Players.LocalPlayer.Team.TeamColor then
					table.insert(beds,v)
				end
			end
			return beds
		end

		local Damage = game:GetService("ReplicatedStorage").rbxts_include.node_modules.net.out._NetManaged.DamageBlock

		MainNuker = function(bed)
			local part = bed
			local raycastResult = workspace:Raycast(part.Position + Vector3.new(0,24,0), Vector3.new(0,-27,0), raycastParams)

			if raycastResult then
				local TargetBlock = raycastResult.Instance
				print(TargetBlock.Name,"Nuker output")
				for i,v in pairs(TargetBlock:GetChildren()) do
					if v:IsA("Texture") then
						v:Destroy()
					end
				end
				TargetBlock.Color = HUDColor
				TargetBlock.Material = "Neon"
				Damage:InvokeServer({
					["blockRef"] = {
						["blockPosition"] = Vector3.new(math.round(TargetBlock.Position.X/3),math.round(TargetBlock.Position.Y/3),math.round(TargetBlock.Position.Z/3))
					},
					["hitPosition"] = Vector3.new(math.round(TargetBlock.Position.X/3),math.round(TargetBlock.Position.Y/3),math.round(TargetBlock.Position.Z/3)),
					["hitNormal"] = Vector3.new(math.round(TargetBlock.Position.X/3),math.round(TargetBlock.Position.Y/3),math.round(TargetBlock.Position.Z/3))
				})
			end
		end

		repeat
			task.wait(.25)
			local Beds = GetBeds()
			for i,v in pairs(Beds) do
				if Utilities.IsAlive(game.Players.LocalPlayer.Character) then
					if (v.Position - game.Players.LocalPlayer.Character.PrimaryPart.Position).Magnitude < 28.5 then
						MainNuker(v)
					end
				end
			end
		until Module.Enabled == false or Uninjected == true
	end)
end)
SectionCombat:NewButton("KillAura", "Kills someone without you knowing.", function()
    ocal ScreenGui
		local Target
		local anims = 0
		local cam = game.Workspace.Camera
		local origC0 = game.ReplicatedStorage.Assets.Viewmodel.RightHand.RightWrist.C0
		local up1 = game:GetService"TweenService":Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.06 * 2), {
			C0 = origC0 * CFrame.new(1.29, -0.86, 0.06) * CFrame.Angles(math.rad(-30), math.rad(130), math.rad(60))
		})
		local up2 = game:GetService"TweenService":Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.06 * 2), {
			C0 = origC0 * CFrame.new(1.39, -0.86, 0.26) * CFrame.Angles(math.rad(-10), math.rad(50), math.rad(80))
		})
		local down = game:GetService"TweenService":Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.125 * 2), {
			C0 = origC0 * CFrame.new(1.29, -0.86, 5.06) * CFrame.Angles(math.rad(-30), math.rad(130), math.rad(60))
		})

		local zylaanim1 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.3)), {
			C0 = origC0 * CFrame.new(0.3, -2, 0.5) * CFrame.Angles(-math.rad(190), math.rad(110), -math.rad(90))
		})

		local zylaanim2 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(0.3, -1.5, 1.5) * CFrame.Angles(math.rad(120), math.rad(140), math.rad(320))
		})

		local spinny1 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .5) * CFrame.Angles(math.rad(-30), math.rad(0), math.rad(0))
		})
		
		local spinny2 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .5) * CFrame.Angles(math.rad(-120), math.rad(0), math.rad(0))
		})
		
		local spinny3 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .5) * CFrame.Angles(math.rad(-180), math.rad(0), math.rad(0))
		})
		
		local spinny4 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .5) * CFrame.Angles(math.rad(-240), math.rad(0), math.rad(0))
		})
		
		local spinny5 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .5) * CFrame.Angles(math.rad(-300), math.rad(0), math.rad(0))
		})
		
		local spinny6 = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new((0.1)), {
			C0 = origC0 * CFrame.new(1, -0.5, .6) * CFrame.Angles(math.rad(-360), math.rad(0), math.rad(0))
		})

		task.spawn(function()
			repeat
				task.wait()
				if Dropdowns.Visuals[3] == true then
					if ScreenGui == nil then
						ScreenGui = Instance.new"ScreenGui"
						local Frame = Instance.new"Frame"
						local Frame_2 = Instance.new"Frame"
						local TextLabel = Instance.new"TextLabel"
						local Frame_3 = Instance.new"Frame"
						local Frame_4 = Instance.new"Frame"
						local TextLabel_2 = Instance.new"TextLabel"
						local TextLabel_3 = Instance.new"TextLabel"
						local ViewportFrame = Instance.new"ViewportFrame"
						ScreenGui.Parent = game.CoreGui
						ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
						Frame.Parent = ScreenGui
						Frame.AnchorPoint = Vector2.new(0.5, 0.5)
						Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
						Frame.BackgroundTransparency = 0.6
						Frame.BorderSizePixel = 0
						Frame.Position = UDim2.new(0.5, 0, 0.766917288, 0)
						Frame.Size = UDim2.new(0, 270, 0, 124)
						Frame_2.Parent = Frame
						Frame_2.AnchorPoint = Vector2.new(0.5, 0.5)
						Frame_2.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
						Frame_2.BackgroundTransparency = 0.6
						Frame_2.BorderSizePixel = 0
						Frame_2.Position = UDim2.new(0.5, 0,0.493, 0)
						Frame_2.Size = UDim2.new(0, 254, 0, 27)
						TextLabel.Parent = Frame_2
						TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel.BackgroundTransparency = 1
						TextLabel.Position = UDim2.new(-4.58937883e-3, 0, 5.22103906e-3, 0)
						TextLabel.Size = UDim2.new(1, 0, 1, 0)
						TextLabel.ZIndex = 3
						TextLabel.Font = Enum.Font.GothamMedium
						TextLabel.Text = "17.6"
						TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel.TextScaled = true
						TextLabel.TextSize = 14
						TextLabel.TextStrokeTransparency = 0
						TextLabel.TextWrapped = true
						Frame_3.Parent = Frame_2
						Frame_3.BackgroundColor3 = Color3.fromRGB(183, 0, 255)
						Frame_3.BorderSizePixel = 0
						Frame_3.Size = UDim2.new(1,0,1,0)
						TextLabel_2.Parent = Frame
						TextLabel_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel_2.BackgroundTransparency = 1
						TextLabel_2.Position = UDim2.new(0.032, 0,0.711, 0)
						TextLabel_2.Size = UDim2.new(0, 254, 0, 23)
						TextLabel_2.Font = Enum.Font.GothamMedium
						TextLabel_2.Text = "72ms"
						TextLabel_2.TextColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel_2.TextScaled = true
						TextLabel_2.TextSize = 14
						TextLabel_2.TextStrokeTransparency = 0
						TextLabel_2.TextWrapped = true
						TextLabel_2.TextXAlignment = Enum.TextXAlignment.Left
						TextLabel_3.Parent = Frame
						TextLabel_3.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel_3.BackgroundTransparency = 1
						TextLabel_3.Position = UDim2.new(0.03, 0, 0.093, 0)
						TextLabel_3.Size = UDim2.new(0, 254, 0, 21)
						TextLabel_3.Font = Enum.Font.GothamMedium
						TextLabel_3.Text = "Player445"
						TextLabel_3.TextColor3 = Color3.fromRGB(255, 255, 255)
						TextLabel_3.TextScaled = true
						TextLabel_3.TextSize = 14
						TextLabel_3.TextStrokeTransparency = 0
						TextLabel_3.TextWrapped = true
						TextLabel_3.TextXAlignment = Enum.TextXAlignment.Left
						task.spawn(function()
							repeat
								task.wait()
								task.spawn(function()
									if Utilities.IsAlive(player.Character) and Target and Utilities.IsAlive(Target.Character) and (game.Players.LocalPlayer.Character.PrimaryPart.Position - Target.Character.PrimaryPart.Position).Magnitude < 25 then
										player.Character.Humanoid.Jump = true
										ScreenGui.Enabled = true
										TextLabel_3.Text = Target.Name
										TextLabel.Text = math.round(Target.Character.Humanoid.Health / 5)
										TextLabel_2.Text = math.round(game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue()).."ms"
										Frame_3.Size = UDim2.new(Target.Character.Humanoid.Health * 0.01, 0, 1, 0)
										Frame_3.BackgroundColor3 = getgenv().HUDColor
									else
										ScreenGui.Enabled = false
									end
								end)
							until not ScreenGui
						end)
					end
				end
				if Target and Utilities.IsAlive(Target.Character) and (game.Players.LocalPlayer.Character.PrimaryPart.Position - Target.Character.PrimaryPart.Position).Magnitude < 25 then
					if Dropdowns.Autoblock[3] == 1 then
						anims = anims + 1
						up1:Play()
						task.wait(0.06 * 2)
						up2:Play()
						task.wait(0.06 * 2)
						if anims == 2 then
							anims = 0
							down:Play()
							task.wait(0.075 * 2)
						end
					elseif Dropdowns.Autoblock[3] == 2 then
						zylaanim1:Play()
						task.wait(.3)
						zylaanim2:Play()
						task.wait(.1)
					elseif Dropdowns.Autoblock[3] == 4 then
						spinny1:Play()
						task.wait(.05)
						spinny2:Play()
						task.wait(.05)
						spinny3:Play()
						task.wait(.05)
						spinny4:Play()
						task.wait(.05)
						spinny5:Play()
						task.wait(.05)
						spinny6:Play()
						task.wait(.05)
					end
				else
					local stop = game:GetService"TweenService":Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.4), { C0 = origC0 })
					stop:Play()
				end
			until Module.Enabled == false
			local stop = game:GetService"TweenService":Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.4), { C0 = origC0 })
			stop:Play()
		end)
		local Delay = false
		local OldVisual
		repeat
			task.spawn(function()
				if Utilities.IsAlive(game.Players.LocalPlayer.Character) then
					local Nearby = Utilities.GetNearbyPlayers(17.5, false)
					for i, v in pairs(Nearby) do
						if Utilities.IsAlive(v.Character) and v.Team ~= game.Players.LocalPlayer.Team then
							Target = v
							if Dropdowns.AutoJump[3] == true and Utilities.IsAlive(player.Character) then
								if Delay == false then
									Delay = true
									player.Character.Humanoid:ChangeState("Jumping")
									task.wait(.5)
									Delay = false
								end
							end
						end
					end
					if Target and Target.Character and Target.Character:FindFirstChild("HumanoidRootPart") and Utilities.IsAlive(Target.Character) and Utilities.IsAlive(player.Character) then
						local mouse = Ray.new(cam.CFrame.Position, Target.Character.HumanoidRootPart.Position).Unit.Direction
						local cameraposition = cam.CFrame.Position
							KillauraRemote:FireServer{
								["entityInstance"] = Target.Character,
								["chargedAttack"] = {
									["chargeRatio"] = 1
								},
								["validate"] = {
									["raycast"] = {
										["cursorDirection"] = {
											["value"] = mouse
										},
										["cameraPosition"] = {
											["value"] = cameraposition }
										},
										["selfPosition"] = {
											["value"] = getcloserpos(game.Players.LocalPlayer.Character.PrimaryPart.Position, Target.Character.PrimaryPart.Position, 4)
										},
										["targetPosition"] = {
											["value"] = Utilities.Predict(Target)
										}
									},
								["weapon"] = Utilities.GetBestSword()
							}
						if Dropdowns.Swing[3] == true then
							local Loaded = game.Players.LocalPlayer.Character.Humanoid:LoadAnimation(SwingAnimation)
							Loaded:Play()
						end
					end
				end
			end)
			task.wait(.32)
end)
