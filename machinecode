----- Combat Visualiser v1.0 -----
----- Load Player -----
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local char = player.Character
local humanoid = char.Humanoid
local currentSpell = ""
local childaddedConnection
local childremovedConnection
local buttondownConnection
local buttondowntwoConnection
local childaddedtwoConnection
local childremovedtwoConnection
local lgui
local rgui
local mgui
game.Players.LocalPlayer.CharacterAdded:Connect(function(newChar)
	player = game.Players:FindFirstChild(newChar.Name)
	char = newChar
	humanoid = newChar:WaitForChild("Humanoid")
	createHPDisplay()
	humanoid.Died:Connect(function()
		resetPanel()
		if childaddedConnection ~= nil then
			childaddedConnection:Disconnect()
		end
		if childremovedConnection ~= nil then
			childremovedConnection:Disconnect()
		end
		if childaddedtwoConnection ~= nil then
			childaddedtwoConnection:Disconnect()
		end
		if childremovedtwoConnection ~= nil then
			childremovedtwoConnection:Disconnect()
		end
	end)
	repeat
		wait()
	until newChar:FindFirstChild("HumanoidRootPart") ~= nil
	if lgui.Parent.Enabled == true then
		panelSetup(false)
	else
		panelSetup(true)
	end
	repeat
		wait()
	until newChar:FindFirstChild("RightRuneArm") ~= nil and newChar:FindFirstChild("LeftRuneArm") ~= nil
	wait(3)
	playerLoop("Pre-Load")
end)
----- Load Interface -----
local manaColor = Color3.fromRGB(255,255,255)
local ambientColor = Color3.fromRGB(160,160,160)
local UIColor = Color3.fromRGB(32, 41, 59)
local cp = game:GetObjects("rbxassetid://5433782451")[1]
local rp = cp.RightPanel
local lp = cp.LeftPanel
local mp = cp.MidPart
local midp = cp.MidPanel
midp.Transparency = 1


for i,v in pairs(cp:GetChildren()) do
	if v.ClassName == "SurfaceGui" then
		if v.Name == "LeftGUI" then
			v.Adornee = lp
			lgui = v.Backdrop
		elseif v.Name == "RightGUI" then
			v.Adornee = rp
			rgui = v.Backdrop
		elseif v.Name == "MidGUI" then
			v.Adornee = midp
			mgui = v.Backdrop
		end
		v.Parent = game.CoreGui
	end
end

local colorAdjust = mgui.UIColorAdjustSlider
colorAdjust.Green.Text = math.floor(UIColor.G*255)
colorAdjust.Blue.Text = math.floor(UIColor.B*255)
local manaAdjust = mgui.ManaColorSlider
manaAdjust.Red.Text = math.floor(manaColor.R*255)
manaAdjust.Green.Text = math.floor(manaColor.G*255)
manaAdjust.Blue.Text = math.floor(manaColor.B*255)
local ambientAdjust = mgui.AmbientSlider
ambientAdjust.Red.Text = math.floor(ambientColor.R*255)
ambientAdjust.Green.Text = math.floor(ambientColor.G*255)
ambientAdjust.Blue.Text = math.floor(ambientColor.B*255)
cp.Parent = game.Workspace
cp.PrimaryPart = mp

function resetPanel()
	for q,r in pairs(cp:GetChildren()) do
		r.Anchored = true
	end
	for i,v in pairs(char.HumanoidRootPart:GetChildren()) do
		if v.ClassName == "WeldConstraint" then
			v:Destroy()
		end
	end
end

humanoid.Died:Connect(function()
	resetPanel()
end)

function panelSetup(disabled)
	for q,r in pairs(cp:GetChildren()) do
		if r.ClassName == "Part" then
			r.Anchored = false
		end
	end
	if disabled == true then
		cp:SetPrimaryPartCFrame(char.HumanoidRootPart.CFrame * CFrame.new(0,20,0) * CFrame.Angles(0,math.rad(180),0))
	else
		cp:SetPrimaryPartCFrame(char.HumanoidRootPart.CFrame * CFrame.Angles(0,math.rad(180),0))
	end
	local attachList = {
		{"HumanoidRootPart",mp},
		{"HumanoidRootPart",rp},
		{"HumanoidRootPart",lp},
		{"HumanoidRootPart",midp}
	}
	for i = 1,4 do
		local weld = Instance.new("WeldConstraint")
		weld.Parent = char:FindFirstChild(attachList[i][1])
		weld.Part0 = char:FindFirstChild(attachList[i][1])
		weld.Part1 = attachList[i][2]
	end
end
panelSetup(false)

----- Interface Settings -----
local guiSettings = {
	["Cooldowns Indicator"] = false,
	["Clear Ambient"] = false,
	["Player Spectate"] = false,
	["Chat / illusionist"] = false,
	["GUI Settings"] = false,
	["Damage Indicator"] = false,
  --------------------------
	["Vitality Stats"] = false,
	["Spell Assist"] = false,
	["Player ESP"] = false,
	["Seer - Intent"] = false,
	["Mana Display"] = false,
	["Backpack Display"] = false
}
local playerList = {}

function changeSettings(btn)
	local btnText = btn.Text
	for name,value in pairs(guiSettings) do
		if btnText == name and value == false then
			guiSettings[name] = not guiSettings[name]
			btn.BackgroundColor3 = Color3.fromRGB(126, 255, 126)
			btn.TextColor3 = Color3.new(0,0,0)
		elseif btnText == name and value == true then
			guiSettings[name] = not guiSettings[name]
			btn.BackgroundColor3 = UIColor
			btn.TextColor3 = Color3.new(255, 255, 255)
		end
	end
end

function createHPDisplay()
	local hpFrame = game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("Container"):WaitForChild("Health"):WaitForChild("Slider")
	local manaFrame = game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana")
	local Details = Instance.new("TextLabel")
	local manaDetails = Instance.new("TextLabel")
	local mana = game.Players.LocalPlayer.Character:WaitForChild("Mana")
	local hum = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
	local hpNotZero = true
	Details.Name = "HealthDisplay"
	manaDetails.Name = "ManaDisplay"
	if guiSettings["Vitality Stats"] == true then
		Details.Visible = true
		manaDetails.Visible = true
	else
		Details.Visible = false
		manaDetails.Visible = false
	end
	Details.Parent = hpFrame
	Details.BackgroundColor3 = Color3.fromRGB(255,255,255)
	Details.BackgroundTransparency = 1.000
	Details.LayoutOrder = 1
	manaDetails.Parent = manaFrame
	manaDetails.BackgroundTransparency = 1
	manaDetails.LayoutOrder = 1
	if hpFrame.AbsoluteSize.X <= 58 then
		Details.Position = UDim2.new(0, math.abs(58 - hpFrame.AbsoluteSize.X)/2, 0, 0)
	else
		Details.Position = UDim2.new(0, 0, 0, 0)
	end	
	Details.Size = UDim2.new(1, 0, 1, 0)
	Details.Font = Enum.Font.Fantasy
	Details.Text = "[ " .. math.floor(hum.Health)  .. " | " .. math.floor(hum.MaxHealth) .. " ]"
	Details.TextColor3 = Color3.fromRGB(255,255,255)
	Details.TextSize = 16
	Details.ZIndex = 5
	Details.TextStrokeColor3 = Color3.fromRGB(0,0,0)
	Details.TextStrokeTransparency = 0
	manaDetails.Size = UDim2.new(1, 0, 1, 0)
	manaDetails.Font = Enum.Font.Fantasy
	manaDetails.Text = math.floor(mana.Value)
	manaDetails.TextColor3 = Color3.fromRGB(255,255,255)
	manaDetails.TextSize = 16
	manaDetails.ZIndex = 5
	manaDetails.TextStrokeColor3 = Color3.fromRGB(0,0,0)
	manaDetails.TextStrokeTransparency = 0
	local barFrame = game.Players.LocalPlayer.PlayerGui.StatGui.LeftContainer.Mana
	local folder = Instance.new("Folder")
	folder.Name = "SpellAssistFolder"
	folder.Parent = barFrame
	for i = 1,18 do
	    spawn(function()
	        if i ~= 5 and i <= 9 then
	            local manaDetails = Instance.new("TextLabel")
	    		manaDetails.Size = UDim2.new(1, 0, 0, 0)
	    		manaDetails.Position = UDim2.new(0, 0, i/10, 0)
	    		manaDetails.BackgroundTransparency = 1
	    		manaDetails.Font = Enum.Font.Fantasy
	    		manaDetails.Text = 100-i*10
	    		manaDetails.TextColor3 = Color3.fromRGB(255, 255, 255)
	    		manaDetails.TextSize = 14
	    		manaDetails.ZIndex = 5
	    		manaDetails.TextStrokeColor3 = Color3.fromRGB(0,0,0)
	    		manaDetails.TextStrokeTransparency = 0
				manaDetails.Parent = folder
				if guiSettings["Spell Assist"] == true then
					manaDetails.Visible = true
				else
					manaDetails.Visible = false
				end
	    	end
	        local Frame = Instance.new("Frame")
	        Frame.Parent = folder
	        Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
			Frame.BorderSizePixel = 0
			if guiSettings["Spell Assist"] == true then
				Frame.Visible = true
			else
				Frame.Visible = false
			end
	        if i <= 9 then
	            Frame.Position = UDim2.new(0, 0, i/10, 0)
	        else
	            Frame.Position = UDim2.new(1, -5, i/10-1+0.1, 0)
	        end
	        Frame.Size = UDim2.new(0.17, 0, 0, 1)
	        if i == 5 or i == 14 then
	            Frame.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
	        else
	            Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	        end
	        Frame.ZIndex = 5
	    end)
	end
	hum.HealthChanged:Connect(function(health)
		Details.Text = "[ " .. math.floor(health)  .. " | " .. math.floor(hum.MaxHealth) .. " ]"
		if hpFrame.AbsoluteSize.X <= 58 then
			Details.Position = UDim2.new(0, math.abs(58 - hpFrame.AbsoluteSize.X)/2, 0, 0)
		else
			Details.Position = UDim2.new(0, 0, 0, 0)
		end
		if health == 0 and hpNotZero == true then
			hpNotZero = false
			spawn(function()
				for i = 1,16 do
					wait(0.25)
					Details.Position = UDim2.new(0, 30, 0, 0)
					Details.TextSize = Details.TextSize - 1
				end
			end)
		end
	end)
	mana.Changed:Connect(function(manaVal)
		manaDetails.Text = math.floor(manaVal)
	end)
