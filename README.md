-- Variáveis principais
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Criar a GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DinoCHEATS"
screenGui.Parent = PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 400, 0, 600)
frame.Position = UDim2.new(0.5, -200, 0.5, -300)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.5
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.Text = "Dino CHEATS (KRUSH PVP)"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24
title.BackgroundTransparency = 1
title.Parent = frame

-- Função para abrir/fechar a GUI
local function toggleMenu()
    frame.Visible = not frame.Visible
end

-- Fly (Ativar/Desativar)
local flying = false
local bodyGyro, bodyVelocity

local function toggleFly()
    flying = not flying
    if flying then
        -- Criar os objetos necessários para o Fly
        bodyGyro = Instance.new("BodyGyro")
        bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
        bodyGyro.CFrame = player.Character.HumanoidRootPart.CFrame
        bodyGyro.Parent = player.Character.HumanoidRootPart
        
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
        bodyVelocity.Velocity = Vector3.new(0, 50, 0)  -- Ajuste a velocidade de voo
        bodyVelocity.Parent = player.Character.HumanoidRootPart
    else
        -- Remover o Fly
        if bodyGyro then bodyGyro:Destroy() end
        if bodyVelocity then bodyVelocity:Destroy() end
    end
end

-- Botão de Fly
local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0, 200, 0, 50)
flyButton.Position = UDim2.new(0, 100, 0, 100)
flyButton.Text = "Fly (Ativar/Desativar)"
flyButton.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.Parent = frame
flyButton.MouseButton1Click:Connect(toggleFly)

-- ESP (Caixa de ESP)
local function createESP(target)
    if target.Character then
        local espBox = Instance.new("BoxHandleAdornment")
        espBox.Size = target.Character:GetBoundingBox().Size
        espBox.Adornee = target.Character
        espBox.Color3 = Color3.fromRGB(255, 0, 0)
        espBox.Parent = target.Character
    end
end

local function toggleESP()
    for _, p in ipairs(Players:GetPlayers()) do
        if p.Character and p ~= player then
            createESP(p)
        end
    end
end

-- Botão de ESP
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0, 200, 0, 50)
espButton.Position = UDim2.new(0, 100, 0, 160)
espButton.Text = "ESP (Ativar/Desativar)"
espButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.Parent = frame
espButton.MouseButton1Click:Connect(toggleESP)

-- Aimbot com FOV
local fovSlider = Instance.new("Slider")
fovSlider.Size = UDim2.new(0, 200, 0, 20)
fovSlider.Position = UDim2.new(0, 100, 0, 220)
fovSlider.MinValue = 10
fovSlider.MaxValue = 100
fovSlider.Value = 50
fovSlider.Parent = frame

-- Função de AimLock
local function aimLock()
    local closestPlayer, closestDistance
    for _, target in ipairs(Players:GetPlayers()) do
        if target.Character and target ~= player then
            local distance = (player.Character.HumanoidRootPart.Position - target.Character.HumanoidRootPart.Position).Magnitude
            if not closestDistance or distance < closestDistance then
                closestPlayer = target
                closestDistance = distance
            end
        end
    end

    if closestPlayer then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(closestPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 0, 2))
    end
end

-- Botão de Aimlock
local aimlockButton = Instance.new("TextButton")
aimlockButton.Size = UDim2.new(0, 200, 0, 50)
aimlockButton.Position = UDim2.new(0, 100, 0, 280)
aimlockButton.Text = "Aimlock"
aimlockButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
aimlockButton.TextColor3 = Color3.fromRGB(255, 255, 255)
aimlockButton.Parent = frame
aimlockButton.MouseButton1Click:Connect(aimLock)

-- Teleporte para outro jogador
local function tpPlayer(target)
    if target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
    end
end

-- Botão de Teleporte
local tpButton = Instance.new("TextButton")
tpButton.Size = UDim2.new(0, 200, 0, 50)
tpButton.Position = UDim2.new(0, 100, 0, 340)
tpButton.Text = "Teleportar para Jogador"
tpButton.BackgroundColor3 = Color3.fromRGB(100, 255, 255)
tpButton.TextColor3 = Color3.fromRGB(255, 255, 255)
tpButton.Parent = frame
tpButton.MouseButton1Click:Connect(function()
    local target = Players:GetPlayers()[math.random(1, #Players:GetPlayers())]  -- Teleportar para um jogador aleatório
    tpPlayer(target)
end)

-- Função de Minimizar/Maximizar a GUI
local function minimizeMenu()
    frame.Visible = false
end

-- Botão de Minimizar
local minimizeButton = Instance.new("TextButton")
minimizeButton.Size = UDim2.new(0, 50, 0, 50)
minimizeButton.Position = UDim2.new(1, -50, 0, 10)
minimizeButton.Text = "-"
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeButton.Parent = frame
minimizeButton.MouseButton1Click:Connect(minimizeMenu)
