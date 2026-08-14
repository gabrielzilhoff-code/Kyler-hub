    -- =========================================================
-- ⚡ KYLER HUB 3.1 (COMPLETO E CORRIGIDO)
-- 🔥 FLING SOFÁ FUNCIONANDO NO BROOKHAVEN
-- 💕 Feito com amor pro meu baby
-- =========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local MarketplaceService = game:GetService("MarketplaceService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("KylerHubGui") then
    PlayerGui.KylerHubGui:Destroy()
end

-- =========================================================
-- 1. MENU MENOR (420x500)
-- =========================================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KylerHubGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 420, 0, 500)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -250)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 22)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

-- Barra de Título (menor)
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 35)
TitleBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 12)
TitleCorner.Parent = TitleBar

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, -80, 1, 0)
TitleText.Position = UDim2.new(0, 10, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "⚡ KYLER HUB V3"
TitleText.TextColor3 = Color3.fromRGB(170, 85, 255)
TitleText.TextSize = 16
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Parent = TitleBar

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
MinimizeButton.Position = UDim2.new(1, -60, 0, 5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.Text = "➖"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 14
MinimizeButton.Parent = TitleBar

MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -32, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14
CloseButton.Parent = TitleBar

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Scroll (mais compacto)
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Name = "ScrollFrame"
ScrollFrame.Size = UDim2.new(1, -16, 1, -45)
ScrollFrame.Position = UDim2.new(0, 8, 0, 40)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.BorderSizePixel = 0
ScrollFrame.ScrollBarThickness = 5
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 2600)
ScrollFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ScrollFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 4)

-- =========================================================
-- 2. COMPONENTES COMPACTOS
-- =========================================================
local function CreateToggleButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -8, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    button.Text = text .. " [OFF]"
    button.TextColor3 = Color3.fromRGB(200, 200, 200)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 13
    button.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = button

    local state = false
    button.MouseButton1Click:Connect(function()
        state = not state
        button.Text = state and (text .. " [ON]") or (text .. " [OFF]")
        button.TextColor3 = state and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(200, 200, 200)
        button.BackgroundColor3 = state and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(30, 30, 35)
        callback(state)
    end)
    return button
end

local function CreateSlider(text, min, max, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 18)
    label.BackgroundTransparency = 1
    label.Text = text .. ": " .. tostring(default)
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 12
    label.Parent = frame

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -16, 0, 10)
    button.Position = UDim2.new(0, 8, 0, 22)
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.Text = ""
    button.Parent = frame

    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(170, 85, 255)
    fill.BorderSizePixel = 0
    fill.Parent = button

    local dragging = false
    
    local function update(input)
        if not input or not button or not button.AbsoluteSize then return end
        local pos = math.clamp((input.Position.X - button.AbsolutePosition.X) / button.AbsoluteSize.X, 0, 1)
        fill.Size = UDim2.new(pos, 0, 1, 0)
        local value = math.floor(min + (max - min) * pos)
        label.Text = text .. ": " .. tostring(value)
        callback(value)
    end

    button.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then 
            dragging = true 
            update(input) 
        end
    end)
    
    button.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then 
            dragging = false 
        end
    end)
    
    local changedConnection
    changedConnection = UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then 
            update(input) 
        end
    end)
    
    frame.AncestryChanged:Connect(function()
        if not frame.Parent then
            if changedConnection then changedConnection:Disconnect() end
        end
    end)
end

local function CreateSectionHeader(text)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -8, 0, 22)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(180, 120, 255)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 14
    label.Parent = ScrollFrame
end

local function CreateActionButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -8, 0, 28)
    button.BackgroundColor3 = Color3.fromRGB(40, 35, 45)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(240, 240, 240)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 12
    button.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = button

    button.MouseButton1Click:Connect(callback)
    return button
end

local function CreateTextBox(text, placeholder, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 32)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = ScrollFrame
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.4, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 12
    label.Parent = frame
    
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0.55, -8, 0.8, 0)
    box.Position = UDim2.new(0.4, 0, 0.1, 0)
    box.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
    box.TextColor3 = Color3.fromRGB(255, 255, 255)
    box.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
    box.PlaceholderText = placeholder
    box.Font = Enum.Font.SourceSans
    box.TextSize = 12
    box.Parent = frame
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 6)
    boxCorner.Parent = box
    
    box.FocusLost:Connect(function()
        callback(box.Text)
    end)
    return box
