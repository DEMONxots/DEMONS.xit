-- Função para pegar fruta
function pegarFruta()
    for _, fruta in pairs(game.Workspace:GetDescendants()) do
        if fruta:IsA("Tool") and fruta:FindFirstChild("Handle") then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = fruta.Handle.CFrame
            wait(1)
            firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, fruta.Handle, 0)
            wait(0.5)
            firetouchinterest(game.Players.LocalPlayer.Character.HumanoidRootPart, fruta.Handle, 1)
            wait(1)
            print("Fruta coletada: " .. fruta.Name)
            return true
        end
    end
    return false
end

-- Função para guardar fruta
function guardarFruta()
    local player = game.Players.LocalPlayer
    local fruta = player.Backpack:FindFirstChildWhichIsA("Tool")
    if fruta then
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StoreFruit", fruta.Name)
        print("Fruta guardada: " .. fruta.Name)
    end
end

-- Função para trocar de servidor
function trocarServidor()
    local HttpService = game:GetService("HttpService")
    local Servers = HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/2753915549/servers/Public?sortOrder=Asc&limit=100"))
    for _, v in pairs(Servers.data) do
        if v.playing < v.maxPlayers then
            if v.id ~= game.JobId then
                print("Teleportando para outro servidor...")
                game:GetService("TeleportService"):TeleportToPlaceInstance(2753915549, v.id, game.Players.LocalPlayer)
                break
            end
        end
    end
end

-- Executando o processo
if pegarFruta() then
    wait(2)
    guardarFruta()
    wait(2)
    trocarServidor()
else
    print("Nenhuma fruta encontrada.")
    trocarServidor()
end
