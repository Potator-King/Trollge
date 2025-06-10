-- GUI estilo original com sistema de coleta e teleporte e correções de toggle
workspace.FallenPartsDestroyHeight = -50000

-- Variáveis globais
_G.TeleportChests = false
_G.TeleportCoins = false
_G.TeleportPellet = false
_G.Collect = false

-- Cria GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "AutoChestsGui"

-- Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 180, 0, 200)
frame.Position = UDim2.new(0, 50, 0, 100)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 2
frame.Active = true
frame.Draggable = true
frame.Parent = gui

-- Minimizar e Fechar
local close = Instance.new("TextButton", frame)
close.Text = "X"
close.Size = UDim2.new(0, 25, 0, 25)
close.Position = UDim2.new(1, -25, 0, 0)
close.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
close.TextColor3 = Color3.new(1,1,1)
close.MouseButton1Click:Connect(function() gui:Destroy() end)

local minimize = Instance.new("TextButton", frame)
minimize.Text = "-"
minimize.Size = UDim2.new(0, 25, 0, 25)
minimize.Position = UDim2.new(0, 0, 0, 0)
minimize.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
minimize.TextColor3 = Color3.new(1,1,1)
local minimized = false
minimize.MouseButton1Click:Connect(function()
    minimized = not minimized
    for _, v in ipairs(frame:GetChildren()) do
        if v:IsA("TextButton") and v ~= minimize and v ~= close then v.Visible = not minimized end
    end
    frame.Size = minimized and UDim2.new(0,180,0,30) or UDim2.new(0,180,0,200)
    minimize.Text = minimized and "+" or "-"
end)

-- Função de criação de botões
local function makeBtn(label, y)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(0, 160, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, 30 + (y-1)*35)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.BackgroundColor3 = Color3.fromRGB(120,0,0)
    btn.Text = label .. " (Off)"
    btn.AutoButtonColor = false
    return btn
end

-- Botões
local btnChests = makeBtn("Teleport Chests",1)
local btnCoins  = makeBtn("Teleport Coins",2)
local btnPellet = makeBtn("Teleport Pellet",3)
local btnCollect= makeBtn("Auto Collect",4)

-- Logic functions
local function toggleLoop(stateVar, btn, loopFunc)
    _G[stateVar] = not _G[stateVar]
    local on = _G[stateVar]
    btn.Text = btn.Text:sub(1, #btn.Text-6) .. (on and " (On)" or " (Off)")
    btn.BackgroundColor3 = on and Color3.fromRGB(0,170,0) or Color3.fromRGB(120,0,0)
    if on then task.spawn(loopFunc) end
end

-- Teleport Chests
btnChests.MouseButton1Click:Connect(function()
    toggleLoop("TeleportChests", btnChests, function()
        while _G.TeleportChests do
            for _, v in ipairs(workspace.chests:GetChildren()) do
                if not _G.TeleportChests then return end
                if v:IsA("BasePart") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    wait(0.2)
                end
            end
            wait(0.5)
        end
    end)
end)

-- Teleport Coins
btnCoins.MouseButton1Click:Connect(function()
    toggleLoop("TeleportCoins", btnCoins, function()
        while _G.TeleportCoins do
            for _, v in ipairs(workspace:GetDescendants()) do
                if not _G.TeleportCoins then return end
                if v:IsA("BasePart") and (v.Name=="Coin1" or v.Name=="Coin2" or v.Name=="Coin4") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame+Vector3.new(0,3,0)
                    wait(0.2)
                end
            end
            wait(0.5)
        end
    end)
end)

-- Teleport Pellet
btnPellet.MouseButton1Click:Connect(function()
    toggleLoop("TeleportPellet", btnPellet, function()
        while _G.TeleportPellet do
            for _, v in ipairs(workspace:GetDescendants()) do
                if not _G.TeleportPellet then return end
                if v:IsA("BasePart") and v.Name=="WhiteSpiritPellet" then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame+Vector3.new(0,3,0)
                    wait(0.2)
                end
            end
            wait(0.5)
        end
    end)
end)

-- Auto Collect
btnCollect.MouseButton1Click:Connect(function()
    toggleLoop("Collect", btnCollect, function()
        while _G.Collect do
            -- normal chests
            for _, v in ipairs(workspace.chests:GetChildren()) do
                if v:IsA("BasePart") then fireproximityprompt(v:FindFirstChildOfClass("ProximityPrompt")) end
            end
            -- special chests
            for _, v in ipairs(workspace:GetChildren()) do
                if v.Name=="DarkChest_p" or v.Name=="Weeping Chest-p" then fireproximityprompt(v:FindFirstChildOfClass("ProximityPrompt")) end
            end
            -- coins & pellet
            for _, v in ipairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") and (v.Name=="Coin1"or v.Name=="Coin2"or v.Name=="Coin4"or v.Name=="WhiteSpiritPellet") then
                    fireproximityprompt(v:FindFirstChildOfClass("ProximityPrompt"))
                end
            end
            wait(0.1)
        end
    end)
end)