end

-- =========================================================
-- 3. CAMPO DE SELEÇÃO DE JOGADOR
-- =========================================================
CreateSectionHeader("🎯 ALVO")

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, -8, 0, 30)
TargetInput.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TargetInput.PlaceholderText = "Digite o nome do alvo..."
TargetInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetInput.Font = Enum.Font.SourceSans
TargetInput.TextSize = 13
TargetInput.Parent = ScrollFrame

local TargetCorner = Instance.new("UICorner")
TargetCorner.CornerRadius = UDim.new(0, 6)
TargetCorner.Parent = TargetInput

local function GetTargetPlayer()
    local name = TargetInput.Text:lower()
    if name == "" then return nil end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and (p.Name:lower():sub(1, #name) == name or p.DisplayName:lower():sub(1, #name) == name) then
            return p
        end
    end
    return nil
end

-- =========================================================
-- 4. STATUS (COMPACTO)
-- =========================================================
CreateSectionHeader("📊 STATUS")

local startTime = tick()
local playTimeLabel = Instance.new("TextLabel")
playTimeLabel.Size = UDim2.new(1, -8, 0, 20)
playTimeLabel.BackgroundTransparency = 1
playTimeLabel.Text = "⏱️ 00:00"
playTimeLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playTimeLabel.Font = Enum.Font.SourceSans
playTimeLabel.TextSize = 12
playTimeLabel.Parent = ScrollFrame

task.spawn(function()
    while true do
        task.wait(1)
        local elapsed = math.floor(tick() - startTime)
        local minutes = math.floor(elapsed / 60)
        local seconds = elapsed % 60
        playTimeLabel.Text = string.format("⏱️ %02d:%02d", minutes, seconds)
    end
end)

local playersOnlineLabel = Instance.new("TextLabel")
playersOnlineLabel.Size = UDim2.new(1, -8, 0, 20)
playersOnlineLabel.BackgroundTransparency = 1
playersOnlineLabel.Text = "👥 Jogadores: 0/" .. game.Players.MaxPlayers
playersOnlineLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playersOnlineLabel.Font = Enum.Font.SourceSans
playersOnlineLabel.TextSize = 12
playersOnlineLabel.Parent = ScrollFrame

task.spawn(function()
    while true do
        task.wait(1)
        playersOnlineLabel.Text = "👥 Jogadores: " .. #Players:GetPlayers() .. "/" .. game.Players.MaxPlayers
    end
end)

local executorName = identifyexecutor and identifyexecutor() or "Desconhecido"
local executorLabel = Instance.new("TextLabel")
executorLabel.Size = UDim2.new(1, -8, 0, 20)
executorLabel.BackgroundTransparency = 1
executorLabel.Text = "⚡ Executor: " .. executorName
executorLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
executorLabel.Font = Enum.Font.SourceSans
executorLabel.TextSize = 12
executorLabel.Parent = ScrollFrame

CreateActionButton("🔄 Reconectar", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId)
end)

CreateActionButton("👥 Server Cheio", function()
    local PlaceId = game.PlaceId
    local Api = "https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Desc&limit=100"
    
    local function FindServer()
        local cursor = nil
        repeat
            local url = Api .. (cursor and "&cursor="..cursor or "")
            local data = HttpService:JSONDecode(game:HttpGet(url))
            for _, server in pairs(data.data) do
                if server.playing < server.maxPlayers and server.playing >= 21 then
                    return server.id
                end
            end
            cursor = data.nextPageCursor
        until not cursor
    end
    
    local serverId = FindServer()
    if serverId then
        TeleportService:TeleportToPlaceInstance(PlaceId, serverId, LocalPlayer)
    end
end)

CreateActionButton("🔄 Mudar Server", function()
    local _place = game.PlaceId
    local _servers = "https://games.roblox.com/v1/games/".._place.."/servers/Public?sortOrder=Asc&limit=100"
    
    function ListServers(cursor)
        local Raw = game:HttpGet(_servers .. ((cursor and "&cursor="..cursor) or ""))
        return HttpService:JSONDecode(Raw)
    end
    
    local Server, Next
    repeat
        local Servers = ListServers(Next)
        Server = Servers.data[1]
        Next = Servers.nextPageCursor
    until Server
    
    TeleportService:TeleportToPlaceInstance(_place, Server.id, LocalPlayer)
end)

-- =========================================================
-- 5. JOGADOR (COMPACTO)
-- =========================================================
CreateSectionHeader("🏃 JOGADOR")

CreateActionButton("🕊️ Fly", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/QjjQvMsE"))()
end)

local noclipActive = false
local noclipConnection = nil

CreateToggleButton("🚪 Noclip", function(state)
    noclipActive = state
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
    if noclipActive then
        noclipConnection = RunService.Stepped:Connect(function()
            if not noclipActive or not LocalPlayer.Character then return end
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end)
    end
end)

local currentWalkSpeed = 16
CreateSlider("Velocidade", 1, 200, 16, function(val)
    currentWalkSpeed = val
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

local currentJumpPower = 50
CreateSlider("Pulo", 1, 500, 50, function(val)
    currentJumpPower = val
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.UseJumpPower = true
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

local currentGravity = 196.2
CreateSlider("Gravidade", 0, 500, 196, function(val)
    currentGravity = val
    Workspace.Gravity = val
end)

-- =========================================================
-- 6. ESP (COMPACTO)
-- =========================================================
CreateSectionHeader("👁️ ESP")

local espActive = false
local espConnection = nil

CreateToggleButton("👤 ESP Nome", function(state)
    espActive = state
    if espConnection then
        espConnection:Disconnect()
        espConnection = nil
    end
    if espActive then
        espConnection = RunService.Heartbeat:Connect(function()
            local localChar = LocalPlayer.Character
            if not localChar or not localChar:FindFirstChild("Head") then return end
            
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                    local head = player.Character.Head
                    local distance = (localChar.Head.Position - head.Position).Magnitude
                    
                    local billboardGui = head:FindFirstChild("KylerESP")
                    if not billboardGui then
                        billboardGui = Instance.new("BillboardGui")
                        billboardGui.Name = "KylerESP"
                        billboardGui.Adornee = head
                        billboardGui.Size = UDim2.new(0, 100, 0, 40)
                        billboardGui.StudsOffset = Vector3.new(0, 2, 0)
                        billboardGui.AlwaysOnTop = true
                        billboardGui.Parent = head
                        
                        local textLabel = Instance.new("TextLabel")
                        textLabel.Name = "NameLabel"
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                        textLabel.BackgroundTransparency = 1
                        textLabel.TextColor3 = Color3.fromRGB(170, 85, 255)
                        textLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
                        textLabel.TextStrokeTransparency = 0
                        textLabel.TextScaled = true
                        textLabel.Text = player.Name .. "\n" .. math.floor(distance) .. "m"
                        textLabel.Parent = billboardGui
                    else
                        local label = billboardGui:FindFirstChild("NameLabel")
                        if label then
                            label.Text = player.Name .. "\n" .. math.floor(distance) .. "m"
                        end
                    end
                end
            end
        end)
    else
        for _, player in ipairs(Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Head") then
                local tag = player.Character.Head:FindFirstChild("KylerESP")
                if tag then tag:Destroy() end
            end
        end
    end
end)

local espHighlightActive = false
local highlightStorage = nil
local highlightConnections = {}

CreateToggleButton("✨ ESP Holograma", function(state)
    espHighlightActive = state
    if not espHighlightActive and highlightStorage then
        highlightStorage:Destroy()
        highlightStorage = nil
        for _, conn in pairs(highlightConnections) do
            if conn and conn.Disconnect then conn:Disconnect() end
        end
        highlightConnections = {}
        return
    end
    
    if espHighlightActive then
        highlightStorage = Instance.new("Folder")
        highlightStorage.Name = "KylerHighlightStorage"
        highlightStorage.Parent = CoreGui
        
        local function HighlightPlayer(plr)
            if plr == LocalPlayer then return end
            local highlight = Instance.new("Highlight")
            highlight.Name = plr.Name
            highlight.FillColor = Color3.fromRGB(170, 85, 255)
            highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
            highlight.FillTransparency = 0.5
            highlight.OutlineColor = Color3.fromRGB(0, 0, 0)
            highlight.OutlineTransparency = 0
            highlight.Parent = highlightStorage
            
            if plr.Character then
                highlight.Adornee = plr.Character
            end
            
            highlightConnections[plr] = plr.CharacterAdded:Connect(function(char)
                highlight.Adornee = char
            end)
        end
        
        highlightConnections["_PlayerAdded"] = Players.PlayerAdded:Connect(HighlightPlayer)
        for _, v in ipairs(Players:GetPlayers()) do
            HighlightPlayer(v)
        end
    end
end)

-- =========================================================
-- 7. AVATAR RGB (COMPACTO)
-- =========================================================
CreateSectionHeader("🎨 RGB")

CreateToggleButton("🌈 Nome RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {
                Color3.fromRGB(255,0,0), Color3.fromRGB(255,102,0), Color3.fromRGB(255,255,0),
                Color3.fromRGB(0,255,0), Color3.fromRGB(0,255,255), Color3.fromRGB(0,102,255),
                Color3.fromRGB(153,0,255), Color3.fromRGB(255,0,255)
            }
            local i = 1
            while state do
                if not state then return end
                pcall(function()
                    local remote = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1RPNam1eColo1r")
                    if remote then
                        remote:FireServer("PickingRPNameColor", colors[i])
                    end
                end)
                i = i % #colors + 1
                task.wait(0.3)
            end
        end)
    end
end)

CreateToggleButton("🌈 Corpo RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {"Bright red", "Lime green", "Bright blue", "Bright yellow", "Bright cyan", "Hot pink"}
            local i = 1
            while state do
                if not state then return end
                pcall(function()
                    local remote = ReplicatedStorage:FindFirstChild("Remotes") and ReplicatedStorage.Remotes:FindFirstChild("ChangeBodyColor")
                    if remote then
                        remote:FireServer(colors[i])
                    end
                end)
                i = i % #colors + 1
                task.wait(0.5)
            end
        end)
    end
end)

local copyTarget = nil

CreateTextBox("Alvo para copiar", "Digite o nome", function(value)
    copyTarget = value
end)

CreateActionButton("📋 Copiar Avatar", function()
    if not copyTarget or copyTarget == "" then
        print("❌ Digite um nome de jogador!")
        return
    end
    local TPlayer = Players:FindFirstChild(copyTarget)
    if not TPlayer or not TPlayer.Character then
        print("❌ Jogador não encontrado!")
        return
    end
    
    local LChar = LocalPlayer.Character
    if not LChar then return end
    
    local LHumanoid = LChar:FindFirstChildOfClass("Humanoid")
    local THumanoid = TPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not (LHumanoid and THumanoid) then return end
    
    local Remotes = ReplicatedStorage:WaitForChild("Remotes")
    local PDesc = THumanoid:GetAppliedDescription()
    
    local argsBody = {
        [1] = {
            [1] = PDesc.Torso,
            [2] = PDesc.RightArm,
            [3] = PDesc.LeftArm,
            [4] = PDesc.RightLeg,
            [5] = PDesc.LeftLeg,
            [6] = PDesc.Head
        }
    }
    pcall(function()
        Remotes.ChangeCharacterBody:InvokeServer(unpack(argsBody))
    end)
    task.wait(0.5)
    
    if tonumber(PDesc.Shirt) then
        Remotes.Wear:InvokeServer(tonumber(PDesc.Shirt))
        task.wait(0.3)
    end
    if tonumber(PDesc.Pants) then
        Remotes.Wear:InvokeServer(tonumber(PDesc.Pants))
        task.wait(0.3)
    end
    if tonumber(PDesc.Face) then
        Remotes.Wear:InvokeServer(tonumber(PDesc.Face))
        task.wait(0.3)
    end
    
    for _, acc in ipairs(PDesc:GetAccessories(true)) do
        if acc.AssetId and tonumber(acc.AssetId) then
            Remotes.Wear:InvokeServer(tonumber(acc.AssetId))
            task.wait(0.3)
        end
    end
    
    print("✅ Avatar copiado de " .. TPlayer.Name)
end)

-- =========================================================
-- 8. FLING (CORRIGIDO - SOFÁ FUNCIONA NO BROOKHAVEN)
-- =========================================================
CreateSectionHeader("🔥 FLING")

local selectedPlayer = nil

CreateTextBox("Alvo do Fling", "Digite o nome", function(value)
    selectedPlayer = value
end)

-- FLING BOLA
CreateActionButton("⚽ Fling Bola", function()
    if not selectedPlayer then
        print("❌ Digite um alvo!")
        return
    end
    
    local target = Players:FindFirstChild(selectedPlayer)
    if not target or not target.Character then
        print("❌ Alvo não encontrado!")
        return
    end
    
    local targetHRP = target.Character:FindFirstChild("HumanoidRootPart")
    if not targetHRP then
        print("❌ Alvo sem HumanoidRootPart!")
        return
    end
    
    local myHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not myHRP then
        print("❌ Você sem HumanoidRootPart!")
        return
    end
    
    local originalPos = myHRP.CFrame
    
    local ball = Instance.new("Part")
    ball.Size = Vector3.new(2, 2, 2)
    ball.Shape = Enum.PartType.Ball
    ball.Material = Enum.Material.SmoothPlastic
    ball.BrickColor = BrickColor.new("Bright red")
    ball.Anchored = false
    ball.CanCollide = true
    ball.Transparency = 0.3
    ball.Parent = Workspace
    
    ball.CFrame = targetHRP.CFrame * CFrame.new(0, 2, 0)
    
    for i = 1, 30 do
        if not target.Character or not targetHRP.Parent then break end
        
        local randomDir = Vector3.new(
            math.random(-5000, 5000),
            math.random(1000, 10000),
            math.random(-5000, 5000)
        )
        
        targetHRP.AssemblyLinearVelocity = randomDir
        targetHRP.AssemblyAngularVelocity = Vector3.new(
            math.random(-1000, 1000),
            math.random(-1000, 1000),
            math.random(-1000, 1000)
        )
        
        ball.CFrame = targetHRP.CFrame * CFrame.new(0, 2, 0)
        ball.AssemblyLinearVelocity = randomDir
        
        if i % 5 == 0 then
            local humanoid = target.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.Health = humanoid.Health - 10
            end
        end
        
        task.wait()
    end
    
    ball:Destroy()
    print("✅ Fling Bola em " .. target.Name)
end)

-- 🔥 FLING SOFA - CORRIGIDO PARA BROOKHAVEN
CreateActionButton("🛋️ Fling Sofá", function()
    if not selectedPlayer then
        print("❌ Digite um alvo!")
        return
    end
    
    local target = Players:FindFirstChild(selectedPlayer)
    if not target or not target.Character then
        print("❌ Alvo não encontrado!")
        return
    end
    
    local char = LocalPlayer.Character
    if not char then
        print("❌ Você sem personagem!")
        return
    end
    
    local hum = char:FindFirstChildOfClass("Humanoid")
    local root = char:FindFirstChild("HumanoidRootPart")
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    
    if not hum or not root or not tRoot then
        print("❌ Componentes necessários não encontrados!")
        return
    end
    
    -- LIMPA FERRAMENTAS
    pcall(function()
        ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools")
    end)
    task.wait(0.2)
    
    -- PEGA O SOFÁ
    pcall(function()
        ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
    end)
    task.wait(0.3)
    
    -- EQUIPA O SOFÁ
    local tool = LocalPlayer.Backpack:FindFirstChild("Couch")
    if tool then
        tool.Parent = char
        print("✅ Sofá equipado!")
    else
        print("❌ Sofá não encontrado!")
        return
    end
    task.wait(0.1)
    
    -- SIMULA SENTAR
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
    task.wait(0.1)
    
    local originalPos = root.CFrame
    local sitPos = Vector3.new(145.51, -350.09, 21.58)
    
    hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
    hum.PlatformStand = false
    Workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChild("Head") or tRoot or hum
    
    local align = Instance.new("BodyPosition")
    align.Name = "BringPosition"
    align.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    align.D = 10
    align.P = 30000
    align.Position = root.Position
    align.Parent = tRoot
    
    task.spawn(function()
        local angle = 0
        local startTime = tick()
        while tick() - startTime < 5 and target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
            local tHum = target.Character:FindFirstChildOfClass("Humanoid")
            if not tHum or tHum.Sit then break end
            
            local hrp = target.Character.HumanoidRootPart
            local adjustedPos = hrp.Position + (hrp.Velocity / 1.5)
            
            angle = angle + 50
            root.CFrame = CFrame.new(adjustedPos + Vector3.new(0, 2, 0)) * CFrame.Angles(math.rad(angle), 0, 0)
            align.Position = root.Position + Vector3.new(2, 0, 0)
            task.wait()
        end
        
        align:Destroy()
        hum:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
        hum.PlatformStand = false
        Workspace.CurrentCamera.CameraSubject = hum
        
        for _, p in pairs(char:GetDescendants()) do
            if p:IsA("BasePart") then
                p.Velocity = Vector3.zero
                p.RotVelocity = Vector3.zero
            end
        end
        
        task.wait(0.1)
        root.CFrame = CFrame.new(sitPos)
        task.wait(0.3)
        
        -- DEVOLVE O SOFÁ
        local tool = char:FindFirstChild("Couch")
        if tool then
            tool.Parent = LocalPlayer.Backpack
        end
        
        task.wait(0.01)
        pcall(function()
            ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
        end)
        task.wait(0.2)
        root.CFrame = originalPos
        
        print("✅ Fling Sofá em " .. target.Name)
    end)
end)

-- FLING CARRO
CreateActionButton("🚗 Fling Carro", function()
    if not selectedPlayer then
        print("❌ Digite um alvo!")
        return
    end
    
    local target = Players:FindFirstChild(selectedPlayer)
    if not target or not target.Character then
        print("❌ Alvo não encontrado!")
        return
    end
    
    local targetHRP = target.Character:FindFirstChild("HumanoidRootPart")
    if not targetHRP then
        print("❌ Alvo sem HumanoidRootPart!")
        return
    end
    
    local vehicle = nil
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("VehicleSeat") and v.Parent then
            local dist = (v.Position - targetHRP.Position).Magnitude
            if dist < 50 then
                vehicle = v.Parent
                break
            end
        end
    end
    
    if not vehicle then
        print("❌ Nenhum veículo próximo!")
        return
    end
    
    local primaryPart = vehicle.PrimaryPart or vehicle:FindFirstChild("Body") or vehicle:FindFirstChildOfClass("BasePart")
    if not primaryPart then
        print("❌ Veículo sem parte primária!")
        return
    end
    
    for i = 1, 30 do
        if not target.Character or not targetHRP.Parent then break end
        
        local randomDir = Vector3.new(
            math.random(-5000, 5000),
            math.random(1000, 10000),
            math.random(-5000, 5000)
        )
        
        vehicle:SetPrimaryPartCFrame(targetHRP.CFrame * CFrame.new(0, 2, 0))
        primaryPart.AssemblyLinearVelocity = randomDir
        targetHRP.AssemblyLinearVelocity = randomDir
        
        task.wait()
    end
    
    print("✅ Fling Carro em " .. target.Name)
end)

CreateActionButton("💀 Fling Todos", function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character then
            local targetHRP = player.Character:FindFirstChild("HumanoidRootPart")
            if targetHRP then
                targetHRP.AssemblyLinearVelocity = Vector3.new(
                    math.random(-10000, 10000),
                    math.random(5000, 15000),
                    math.random(-10000, 10000)
                )
            end
            task.wait(0.2)
        end
    end
    print("✅ Fling em todos!")
end)

-- =========================================================
-- 9. TELEPORT (COMPACTO)
-- =========================================================
CreateSectionHeader("📍 TELEPORT")

local teleportPoints = {
    {"🏠 Spawn", CFrame.new(-26, 4, -23)},
    {"🏥 Hospital", CFrame.new(-309, 4, 71)},
    {"🏫 Escola", CFrame.new(-312, 4, 211)},
    {"🏦 Banco", CFrame.new(2.28, 4.65, 254.58)},
    {"🛍️ Loja", CFrame.new(-46.15, 4.65, 253.20)},
    {"🏝️ Ilha", CFrame.new(-1925, 23, 127)},
}

for _, tp in ipairs(teleportPoints) do
    CreateActionButton(tp[1], function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tp[2]
        end
    end)
end

-- =========================================================
-- 10. ÁUDIO (COMPACTO)
-- =========================================================
CreateSectionHeader("🎵 ÁUDIO")

local selectedAudioID = nil

CreateTextBox("ID do Áudio", "Digite o ID", function(value)
    local id = tonumber(value)
    if id then selectedAudioID = id end
end)

CreateActionButton("▶️ Tocar Áudio", function()
    if not selectedAudioID then
        print("❌ Selecione um áudio!")
        return
    end
    
    pcall(function()
        local args = { Workspace, selectedAudioID, 1 }
        ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(unpack(args))
    end)
    
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://" .. selectedAudioID
    sound.Parent = LocalPlayer.Character:FindFirstChild("HumanoidRootPart") or Workspace
    sound:Play()
    task.wait(sound.TimeLength or 3)
    sound:Destroy()
    print("✅ Áudio tocado!")
end)

CreateToggleButton("🔄 Loop Áudio", function(state)
    if state then
        task.spawn(function()
            while state do
                if selectedAudioID then
                    pcall(function()
                        local args = { Workspace, selectedAudioID, 1 }
                        ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(unpack(args))
                    end)
                    local sound = Instance.new("Sound")
                    sound.SoundId = "rbxassetid://" .. selectedAudioID
                    sound.Parent = LocalPlayer.Character:FindFirstChild("HumanoidRootPart") or Workspace
                    sound:Play()
                    task.wait(0.5)
                    sound:Destroy()
                end
                task.wait(0.5)
            end
        end)
    end
end)

-- =========================================================
-- 11. VEÍCULO (COMPACTO)
-- =========================================================
CreateSectionHeader("🚗 VEÍCULO")

CreateActionButton("🚗 Fly Car", function()
    loadstring(game:HttpGet("https://pastefy.app/RtliHFjP/raw"))()
end)

CreateActionButton("💥 Remover Carros", function()
    local Vehicles = Workspace:FindFirstChild("Vehicles")
    if Vehicles then
        for _, v in pairs(Vehicles:GetChildren()) do
            v:Destroy()
        end
        print("✅ Carros removidos!")
    end
end)

CreateToggleButton("🌈 Carro RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {
                Color3.new(1, 0, 0), Color3.new(0, 1, 0), Color3.new(0, 0, 1),
                Color3.new(1, 1, 0), Color3.new(1, 0, 1), Color3.new(0, 1, 1),
                Color3.new(0.5, 0, 0.5), Color3.new(1, 0.5, 0)
            }
            while state do
                for _, color in ipairs(colors) do
                    if not state then return end
                    pcall(function()
                        local remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Player1sCa1r")
                        if remote then
                            remote:FireServer("PickingCarColor", color)
                        end
                    end)
                    task.wait(1)
                end
            end
        end)
    end
