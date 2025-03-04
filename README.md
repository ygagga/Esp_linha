-- INÍCIO: Configuração Inicial
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Função para criar a linha entre o jogador e o alvo
local function createLineToPlayer(targetPlayer)
    -- Cria o objeto da linha
    local line = Instance.new("Part")
    line.Size = Vector3.new(0.2, 0.2, 0.2)  -- Definindo a espessura da linha
    line.Anchored = true
    line.CanCollide = false
    line.Transparency = 0.5  -- Tornando a linha semi-transparente
    line.Color = Color3.fromRGB(0, 255, 255)  -- Cor da linha (aqui, um azul claro)
    line.Parent = game.Workspace

    -- Atualiza a linha a cada frame para que ela siga o personagem
    game:GetService("RunService").Heartbeat:Connect(function()
        if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
            local startPosition = character.HumanoidRootPart.Position

            -- Atualiza a posição da linha
            line.CFrame = CFrame.new(startPosition, targetPosition)
            line.Size = Vector3.new(0.2, 0.2, (startPosition - targetPosition).Magnitude)  -- Tamanho da linha baseado na distância
        else
            line:Destroy()  -- Se o personagem alvo não estiver mais disponível, destrói a linha
        end
    end)

    return line
end

-- Função para calcular a distância entre o espectador e o jogador
local function calculateDistance(targetPlayer)
    if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetRootPart = targetPlayer.Character.HumanoidRootPart
        local distance = (targetRootPart.Position - character.HumanoidRootPart.Position).Magnitude
        return distance
    end
    return 0
end

-- Função para mostrar a distância na tela
local function displayDistance(distance)
    local distanceLabel = Instance.new("TextLabel")
    distanceLabel.Size = UDim2.new(0, 200, 0, 50)
    distanceLabel.Position = UDim2.new(0.5, -100, 0.5, -25)
    distanceLabel.BackgroundTransparency = 1
    distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    distanceLabel.TextStrokeTransparency = 0.8
    distanceLabel.Text = "Distância: " .. math.floor(distance) .. " studs"
    distanceLabel.Parent = game.Players.LocalPlayer.PlayerGui
end

-- Função para começar a espectar o jogador
local function startSpectating(targetPlayer)
    -- Cria a linha para o ESP
    local line = createLineToPlayer(targetPlayer)

    -- Mostrar a distância na tela
    game:GetService("RunService").Heartbeat:Connect(function()
        local distance = calculateDistance(targetPlayer)
        displayDistance(distance)
    end)
end

-- Função para selecionar o jogador (pode ser através de comando ou interface personalizada)
local function onCommandReceived(command)
    local targetPlayerName = string.match(command, "^!espectar%s(.+)$") -- Comando para espectar um jogador, ex: !espectar NomeDoJogador
    if targetPlayerName then
        -- Encontrar o jogador com o nome especificado
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer then
            startSpectating(targetPlayer)
        else
            print("Jogador não encontrado!")
        end
    end
end

-- Detectar entrada de comando no chat
game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.OnMessageDoneFiltering.OnClientEvent:Connect(function(message)
    -- Quando o jogador enviar um comando no chat, verifica se é o comando de espectar
    onCommandReceived(message)
end)

-- FIM: Finalização e Mensagem de Log
print("Script carregado com sucesso! Você pode usar o comando !espectar NomeDoJogador para começar a espectar e ver a distância.")
