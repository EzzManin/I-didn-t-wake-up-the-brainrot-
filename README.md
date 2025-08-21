-- LocalScript (StarterPlayerScripts)
local player = game.Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")

-- GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "EzzHub"
gui.ResetOnSpawn = false
gui.Parent = CoreGui

-- Botão abrir/fechar
local openClose = Instance.new("ImageButton", gui)
openClose.Size = UDim2.new(0, 50, 0, 50)
openClose.Position = UDim2.new(0, 100, 0, 100)
openClose.Image = "rbxassetid://133393242441626"
openClose.Active = true
openClose.Draggable = true

-- Frame principal
local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 250, 0, 250)
main.Position = UDim2.new(0.5, -125, 0.5, -125)
main.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
main.Visible = true
main.Active = true
main.Draggable = true

-- Título
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
title.Text = "EzzHub"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20

-- Variável speed
local speedActive = false
local function getHumanoid()
	local char = player.Character or player.CharacterAdded:Wait()
	return char:WaitForChild("Humanoid")
end

-- Toggle Speed
local speedToggle = Instance.new("TextButton", main)
speedToggle.Size = UDim2.new(0, 200, 0, 40)
speedToggle.Position = UDim2.new(0, 25, 0, 50)
speedToggle.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
speedToggle.Text = "Speed [OFF]"
speedToggle.TextColor3 = Color3.new(1,1,1)
speedToggle.Font = Enum.Font.SourceSans
speedToggle.TextSize = 18

speedToggle.MouseButton1Click:Connect(function()
	speedActive = not speedActive
	speedToggle.Text = speedActive and "Speed [ON]" or "Speed [OFF]"
	speedToggle.BackgroundColor3 = speedActive and Color3.fromRGB(0,170,0) or Color3.fromRGB(70,70,70)
end)

-- Proteção contra mudanças de WalkSpeed
RunService.RenderStepped:Connect(function()
	local hum = getHumanoid()
	if speedActive then
		if hum.WalkSpeed ~= 100 then
			hum.WalkSpeed = 100
		end
	else
		if hum.WalkSpeed ~= 16 then
			hum.WalkSpeed = 16
		end
	end
end)

-- Toggle Instant Interact
local instantActive = false
local interactToggle = Instance.new("TextButton", main)
interactToggle.Size = UDim2.new(0, 200, 0, 40)
interactToggle.Position = UDim2.new(0, 25, 0, 100)
interactToggle.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
interactToggle.Text = "Instant Interact [OFF]"
interactToggle.TextColor3 = Color3.new(1,1,1)
interactToggle.Font = Enum.Font.SourceSans
interactToggle.TextSize = 18

interactToggle.MouseButton1Click:Connect(function()
	instantActive = not instantActive
	interactToggle.Text = instantActive and "Instant Interact [ON]" or "Instant Interact [OFF]"
	interactToggle.BackgroundColor3 = instantActive and Color3.fromRGB(0,170,0) or Color3.fromRGB(70,70,70)
end)

-- Loop para interação instantânea
RunService.RenderStepped:Connect(function()
	if instantActive then
		for _,prompt in pairs(workspace:GetDescendants()) do
			if prompt:IsA("ProximityPrompt") and prompt.Enabled then
				prompt.HoldDuration = 0
			end
		end
	end
end)

-- Botão Teleporte
local tpBtn = Instance.new("TextButton", main)
tpBtn.Size = UDim2.new(0, 200, 0, 40)
tpBtn.Position = UDim2.new(0, 25, 0, 150)
tpBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 120)
tpBtn.Text = "Teleportar"
tpBtn.TextColor3 = Color3.new(1,1,1)
tpBtn.Font = Enum.Font.SourceSans
tpBtn.TextSize = 18

tpBtn.MouseButton1Click:Connect(function()
	local char = player.Character or player.CharacterAdded:Wait()
	local hrp = char:WaitForChild("HumanoidRootPart")
	hrp.CFrame = CFrame.new(-31, 1, -114)
end)

-- Abrir/fechar menu
openClose.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
end)