end)

-- =========================================================
-- 12. CASA (COMPACTO)
-- =========================================================
CreateSectionHeader("🏠 CASA")

CreateToggleButton("🌈 Casa RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {
                Color3.new(1, 0, 0), Color3.new(0, 1, 0), Color3.new(0, 0, 1),
                Color3.new(1, 1, 0), Color3.new(0, 1, 1), Color3.new(1, 0, 1)
            }
            while state do
                for _, color in ipairs(colors) do
                    if not state then return end
                    pcall(function()
                        local remote = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1Player1sHous1e")
                        if remote then
                            remote:FireServer("ColorPickHouse", color)
                        end
                    end)
                    task.wait(0.8)
                end
            end
        end)
    end
end)

CreateActionButton("🔓 Unban Casa", function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Part") and obj.Transparency > 0.5 and obj.Size.X > 20 then
            if tostring(obj.BrickColor):lower():find("red") then
                obj.CanCollide = false
                obj.Transparency = 1
            end
        end
    end
    
    local lots = Workspace:FindFirstChild("001_Lots")
    if lots then
        for _, v in pairs(lots:GetDescendants()) do
            if v.Name == "Permissions" and v:IsA("Folder") then
                local allow = v:FindFirstChild("Allow")
                if allow and allow:IsA("RemoteEvent") then
                    allow:FireServer(LocalPlayer)
                    print("✅ Unban enviado!")
                end
            end
        end
    end
end)

