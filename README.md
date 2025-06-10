-- Configurações iniciais
game.Workspace.FallenPartsDestroyHeight = -50000
_G.TeleportChests = false
_G.TeleportCoins  = false
_G.TeleportPellet = false
_G.Collect        = false

-- Cria ScreenGui
gui = Instance.new("ScreenGui")
gui.Name   = "AutoFarmGui"
gui.Parent = game.CoreGui

-- Cria frame principal (draggable, transparente)
local frame = Instance.new("Frame")
frame.Name                 = "MainFrame"
frame.Size                 = UDim2.new(0, 240, 0, 260)
frame.Position             = UDim2.new(0.1, 0, 0.1, 0)
frame.BackgroundColor3     = Color3.fromRGB(30,30,30)
frame.BackgroundTransparency = 0.3
frame.BorderSizePixel      = 0
frame.Active               = true
frame.Draggable            = true
frame.Parent               = gui

-- Guardar tamanho e posição originais para minimizar/restaurar
local origSize, origPos = frame.Size, frame.Position
local minimized = false

-- Botão fechar (X)
local closeBtn = Instance.new("TextButton")
closeBtn.Name             = "CloseBtn"
closeBtn.Size             = UDim2.new(0, 24, 0, 24)
closeBtn.Position         = UDim2.new(1, -26, 0, 2)
closeBtn.Text             = "X"
closeBtn.TextSize         = 18
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
closeBtn.TextColor3       = Color3.fromRGB(255,255,255)
closeBtn.Parent           = frame
closeBtn.MouseButton1Up:Connect(function()
    gui:Destroy()
end)

-- Botão minimizar (- / +)
local minBtn = Instance.new("TextButton")
minBtn.Name             = "MinimizeBtn"
minBtn.Size             = UDim2.new(0, 24, 0, 24)
minBtn.Position         = UDim2.new(0, 2, 0, 2)
minBtn.Text             = "-"
minBtn.TextSize         = 18
minBtn.BackgroundColor3 = Color3.fromRGB(100,100,100)
minBtn.TextColor3       = Color3.fromRGB(255,255,255)
minBtn.Parent           = frame

minBtn.MouseButton1Up:Connect(function()
    if not minimized then
        -- minimizar
        origSize, origPos = frame.Size, frame.Position
        for _, child in ipairs(frame:GetChildren()) do
            if child ~= closeBtn and child ~= minBtn then child.Visible = false end
        end
        frame.Size = UDim2.new(0, 240, 0, 40)
        minBtn.Text = "+"
        minimized = true
    else
        -- restaurar
        frame.Size = origSize
        frame.Position = origPos
        for _, child in ipairs(frame:GetChildren()) do
            child.Visible = true
        end
        minBtn.Text = "-"
        minimized = false
    end
end)

-- Função auxiliar para criar botões
local function createButton(label, y)
    local btn = Instance.new("TextButton")
    btn.Size             = UDim2.new(0, 200, 0, 40)
    btn.Position         = UDim2.new(0, 20, 0, y)
    btn.Text             = label
    btn.TextSize         = 18
    btn.TextColor3       = Color3.fromRGB(255,255,255)
    btn.BackgroundColor3 = Color3.fromRGB(50,50,50)
    btn.Parent           = frame
    return btn
end

-- Cria os botões com posições iniciais
local btnTeleportChests = createButton("Teleport to Chests (Off)", 30)
local btnTeleportCoins  = createButton("Teleport to Coins (Off)", 80)
local btnTeleportPellet = createButton("Teleport to Pellet (Off)", 130)
local btnAutoCollect    = createButton("Auto Collect (Off)", 180)

-- Lógica Teleport Chests
btnTeleportChests.MouseButton1Up:Connect(function()
    _G.TeleportChests = not _G.TeleportChests
    btnTeleportChests.BackgroundColor3 = _G.TeleportChests and Color3.fromRGB(0,200,0) or Color3.fromRGB(200,0,0)
    btnTeleportChests.Text = _G.TeleportChests and "Teleport to Chests (On)" or "Teleport to Chests (Off)"
    if _G.TeleportChests then
        task.spawn(function()
            while _G.TeleportChests do
                for _, v in ipairs(workspace.chests:GetChildren()) do
                    if not _G.TeleportChests then break end
                    if v:IsA("BasePart") then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                        wait(0.2)
                    end
                end
                wait(0.5)
            end
        end)
    end
end)

-- Lógica Teleport Coins
btnTeleportCoins.MouseButton1Up:Connect(function()
    _G.TeleportCoins = not _G.TeleportCoins
    btnTeleportCoins.BackgroundColor3 = _G.TeleportCoins and Color3.fromRGB(0,200,0) or Color3.fromRGB(200,0,0)
    btnTeleportCoins.Text = _G.TeleportCoins and "Teleport to Coins (On)" or "Teleport to Coins (Off)"
    if _G.TeleportCoins then
        task.spawn(function()
            while _G.TeleportCoins do
                local coins = {}
                for _, v in ipairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") and (v.Name == "Coin1" or v.Name == "Coin2" or v.Name == "Coin4") then
                        table.insert(coins, v)
                    end
                end
                for _, v in ipairs(coins) do
                    if not _G.TeleportCoins then break end
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0,3,0)
                    wait(0.2)
                end
                wait(0.5)
            end
        end)
    end
end)

-- Lógica Teleport Pellet
btnTeleportPellet.MouseButton1Up:Connect(function()
    _G.TeleportPellet = not _G.TeleportPellet
    btnTeleportPellet.BackgroundColor3 = _G.TeleportPellet and Color3.fromRGB(0,200,0) or Color3.fromRGB(200,0,0)
    btnTeleportPellet.Text = _G.TeleportPellet and "Teleport to Pellet (On)" or "Teleport to Pellet (Off)"
    if _G.TeleportPellet then
        task.spawn(function()
            while _G.TeleportPellet do
                local pellets = {}
                for _, v in ipairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") and v.Name == "WhiteSpiritPellet" then
                        table.insert(pellets, v)
                    end
                end
                for _, v in ipairs(pellets) do
                    if not _G.TeleportPellet then break end
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0,3,0)
                    wait(0.2)
                end
                wait(0.5)
            end
        end)
    end
end)

-- Lógica Auto Collect
btnAutoCollect.MouseButton1Up:Connect(function()
    _G.Collect = not _G.Collect
    btnAutoCollect.BackgroundColor3 = _G.Collect and Color3.fromRGB(0,200,0) or Color3.fromRGB(200,0,0)
    btnAutoCollect.Text = _G.Collect and "Auto Collect (On)" or "Auto Collect (Off)"
    if _G.Collect then
        task.spawn(function()
            while _G.Collect do
                for _, v in ipairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") then
                        local prom = v:FindFirstChildOfClass("ProximityPrompt")
                        if prom then fireproximityprompt(prom) end
                    end
                end
                wait(0.1)
            end
        end)
    end
end)