end
createHPDisplay()

----- Spell Information -----
local spellList = {
    ["Armis"] = {{35,60,75,85}, 10, false},
    ["Celeritas"] = {{70,90}, 3, false},
    ["Gate"] = {{70,80,80,85}, 1, false}, 
    ["Gelidus"] = {{80,100,95,100}, 3, false}, 
    ["Ignis"] = {{80,100,50,60}, 10, false},
    ["Nocere"] = {{65,80,55,85}, 7, false},
    ["Sagitta Sol"] = {{40,65,30,40}, 3, false},
    ["Shrieker"] = {{25,60}},
    ["Snarvindur"] = {{70,80,10,35}, 5, false},
    ["Telorum"] = {{75,90,60,75}, 4.5, false},
    ["Tenebris"] = {{90,100,40,60}},
    ["Trickstus"] = {{30,70,15,40}, 3, false},
    ["Velo"] = {{60,90,50,60}, 1, false}, 
    ["Viribus"] = {{15,30,60,75}, 3, false},
    ["Contrarium"] = {{70,90,60,90}, 10, false},
    ["Fimbulvetr"] = {{85,95}},
    ["Hoppa"] = {{30,60,45,60}, 5, false},
    ["Manus Dei"] = {{85,95,45,55}},
    ["Percutiens"] = {{60,70,70,80}, 4, false},
    ["Sraunus"] = {{10,30}},
    ["Gourdus"] = {{90,100}},
    ["Hystericus"] = {{70,90,10,35}, 3, false},
	["Nosferatus"] = {{90,100}},
	["Trahere"] = {{70,90}},
    ["Pondus"] = {{70,90,10,50}},
	["Mori"] = {{97,100}},
	["Howler"] = {{60,80}},
	["Intermissium"] = {{60,100}},
	["Ligans"] = {{50,70}},
	["Reditus"] = {{40,100}},
	["Custos"] = {{50,70}},
	["Globus"] = {{50,100}},
	["Dominus"] = {{50,90}},
	["Claritum"] = {{95,100}},
	["Verdien"] = {{70,100}, 3, false},
	["Fons Vitae"] = {{70,100}, 6.5, false},
	["Floresco"] = {{90,100}, 3, false},
	["Perflora"] = {{70,80}}
}

local specialSpells = {
	["Floresco"] = {5, false},
}

----- Spell Identifier -----
local classIdentify = {
	["DRUID"] = {"Perflora", "Floresco", "WiseCasting", "FastSigns"},
	["WHPSRR"] = {"Elegant Slash", "Needle's Eye", "RapierTraining", "Acrobat"},
	["D-SIGL"] = {"Dark Eruption", "Dark Flame Burst", "PlateTraining", "MercenaryCarry"},
	["SHNOBI"] = {"Grapple", "Resurrection", "FeatherFall", "UpgradedAgility"},
	["FACLES"] = {"Ethereal Strike", "Shadow Fan", "Chain Lethality", "UpgradedBane"},
	["M-NCRO"] = {"Secare", "Furantur", "WiseCasting", "FastSigns"},
	["DEEP-K"] = {"Chain Pull", "Leviathan Plunge", "FighterFootwork", "PrinceBlessing"},
	["ABYS-W"] = {"Abyssal Scream", "Wrathful Leap", "Abysswalker", "MercenaryCarry"},
	["ONI"] = {"Axe Kick", "Demon Step", "Misogi", "TrainedCombat"},
	["LPRDST"] = {"Ruby Shard", "Sapphire Shard", "Sharpener", "Gemcutter"},
	["U-BARD"] = {"Sweet Soothing", "Joyous Dance", "MusiciansResolve", "BlastMeter"},
	["M-ILUS"] = {"Globus", "Dominus", "WiseCasting", "FastSigns"},
	["SIGL-C"] = {"Charged Blow", "Hyper Body", "ChargeMastery", "PlateTraining"},
	["DSAGE"] = {"Lightning Drop", "Lightning Elbow", "MonasteryShield", "ChiBlock"},
	["DSLAYR"] = {"Thunder Spear Crash", "Dragon Blood", "FighterFootwork", "Dragon Awakening"}
}

----- Artifacts -----
local artifactList = {
	["PhilosophersStone"] = Color3.fromRGB(200,50,50),
	["HowlerFriend"] = Color3.fromRGB(212, 145, 35),
	["LannisAmulet"] = Color3.fromRGB(183, 0, 255),
	["Rift Gem"] = Color3.fromRGB(255,0,0),
	["NightStone"] = Color3.fromRGB(0,0,0),
	["Fairfrozen"] = Color3.fromRGB(50,50,255),
	["Spider Cloak"] = Color3.fromRGB(254, 247, 16),
	["ScroomKey"] = Color3.fromRGB(104, 104, 104),
}

