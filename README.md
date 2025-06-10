-- Configurações iniciais
game.Workspace.FallenPartsDestroyHeight = -50000

_G.Teleport = false
_G.Collect = false
_G.OpenAllChests = false
_G.Delete = false

-- Remove GUI anterior, se existir
if game:GetService("CoreGui"):FindFirstChild("AutoChestsGui") then
    game:GetService("CoreGui"):FindFirstChild("AutoChestsGui"):Destroy()
end

-- Limpa antigos FloatPath
for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
    if v.Name == "FloatPath" then
        v:Destroy()
    end
end

-- Criação da GUI
local AutoChestsGui = Instance.new("ScreenGui")
local Frame       = Instance.new("Frame")
local Frame2      = Instance.new("Frame")
local TextLabel   = Instance.new("TextLabel")
local KillGui     = Instance.new("TextButton")
local Mini        = Instance.new("TextButton")
local TeleportToChests        = Instance.new("TextButton")
local TeleportToChestsSetting = Instance.new("TextButton")
local TeleportToChestsDelay   = Instance.new("TextBox")
local AutoCollectChests       = Instance.new("TextButton")
local AutoCollectSetting      = Instance.new("TextButton")
local AutoCollectDelay        = Instance.new("TextBox")
local GotoShop                = Instance.new("TextButton")
local Fix                     = Instance.new("TextButton")
local Next                    = Instance.new("TextButton")
local Back                    = Instance.new("TextButton")
local Page                    = Instance.new("TextLabel")
local AutoOpenAllChests       = Instance.new("TextButton")
local DeleteUselessItems      = Instance.new("TextButton")
local TeleportTool            = Instance.new("TextButton")
local ServerHop               = Instance.new("TextButton")
local Rejoin                  = Instance.new("TextButton")
local Part                    = Instance.new("Part")
local Part1 = Instance.new("Part")
local Part2 = Instance.new("Part")
local Part3 = Instance.new("Part")
local Part4 = Instance.new("Part")
local Part5 = Instance.new("Part")
local Part6 = Instance.new("Part")

-- Criação das plataformas de teleporte visual
for _, p in ipairs({Part1,Part2,Part3,Part4,Part5,Part6}) do
    p.Name = "FloatPath"
    p.Parent = Workspace
    p.Anchored = true
    p.Transparency = .9
    if p == Part1 then p.Size = Vector3.new(10,1,10)
    elseif p == Part2 or p == Part3 then p.Size = Vector3.new(1,10,10)
    elseif p == Part4 or p == Part5 then p.Size = Vector3.new(10,10,1)
    else p.Size = Vector3.new(10,1,10) end
end

-- Propriedades da GUI
AutoChestsGui.Name   = "AutoChestsGui"
AutoChestsGui.Parent = game.CoreGui

Frame.Name                   = "Frame"
Frame.Parent                 = AutoChestsGui
Frame.BackgroundColor3       = Color3.fromRGB(255, 255, 255)
Frame.BackgroundTransparency = .5
Frame.BorderColor3           = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel        = 2
Frame.Position               = UDim2.new(0, 100, 0, 100)
Frame.Size                   = UDim2.new(0, 150, 0, 25)
Frame.Active                 = true
Frame.Draggable              = true

Frame2.Name                   = "Frame2"
Frame2.Parent                 = Frame
Frame2.BackgroundColor3       = Color3.fromRGB(102, 0, 204)
Frame2.BackgroundTransparency = .5
Frame2.BorderColor3           = Color3.fromRGB(255, 255, 255)
Frame2.BorderSizePixel        = 2
Frame2.Position               = UDim2.new(0, 0, 0, 30)
Frame2.Size                   = UDim2.new(0, 150, 0, 185)

TextLabel.Name               = "TextLabel"
TextLabel.Parent             = Frame
TextLabel.Size               = UDim2.new(0, 100, 0, 25)
TextLabel.Position           = UDim2.new(0, 25, 0, 0)
TextLabel.Text               = "Made by Red_BloxZ"
TextLabel.BackgroundTransparency = 1
TextLabel.TextScaled         = true

