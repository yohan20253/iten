local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local backpack = player:WaitForChild("Backpack")

-- Garantir pasta 'Itens'
local itensFolder = ReplicatedStorage:FindFirstChild("Itens")
if not itensFolder then
	itensFolder = Instance.new("Folder")
	itensFolder.Name = "Itens"
	itensFolder.Parent = ReplicatedStorage
end

-- Criar GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BuscaItemGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 50)
frame.Position = UDim2.new(0.5, -150, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
frame.Active = true
frame.Draggable = true
frame.Visible = false  -- começa invisível
frame.Parent = screenGui

local textbox = Instance.new("TextBox")
textbox.Size = UDim2.new(1, 0, 1, 0)
textbox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
textbox.PlaceholderText = "Digite o nome do item..."
textbox.Text = ""
textbox.TextScaled = true
textbox.ClearTextOnFocus = false
textbox.Parent = frame

-- Função para ativar/desativar barra
local function toggleBar()
	if frame.Visible then
		frame.Visible = false
		textbox.Text = ""
		userInputFocused = false
	else
		frame.Visible = true
		textbox:CaptureFocus()
	end
end

-- Detectar tecla P para ligar/desligar barra
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.P then
		toggleBar()
	end
end)

-- Ação ao apertar Enter no TextBox
textbox.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		local nomeItem = textbox.Text
		local item = itensFolder:FindFirstChild(nomeItem)

		if item and item:IsA("Tool") then
			local clone = item:Clone()
			clone.Parent = backpack
		else
			warn("Item '" .. nomeItem .. "' não encontrado!")
		end

		textbox.Text = ""
	end
end)