local cooldowns = {
	----- Dragon Slayer -----
	["Spear Crusher"] = {10, false},
	["Wing Soar"] = {15, false},
	["Thunder Spear Crash"] = {15, false},
	["Dragon Roar"] = {15, false},
	["Ensnaring Strikes"] = {21, false},
	["Heroic Volley"] = {11, false},
	["Justice Spears"] = {1, false},
	["Triple Strike"] = {15, false},
	["Serpent Strike"] = {15, false},
	
	----- Abyss Walker -----
	["Great Cyclone"] = {10, false},
	["Void Slicer"] = {10, false},
	["Deflecting Spin"] = {17, false},
	["Spinning Soul"] = {30, false},
	["Wrathful Leap"] = {11, false},
	["Shoulder Bash"] = {15, false},
	["Abyssal Scream"] = {15, false},
	
	----- Oni -----
	["Demon Flip"] = {10, false},
	["Rising Dragon"] = {10, false},
	["Augimas"] = {4, false},
	["Augimas 2"] = {6.5, false},
	["Rampage"] = {20, false},
	["Leg Breaker"] = {10, false},
	["Spin Kick"] = {10, false},
	["Axe Kick"] = {10, false},
	
	----- Spy -----
	["Bomb Jump"] = {15, false},
	["Duelist Dash"] = {15, false},
	["Mana Grenade"] = {10, false},
	["Bullseye"] = {15, false},
	["Auto Reload"] = {20, false},
	["Needle's Eye"] = {10, false},
	["The Wraith"] = {2.5, false},
	["Elegant Slash"] = {10, false},
	
	----- Blacksmith -----
	["Sapphire Shard"] = {15, false},
	["Ruby Shard"] = {15, false},
	["Opal Shard"] = {15, false},
	
	----- Shinobi -----
	["Bane"] = {40, false},
	["Grapple"] = {4, false},
	["Rising Cloud"] = {4, false},
	["Cruel Wind"] = {4, false},
	["Owl Slash"] = {8, false},
	["Shadowrush"] = {8, false},
	
	----- Deep Knight -----
	["Soul Siphon"] = {15, false},
	["Light Piercer"] = {8.5, false},
	["Tethering Lance"] = {1, false},
	["Deep Sacrifice"] = {15, false},
	["Void Spear"] = {10, false},
	["Leviathan Plunge"] = {80, false},
	["Impale"] = {15, false},
	["Chain Pull"] = {15, false},
	
	----- Sigil -----
	["Action Surge"] = {22, false},
	["Pommel Strike"] = {15, false},
	["Disarming Strike"] = {30, false},
	["Hyper Body"] = {120, false},
	["Rod of Narsa"] = {5, false},
	["Chain of Fate"] = {15, false},
	["Parmarktini"] = {5, false},
	["Counter"] = {10, false},
	
	----- Illusionist -----
	["Dominus"] = {20, false},
	["Custos"] = {25, false},
	["Compress"] = {8, false},
	["Duobe"] = {18, false},
	["Terra Rebus"] = {10, false},
	
	----- Necromancer -----
	["Command Monsters"] = {2, false},
	
	----- Druid -----
	["Mirgeti"] = {5, false},
	["Spindylus"] = {5, false},
	["Krusa"] = {10, false},
	
	----- Faceless -----
	["Shadow Fan"] = {10, false},
	["Ethereal Strike"] = {15, false},
	["Dagger Throw"] = {10, false},
	["Lethality"] = {30, false},
	["Triple Dagger Throw"] = {10, false},
	["Falling Darkness"] = {25, false},
	["Flash of Darkness"] = {25, false},
	
	----- Dragon Sage -----
	["Lightning Drop"] = {6, false},
	["Lightning Elbow"] = {10, false},
	["Monastic Stance"] = {30, false},
	["Seismic Toss"] = {10, false},
	["Lightning Smite"] = {15, false},
	["Thundering Leap"] = {15, false},
	
	----- Dark Sigil -----
	["Dark Eruption"] = {15, false},
	["Hunt"] = {20, false},
	["Mirror"] = {10, false},
	["Soul Burst"] = {10, false},
	["Chase"] = {10, false},
	
	----- Race Abilities -----
	["Flock"] = {20, false},
	["Shift"] = {2.5, false},
	["Trinket Shift"] = {1, false},
	["Exhaust"] = {30, false},
	["Swiftfoot"] = {45, false},
	["Fury"] = {300, false},
	
	----- Special Moves -----
	["Subzero Strike"] = {15, false},
	["Pickpocket"] = {3, false},
	["Agility"] = {40, false},
	["Stealth"] = {25, false}
}
local cooldownTool = ""

----- Chat Logger -----
local cl = game:GetObjects("rbxassetid://5433987516")[1]
local iln = game:GetObjects("rbxassetid://5422063768")[1]

cl.ScrollingFrame.UIGridLayout.CellPadding = UDim2.new(0,2,0,0)
cl.Parent = game.CoreGui
cl.Enabled = false
iln.Parent = game.CoreGui
iln.Enabled = false
cl.ScrollingFrame.CanvasPosition = Vector2.new(0,99999)
local chatCount = 1
local chatList = {}

game.Players.LocalPlayer.Chatted:Connect(function(msg)
	local labelClone = cl.Label:Clone()
	labelClone.Parent = cl.ScrollingFrame
	labelClone.Text =  "[" .. game.Players.LocalPlayer.Name .. "]" .. " | [YOU]: " .. msg
	labelClone.BackgroundTransparency = 0.9
	labelClone.TextColor3 = Color3.fromRGB(255,255,255)
	cl.ScrollingFrame.CanvasPosition = Vector2.new(0,99999)
	labelClone.Name = chatCount
	chatCount = chatCount + 1
	table.insert(chatList, chatCount)
	if chatCount > 57 then
		local min = math.min(table.unpack(chatList))
		table.remove(chatList,table.find(chatList, min))
		cl.ScrollingFrame:FindFirstChild(tostring(min)):Destroy()
	end
end)

----- Pre-Load Functions -----
game.Players.PlayerAdded:Connect(function(newPlayer)
	newPlayer.CharacterAdded:Connect(function(newChar)
		repeat
			wait()
		until newChar:FindFirstChild("RightRuneArm") ~= nil and newChar:FindFirstChild("LeftRuneArm") ~= nil
		wait(2)
		playerLoop("Pre-Load")
		playerLoop("Update Player ESP")
	end)	
end)
----- Pre-Load Functions -----
local ng = game:GetObjects("rbxassetid://5371794057")[1]
local ds = game:GetObjects("rbxassetid://5371796279")[1]
local mg = game:GetObjects("rbxassetid://5378742254")[1]
local bg = game:GetObjects("rbxassetid://5384318164")[1]
local dg = game:GetObjects("rbxassetid://5398863815")[1]
bg.ScrollingFrame.UIGridLayout.CellPadding = UDim2.new(0,2,0,6)
bg.Parent = game.CoreGui
bg.Enabled = false