CreateActionButton("🏠 TP Casa", function()
    local lots = Workspace:FindFirstChild("001_Lots")
    if lots then
        for _, House in pairs(lots:GetChildren()) do
            if House.Name ~= "For Sale" and House:IsA("Model") then
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(House.WorldPivot.Position)
                    print("✅ TP para " .. House.Name)
                    return
                end
            end
        end
    end
    print("❌ Nenhuma casa!")
end)

-- =========================================================
-- 13. CONFIG (COMPACTO)
-- =========================================================
CreateSectionHeader("⚙️ CONFIG")

CreateTextBox("ID do Servidor", "Digite o ID", function(value)
    if value and value ~= "" then
        pcall(function()
            TeleportService:TeleportToPlaceInstance(game.PlaceId, value, LocalPlayer)
        end)
    end
end)

CreateActionButton("📋 Copiar ID", function()
    local serverId = tostring(game.JobId or "")
    if setclipboard then
        setclipboard(serverId)
        print("✅ ID copiado: " .. serverId)
    end
end)

CreateActionButton("🔒 Shift Lock", function()
    local shiftlockk = Instance.new("ScreenGui")
    local LockButton = Instance.new("ImageButton")
    local btnIcon = Instance.new("ImageLabel")
    
    shiftlockk.Name = "ShiftLockKyler"
    shiftlockk.Parent = CoreGui
    shiftlockk.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    shiftlockk.ResetOnSpawn = false
    
    LockButton.Size = UDim2.new(0, 50, 0, 50)
    LockButton.Position = UDim2.new(0.785, 0, 0.865, 0)
    LockButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    LockButton.BackgroundTransparency = 0.3
    LockButton.Parent = shiftlockk
    LockButton.Image = ""
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = LockButton
    
    btnIcon.Parent = LockButton
    btnIcon.Size = UDim2.new(0.7, 0, 0.7, 0)
    btnIcon.Position = UDim2.new(0.15, 0, 0.15, 0)
    btnIcon.BackgroundTransparency = 1
    btnIcon.Image = "rbxasset://textures/ui/mouseLock_off.png"
    btnIcon.ImageColor3 = Color3.fromRGB(255, 255, 255)
    btnIcon.ScaleType = Enum.ScaleType.Fit
    
    local active = false
    local GameSettings = UserSettings():GetService("UserGameSettings")
    local connection = nil
    
    LockButton.MouseButton1Click:Connect(function()
        active = not active
        if active then
            btnIcon.ImageColor3 = Color3.fromRGB(0, 170, 255)
            connection = RunService.RenderStepped:Connect(function()
                pcall(function()
                    GameSettings.RotationType = Enum.RotationType.CameraRelative
                end)
            end)
        else
            btnIcon.ImageColor3 = Color3.fromRGB(255, 255, 255)
            if connection then
                connection:Disconnect()
                connection = nil
            end
            pcall(function()
                GameSettings.RotationType = Enum.RotationType.MovementRelative
            end)
        end
    end)
    
    print("✅ Shift Lock!")
end)

