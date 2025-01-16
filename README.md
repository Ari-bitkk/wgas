local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local userInputService = game:GetService("UserInputService")

-- Criação da BillboardGui
local billboardGui = Instance.new("BillboardGui")
billboardGui.Adornee = humanoidRootPart
billboardGui.Size = UDim2.new(5, 0, 5, 0) -- Tamanho inicial
billboardGui.StudsOffset = Vector3.new(0, 5, 0) -- Posição acima do personagem
billboardGui.Enabled = false -- Começa desativado
billboardGui.Parent = character

-- Adicionando uma imagem ao BillboardGui
local imageLabel = Instance.new("ImageLabel")
imageLabel.Image = "rbxassetid://ID_DA_IMAGEM" -- Substitua pelo ID da sua imagem
imageLabel.Size = UDim2.new(1, 0, 1, 0)
imageLabel.BackgroundTransparency = 1
imageLabel.Parent = billboardGui

-- Ajusta o tamanho da BillboardGui com base na distância
local function adjustSize()
    if billboardGui.Enabled then
        local camera = workspace.CurrentCamera
        local distance = (camera.CFrame.Position - humanoidRootPart.Position).Magnitude
        local newSize = math.clamp(distance / 10, 5, 50) -- Ajusta o tamanho entre 5 e 50
        billboardGui.Size = UDim2.new(newSize, 0, newSize, 0)
    end
end

-- Ativa/Desativa a BillboardGui com teclas
userInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end -- Ignorar se o jogo já estiver processando a entrada

    if input.KeyCode == Enum.KeyCode.Insert then
        billboardGui.Enabled = true
    elseif input.KeyCode == Enum.KeyCode.Delete then
        billboardGui.Enabled = false
    end
end)

-- Atualizar o tamanho da BillboardGui continuamente
game:GetService("RunService").RenderStepped:Connect(adjustSize)