----- Common Functions -----
function playerLoop(condition)
	for i,v in pairs(game.Players:GetChildren()) do
		if v.Name ~= game.Players.LocalPlayer.Name then
			if condition == "Pre-Load" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Head") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") == nil then
						local currentClass = ""
						for x,y in pairs(classIdentify) do
							if (v.Backpack:FindFirstChild(y[1]) ~= nil or v.Backpack:FindFirstChild(y[2]) ~= nil) and (v.Backpack:FindFirstChild(y[3]) ~= nil and v.Backpack:FindFirstChild(y[4]) ~= nil) then
								currentClass = x
								break
							else
								currentClass = "N/A"
							end
						end
						if playerList[v.Name] == nil then
							v.Chatted:Connect(function(msg)
								if playerList[v.Name] and playerList[v.Name][2] then
									local labelClone = cl.Label:Clone()
									labelClone.Parent = cl.ScrollingFrame
									labelClone.Text = "[" .. v.Name .. "]" .. " | " .. "[" .. playerList[v.Name][1] .. "]: " .. msg
									labelClone.BackgroundTransparency = 0.9
									labelClone.TextColor3 = playerList[v.Name][2]
									labelClone.Name = chatCount
									chatCount = chatCount + 1
									table.insert(chatList, chatCount)
									if chatCount > 57 then
										local min = math.min(table.unpack(chatList))
										table.remove(chatList,table.find(chatList, min))
										cl.ScrollingFrame:FindFirstChild(tostring(min)):Destroy()
									end
								end
							end)
						end
						
						for q,r in pairs(v.Character:GetChildren()) do
							if r.ClassName == "Model" and r.Name ~= "LeftRuneArm" and r.Name ~= "RightRuneArm" and r.Name ~= "Shadow Buddy" then
								playerList[v.Name] = {r.Name, Color3.fromRGB(math.random(150,255), math.random(150,255), math.random(150,255))}
							end
						end
						
						local ngClone = ng:Clone()
						local dsClone = ds:Clone()
						local mgClone = mg:Clone()
						local dgClone = dg:Clone()
						local dmgLabel = dgClone.Damage
						local currentHealth = v.Character:WaitForChild("Humanoid").Health
						ngClone.Details.Font = Enum.Font.Fantasy
						ngClone.Details.TextColor3 = Color3.fromRGB(200,200,200)
						ngClone.Parent = v.Character:WaitForChild("Head")
						dsClone.Parent = v.Character:WaitForChild("Head")
						mgClone.Parent = v.Character:WaitForChild("Head")
						dgClone.Parent = v.Character:WaitForChild("Head")
						
						if guiSettings["Vitality Stats"] == true then
							print(1)
							playerLoop("Vitality Stats False")
							print(2)
						end
						if guiSettings["Player ESP"] == true then
							print(3)
							playerLoop("Player ESP False")
							print(4)
						end
						if guiSettings["Mana Display"] == true then
							print(5)
							playerLoop("Mana Display False")
							print(6)
						end
						local healthchangedConnection
						local stomachchangedConnection
						local toxicitychangedConnection
						local tempchangedConnection
						local manachangedConnection
						local function healthchanged(health)
							local maxHealth = v.Character.Humanoid.MaxHealth
							v.Character.Head:FindFirstChild("DisplayStats").HealthBackdrop.Healthbar.Size = UDim2.new((health/maxHealth),0,1,0)
							local tool = v.Character:FindFirstChildWhichIsA("BackpackItem")
							
							
							if tool and guiSettings["Seer - Intent"] == true then
								v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]" .. " | " .. tool.Name
							else
								v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]"
							end
							if health < currentHealth and guiSettings["Damage Indicator"] == true then
								if game.Players.LocalPlayer.Character ~= nil then
									if game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") ~= nil then
										local dmgLabelClone = dmgLabel:Clone()
										dmgLabelClone.TextSize = 16
										dmgLabelClone.TextTransparency = 0
									    for i,v in pairs(dgClone:GetChildren()) do
										    if v.Name == "NewLabel" then
										    	v:Destroy()
										    end
									    end
										dmgLabelClone.Name = "NewLabel"
										dmgLabelClone.Parent = dgClone
										if math.floor(currentHealth-health) == 0 then
											dmgLabelClone.Text = "-1"
										else
											dmgLabelClone.Text = "-" .. math.floor(currentHealth-health)
										end
										dmgLabelClone.TextColor3 = Color3.fromRGB(math.random(150,255), math.random(150,255), math.random(150,255))
										local num = math.random(0,1)
										if num == 0 then
											dmgLabelClone.Rotation = 15
											dgClone.SizeOffset = Vector2.new((0.3/math.random(10)),dgClone.SizeOffset.Y)
										elseif num == 1 then
											dmgLabelClone.Rotation = -15
											dgClone.SizeOffset = Vector2.new(-(0.3/math.random(10)),dgClone.SizeOffset.Y)
										end
								        
										local num = math.random(0,1)
										if num == 0 then
											dgClone.SizeOffset = Vector2.new(dgClone.SizeOffset.X,(1/math.random(100)))
										elseif num == 1 then
											dgClone.SizeOffset = Vector2.new(dgClone.SizeOffset.X,-(1/math.random(100)))
										end
										spawn(function()
											wait(1.15)
											for i = 1,10 do
												if dmgLabelClone ~= nil then
													dmgLabelClone.TextTransparency = dmgLabelClone.TextTransparency + 0.1
												end
								                wait(0.05)
											end
											if dmgLabelClone ~= nil then
												dmgLabelClone:Destroy()
											end
										end)
									end
								end
							end
							currentHealth = health
							if health <= 0 then
								healthchangedConnection:Disconnect()
								stomachchangedConnection:Disconnect()
								toxicitychangedConnection:Disconnect()
								tempchangedConnection:Disconnect()
								manachangedConnection:Disconnect()
							end
						end
						healthchangedConnection = v.Character.Humanoid.HealthChanged:Connect(healthchanged)

						local function stomachchanged(newVal)
							local maxHealth = v.Character.Humanoid.MaxHealth
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.FoodBackdrop.Foodbar.Size = UDim2.new((v.Character.Stomach.Value/maxHealth),0,1,0)
						end
						stomachchangedConnection = v.Character.Stomach.Changed:Connect(stomachchanged)
						
						local function toxicitychanged(newVal)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.Toxicity.Toxbar.Size = UDim2.new((v.Character.Toxicity.Value/v.Character.Toxicity.MaxValue),0,1,0)
						end
						toxicitychangedConnection = v.Character.Toxicity.Changed:Connect(toxicitychanged)
						
						local function tempchanged(newVal)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Hot.Size = UDim2.new((0.5+(v.Character.Temperature.Value/2)),0,1,0)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Cold.Size = UDim2.new((0.5+(v.Character.Temperature.Value/-2)),0,1,0)
						end
						tempchangedConnection = v.Character.Temperature.Changed:Connect(tempchanged)
						
						local function manachanged(newVal)
							v.Character.Head:FindFirstChild("ManaGUI").ManaBackdrop.Manabar.Size = UDim2.new(1,0,(v.Character:WaitForChild("Mana").Value/100),0)
						end
						manachangedConnection = v.Character.Mana.Changed:Connect(manachanged)
						function leaderboardColor(color, targetName)
							for x,y in pairs(game.Players.LocalPlayer.PlayerGui.LeaderboardGui.MainFrame.ScrollingFrame:GetChildren()) do
								if string.lower(y.Text) == string.lower(playerList[targetName][1]) then
									y.TextColor3 = color
									if color == Color3.fromRGB(0,0,0) then
										y.TextStrokeColor3 = Color3.fromRGB(255,255,255)
									end
								end
							end
						end
						for x,y in pairs(v.Backpack:GetChildren()) do
							if y.Name == "Observe" then
								leaderboardColor(Color3.fromRGB(150,150,255), v.Name)
							end
						end
						
						for x,y in pairs(artifactList) do
							if v.Character:WaitForChild("Artifacts"):FindFirstChild(x) then
								leaderboardColor(y, v.Name)
							end
						end
						
						if v.Character:FindFirstChild("ScroomHead") then
							if v.Character:FindFirstChild("ScroomHead"):FindFirstChild("MetalScroom") then
								leaderboardColor(artifactList["ScroomKey"], v.Name)
							end
						end 
						
						v.Character.ChildAdded:Connect(function(child)
							if child.ClassName == "Tool" then
								local maxHealth = v.Character.Humanoid.MaxHealth
								local health = v.Character.Humanoid.MaxHealth
								if guiSettings["Seer - Intent"] == true then
									v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]" .. " | " .. child.Name
								end
								
								if child.Name == "Observe" then
									leaderboardColor(Color3.fromRGB(150,150,255), v.Name)
									spawn(function()
										local f = iln.Frame:Clone()
										f.Parent = iln
										f.Label.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\nIs Observing!"
										f:TweenPosition(
											UDim2.new(1,-515,1,-100),
											Enum.EasingDirection.Out,
											Enum.EasingStyle.Sine,
											0.6,
											true
										)
										wait(0.6)
										wait(2)
										f:TweenPosition(
											UDim2.new(1,-515,1,3),
											Enum.EasingDirection.In,
											Enum.EasingStyle.Sine,
											0.6,
											true
										)
										wait(0.6)
										f:Destroy()
									end)						
								end
							end
						end)
					end
				end
			elseif condition == "Vitality Stats False" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Head") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("DisplayStats") ~= nil then
						if v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Visible == false then
							local maxHealth = v.Character.Humanoid.MaxHealth
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Visible = true
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.FoodBackdrop.Visible = true
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.Toxicity.Visible = true
							v.Character.Head:FindFirstChild("DisplayStats").HealthBackdrop.Visible = true
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.FoodBackdrop.Foodbar.Size = UDim2.new((v.Character.Stomach.Value/maxHealth),0,1,0)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.Toxicity.Toxbar.Size = UDim2.new((v.Character.Toxicity.Value/v.Character.Toxicity.MaxValue),0,1,0)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Hot.Size = UDim2.new((0.5+(v.Character.Temperature.Value/2)),0,1,0)
							v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Cold.Size = UDim2.new((0.5+(v.Character.Temperature.Value/-2)),0,1,0)
						end
					end
				end
			elseif condition == "Vitality Stats True" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Head") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("DisplayStats") ~= nil then
						v.Character.Head:FindFirstChild("DisplayStats").OtherStats.TempBackdrop.Visible = false
						v.Character.Head:FindFirstChild("DisplayStats").OtherStats.FoodBackdrop.Visible = false
						v.Character.Head:FindFirstChild("DisplayStats").OtherStats.Toxicity.Visible = false
						v.Character.Head:FindFirstChild("DisplayStats").HealthBackdrop.Visible = false
					end
				end
			elseif condition == "Player ESP False" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") ~= nil then
						local health = v.Character.Humanoid.Health
						local maxHealth = v.Character.Humanoid.MaxHealth
						local tool = v.Character:FindFirstChildWhichIsA("BackpackItem")
						
						spawn(function()
							repeat
								wait()
							until v.Character:FindFirstChild(playerList[v.Name][1]):FindFirstChild("FakeHumanoid")
							v.Character:WaitForChild(playerList[v.Name][1]):WaitForChild("FakeHumanoid").DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
						end)
						
						local currentClass = ""
						for x,y in pairs(classIdentify) do
							if (v.Backpack:FindFirstChild(y[1]) ~= nil or v.Backpack:FindFirstChild(y[2]) ~= nil) and (v.Backpack:FindFirstChild(y[3]) ~= nil and v.Backpack:FindFirstChild(y[4]) ~= nil) then
								currentClass = x
								break
							else
								currentClass = "N/A"
							end
						end
						
						if tool and guiSettings["Seer - Intent"] == true then
							v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]" .. " | " .. tool.Name
						else
							v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]"
						end					
						
						v.Character.Head:FindFirstChild("NameGui").Details.Visible = true
					end
				end
			elseif condition == "Player ESP True" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") ~= nil then
						v.Character.Head:FindFirstChild("NameGui").Details.Visible = false
						spawn(function()
							repeat
								wait()
							until v.Character:FindFirstChild(playerList[v.Name][1]):FindFirstChild("FakeHumanoid")
							v.Character:WaitForChild(playerList[v.Name][1]):WaitForChild("FakeHumanoid").DisplayDistanceType = Enum.HumanoidDisplayDistanceType.Viewer
						end)
					end
				end
			elseif condition == "Update Player ESP" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") then
						if v.Character.Head:FindFirstChild("NameGui"):FindFirstChild("Details").Visible == false then
							local health = v.Character.Humanoid.Health
							local maxHealth = v.Character.Humanoid.MaxHealth
							
							for q,r in pairs(v.Character:GetChildren()) do
								if r.ClassName == "Model" and r.Name ~= "LeftRuneArm" and r.Name ~= "RightRuneArm" and r.Name ~= "Shadow Buddy" then
									playerList[v.Name] = {r.Name, Color3.fromRGB(math.random(150,255), math.random(150,255), math.random(150,255))}
								end
							end
							
							local currentClass = ""
							for x,y in pairs(classIdentify) do
							if (v.Backpack:FindFirstChild(y[1]) ~= nil or v.Backpack:FindFirstChild(y[2]) ~= nil) and (v.Backpack:FindFirstChild(y[3]) ~= nil and v.Backpack:FindFirstChild(y[4]) ~= nil) then
								currentClass = x
								break
							else
								currentClass = "N/A"
							end
						end
							
							if guiSettings["Player ESP"] == true then
								v.Character.Head:FindFirstChild("DisplayStats").HealthBackdrop.Visible = true
								v.Character.Head:FindFirstChild("NameGui").Details.Visible = true
							end
							v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]"
						end	
					end
				end
			elseif condition == "Mana Display False" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("ManaGUI") ~= nil then
						v.Character.Head:FindFirstChild("ManaGUI").ManaBackdrop.Visible = true
						v.Character.Head:FindFirstChild("ManaGUI").ManaBackdrop.Manabar.Size = UDim2.new(1,0,(v.Character:WaitForChild("Mana").Value/100),0)
					end
				end
			elseif condition == "Mana Display True" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("ManaGUI") ~= nil then
						v.Character.Head:FindFirstChild("ManaGUI").ManaBackdrop.Visible = false
					end
				end
			elseif condition == "Seer - Intent False" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") ~= nil then
						local health = v.Character.Humanoid.Health
						local maxHealth = v.Character.Humanoid.MaxHealth
						local tool = v.Character:FindFirstChildWhichIsA("BackpackItem")
						
						local currentClass = ""
						for x,y in pairs(classIdentify) do
							if (v.Backpack:FindFirstChild(y[1]) ~= nil or v.Backpack:FindFirstChild(y[2]) ~= nil) and (v.Backpack:FindFirstChild(y[3]) ~= nil and v.Backpack:FindFirstChild(y[4]) ~= nil) then
								currentClass = x
								break
							else
								currentClass = "N/A"
							end
						end
						
						if tool then
							v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]" .. " | " .. tool.Name
						else
							v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]"
						end
					end
				end
			elseif condition == "Seer - Intent True" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("NameGui") ~= nil then
						local health = v.Character.Humanoid.Health
						local maxHealth = v.Character.Humanoid.MaxHealth
						
						local currentClass = ""
						for x,y in pairs(classIdentify) do
							if (v.Backpack:FindFirstChild(y[1]) ~= nil or v.Backpack:FindFirstChild(y[2]) ~= nil) and (v.Backpack:FindFirstChild(y[3]) ~= nil and v.Backpack:FindFirstChild(y[4]) ~= nil) then
								currentClass = x
								break
							else
								currentClass = "N/A"
							end
						end
						
						v.Character.Head:FindFirstChild("NameGui").Details.Text = v.Name .. " | " .. playerList[v.Name][1] .. "\n" .. " [" .. currentClass .. "]" .. " [" .. math.floor(health) .. "|" ..  math.floor(maxHealth) .. "]"
					end	
				end
			elseif condition == "Damage Indicator False" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("DamageGui") ~= nil then
						v.Character.Head:FindFirstChild("DamageGui").Enabled = true
					end
				end
			elseif condition == "Damage Indicator True" then
				if v.Character ~= nil then
					if v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("Head"):FindFirstChild("DamageGui") ~= nil then
						v.Character.Head:FindFirstChild("DamageGui").Enabled = false
					end
				end
				
			elseif condition == "Illusionist Notifier True" then
				if v.Character ~= nil then
					--------------------------------------------------- set leaderboard to blue color for illusionists
				end
			end
		end
	end
