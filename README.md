-- Simple GUI com botões: Teleport Chests, Teleport Coins, Teleport Pellet e Auto Collect

-- Configurações iniciais
game.Workspace.FallenPartsDestroyHeight = -50000

_G.TeleportChests = false
_G.TeleportCoins  = false
_G.TeleportPellet = false
_G.Collect        = false

-- Cria ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name   = "AutoFarmGui"
screenGui.Parent = game.CoreGui

-- Função de criação de botão
local function criarBotao(nome, posY)
    local btn = Instance.new("TextButton")
    btn.Name               = nome:gsub(" ", "")
    btn.Text               = nome
    btn.Size               = UDim2.new(0, 200, 0, 40)
    btn.Position           = UDim2.new(0, 20, 0, posY)
    btn.BackgroundColor3   = Color3.fromRGB(50,50,50)
    btn.TextColor3         = Color3.fromRGB(255,255,255)
    btn.TextScaled         = true
    btn.Parent             = screenGui
    return btn
end

-- Cria botões na GUI
local btnTeleportChests = criarBotao("Teleport to Chests (Off)", 20)
local btnTeleportCoins  = criarBotao("Teleport to Coins (Off)", 70)
local btnTeleportPellet = criarBotao("Teleport to Pellet (Off)", 120)
local btnAutoCollect    = criarBotao("Auto Collect (Off)", 170)

-- Função Teleport Chests
btnTeleportChests.MouseButton1Up:Connect(function()
    _G.TeleportChests = not _G.TeleportChests
    if _G.TeleportChests then
        btnTeleportChests.BackgroundColor3 = Color3.fromRGB(0,200,0)
        btnTeleportChests.Text = "Teleport to Chests (On)"
        spawn(function()
            while _G.TeleportChests do
                for _, v in pairs(workspace.chests:GetChildren()) do
                    if v:IsA("BasePart") then
                        local cf = v.CFrame
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = cf
                        wait(0.2)
                    end
                end
            end
        end)
    else
        btnTeleportChests.BackgroundColor3 = Color3.fromRGB(200,0,0)
        btnTeleportChests.Text = "Teleport to Chests (Off)"
    end
end)

-- Função Teleport Coins
btnTeleportCoins.MouseButton1Up:Connect(function()
    _G.TeleportCoins = not _G.TeleportCoins
    if _G.TeleportCoins then
        btnTeleportCoins.BackgroundColor3 = Color3.fromRGB(0,200,0)
        btnTeleportCoins.Text = "Teleport to Coins (On)"
        spawn(function()
            while _G.TeleportCoins do
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") and (v.Name == "Coin1" or v.Name == "Coin2" or v.Name == "Coin4") then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0,3,0)
                        wait(0.2)
                    end
                end
            end
        end)
    else
        btnTeleportCoins.BackgroundColor3 = Color3.fromRGB(200,0,0)
        btnTeleportCoins.Text = "Teleport to Coins (Off)"
    end
end)

-- Função Teleport Pellet
btnTeleportPellet.MouseButton1Up:Connect(function()
    _G.TeleportPellet = not _G.TeleportPellet
    if _G.TeleportPellet then
        btnTeleportPellet.BackgroundColor3 = Color3.fromRGB(0,200,0)
        btnTeleportPellet.Text = "Teleport to Pellet (On)"
        spawn(function()
            while _G.TeleportPellet do
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") and v.Name == "WhiteSpiritPellet" then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0,3,0)
                        wait(0.2)
                    end
                end
            end
        end)
    else
        btnTeleportPellet.BackgroundColor3 = Color3.fromRGB(200,0,0)
        btnTeleportPellet.Text = "Teleport to Pellet (Off)"
    end
end)

-- Função Auto Collect
btnAutoCollect.MouseButton1Up:Connect(function()
    _G.Collect = not _G.Collect
    if _G.Collect then
        btnAutoCollect.BackgroundColor3 = Color3.fromRGB(0,200,0)
        btnAutoCollect.Text = "Auto Collect (On)"
        spawn(function()
            while _G.Collect do
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") then
                        local prom = v:FindFirstChildOfClass("ProximityPrompt")
                        if prom then fireproximityprompt(prom) end
                    end
                end
                wait(0.1)
            end
        end)
    else
        btnAutoCollect.BackgroundColor3 = Color3.fromRGB(200,0,0)
        btnAutoCollect.Text = "Auto Collect (Off)"
    end
end)
