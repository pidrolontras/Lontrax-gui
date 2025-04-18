--[[  
   Admin GUI Universal para executores (Synapse, Arceus X, etc.)  
   Nome: Lontrax 🇧🇷
   Funcionalidades: Fly, Invisibility, Noclip, TP Tool, Fling  
--]]

-- Serviços
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

-- GUI Principal
local screen = Instance.new("ScreenGui")
screen.Name = "AdminGUI"
screen.Parent = CoreGui
screen.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local frame = Instance.new("Frame", screen)
frame.Size = UDim2.new(0,200,0,380)
frame.Position = UDim2.new(0,50,0,50)
frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

-- Listras verdes no fundo da GUI
for i = 0, frame.Size.Y.Offset, 20 do
	local stripe = Instance.new("Frame", frame)
	stripe.Size = UDim2.new(1, 0, 0, 2)
	stripe.Position = UDim2.new(0, 0, 0, i)
	stripe.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
	stripe.BackgroundTransparency = 0.3
end

-- Efeito sonoro personalizado
local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://2578125671"
clickSound.Volume = 1
clickSound.Parent = frame

-- Nome do Script no topo (minimiza ao clicar)
local title = Instance.new("TextButton", frame)
title.Size = UDim2.new(0, 180, 0, 30)
title.Position = UDim2.new(0, 10, 0, -35)
title.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
title.BorderSizePixel = 0
title.Text = "Lontrax 🇧🇷"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextSize = 16
title.Font = Enum.Font.SourceSansBold
title.AutoButtonColor = true

-- Botão Minimizar
local minimizeBtn = Instance.new("ImageButton")
minimizeBtn.Size = UDim2.new(0, 20, 0, 20)
minimizeBtn.Position = UDim2.new(1, -25, 0, 5)
minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Image = "rbxassetid://11636146039"
minimizeBtn.Parent = frame
minimizeBtn.ZIndex = 2

-- Botão para reabrir
local openBtn = Instance.new("ImageButton")
openBtn.Size = UDim2.new(0, 40, 0, 40)
openBtn.Position = UDim2.new(0, 10, 0, 10)
openBtn.Image = "rbxassetid://11636146039"
openBtn.BackgroundTransparency = 1
openBtn.Visible = false
openBtn.Parent = screen
openBtn.ZIndex = 2

-- Minimizar/Reabrir lógica
local minimized = false
local function minimizeGUI()
	minimized = true
	frame.Visible = false
	openBtn.Visible = true
	clickSound:Play()
end

minimizeBtn.MouseButton1Click:Connect(minimizeGUI)
title.MouseButton1Click:Connect(minimizeGUI)

openBtn.MouseButton1Click:Connect(function()
	minimized = false
	frame.Visible = true
	openBtn.Visible = false
end)

-- Criador de Botões
local function makeBtn(txt, y, callback)
	local b = Instance.new("TextButton", frame)
	b.Size = UDim2.new(0,180,0,40)
	b.Position = UDim2.new(0,10,0,y)
	b.BackgroundColor3 = Color3.fromRGB(40,40,40)
	b.BorderSizePixel = 0
	b.Font = Enum.Font.SourceSansBold
	b.TextSize = 18
	b.TextColor3 = Color3.new(1,1,1)
	b.Text = txt
	b.MouseButton1Click:Connect(callback)
	return b
end

-- Fly
makeBtn("Fly", 10, function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/5TR9/5TR/refs/heads/main/Dev%205TR%20%7C%20FLY.txt"))()
end)

-- Invisível
local btnInvis = makeBtn("Invisível: Ativar", 60, function()
	pcall(function()
		loadstring(game:HttpGet('https://pastebin.com/raw/3Rnd9rHf'))()
	end)
	btnInvis.Text = "Invisível: Ativado"
	btnInvis.AutoButtonColor = false
	btnInvis.TextColor3 = Color3.fromRGB(150, 150, 150)
end)

-- Noclip
local noclipping = false
local noclipBtn
noclipBtn = makeBtn("Noclip: Desativado", 110, function()
	noclipping = not noclipping
	noclipBtn.Text = "Noclip: " .. (noclipping and "Ativado" or "Desativado")
end)

RunService.Stepped:Connect(function()
	if noclipping and char then
		for _, part in ipairs(char:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)

-- TP Tool
local tpToolOn = false
local tpBtn
tpBtn = makeBtn("TP Tool: Desativado", 160, function()
	tpToolOn = not tpToolOn
	tpBtn.Text = "TP Tool: " .. (tpToolOn and "Ativado" or "Desativado")
	if tpToolOn then
		if not player.Backpack:FindFirstChild("TeleportTool") then
			local tool = Instance.new("Tool")
			tool.Name = "TeleportTool"
			tool.RequiresHandle = false
			tool.Parent = player.Backpack
			tool.Activated:Connect(function()
				local mouse = player:GetMouse()
				if mouse and mouse.Hit then
					hrp.CFrame = CFrame.new(mouse.Hit.p + Vector3.new(0, 3, 0))
				end
			end)
		end
	else
		for _, loc in ipairs({player.Backpack, char}) do
			local t = loc:FindFirstChild("TeleportTool")
			if t then t:Destroy() end
		end
	end
end)

-- Fling Player
makeBtn("Fling Player", 210, function()
	loadstring(game:HttpGet("https://pastefy.app/RHAe4iIS/raw"))()
end)

-- Descrição do Fling
local descriptionFling = Instance.new("TextLabel", frame)
descriptionFling.Size = UDim2.new(0, 180, 0, 60)
descriptionFling.Position = UDim2.new(0, 10, 0, 260)
descriptionFling.Text = "Essa opção \"fling player\"\nquando algum player\nencosta em você ele\né jogado para longe!"
descriptionFling.TextColor3 = Color3.new(1, 1, 1)
descriptionFling.TextSize = 14
descriptionFling.TextWrapped = true
descriptionFling.BackgroundTransparency = 1
descriptionFling.Font = Enum.Font.SourceSans
