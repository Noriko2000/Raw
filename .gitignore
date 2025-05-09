--// Services
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

--// HUD Loading (Anime style)
local starterGui = LocalPlayer:WaitForChild("PlayerGui")
local loadingGui = Instance.new("ScreenGui", starterGui)
loadingGui.Name = "LoadingHUD"

local loadingFrame = Instance.new("TextLabel", loadingGui)
loadingFrame.Size = UDim2.new(0.3, 0, 0.05, 0)
loadingFrame.Position = UDim2.new(0.35, 0, 0.45, 0)
loadingFrame.BackgroundTransparency = 0.4
loadingFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
loadingFrame.BorderSizePixel = 0
loadingFrame.Text = ">> Initializing Neural HUD System..."
loadingFrame.Font = Enum.Font.Arcade
loadingFrame.TextColor3 = Color3.fromRGB(0, 255, 200)
loadingFrame.TextScaled = true

task.delay(2, function()
	loadingGui:Destroy()
end)

--// Infinite Stamina (Mobile-friendly)
local function enableInfiniteStamina()
	if LocalPlayer:GetAttribute("StaminaConsumeMultiplier") ~= 0 then
		LocalPlayer:SetAttribute("StaminaConsumeMultiplier", 0)
	end
	LocalPlayer:GetAttributeChangedSignal("StaminaConsumeMultiplier"):Connect(function()
		if LocalPlayer:GetAttribute("StaminaConsumeMultiplier") ~= 0 then
			LocalPlayer:SetAttribute("StaminaConsumeMultiplier", 0)
		end
	end)
end
enableInfiniteStamina()

--// HUD for other players
local function createHUDForPlayer(player)
	if player == LocalPlayer then return end

	local function setup(character)
		local head = character:WaitForChild("Head", 5)
		local humanoid = character:FindFirstChildOfClass("Humanoid")
		if not head or not humanoid then return end

		local existing = head:FindFirstChild("BackpackHUD")
		if existing then existing:Destroy() end

		local gui = Instance.new("BillboardGui", head)
		gui.Name = "BackpackHUD"
		gui.Size = UDim2.new(0, 65, 0, 26)
		gui.StudsOffset = Vector3.new(0, 1.2, 0)
		gui.AlwaysOnTop = true

		local nameLabel = Instance.new("TextLabel", gui)
		nameLabel.Size = UDim2.new(1, 0, 0.4, 0)
		nameLabel.BackgroundTransparency = 1
		nameLabel.Font = Enum.Font.GothamBold
		nameLabel.TextScaled = true
		nameLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
		nameLabel.TextStrokeTransparency = 0.6
		nameLabel.Text = player.Name

		local hpBar = Instance.new("Frame", gui)
		hpBar.Size = UDim2.new(1, 0, 0.04, 0)
		hpBar.Position = UDim2.new(0, 0, 0.4, 0)
		hpBar.BackgroundTransparency = 0.5
		hpBar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

		local fill = Instance.new("Frame", hpBar)
		fill.Name = "Fill"
		fill.Size = UDim2.new(1, 0, 1, 0)
		fill.BackgroundColor3 = Color3.fromRGB(255, 60, 60)

		local slots = {}
		for i = 1, 4 do
			local slot = Instance.new("TextLabel", gui)
			slot.Size = UDim2.new(0.25, -1, 0.45, 0)
			slot.Position = UDim2.new((i - 1) * 0.25, i - 1, 0.55, 0)
			slot.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
			slot.BackgroundTransparency = 0.2
			slot.BorderSizePixel = 0
			slot.Font = Enum.Font.GothamSemibold
			slot.TextScaled = true
			slot.TextColor3 = Color3.new(1, 1, 1)
			slot.Text = ""
			table.insert(slots, slot)
		end

		RunService.RenderStepped:Connect(function()
			if humanoid and humanoid.Health > 0 then
				local ratio = humanoid.Health / humanoid.MaxHealth
				fill.Size = UDim2.new(ratio, 0, 1, 0)
			end

			local tools = {}
			for _, tool in ipairs(player.Backpack:GetChildren()) do
				if tool:IsA("Tool") then table.insert(tools, tool) end
			end
			for _, tool in ipairs(character:GetChildren()) do
				if tool:IsA("Tool") then table.insert(tools, tool) end
			end
			for i = 1, 4 do
				slots[i].Text = tools[i] and string.upper(tools[i].Name) or ""
			end
		end)
	end

	if player.Character then setup(player.Character) end
	player.CharacterAdded:Connect(setup)
end

--// Start HUD for all players
for _, p in ipairs(Players:GetPlayers()) do createHUDForPlayer(p) end
Players.PlayerAdded:Connect(createHUDForPlayer)

--// Gun check
task.spawn(function()
	while true do
		local ok, result = pcall(function()
			local gun = ReplicatedStorage:WaitForChild("Items", 5):FindFirstChild("gun")
			if gun then
				print("[HUD] Gun found:", gun.Name)
			else
				print("[HUD] Gun not found.")
			end
		end)
		if not ok then
			warn("[HUD] Error:", result)
		end
		task.wait(3)
	end
end)