end
playerLoop("Pre-Load")


spawn(function()
	while true do
		wait(5)
		playerLoop("Pre-Load")
		playerLoop("Update Player ESP")
	end
end)

for i,v in pairs(lgui:GetChildren()) do
	v.MouseButton1Click:Connect(function()
		if humanoid ~= nil and humanoid.Health > 0 then
			----- Clear Ambient -----
			if v.Name == "Clear Ambient" then
				if guiSettings["Clear Ambient"] == false then
					game.Lighting.Ambient = ambientColor
					spawn(function()
						wait(1)
						while guiSettings["Clear Ambient"] == true do
							wait(1)
							game.Lighting.Ambient = ambientColor
							game.Players.LocalPlayer.CameraMaxZoomDistance = 50000000
							game.Players.LocalPlayer.CameraMinZoomDistance = 0
							game.Lighting.FogEnd = 1000000
						end
					end)
				else
					game.Lighting.Ambient = Color3.fromRGB(20,20,20)
					spawn(function()
						wait(1)
						while guiSettings["Clear Ambient"] == false do
							wait(1)
							game.Lighting.Ambient = Color3.fromRGB(20,20,20)
							game.Players.LocalPlayer.CameraMaxZoomDistance = 50
							game.Players.LocalPlayer.CameraMinZoomDistance = 0
							game.Lighting.FogEnd = 1000000
						end
					end)
				end
				
			----- Chat / Illus -----
			elseif v.Name == "Chat / illusionist" then
				if guiSettings["Chat / illusionist"] == false then
					cl.Enabled = true
					iln.Enabled = true
					
				elseif guiSettings["Chat / illusionist"] == true then
					cl.Enabled = false
					iln.Enabled = false
					
				end
				
			----- Player Spectate -----
			elseif v.Name == "Damage Indicator" then
				if guiSettings["Damage Indicator"] == false then
					playerLoop("Damage Indicator False")
				elseif guiSettings["Damage Indicator"] == true then
					playerLoop("Damage Indicator False")
				end
				
			----- Cooldowns Indicator -----
			elseif v.Name == "Cooldowns Indicator" then
				local backpack = game.Players.LocalPlayer.PlayerGui:WaitForChild("BackpackGui"):WaitForChild("MainFrame")
				local storagepack = game.Players.LocalPlayer.PlayerGui:WaitForChild("BackpackGui"):WaitForChild("BackpackFrame"):WaitForChild("ScrollingFrame")
				local currentSpell = ""
				local buttondownConnection
				local buttondowntwoConnection
				local checkspellConnection
				if guiSettings["Cooldowns Indicator"] == false then
					for i,v in pairs(backpack:GetChildren()) do
						if v:FindFirstChild("SpellFrame") then
							v:FindFirstChild("SpellFrame").Visible = true
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(80, 80, 255)
						elseif v:FindFirstChild("MeleeFrame") then
							v:FindFirstChild("MeleeFrame").Visible = true
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(255, 80, 255)
						end
					end
					for i,v in pairs(storagepack:GetChildren()) do
						if v:FindFirstChild("SpellFrame") then
							v:FindFirstChild("SpellFrame").Visible = true
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(80, 80, 255)
						elseif v:FindFirstChild("MeleeFrame") then
							v:FindFirstChild("MeleeFrame").Visible = true
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(255, 80, 255)
						end
					end
					local function childaddedtwo(child)
						if child.ClassName == "Tool" and (cooldowns[child.Name] ~= nil or spellList[child.Name] ~= nil) and cooldownTool == "" then
							cooldownTool = child.Name
							local char = game.Players.LocalPlayer.Character
							local function createFrame(color, isSpell, isSpecial)
								if char:FindFirstChild("Head").Transparency ~= 1 or child.Name == "Agility" or child.Name == "Bane" or child.Name == "Ethereal Strike" then
									if char:FindFirstChild("SpellBlocking") == nil or isSpell then
										if char:FindFirstChild("Blocking") == nil and char:FindFirstChild("Climbing") == nil and char:FindFirstChild("Stun") == nil 
										and char:FindFirstChild("NoDash") == nil and char:FindFirstChild("NoDam") == nil and char:FindFirstChild("Head"):FindFirstChild("Bone") == nil
										and char:FindFirstChild("BeingExecuted") == nil and game.Workspace.Live:FindFirstChild(char.Name) ~= nil then
											if not isSpell and not isSpecial then
												cooldowns[child.Name][2] = true
											end
											local frame = Instance.new("Frame")
											local desiredTool
											local localCS = currentSpell
											if isSpecial then
												frame.Name = "SpecialFrame"
											end
											spawn(function()
												backpack = game.Players.LocalPlayer.PlayerGui:WaitForChild("BackpackGui"):WaitForChild("MainFrame")
												storagepack = game.Players.LocalPlayer.PlayerGui:WaitForChild("BackpackGui"):WaitForChild("BackpackFrame"):WaitForChild("ScrollingFrame")
												for q,r in pairs(backpack:GetChildren()) do
													if r.ClassName == "TextButton" then
														if not isSpell then
															if r.Text == child.Name then
																desiredTool = r
																break
															end
														else
															if r.Text == localCS then
																desiredTool = r
																break
															end
														end
													end
												end
												if desiredTool == nil then
													for q,r in pairs(storagepack:GetChildren()) do
														if r.ClassName == "TextButton" then
															if not isSpell then
																if r.Text == child.Name then
																	desiredTool = r
																	break
																end
															else
																if r.Text == localCS then
																	desiredTool = r
																	break
																end
															end
														end
													end
												end
												if desiredTool ~= nil or isSpell then
													frame.BorderSizePixel = 0
													if isSpell then
														frame.Name = "SpellFrame"
													else
														frame.Name = "MeleeFrame"
													end
													frame.AnchorPoint = Vector2.new(0,1)
													frame.Position = UDim2.new(0,0,1,0)
													frame.Size = UDim2.new(1,0,1,0)
													frame.BackgroundTransparency = 0.7
													frame.ZIndex = 1
													frame.BackgroundColor3 = color
													frame.Parent = desiredTool
													for i = 1,100 do
														frame.Size = UDim2.new(1,0,frame.Size.Y.Scale-0.01,0)
														if color == Color3.fromRGB(255, 80, 80) then
															if child.Name == "Bane" and game.Players.LocalPlayer.Backpack:FindFirstChild("UpgradedBane") then
																wait((cooldowns[child.Name][1]+5)/100)
															elseif child.Name == "Bane" and game.Players.LocalPlayer.Backpack:FindFirstChild("UpgradedBane") == nil then
																wait((cooldowns[child.Name][1]-5)/100)
															else
																wait((cooldowns[child.Name][1])/100)
															end 
														elseif color == Color3.fromRGB(80, 80, 255) then
															if spellList[localCS][2] ~= nil then
																wait(spellList[localCS][2]/100)
															end
														elseif color == Color3.fromRGB(80, 255, 80) then
															wait(specialSpells[localCS][1]/100)
														end
														if frame.Parent:FindFirstChild("Overlay").ImageColor3 ~= color and not isSpecial and guiSettings["Cooldowns Indicator"] == true then
															frame.Parent:FindFirstChild("Overlay").ImageColor3 = color
														end
													end
													if frame.Parent:FindFirstChild("SpecialFrame") == nil then
														frame.Parent:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(245, 197, 130)
													else
														frame.Parent:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(80, 255, 80)
													end
													if frame.Name == "SpecialFrame" then
														frame.Parent:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(245, 197, 130)
													end
													frame:Destroy()
												end
											end)
											spawn(function()
												if color == Color3.fromRGB(255, 80, 80) then
													if child.Name == "Bane" and game.Players.LocalPlayer.Backpack:FindFirstChild("UpgradedBane") then
														wait(cooldowns[child.Name][1]+5)
													elseif child.Name == "Bane" and game.Players.LocalPlayer.Backpack:FindFirstChild("UpgradedBane") == nil then
														wait(cooldowns[child.Name][1]-5)
													else
														wait((cooldowns[child.Name][1]))
													end
													cooldowns[child.Name][2] = false
												elseif color == Color3.fromRGB(80, 80, 255) then
													if spellList[localCS][2] ~= nil then
														wait(spellList[localCS][2])
													end
													for x,y in pairs(spellList) do
														y[3] = false
													end
												elseif color == Color3.fromRGB(80,255,80) then
													wait(specialSpells[localCS][1])
													specialSpells[localCS][2] = false
												end
											end)
										end
									end
								end
							end
							
							local function buttondown()
								if guiSettings["Cooldowns Indicator"] == true then
									if cooldowns[child.Name][2] == false then
										createFrame(Color3.fromRGB(255, 80, 80), false, false)
									end
								end
							end
							buttondownConnection = game.Players.LocalPlayer:GetMouse().Button1Down:Connect(buttondown)
							
							
							local function checkspell()
								if guiSettings["Cooldowns Indicator"] == true then
									if child:FindFirstChild("Spell") or child:FindFirstChild("GodSpell") or child:FindFirstChild("SkillSpell") then
										currentSpell = child.Name
									end
								end
							end
							checkspellConnection = game.Players.LocalPlayer:GetMouse().Button2Down:Connect(checkspell)
							
							local function buttondowntwo(animationTrack)
								if guiSettings["Cooldowns Indicator"] == true then
									if animationTrack.Animation.AnimationId == "rbxassetid://2960432568" or animationTrack.Animation.AnimationId == "rbxassetid://2818022247" then
										if spellList[currentSpell][3] ~= nil then
											spellList[currentSpell][3] = true
											for x,y in pairs(spellList) do
												y[3] = true
											end
											spawn(function()
												if specialSpells[currentSpell] ~= nil then
													specialSpells[currentSpell][2] = true
													createFrame(Color3.fromRGB(80, 255, 80), true, true)
												end
											end)
											createFrame(Color3.fromRGB(80, 80, 255), true, false)
										end
									end
								end
							end
							buttondowntwoConnection = game.Players.LocalPlayer.Character.Humanoid.AnimationPlayed:Connect(buttondowntwo)
						end
					end
					childaddedtwoConnection = game.Players.LocalPlayer.Character.ChildAdded:Connect(childaddedtwo)

					local function childremovedtwo(child)
						if child.Name == cooldownTool then
							cooldownTool = ""
							buttondownConnection:Disconnect()
							buttondowntwoConnection:Disconnect()
							checkspellConnection:Disconnect()
						end
					end	
					childremovedtwoConnection = game.Players.LocalPlayer.Character.ChildRemoved:Connect(childremovedtwo)
					
					game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
						if guiSettings["Cooldowns Indicator"] == true then
							childaddedtwoConnection = game.Players.LocalPlayer.Character.ChildAdded:Connect(childaddedtwo)
							childremovedtwoConnection = game.Players.LocalPlayer.Character.ChildRemoved:Connect(childremovedtwo)
							for x,y in pairs(cooldowns) do
								y[2] = false
							end
							currentSpell = ""
						end
					end)
				elseif guiSettings["Cooldowns Indicator"] == true then
					cooldownTool = ""
					for i,v in pairs(backpack:GetChildren()) do
						if v:FindFirstChild("SpellFrame") then
							v:FindFirstChild("SpellFrame").Visible = false
						elseif v:FindFirstChild("MeleeFrame") then
							v:FindFirstChild("MeleeFrame").Visible = false
						end
						if v:FindFirstChild("Overlay") then
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(245, 197, 130)
						end
					end
					for i,v in pairs(storagepack:GetChildren()) do
						if v:FindFirstChild("SpellFrame") then
							v:FindFirstChild("SpellFrame").Visible = false
						elseif v:FindFirstChild("MeleeFrame") then
							v:FindFirstChild("MeleeFrame").Visible = false
						end
						if v:FindFirstChild("Overlay") then
							v:FindFirstChild("Overlay").ImageColor3 = Color3.fromRGB(245, 197, 130)
						end
					end
				end
			elseif v.Name == "GUI Settings" then
				if guiSettings["GUI Settings"] == false then
					midp.Transparency = 0
					mgui.Parent.Enabled = true
				elseif guiSettings["GUI Settings"] == true then
					midp.Transparency = 1
					mgui.Parent.Enabled = false
				end
			end
			changeSettings(v)
		end
	end)
