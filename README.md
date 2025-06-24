KurdishHub.lua

-- بارکردنی OrionLib
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/jensonhirst/Orion/main/source"))()

-- دروستکردنی پەنجەرەی سەرەکی
local Window = OrionLib:MakeWindow({
	Name = "Kurdish Hub 👀",
	HidePremium = false,
	SaveConfig = true,
	ConfigFolder = "KurdishHub_Config",
	IntroEnabled = true,
	IntroText = "بەخێربێیت بۆ Kurdish Hub 👋",
	IntroIcon = "rbxassetid://4483345998",
	Icon = "rbxassetid://4483345998"
})

-------------------------------------------------------
-- 🌐 Tab: زانیاری سەرەکی
-------------------------------------------------------
local InfoTab = Window:MakeTab({
	Name = "🌐 زانیاری",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

InfoTab:AddSection({
	Name = "هەر ڕاهێنانێک پێشنیار هەیە، دەتوانیت ناردن!"
})

InfoTab:AddButton({
	Name = "👤 TikTok - hama_x72",
	Callback = function()
		setclipboard("https://tiktok.com/@hama_x72")
		OrionLib:MakeNotification({
			Name = "کۆپی کرا",
			Content = "لینکی TikTok کۆپی کرا",
			Image = "rbxassetid://4483345998",
			Time = 5
		})
	end
})

InfoTab:AddButton({
	Name = "📢 Telegram - Hama_x72",
	Callback = function()
		setclipboard("https://t.me/Hama_x72")
		OrionLib:MakeNotification({
			Name = "کۆپی کرا",
			Content = "لینکی Telegram کۆپی کرا",
			Image = "rbxassetid://4483345998",
			Time = 5
		})
	end
})

-------------------------------------------------------
-- 💣 Tab: v1 هاک
-------------------------------------------------------
local HackTab1 = Window:MakeTab({
	Name = "💣 هاک v1",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-------------------------------------------------------
-- 🏃 Tab: فڕین / خێرایی
-------------------------------------------------------
local SpeedTab = Window:MakeTab({
	Name = "🏃 فڕین / خێرایی",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-- ⚙️ گۆڕینی WalkSpeed
SpeedTab:AddSlider({
	Name = "🏃 خێرایی (WalkSpeed)",
	Min = 16,
	Max = 200,
	Default = 16,
	Color = Color3.fromRGB(0, 200, 255),
	Increment = 1,
	ValueName = "Speed",
	Callback = function(value)
		game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
	end
})

-- 🦘 گۆڕینی JumpPower
SpeedTab:AddSlider({
	Name = "🦘 بەرزپەڕین (JumpPower)",
	Min = 50,
	Max = 300,
	Default = 50,
	Color = Color3.fromRGB(255, 120, 0),
	Increment = 5,
	ValueName = "Jump",
	Callback = function(value)
		game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
	end
})

-- 🪶 Fly Mode
local flying = false
local UIS = game:GetService("UserInputService")

local function fly()
	local plr = game.Players.LocalPlayer
	local char = plr.Character or plr.CharacterAdded:Wait()
	local humanoidRoot = char:WaitForChild("HumanoidRootPart")

	local bodyGyro = Instance.new("BodyGyro", humanoidRoot)
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.cframe = humanoidRoot.CFrame

	local bodyVelocity = Instance.new("BodyVelocity", humanoidRoot)
	bodyVelocity.velocity = Vector3.new(0, 0, 0)
	bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)

	flying = true
	notify("✈️ فڕین چالاک بوو", "بۆ وەستاندن دوگمەی دوبارە بکە")

	while flying do
		game:GetService("RunService").Heartbeat:Wait()
		local moveDir = plr:GetMouse().Hit.Position - humanoidRoot.Position
		bodyGyro.cframe = CFrame.new(humanoidRoot.Position, humanoidRoot.Position + moveDir)
		bodyVelocity.velocity = plr:GetMouse().Hit.LookVector * 50
	end

	bodyGyro:Destroy()
	bodyVelocity:Destroy()
end

SpeedTab:AddButton({
	Name = "✈️ فڕین (Fly Mode)",
	Callback = function()
		if flying then
			flying = false
			notify("🛬 فڕین داخرا", "فڕین ڕاگیرا")
		else
			fly()
		end
	end
})

local noclipEnabled = false
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

HackTab1:AddButton({
	Name = "🚪 Noclip (ڕەوانەبوون)",
	Callback = function()
		noclipEnabled = not noclipEnabled
		if noclipEnabled then
			notify("🚪 Noclip چالاک بوو", "تۆ دەتوانی لەناو هەموو شتێک ڕەوانە بیت")
		else
			notify("❌ Noclip داخرا", "نوکلیپ وەستاندرا")
		end
	end
})

-- نوسینی گەڕان بۆ پارسکردنی نوکلیپ
RunService.Stepped:Connect(function()
	if noclipEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
		for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide == true then
				part.CanCollide = false
			end
		end
	end
end)

-------------------------------------------------------
-- 🎯 Tab: هاکی خەڵک
-------------------------------------------------------
local PeopleHackTab = Window:MakeTab({
	Name = "🎯 هاکی خەڵک",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-- Spectate بۆ بینینی یاریزانە دیاری کراوەکان
local selectedPlayer = nil

PeopleHackTab:AddDropdown({
	Name = "👥 هەڵبژاردنی یاریزان",
	Default = nil,
	Options = {},
	Callback = function(value)
		selectedPlayer = value
		OrionLib:MakeNotification({
			Name = "یاریزان دیاری کرا",
			Content = "تۆ یاریزانی '" .. value .. "' دیاری کرد",
			Image = "rbxassetid://4483345998",
			Time = 3
		})
	end
})

task.spawn(function()
	while true do
		wait(2)
		local playerNames = {}
		for _, player in pairs(game.Players:GetPlayers()) do
			if player.Name ~= game.Players.LocalPlayer.Name then
				table.insert(playerNames, player.Name)
			end
		end
		PeopleHackTab:UpdateDropdown("👥 هەڵبژاردنی یاریزان", playerNames)
	end
end)

PeopleHackTab:AddButton({
	Name = "🎥 Spectate (بینینی یاریزان)",
	Callback = function()
		if selectedPlayer and game.Players:FindFirstChild(selectedPlayer) then
			game.Workspace.CurrentCamera.CameraSubject = game.Players[selectedPlayer].Character:FindFirstChild("Humanoid")
		else
			OrionLib:MakeNotification({
				Name = "هەڵە",
				Content = "تکایە یاریزانی دروست دیاری بکە",
				Image = "rbxassetid://4483345998",
				Time = 3
			})
		end
	end
})

-------------------------------------------------------
-- 🎮 Tab: هاکی یاری
-------------------------------------------------------
local GameHackTab = Window:MakeTab({
	Name = "🎮 هاکی یاری",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-------------------------------------------------------
-- 💣 Tab: هاک v2
-------------------------------------------------------
local HackTab2 = Window:MakeTab({
	Name = "💣 هاک v2",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-- 🛡️ Tab: دژە هاک
local AntiHackTab = Window:MakeTab({
	Name = "🛡️ دژە هاک",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

-- نوتیفیکەیشنی یەکسان
local function notify(title, msg)
	OrionLib:MakeNotification({
		Name = title,
		Content = msg,
		Image = "rbxassetid://4483345998",
		Time = 4
	})
end

-- Anti Kick
local function antiKick()
	local mt = getrawmetatable(game)
	local old = mt.__namecall
	setreadonly(mt, false)
	mt.__namecall = newcclosure(function(self, ...)
		local method = getnamecallmethod()
		if method == "Kick" then
			return warn("[Anti Kick] هەوڵی کێک ڕەتکرایەوە")
		end
		return old(self, ...)
	end)
	setreadonly(mt, true)
	notify("✅ چالاک کرا", "Anti Kick دامەزرابوو")
end

-- Anti Ban
local function antiBan()
	game:GetService("Players").LocalPlayer.Kick = function() return nil end
	notify("✅ چالاک کرا", "Anti Ban دامەزرابوو")
end

-- Anti Teleport
local function antiTeleport()
	local plr = game.Players.LocalPlayer
	local char = plr.Character or plr.CharacterAdded:Wait()
	local root = char:WaitForChild("HumanoidRootPart")
	local lastPos = root.Position

	game:GetService("RunService").Heartbeat:Connect(function()
		if (root.Position - lastPos).Magnitude > 50 then
			root.CFrame = CFrame.new(lastPos)
			warn("[Anti Teleport] ڕەتکردنەوەی تێله‌پۆرت")
		else
			lastPos = root.Position
		end
	end)

	notify("✅ چالاک کرا", "Anti Teleport دامەزرابوو")
end

-- Anti Crash
local function antiCrash()
	setfpscap(60)
	game:GetService("LogService").MessageOut:Connect(function(msg)
		if string.find(msg:lower(), "crash") then
			warn("[Anti Crash] کراشی ڕەوانەکرا")
		end
	end)
	notify("✅ چالاک کرا", "Anti Crash دامەزرابوو")
end

-- Anti Fling
local function antiFling()
	game:GetService("RunService").Heartbeat:Connect(function()
		for _, v in pairs(game.Players:GetPlayers()) do
			if v ~= game.Players.LocalPlayer then
				local char = v.Character
				if char then
					local root = char:FindFirstChild("HumanoidRootPart")
					if root then
						root.Velocity = Vector3.zero
						root.RotVelocity = Vector3.zero
					end
				end
			end
		end
	end)
	notify("✅ چالاک کرا", "Anti Fling دامەزرابوو")
end

-- Anti Log
local function antiLog()
	for _, service in pairs(game:GetChildren()) do
		pcall(function()
			service.Changed:Connect(function() end)
		end)
	end
	notify("✅ چالاک کرا", "Anti Log دامەزرابوو")
end

-- 🔘 بەشەکانی تایبەت بە دوگمەکان
AntiHackTab:AddButton({Name = "🚫 Anti Kick", Callback = antiKick})
AntiHackTab:AddButton({Name = "🛑 Anti Ban", Callback = antiBan})
AntiHackTab:AddButton({Name = "📡 Anti Teleport", Callback = antiTeleport})
AntiHackTab:AddButton({Name = "💥 Anti Crash", Callback = antiCrash})
AntiHackTab:AddButton({Name = "🌀 Anti Fling", Callback = antiFling})
AntiHackTab:AddButton({Name = "📉 Anti Log", Callback = antiLog})

-- 🔰 دوگمەی چالاککردنی هەموو بەیەکجار
AntiHackTab:AddButton({
	Name = "🔰 چالاککردنی هەموو پاراستنەکان",
	Callback = function()
		antiKick()
		antiBan()
		antiTeleport()
		antiCrash()
		antiFling()
		antiLog()
		notify("🔰 تەواو", "هەموو پاراستنەکان چالاک کران")
	end
})


-- بارکردنی OrionLib
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/jensonhirst/Orion/main/source"))()
-- (و هتد...)
