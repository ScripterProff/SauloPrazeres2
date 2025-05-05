local function notify(text)
    local StarterGui = game:GetService("StarterGui")
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Tsuo Hub",
            Text = text,
            Duration = 3
        })
    end)
end

local function pegarArmasMelhorado()
    local player = game.Players.LocalPlayer
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")
    local backpack = player:WaitForChild("Backpack")

    -- Ativar noclip
    local RunService = game:GetService("RunService")
    local noclip = true
    local con = RunService.Stepped:Connect(function()
        if noclip and char then
            for _, v in ipairs(char:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end)

    local armas = { "Remington 870", "M9", "AK-47" }

    for _, nome in ipairs(armas) do
        local giver = workspace:WaitForChild("Prison_ITEMS"):WaitForChild("giver"):FindFirstChild(nome)
        if giver and giver:FindFirstChild("Head") then
            root.CFrame = giver.Head.CFrame + Vector3.new(0, 3, 0)
            task.wait(0.7)

            local detector = giver:FindFirstChildOfClass("ClickDetector")
            if detector and fireclickdetector then
                local tentativas = 0
                repeat
                    fireclickdetector(detector)
                    task.wait(0.5)
                    tentativas += 1
                until backpack:FindFirstChild(nome) or tentativas > 10

                if backpack:FindFirstChild(nome) then
                    backpack[nome].Parent = char
                    notify("Pegou: " .. nome)
                else
                    notify("Falha: " .. nome)
                end
            end
        else
            notify("Giver não encontrado: " .. nome)
        end
    end

    noclip = false
    con:Disconnect()

    task.wait(0.5)
    root.CFrame = CFrame.new(779, 97, 2467) -- Voltar ao pátio
    notify("Armas coletadas e equipadas.")
end