end


for i,v in pairs(rgui:GetChildren()) do
	v.MouseButton1Click:Connect(function()
		if humanoid ~= nil and humanoid.Health > 0 then
			----- Vitality Stats -----
			if v.Name == "Vitality Stats" then
				local hpDisplay = game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("Container"):WaitForChild("Health"):WaitForChild("Slider"):WaitForChild("HealthDisplay")
				local manaDisplay = game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana"):WaitForChild("ManaDisplay")
				if guiSettings["Vitality Stats"] == false then
					hpDisplay.Visible = true
					manaDisplay.Visible = true
					playerLoop("Vitality Stats False")
				else
					hpDisplay.Visible = false
					manaDisplay.Visible = false
					playerLoop("Vitality Stats True")
				end
				
			----- Player ESP -----
			elseif v.Name == "Player ESP" then
				if guiSettings["Player ESP"] == false then
					playerLoop("Player ESP False")
				else
					playerLoop("Player ESP True")
				end
				
			----- Mana Display -----
			elseif v.Name == "Mana Display" then
				if guiSettings["Mana Display"] == false then
					playerLoop("Mana Display False")
				else
					playerLoop("Mana Display True")
				end
				
			----- Seer - Intent -----
			elseif v.Name == "Seer - Intent" then
				if guiSettings["Seer - Intent"] == false then
					playerLoop("Seer - Intent False")
				else
					playerLoop("Seer - Intent True")
				end
				
			----- Backpack Display -----
			elseif v.Name == "Backpack Display" then
				if guiSettings["Backpack Display"] == false then
					bg.Enabled = true
				else
					bg.Enabled = false
					if guiSettings["Player Spectate"] == false and guiSettings["Backpack Display"] == true then
					end
				end
				
			----- Spell Assist -----
			elseif v.Name == "Spell Assist" then
				if game.Players.LocalPlayer.Character ~= nil then
					if guiSettings["Spell Assist"] == false then
						local tool = game.Players.LocalPlayer.Character:FindFirstChildWhichIsA("BackpackItem")							
						if tool ~= nil then
							if spellList[tool.Name] ~= nil then
								if spellList[tool.Name][1] then
									currentSpell = tool.Name
								end
							end
						end
						local function childadded(child)
							if child.ClassName == "Tool" and spellList[child.Name][1] then
								currentSpell = child.Name
								local spellFolder = game.Players.LocalPlayer.PlayerGui.StatGui.LeftContainer.Mana:WaitForChild("SpellAssistFolder")
								game.Players.LocalPlayer.PlayerGui.StatGui.LeftContainer.Mana.Overlay.ZIndex = 6
								local Frame = Instance.new("Frame")
								Frame.Parent = spellFolder
								Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
								Frame.BorderSizePixel = 0
								Frame.BackgroundTransparency = 0.5
								Frame.AnchorPoint = Vector2.new(0,1)
								Frame.Name = currentSpell
								Frame.Position = UDim2.new(0, 0, 1-math.abs(spellList[currentSpell][1][1]/100), 0)
								Frame.Size = UDim2.new(1, 0, math.abs(spellList[currentSpell][1][1]-spellList[currentSpell][1][2])/100, 0)
								Frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
								Frame.ZIndex = 4
								if spellList[child.Name][1][3] ~= nil then
									local Frame = Instance.new("Frame")
									Frame.Parent = spellFolder
									Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
									Frame.BorderSizePixel = 0
									Frame.BackgroundTransparency = 0.5
									Frame.AnchorPoint = Vector2.new(0,1)
									Frame.Name = currentSpell
									Frame.Position = UDim2.new(0, 0, 1-math.abs(spellList[currentSpell][1][3]/100), 0)
									Frame.Size = UDim2.new(1, 0, math.abs(spellList[currentSpell][1][3]-spellList[currentSpell][1][4])/100, 0)
									Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
									Frame.ZIndex = 4
								end	
							end
						end
						if childaddedConnection == nil then
							childaddedConnection = game.Players.LocalPlayer.Character.ChildAdded:Connect(childadded)
						end
						local function childremoved(child)
							if child.ClassName == "Tool" and child.Name == currentSpell and guiSettings["Spell Assist"] == true then
								local spellFolder = game.Players.LocalPlayer.PlayerGui.StatGui.LeftContainer.Mana:WaitForChild("SpellAssistFolder")	
								for x,y in pairs(spellFolder:GetChildren()) do
									if y.Name == currentSpell then
										y:Destroy()
									end
								end
								currentSpell = ""
							end
						end
						if childremovedConnection == nil then
							childremovedConnection = game.Players.LocalPlayer.Character.ChildRemoved:Connect(childremoved)
						end
						if game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui") then
							if game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana") then
								for x,y in pairs(game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana"):WaitForChild("SpellAssistFolder"):GetChildren()) do
									y.Visible = true
								end
							end
						end
						game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
							if guiSettings["Spell Assist"] == true then
								childaddedConnection = game.Players.LocalPlayer.Character.ChildAdded:Connect(childadded)
								childremovedConnection = game.Players.LocalPlayer.Character.ChildRemoved:Connect(childremoved)
							end
						end)
						
					elseif guiSettings["Spell Assist"] == true then
						local spellFolder = game.Players.LocalPlayer.PlayerGui.StatGui.LeftContainer.Mana:WaitForChild("SpellAssistFolder")	
						if childaddedConnection ~= nil then
							childaddedConnection:Disconnect()
							childaddedConnection = nil
						end
						if childremovedConnection ~= nil then
							childremovedConnection:Disconnect()
							childremovedConnection = nil
						end
						for x,y in pairs(spellFolder:GetChildren()) do
							if y.Name == currentSpell then
								y:Destroy()
							end
						end
						currentSpell = ""
						if game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui") then
							if game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana") then
								for x,y in pairs(game.Players.LocalPlayer.PlayerGui:WaitForChild("StatGui"):WaitForChild("LeftContainer"):WaitForChild("Mana"):WaitForChild("SpellAssistFolder"):GetChildren()) do
									y.Visible = false
								end
							end
						end
					end
				end
			end
			changeSettings(v)
		end
	end)
