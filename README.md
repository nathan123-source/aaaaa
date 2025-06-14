local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local ProximityPromptService = game:GetService("ProximityPromptService")

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Configurações
local config = {
    LocaisAleatorios = {
        CFrame.new(-550.090454, 3.28816128, 40.9795532, 1, 0, 0, 0, 1, 0, 0, 0, 1),
        CFrame.new(-495.288879, 3.20863581, 10.057312, 0.980359316, 0, 0.197219774, 0, 1, 0, -0.197219774, 0, 0.980359316),
        CFrame.new(-498.865448, 3.35259485, -68.842926, 1, 0, 0, 0, 1, 0, 0, 0, 1),
        CFrame.new(-616.710205, 3.24663591, 9.45095444, 0.904720604, 0, 0.426005453, 0, 1, 0, -0.426005453, 0, 0.904720604),
        CFrame.new(-378.266357, 3.46769452, -30.3575802, 0, 0, 1, 0, 1, -0, -1, 0, 0),
        CFrame.new(-378.03656, 3.38713574, -27.1200333, -1, 0, 0, 0, 1, 0, 0, 0, -1),
        CFrame.new(-454.901733, 3.35945153, -3.81340384, -0.867699981, 0, -0.497088224, 0, 1, 0, 0.497088224, 0, -0.867699981),
        CFrame.new(-454.89386, 3.24043727, -37.3385849, 1, 0, 0, 0, 1, 0, 0, 0, 1),
        CFrame.new(-455.081329, 3.26448464, -40.1521721, -0.879374862, 0, -0.476129979, 0, 1, 0, 0.476129979, 0, -0.879374862),
        CFrame.new(-454.84436, 3.35945153, -7.08071899, 0.312709093, -0, -0.94984895, 0, 1, -0, 0.94984895, 0, 0.312709093),
        CFrame.new(-498.832886, 3.20863581, 10.057312, -1, 0, 0, 0, 1, 0, 0, 0, -1),
        CFrame.new(-485.288635, 3.25063586, 36.3800468, -1, 0, 0, 0, 1, 0, 0, 0, -1),
        CFrame.new(-493.315643, 3.25063586, 41.082119, 0, 0, -1, 0, 1, 0, 1, 0, 0),
        CFrame.new(-495.288879, 3.20863581, 10.057312, 0.980359316, 0, 0.197219774, 0, 1, 0, -0.197219774, 0, 0.980359316),
        CFrame.new(-346.349335, 11.2034531, -15.1218395, -0.31654954, 0, 0.948575974, 0, 1, 0, -0.948575974, 0, -0.31654954),
        CFrame.new(-485.425842, 3.30863595, 40.0470543, 0, 0, -1, 0, 1, 0, 1, 0, 0),
        CFrame.new(-529.422485, 7.59310627, 41.1137466, -0.941578388, 0.00161392801, -0.336789817, 0.03796231, 0.994124353, -0.101368994, 0.334647328, -0.108232185, -0.936107278),
        CFrame.new(-547.876831, 3.34044552, 4.75392103, 1, 0, 0, 0, 1, 0, 0, 0, 1),
        CFrame.new(-530.51886, 5.72843599, 40.7209358, -0.214117646, 0.0016499348, 0.976806521, -0.105338335, 0.994127929, -0.0247695222, -0.971111536, -0.108198754, -0.212686539),
        CFrame.new(-546.534424, 3.28816128, 41.2314491, 0.363939464, -0, -0.931422591, 0, 1, -0, 0.931422591, 0, 0.363939464),
        CFrame.new(-550.090454, 3.28816128, 40.9795532, 1, 0, 0, 0, 1, 0, 0, 0, 1),
        CFrame.new(-556.973206, 3.34377933, 45.7063637, -0.799710631, 0, 0.600385725, 0, 1, 0, -0.600385725, 0, -0.799710631),
        CFrame.new(-555.154724, 3.26677322, 9.59934711, 0.962701917, -0, -0.270564169, 0, 1, -0, 0.270564169, 0, 0.962701917),
        CFrame.new(-552.148682, 3.26677322, 9.04824924, -0.607952714, 0, 0.793973267, 0, 1, 0, -0.793973267, 0, -0.607952714),
        CFrame.new(-585.328186, 3.89165664, 4.08425522, 1, 0, 0, 0, 1, 0, 0, 0, 1)
    },
    MaxDistance = 15 -- Aumentado para garantir detecção
}

-- Função para ativar/desativar noclip (método padrão)
local function setNoclip(enabled)
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not enabled
            print("Part " .. part.Name .. " CanCollide set to " .. tostring(not enabled))
        end
    end
end

-- Bypass alternativo para noclip (usando manipulação de assembly)
local function bypassNoclip(enabled)
    if enabled then
        local oldCFrame = humanoidRootPart.CFrame
        for _ = 1, 5 do -- Tenta mover o jogador para evitar colisões
            humanoidRootPart.Velocity = Vector3.new(0, 0, 0)
            humanoidRootPart.CFrame = oldCFrame * CFrame.new(0, 0.1, 0) -- Ajusta levemente para cima
            wait(0.01)
        end
        print("Bypass noclip aplicado em " .. tostring(humanoidRootPart.Position))
    else
        print("Bypass noclip desativado")
    end
