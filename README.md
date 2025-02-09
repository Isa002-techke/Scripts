-- Função para criar um ESP (Extra Sensory Perception) que permite ver jogadores através das paredes
local function createESP(character)
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            -- Cria uma caixa ao redor de cada parte do corpo
            local box = Instance.new("BoxHandleAdornment")
            box.Size = part.Size
            box.Adornee = part
            box.AlwaysOnTop = true
            box.ZIndex = 10
            box.Transparency = 1 -- Torna a caixa totalmente transparente
            box.Color3 = Color3.fromRGB(255, 255, 255) -- Cor da caixa (transparente)
            box.Parent = part
        end
    end

    -- Cria uma etiqueta com o nome e o time do jogador
    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = character.Head
    billboard.Size = UDim2.new(0, 100, 0, 50)
    billboard.AlwaysOnTop = true

    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = character.Name .. "\n" .. (game.Players:GetPlayerFromCharacter(character).Team and game.Players:GetPlayerFromCharacter(character).Team.Name or "No Team")
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.Parent = billboard

    billboard.Parent = character.Head
end

-- Função para adicionar ESP a todos os jogadores
local function addESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Character then
            createESP(player.Character)
        end
    end
end

-- Adiciona ESP a novos jogadores que entrarem no jogo
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        createESP(character)
    end)
end)

-- Adiciona ESP aos jogadores existentes
addESP()
