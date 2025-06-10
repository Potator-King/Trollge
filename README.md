-- Atualização: GUI separada com botões individuais para teleportar para baús, moedas e WhiteSpiritPellet,
-- mais um botão único para coletar todos os itens com ProximityPrompt.

-- Configuração
_G.TeleportChests = false
_G.TeleportCoins = false
_G.TeleportPellet = false
_G.Collect = false

-- Botão: Teleport to Chests
TeleportToChests.MouseButton1Up:Connect(function()
    if not _G.TeleportChests then
        _G.TeleportChests = true
        TeleportToChests.Text = "Teleport to Chests (On)"
        TeleportToChests.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

        while _G.TeleportChests do
            for _, v in pairs(workspace.chests:GetChildren()) do
                if v:IsA("BasePart") then
                    local cf = v.CFrame
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = cf
                    Part1.CFrame = cf * CFrame.new(0,-3.5,0)
                    Part2.CFrame = cf * CFrame.new(-5.5,2,0)
                    Part3.CFrame = cf * CFrame.new(5.5,2,0)
                    Part4.CFrame = cf * CFrame.new(0,2,5.5)
                    Part5.CFrame = cf * CFrame.new(0,2,-5.5)
                    Part6.CFrame = cf * CFrame.new(0,7.5,0)
                    wait(tonumber(TeleportToChestsDelay.Text) or 0.2)
                end
            end
        end
    else
        _G.TeleportChests = false
        TeleportToChests.Text = "Teleport to Chests (Off)"
        TeleportToChests.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

-- Botão: Teleport to Coins
TeleportCoins = Instance.new("TextButton")
TeleportCoins.Parent = Frame2
TeleportCoins.Size = UDim2.new(0, 125, 0, 25)
TeleportCoins.Position = UDim2.new(0, 12.5, 0, 87.5)
TeleportCoins.Text = "Teleport to Coins (Off)"
TeleportCoins.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
TeleportCoins.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportCoins.TextScaled = true
TeleportCoins.MouseButton1Up:Connect(function()
    if not _G.TeleportCoins then
        _G.TeleportCoins = true
        TeleportCoins.Text = "Teleport to Coins (On)"
        TeleportCoins.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        while _G.TeleportCoins do
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and (v.Name == "Coin1" or v.Name == "Coin2" or v.Name == "Coin4") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0, 3, 0)
                    wait(tonumber(AutoCollectDelay.Text) or 0.1)
                end
            end
        end
    else
        _G.TeleportCoins = false
        TeleportCoins.Text = "Teleport to Coins (Off)"
        TeleportCoins.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

-- Botão: Teleport to Pellet
TeleportPellet = Instance.new("TextButton")
TeleportPellet.Parent = Frame2
TeleportPellet.Size = UDim2.new(0, 125, 0, 25)
TeleportPellet.Position = UDim2.new(0, 12.5, 0, 125)
TeleportPellet.Text = "Teleport to Pellet (Off)"
TeleportPellet.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
TeleportPellet.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportPellet.TextScaled = true
TeleportPellet.MouseButton1Up:Connect(function()
    if not _G.TeleportPellet then
        _G.TeleportPellet = true
        TeleportPellet.Text = "Teleport to Pellet (On)"
        TeleportPellet.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        while _G.TeleportPellet do
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and v.Name == "WhiteSpiritPellet" then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0, 3, 0)
                    wait(tonumber(AutoCollectDelay.Text) or 0.1)
                end
            end
        end
    else
        _G.TeleportPellet = false
        TeleportPellet.Text = "Teleport to Pellet (Off)"
        TeleportPellet.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end)

-- Botão: Coletar todos
AutoCollectChests.MouseButton1Up:Connect(function()
    if AutoCollectChests.Text == "Auto Collect Chests (Off)" then
        AutoCollectChests.Text = "Auto Collect Chests (On)"
        AutoCollectChests.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        _G.Collect = true

        while _G.Collect do
            wait(tonumber(AutoCollectDelay.Text) or 0.1)
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") then
                    local prompt = v:FindFirstChildOfClass("ProximityPrompt")
                    if prompt then
                        fireproximityprompt(prompt)
                    end
                end
            end
        end
    else
        AutoCollectChests.Text = "Auto Collect Chests (Off)"
        AutoCollectChests.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        _G.Collect = false
    end
end)
