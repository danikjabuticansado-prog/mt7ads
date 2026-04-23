local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local attackDistance = 10 -- distância para atacar
local attackCooldown = 1 -- tempo entre ataques (em segundos)

-- Função para encontrar o inimigo mais próximo
local function getNearestEnemy()
    local nearestEnemy = nil
    local shortestDistance = attackDistance

    for _, enemy in pairs(workspace:GetChildren()) do
        if enemy:FindFirstChild("Humanoid") 
        and enemy ~= character then

            local enemyRoot = enemy:FindFirstChild("HumanoidRootPart")

            if enemyRoot then
                local distance = (humanoidRootPart.Position - enemyRoot.Position).Magnitude

                if distance < shortestDistance then
                    shortestDistance = distance
                    nearestEnemy = enemy
                end
            end
        end
    end

    return nearestEnemy
end

-- Loop de ataque automático
while true do
    task.wait(attackCooldown)

    local enemy = getNearestEnemy()

    if enemy then
        local humanoid = enemy:FindFirstChild("Humanoid")

        if humanoid then
            humanoid:TakeDamage(10) -- dano causado
            print("Atacando:", enemy.Name)
        end
    end
end