end

----- GUI Settings -----
for i,v in pairs(mgui:GetChildren()) do
	if v.ClassName == "TextButton" then
		v.MouseButton1Click:Connect(function()
			if humanoid ~= nil and humanoid.Health > 0 then
				if v.Name == "AdjustManaColor" then
					if game.Players.LocalPlayer.PlayerGui:FindFirstChild("StatGui"):FindFirstChild("LeftContainer"):FindFirstChild("Mana"):FindFirstChild("Slider") ~= nil then
						local s = game.Players.LocalPlayer.PlayerGui:FindFirstChild("StatGui"):FindFirstChild("LeftContainer"):FindFirstChild("Mana"):FindFirstChild("Slider")
						local oL = game.Players.LocalPlayer.PlayerGui:FindFirstChild("StatGui"):FindFirstChild("LeftContainer"):FindFirstChild("Mana"):FindFirstChild("Overlay")
						if type(math.floor(tonumber(manaAdjust.Red.Text))) == "number" and type(math.floor(tonumber(manaAdjust.Green.Text))) == "number" and type(math.floor(tonumber(manaAdjust.Blue.Text))) == "number" then
							manaColor = Color3.fromRGB(math.floor(tonumber(manaAdjust.Red.Text)), math.floor(tonumber(manaAdjust.Green.Text)), math.floor(tonumber(manaAdjust.Blue.Text)))
							oL.BackgroundColor3 = manaColor
							s.BackgroundColor3 = manaColor
						end
					end
				elseif v.Name == "AdjustUIColor" then
					if type(math.floor(tonumber(colorAdjust.Red.Text))) == "number" and type(math.floor(tonumber(colorAdjust.Green.Text))) == "number" and type(math.floor(tonumber(colorAdjust.Blue.Text))) == "number" then
						UIColor = Color3.fromRGB(math.floor(tonumber(colorAdjust.Red.Text)), math.floor(tonumber(colorAdjust.Green.Text)), math.floor(tonumber(colorAdjust.Blue.Text)))
						for q,r in pairs(lgui:GetChildren()) do
							if r.BackgroundColor3 ~= Color3.fromRGB(126, 255, 126) then
								r.BackgroundColor3 = UIColor
							end
						end
						for q,r in pairs(rgui:GetChildren()) do
							if r.BackgroundColor3 ~= Color3.fromRGB(126, 255, 126) then
								r.BackgroundColor3 = UIColor
							end
						end
						for q,r in pairs(mgui:GetChildren()) do
							if r.ClassName == "TextButton" then
								if r.BackgroundColor3 ~= Color3.fromRGB(126, 255, 126) then
									r.BackgroundColor3 = UIColor
								end
							end
						end
					end
				elseif v.Name == "AdjustAmbient" then
					if type(math.floor(tonumber(ambientAdjust.Red.Text))) == "number" and type(math.floor(tonumber(ambientAdjust.Green.Text))) == "number" and type(math.floor(tonumber(ambientAdjust.Blue.Text))) == "number" then
						ambientColor = Color3.fromRGB(math.floor(tonumber(ambientAdjust.Red.Text)), math.floor(tonumber(ambientAdjust.Green.Text)), math.floor(tonumber(ambientAdjust.Blue.Text)))
					end
				end
			end
		end)
	end