-- =========================================================
-- 14. GRÁFICO LITE (COMPACTO)
-- =========================================================
CreateSectionHeader("🎮 GRÁFICO LITE")

CreateToggleButton("📉 Gráfico Lite", function(state)
    if state then
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 9e9
        Lighting.Brightness = 1
        Lighting.EnvironmentDiffuseScale = 0
        Lighting.EnvironmentSpecularScale = 0
        Lighting.Technology = Enum.Technology.Legacy
        
        for _, v in pairs(Lighting:GetDescendants()) do
            if v:IsA("PostEffect") or v:IsA("BloomEffect") or v:IsA("BlurEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("Sky") then
                pcall(function() v:Destroy() end)
            end
        end
        
        local function limpar(obj)
            if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") or obj:IsA("Fire") or obj:IsA("Explosion") then
                obj:Destroy()
            elseif obj:IsA("Accessory") or obj:IsA("Hat") or obj:IsA("Clothing") then
                obj:Destroy()
            elseif obj:IsA("BasePart") then
                obj.Material = Enum.Material.SmoothPlastic
                obj.CastShadow = false
                obj.Reflectance = 0
            end
        end
        
        for _, obj in pairs(Workspace:GetDescendants()) do
            limpar(obj)
        end
        
        for _, plr in pairs(Players:GetPlayers()) do
            if plr.Character then
                for _, child in pairs(plr.Character:GetChildren()) do
                    if child:IsA("Accessory") or child:IsA("Clothing") then
                        child:Destroy()
                    end
                end
            end
        end
        
        print("✅ Gráfico Lite!")
    end
end)

-- =========================================================
-- 15. HOTKEY
-- =========================================================
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

print("✅ KYLER HUB 3.1 CARREGADO! 🔥")
print("💡 RightShift para abrir/fechar")
print("💕 Feito com amor pro meu baby!")