-- Botão para fechar GUI
KillGui.Name   = "KillGui"
KillGui.Parent = Frame
KillGui.Size   = UDim2.new(0, 25, 0, 25)
KillGui.Position = UDim2.new(0, 0, 0, 0)
KillGui.Text     = "X"
KillGui.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
KillGui.TextScaled       = true
KillGui.MouseButton1Up:Connect(function()
    _G.Teleport = false
    _G.Collect  = false
    _G.OpenAllChests = false
    _G.Delete   = false
    AutoChestsGui:Destroy()
    for _, p in ipairs({Part1,Part2,Part3,Part4,Part5,Part6}) do
        p:Destroy()
    end
    local hrp = game.Players.LocalPlayer.Character.HumanoidRootPart
    if hrp:FindFirstChildOfClass("BodyVelocity") then hrp:FindFirstChildOfClass("BodyVelocity"):Destroy() end
    if hrp:FindFirstChildOfClass("BodyGyro")    then hrp:FindFirstChildOfClass("BodyGyro"):Destroy() end
end)

-- Botão minimizar
Mini.Name   = "Mini"
Mini.Parent = Frame
Mini.Size   = UDim2.new(0, 25, 0, 25)
Mini.Position = UDim2.new(0, 125, 0, 0)
Mini.Text     = "-"
Mini.BackgroundTransparency = .75
Mini.MouseButton1Up:Connect(function()
    if Mini.Text == "-" then
        Mini.Text = "+"
        Frame2.Visible = false
    else
        Mini.Text = "-"
        Frame2.Visible = true
    end
end)

-- Plataforma segura (infinita)
Part.Name     = "Safe Path"
Part.Parent   = Workspace
Part.Anchored = true
Part.Size     = Vector3.new(1000,25,1000)
Part.CFrame   = CFrame.new(1,99970,1)

-- (Outros botões e lógica de TeleportToChests, GotoShop, Fix, paginação etc. mantidos sem alterações)

-- CONFIGURAÇÃO DO BOTÃO AutoCollectChests
AutoCollectChests.Name            = "AutoCollectChests"
AutoCollectChests.Parent          = Frame2
AutoCollectChests.Size            = UDim2.new(0, 96, 0, 25)
AutoCollectChests.Position        = UDim2.new(0, 12.5, 0, 50)
AutoCollectChests.Text            = "Auto Collect Chests (Off)"
AutoCollectChests.BackgroundColor3= Color3.fromRGB(255, 0, 0)
AutoCollectChests.TextScaled      = true
AutoCollectDelay.Parent           = AutoCollectChests
AutoCollectDelay.Name             = "AutoCollectDelay"
AutoCollectDelay.Size             = UDim2.new(0, 50, 0, 25)
AutoCollectDelay.Position         = UDim2.new(0, 100, 0, 0)
AutoCollectDelay.Text             = "0.1"
AutoCollectDelay.ClearTextOnFocus = true
AutoCollectDelay.BackgroundTransparency = .5

AutoCollectChests.MouseButton1Up:Connect(function()
    if AutoCollectChests.Text == "Auto Collect Chests (Off)" then
        AutoCollectChests.Text = "Auto Collect Chests (On)"
        AutoCollectChests.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        _G.Collect = true

        while _G.Collect == true do
            wait(tonumber(AutoCollectDelay.Text) or 0.1)

            -- 1) Coletar baús dentro de Workspace.chests
            for _, v in pairs(workspace.chests:GetChildren()) do
                if v:IsA("BasePart") then
                    local prompt = v:FindFirstChildOfClass("ProximityPrompt")
                    if prompt then
                        fireproximityprompt(prompt)
                    end
                end
            end

            -- 2) Coletar baús especiais fora da pasta chests
            for _, v in pairs(workspace:GetChildren()) do
                if v.Name == "DarkChest_p" or v.Name == "Weeping Chest-p" then
                    local prompt = v:FindFirstChildOfClass("ProximityPrompt")
                    if prompt then
                        fireproximityprompt(prompt)
                    end
                end
            end

            -- 3) Coletar moedas e WhiteSpiritPellet
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and (
                   v.Name == "Coin1" or
                   v.Name == "Coin2" or
                   v.Name == "Coin4" or
                   v.Name == "WhiteSpiritPellet"
                ) then
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

-- (O restante do seu script continua aqui, sem alterações...)