end

-- Função para teleportar o jogador com noclip/bypass
local function teleportToPosition(position)
    setNoclip(true) -- Tenta noclip padrão
    bypassNoclip(true) -- Aplica bypass como fallback
    local tweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear)
    local tween = TweenService:Create(humanoidRootPart, tweenInfo, {CFrame = position})
    tween:Play()
    tween.Completed:Wait()
    wait(0.5) -- Aguarda estabilização
    setNoclip(false) -- Desativa noclip padrão
    bypassNoclip(false) -- Desativa bypass
    print("Teleporte concluído para: " .. tostring(position.Position))
end

-- Função para prevenir dano e morte
local function protectPlayer()
    humanoid.HealthChanged:Connect(function(health)
        if health <= 0 then
            humanoid.Health = 100 -- Restaura vida
            print("Vida restaurada para 100")
        elseif health < 100 then
            humanoid.Health = 100 -- Mantém vida cheia
            print("Vida mantida em 100")
        end
    end)
end

-- Função para evitar quedas no void
local function stabilizePosition()
    spawn(function()
        while wait(0.1) do
            if humanoidRootPart.Position.Y < -50 then -- Detecta queda no void
                humanoidRootPart.CFrame = config.LocaisAleatorios[1] -- Volta para a primeira posição
                humanoid.Health = 100 -- Restaura vida
                print("Jogador resgatado do void para: " .. tostring(config.LocaisAleatorios[1].Position))
            end
        end
    end)
end

-- Função para encontrar o ProximityPrompt mais próximo (ignora direção da câmera)
local function findNearestPrompt()
    local nearestPrompt = nil
    local minDistance = config.MaxDistance
    local prompts = workspace:GetDescendants()

    for _, obj in pairs(prompts) do
        if obj:IsA("ProximityPrompt") and obj.Enabled then
            local distance = (humanoidRootPart.Position - obj.Parent.Position).Magnitude
            if distance < minDistance then
                minDistance = distance
                nearestPrompt = obj
            end
        end
    end
    if nearestPrompt then
        nearestPrompt.RequiresLineOfSight = false -- Ignora direção da câmera
        print("Nearest prompt found at distance: " .. minDistance)
    else
        print("No prompt found within " .. config.MaxDistance .. " studs")
    end
    return nearestPrompt
end

-- Função para interagir com o ProximityPrompt
local function interactWithPrompt(prompt)
    if prompt and prompt.Enabled then
        setNoclip(true) -- Ativa noclip durante a interação
        bypassNoclip(true) -- Aplica bypass como fallback
        print("Interacting with prompt at " .. tostring(prompt.Parent.Position))
        if prompt.HoldDuration > 0 then
            print("Iniciando interação de segurar por " .. prompt.HoldDuration .. " segundos")
            prompt:InputHoldBegin()
            wait(prompt.HoldDuration + 0.1)
            prompt:InputHoldEnd()
        else
            print("Executando clique único")
            prompt:Trigger()
        end
        wait(0.5)
        setNoclip(false) -- Desativa noclip
        bypassNoclip(false) -- Desativa bypass
    else
        warn("ProximityPrompt não está habilitado ou não encontrado")
    end
end

-- Função principal para coletar lixo
local function collectTrash()
    protectPlayer() -- Ativa proteção contra dano
    stabilizePosition() -- Ativa estabilização contra void
    local currentIndex = 1

    while true do
        local targetPosition = config.LocaisAleatorios[currentIndex]
        print("Teleportando para posição: " .. tostring(targetPosition.Position))
        teleportToPosition(targetPosition)
        wait(0.5)

        local prompt = findNearestPrompt()
        if prompt then
            print("ProximityPrompt detectado a " .. (humanoidRootPart.Position - prompt.Parent.Position).Magnitude .. " studs")
            local promptActivated = false
            local connection = prompt.Triggered:Connect(function(playerWhoTriggered)
                if playerWhoTriggered == player then
                    promptActivated = true
                    print("ProximityPrompt ativado por " .. player.Name)
                end
            end)

            interactWithPrompt(prompt)
            wait(1)
            connection:Disconnect()
            if promptActivated then
                print("Prompt ativado, avançando para a próxima posição")
                currentIndex = currentIndex + 1
                if currentIndex > #config.LocaisAleatorios then
                    currentIndex = 1
                    print("Reiniciando ciclo de posições!")
                end
            else
                warn("Prompt não ativado na posição: " .. tostring(targetPosition.Position))
                currentIndex = currentIndex + 1
                if currentIndex > #config.LocaisAleatorios then
                    currentIndex = 1
                end
            end
        else
            warn("Nenhum ProximityPrompt encontrado na posição: " .. tostring(targetPosition.Position))
            currentIndex = currentIndex + 1
            if currentIndex > #config.LocaisAleatorios then
                currentIndex = 1
            end
        end
        wait(0.5)
    end
end

-- Inicia a coleta para o jogador atual
collectTrash()