end

----- Leaderboard Interaction -----
local selectedLabel
local selectedName
local previousSelected = ""	
local dragging
local dragInput
local dragStart
local startPos
local dragging2
local dragInput2
local dragStart2
local startPos2
local draggableList = {
	bg.ScrollingFrame,
	cl.ScrollingFrame
}

----- GUI Draggability -----
for i,v in pairs(draggableList) do
	local function update(input)
		
		if v == bg.ScrollingFrame then
			local delta = input.Position - dragStart
			v.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
		elseif v == cl.ScrollingFrame then
			local delta = input.Position - dragStart2
			v.Position = UDim2.new(startPos2.X.Scale, startPos2.X.Offset + delta.X, startPos2.Y.Scale, startPos2.Y.Offset + delta.Y)
		end
	end
	v.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			if v == bg.ScrollingFrame then
				dragging = true
				dragStart = input.Position
				startPos = v.Position
			elseif v == cl.ScrollingFrame then
				dragging2 = true
				dragStart2 = input.Position
				startPos2 = v.Position
			end
			input.Changed:Connect(function()
				if input.UserInputState == Enum.UserInputState.End then
					if v == bg.ScrollingFrame then
						dragging = false
					elseif v == cl.ScrollingFrame then
						dragging2 = false
					end
				end
			end)
		end
	end)
	v.InputChanged:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
			if v == bg.ScrollingFrame then
				dragInput = input
			elseif v == cl.ScrollingFrame then
				dragInput2 = input
			end
		end
	end)
		
	UIS.InputChanged:Connect(function(input)
		if v == bg.ScrollingFrame then
			if input == dragInput and dragging then
				update(input)
			end
		elseif v == cl.ScrollingFrame then
			if input == dragInput2 and dragging2 then
				update(input)
			end
		end
	end)
end

----- User Input -----
local held = false
UIS.InputBegan:connect(function(Input, proccessed)
	local KeyCode = Input.KeyCode
	local MouseInput = Input.UserInputType
	for i,v in pairs(game.Players.LocalPlayer.PlayerGui.LeaderboardGui.MainFrame.ScrollingFrame:GetChildren()) do
		if v ~= nil then
			v.MouseEnter:Connect(function()
				selectedLabel = v
			end)
		end
	end
	if KeyCode == Enum.KeyCode.F1 and not proccessed then
		lgui.Parent.Enabled = not lgui.Parent.Enabled
		rgui.Parent.Enabled = not rgui.Parent.Enabled
		if rgui.Parent.Enabled == false then
			resetPanel()
			panelSetup(true)
			mgui.Parent.Enabled = false
		elseif rgui.Parent.Enabled == true and guiSettings["GUI Settings"] == true then
			mgui.Parent.Enabled = true
		end
		if rgui.Parent.Enabled == true then
			resetPanel()
			panelSetup(false)
		end
		for i,v in pairs(cp:GetChildren()) do
			if v.Transparency == 1 and v.Name ~= "MidPart" then
				v.Transparency = 0
				if v.Name == "MidPanel" and guiSettings["GUI Settings"] == true then
					v.Transparency = 0
				elseif v.Name == "MidPanel" and guiSettings["GUI Settings"] == false then
					v.Transparency = 1
				end
			elseif v.Transparency == 0 and v.Name ~= "MidPart" then
				v.Transparency = 1
				if v.Name == "MidPanel" and guiSettings["GUI Settings"] == true then
					v.Transparency = 1
				elseif v.Name == "MidPanel" and guiSettings["GUI Settings"] == false then
					v.Transparency = 0
				end
			end
		end
	elseif KeyCode == Enum.KeyCode.F6 and not proccessed then
		game:GetService('Players').LocalPlayer:Kick("Emergency Log")
	elseif MouseInput == Enum.UserInputType.MouseButton1 then
		if selectedLabel ~= nil and guiSettings["Backpack Display"] == true then
			local selectedName = ""
			if game.Workspace:FindFirstChild("BigHossFightSpawns") then
				selectedName = selectedLabel.Text
			else
				selectedName = string.sub(selectedLabel.Text,1,1) .. string.sub(selectedLabel.Text,5,#selectedLabel.Text)
			end
			bg.ScrollingFrame.CanvasPosition = Vector2.new(0,0)
			if game.Players:FindFirstChild(selectedName) ~= nil and game.Players:FindFirstChild(selectedName).Character ~= nil then
				bg.ScrollingFrame.Title.Text = ""
				if #bg.ScrollingFrame:GetChildren() > 2 then
					for i,v in pairs(bg.ScrollingFrame:GetChildren()) do
						if v.Name == "LabelClone" then
							v:Destroy()
						end
					end
				end
				bg.ScrollingFrame.Title.Text = selectedName .. " 's Inventory:"
				local inventoryDict = {}
				local inventoryColor = {}
				for i,v in pairs(game.Players:FindFirstChild(selectedName).Backpack:GetChildren()) do
					if v.ClassName == "Tool" then
						if inventoryDict[v.Name] ~= nil then
							inventoryDict[v.Name] = inventoryDict[v.Name] + 1
						else
							inventoryDict[v.Name] = 1
						end
						if v:FindFirstChild("PrimaryWeapon") or v:FindFirstChild("RequiresWeapon") then
							inventoryColor[v.Name] = Color3.fromRGB(255, 222, 34) -- Weapons / Attacks
						elseif v:FindFirstChild("Spell") or v:FindFirstChild("GodSpell") or v:FindFirstChild("SkillSpell") then
							inventoryColor[v.Name] = Color3.fromRGB(61, 223, 255) -- Weapons / Attacks
						elseif v:FindFirstChild("isPotion") then
							inventoryColor[v.Name] = Color3.fromRGB(255, 130, 130) -- Weapons / Attacks
						elseif v:FindFirstChild("isIngredient") then
							inventoryColor[v.Name] = Color3.fromRGB(130, 255, 130) -- Weapons / Attacks
						end
					end
				end
				for i,v in pairs(inventoryDict) do
					local labelClone = bg.ScrollingFrame.Title:Clone()
					if v == 1 then
						labelClone.Text = i
					else
						labelClone.Text = i .. " x" .. v
					end
					labelClone.Name = "LabelClone"
					labelClone.Parent = bg.ScrollingFrame
					if inventoryColor[i] ~= nil then
						labelClone.TextColor3 = inventoryColor[i]
					end
				end
			end
		end
		if selectedLabel ~= nil and guiSettings["Player Spectate"] == true then
			if previousSelected ~= selectedLabel.Text then
				previousSelected = selectedLabel.Text
				local selectedName = ""
				if game.Workspace:FindFirstChild("BigHossFightSpawns") then
					selectedName = selectedLabel.Text
				else
					selectedName = string.sub(selectedLabel.Text,1,1) .. string.sub(selectedLabel.Text,5,#selectedLabel.Text)
				end
				if game.Players:FindFirstChild(selectedName) ~= nil and game.Players:FindFirstChild(selectedName).Character ~= nil and game.Players:FindFirstChild(selectedName).Character.Humanoid ~= nil then
					game.Workspace.CurrentCamera.CameraSubject = game.Players:FindFirstChild(selectedName).Character.Humanoid
				end
			else
				if game.Players.LocalPlayer.Character then
					if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
						game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
						previousSelected = ""
					end
				end
			end
		end
	end
end)
lgui.Parent.Enabled = false
lgui.Parent.Enabled = true
rgui.Parent.Enabled = false
rgui.Parent.Enabled = true
mgui.Parent.Enabled = false
mgui.Parent.Enabled = true
mgui.Parent.Enabled = false
