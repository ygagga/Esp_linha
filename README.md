-- Carrega as bibliotecas Fluent, SaveManager e InterfaceManager
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criando a Janela da Interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    SubTitle = "🔥 Zoando geral! 💀",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Criando Abas
local Tabs = {
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 🤡 Troll (ESP + Matar Jogador)
-----------------------------------------------------------
Tabs.Troll:AddSection("Trollando no servidor!")

-- Lista de jogadores para seleção
local jogadores = {}
local jogadorSelecionado = nil

-- Atualiza a lista de jogadores
local function atualizarListaJogadores()
    jogadores = {}
    for _, player in pairs(game.Players:GetPlayers()) do
        table.insert(jogadores, player.Name)
    end
end

-- Criar Dropdown de seleção de jogador
Tabs.Troll:AddDropdown("Selecionar Jogador", {
    Title = "Escolha um jogador",
    Values = jogadores,
    Multi = false,
    Callback = function(valor)
        jogadorSelecionado = valor
    end
})

-- Atualiza a lista de jogadores ao abrir a GUI
atualizarListaJogadores()

-- Botão para matar jogador selecionado
Tabs.Troll:AddButton({
    Title = "Matar Jogador ☠️",
    Description = "Teleporta o jogador para a morte",
    Callback = function()
        if jogadorSelecionado then
            local playerAlvo = game.Players:FindFirstChild(jogadorSelecionado)
            if playerAlvo and playerAlvo.Character and playerAlvo.Character:FindFirstChild("HumanoidRootPart") then
                playerAlvo.Character.HumanoidRootPart.CFrame = CFrame.new(-212, -499, -627)
            end
        else
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Erro",
                Text = "Selecione um jogador primeiro!",
                Duration = 3
            })
        end
    end
})
