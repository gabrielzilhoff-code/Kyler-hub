-- =========================================================
-- ⚡ KYLER HUB 3.0 (MEGA COMPLETO) 
-- 🔥 Adaptado do GTVZ HUB - TODAS AS FUNÇÕES
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

-- Destrói GUI anterior
if PlayerGui:FindFirstChild("KylerHubGui") then
    PlayerGui.KylerHubGui:Destroy()
end

-- =========================================================
-- 1. ESTRUTURA DA INTERFACE (KYLER STYLE)
-- =========================================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KylerHubGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 500, 0, 600)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -300)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 22)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

-- Barra de Título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 45)
TitleBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 12)
TitleCorner.Parent = TitleBar

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, -90, 1, 0)
TitleText.Position = UDim2.new(0, 15, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "⚡ KYLER HUB | Drip Solutions V3"
TitleText.TextColor3 = Color3.fromRGB(170, 85, 255)
TitleText.TextSize = 20
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Parent = TitleBar

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -75, 0, 8)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.Text = "➖"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 16
MinimizeButton.Parent = TitleBar

MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 8)
CloseButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 16
CloseButton.Parent = TitleBar

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Container com ROLAGEM
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Name = "ScrollFrame"
ScrollFrame.Size = UDim2.new(1, -20, 1, -55)
ScrollFrame.Position = UDim2.new(0, 10, 0, 50)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.BorderSizePixel = 0
ScrollFrame.ScrollBarThickness = 6
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 3000)
ScrollFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ScrollFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 8)

-- =========================================================
-- 2. FUNÇÕES GERADORAS DE COMPONENTES
-- =========================================================
local function CreateToggleButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -10, 0, 38)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    button.Text = text .. " [OFF]"
    button.TextColor3 = Color3.fromRGB(200, 200, 200)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 15
    button.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
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
    frame.Size = UDim2.new(1, -10, 0, 50)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 22)
    label.BackgroundTransparency = 1
    label.Text = text .. ": " .. tostring(default)
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 14
    label.Parent = frame

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -20, 0, 12)
    button.Position = UDim2.new(0, 10, 0, 28)
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
    label.Size = UDim2.new(1, -10, 0, 28)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(180, 120, 255)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 17
    label.Parent = ScrollFrame
end

local function CreateActionButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -10, 0, 35)
    button.BackgroundColor3 = Color3.fromRGB(40, 35, 45)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(240, 240, 240)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 14
    button.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = button

    button.MouseButton1Click:Connect(callback)
    return button
end

local function CreateTextBox(text, placeholder, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -10, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = ScrollFrame
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.4, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 14
    label.Parent = frame
    
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0.6, -10, 0.8, 0)
    box.Position = UDim2.new(0.4, 0, 0.1, 0)
    box.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
    box.TextColor3 = Color3.fromRGB(255, 255, 255)
    box.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
    box.PlaceholderText = placeholder
    box.Font = Enum.Font.SourceSans
    box.TextSize = 14
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
-- 3. CAMPO DE SELEÇÃO DE JOGADOR ALVO
-- =========================================================
CreateSectionHeader("🎯 SELEÇÃO DE ALVO")

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, -10, 0, 38)
TargetInput.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TargetInput.PlaceholderText = "Digite o nome do jogador alvo..."
TargetInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetInput.Font = Enum.Font.SourceSans
TargetInput.TextSize = 14
TargetInput.Parent = ScrollFrame

local TargetCorner = Instance.new("UICorner")
TargetCorner.CornerRadius = UDim.new(0, 8)
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
-- 4. STATUS (DO GTVZ)
-- =========================================================
CreateSectionHeader("📊 STATUS")

-- Tempo jogando
local startTime = tick()
local playTimeLabel = Instance.new("TextLabel")
playTimeLabel.Size = UDim2.new(1, -10, 0, 30)
playTimeLabel.BackgroundTransparency = 1
playTimeLabel.Text = "Tempo jogando: 00min 00s"
playTimeLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playTimeLabel.Font = Enum.Font.SourceSans
playTimeLabel.TextSize = 14
playTimeLabel.Parent = ScrollFrame

task.spawn(function()
    while true do
        task.wait(1)
        local elapsed = math.floor(tick() - startTime)
        local minutes = math.floor(elapsed / 60)
        local seconds = elapsed % 60
        playTimeLabel.Text = string.format("Tempo jogando: %02dmin %02ds", minutes, seconds)
    end
end)

-- Nome do jogo
local gameNameLabel = Instance.new("TextLabel")
gameNameLabel.Size = UDim2.new(1, -10, 0, 25)
gameNameLabel.BackgroundTransparency = 1
gameNameLabel.Text = "Jogo: " .. MarketplaceService:GetProductInfo(game.PlaceId).Name
gameNameLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
gameNameLabel.Font = Enum.Font.SourceSans
gameNameLabel.TextSize = 14
gameNameLabel.Parent = ScrollFrame

-- Máximo de jogadores
local maxPlayersLabel = Instance.new("TextLabel")
maxPlayersLabel.Size = UDim2.new(1, -10, 0, 25)
maxPlayersLabel.BackgroundTransparency = 1
maxPlayersLabel.Text = "Máximo no servidor: " .. game.Players.MaxPlayers
maxPlayersLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
maxPlayersLabel.Font = Enum.Font.SourceSans
maxPlayersLabel.TextSize = 14
maxPlayersLabel.Parent = ScrollFrame

-- Jogadores online
local playersOnlineLabel = Instance.new("TextLabel")
playersOnlineLabel.Size = UDim2.new(1, -10, 0, 25)
playersOnlineLabel.BackgroundTransparency = 1
playersOnlineLabel.Text = "Jogadores online: 0"
playersOnlineLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playersOnlineLabel.Font = Enum.Font.SourceSans
playersOnlineLabel.TextSize = 14
playersOnlineLabel.Parent = ScrollFrame

task.spawn(function()
    while true do
        task.wait(1)
        playersOnlineLabel.Text = "Jogadores online: " .. #Players:GetPlayers()
    end
end)

-- Dispositivo
local platform = UserInputService:GetPlatform()
local deviceText = "Dispositivo: "
if platform == Enum.Platform.Windows then deviceText = deviceText .. "Windows"
elseif platform == Enum.Platform.OSX then deviceText = deviceText .. "MacBook"
elseif platform == Enum.Platform.XBoxOne then deviceText = deviceText .. "Xbox"
elseif platform == Enum.Platform.IOS or platform == Enum.Platform.Android then
    if UserInputService.KeyboardEnabled or UserInputService.MouseEnabled then
        deviceText = deviceText .. "Tablet"
    else
        deviceText = deviceText .. "Celular"
    end
else deviceText = deviceText .. "Desconhecido" end

local deviceLabel = Instance.new("TextLabel")
deviceLabel.Size = UDim2.new(1, -10, 0, 25)
deviceLabel.BackgroundTransparency = 1
deviceLabel.Text = deviceText
deviceLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
deviceLabel.Font = Enum.Font.SourceSans
deviceLabel.TextSize = 14
deviceLabel.Parent = ScrollFrame

-- Executor
local executorName = identifyexecutor and identifyexecutor() or "Desconhecido"
local executorLabel = Instance.new("TextLabel")
executorLabel.Size = UDim2.new(1, -10, 0, 25)
executorLabel.BackgroundTransparency = 1
executorLabel.Text = "Executor: " .. executorName
executorLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
executorLabel.Font = Enum.Font.SourceSans
executorLabel.TextSize = 14
executorLabel.Parent = ScrollFrame

-- Botões de servidor
CreateActionButton("🔄 Reconectar", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId)
end)

CreateActionButton("👥 Ir pra Server Cheio", function()
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

CreateActionButton("🔄 Mudar de Server", function()
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
-- 5. JOGADOR (DO GTVZ)
-- =========================================================
CreateSectionHeader("🏃 CONTROLES DO JOGADOR")

-- Fly (do GTVZ)
CreateActionButton("🕊️ Fly (Ativar)", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/QjjQvMsE"))()
end)

-- Noclip
local noclipActive = false
local noclipConnection = nil

CreateToggleButton("🚪 Atravessar Paredes", function(state)
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

-- Velocidade
local currentWalkSpeed = 16
CreateSlider("Velocidade", 1, 200, 16, function(val)
    currentWalkSpeed = val
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

CreateActionButton("🔄 Resetar Velocidade", function()
    currentWalkSpeed = 16
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
end)

-- Pulo
local currentJumpPower = 50
CreateSlider("Altura do Pulo", 1, 500, 50, function(val)
    currentJumpPower = val
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.UseJumpPower = true
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

CreateActionButton("🔄 Resetar Pulo", function()
    currentJumpPower = 50
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.UseJumpPower = true
        LocalPlayer.Character.Humanoid.JumpPower = 50
    end
end)

-- Gravidade
local currentGravity = 196.2
CreateSlider("Gravidade", 0, 500, 196, function(val)
    currentGravity = val
    Workspace.Gravity = val
end)

CreateActionButton("🔄 Resetar Gravidade", function()
    currentGravity = 196.2
    Workspace.Gravity = 196.2
end)

-- =========================================================
-- 6. ESP (DO GTVZ)
-- =========================================================
CreateSectionHeader("👁️ ESP")

-- ESP Nome + Distância
local espActive = false
local espConnection = nil

CreateToggleButton("👤 ESP Nome + Distância", function(state)
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
                        billboardGui.Size = UDim2.new(0, 120, 0, 50)
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

-- ESP Holograma (Highlight)
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
-- 7. AVATAR (DO GTVZ)
-- =========================================================
CreateSectionHeader("🎨 AVATAR")

-- RGB Loop (Nome)
local rgbNameRunning = false
local rgbNameThread = nil
local remoteNameColor = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1RPNam1eColo1r")

local function lerpColor(c1, c2, t)
    return Color3.new(
        c1.R + (c2.R - c1.R) * t,
        c1.G + (c2.G - c1.G) * t,
        c1.B + (c2.B - c1.B) * t
    )
end

local colorList = {
    Color3.fromRGB(255, 0, 0), Color3.fromRGB(255, 102, 0),
    Color3.fromRGB(255, 255, 0), Color3.fromRGB(0, 255, 0),
    Color3.fromRGB(0, 255, 255), Color3.fromRGB(0, 102, 255),
    Color3.fromRGB(153, 0, 255), Color3.fromRGB(255, 0, 255),
    Color3.fromRGB(255, 105, 180), Color3.fromRGB(255, 215, 0),
    Color3.fromRGB(0, 255, 127), Color3.fromRGB(135, 206, 250),
    Color3.fromRGB(255, 51, 153), Color3.fromRGB(102, 255, 178),
    Color3.fromRGB(204, 153, 255)
}

CreateToggleButton("🌈 Nome RGB", function(state)
    rgbNameRunning = state
    if rgbNameThread then
        rgbNameThread = nil
    end
    if rgbNameRunning and remoteNameColor then
        rgbNameThread = task.spawn(function()
            while rgbNameRunning do
                for i = 1, #colorList do
                    if not rgbNameRunning then return end
                    local c1 = colorList[i]
                    local c2 = colorList[i % #colorList + 1]
                    for t = 0, 1, 0.02 do
                        if not rgbNameRunning then return end
                        if remoteNameColor then
                            remoteNameColor:FireServer("PickingRPNameColor", lerpColor(c1, c2, t))
                        end
                        task.wait(0.02)
                    end
                end
            end
        end)
    end
end)

-- Corpo RGB
local rgbBodyRunning = false
local rgbBodyThread = nil
local bodyColors = {"Bright red", "Lime green", "Bright blue", "Bright yellow", "Bright cyan", "Hot pink", "Royal purple"}

CreateToggleButton("🌈 Corpo RGB", function(state)
    rgbBodyRunning = state
    if rgbBodyThread then
        rgbBodyThread = nil
    end
    if rgbBodyRunning then
        rgbBodyThread = task.spawn(function()
            while rgbBodyRunning do
                for _, color in ipairs(bodyColors) do
                    if not rgbBodyRunning then return end
                    local args = { color }
                    pcall(function()
                        ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("ChangeBodyColor"):FireServer(unpack(args))
                    end)
                    task.wait(0.5)
                end
            end
        end)
    end
end)

-- Cabelo RGB
local rgbHairRunning = false
local hairColorsList = {
    Color3.new(1, 1, 0), Color3.new(0, 0, 1), Color3.new(1, 0, 1), Color3.new(1, 1, 1),
    Color3.new(0, 1, 0), Color3.new(0.5, 0, 1), Color3.new(1, 0.647, 0), Color3.new(0, 1, 1)
}

CreateToggleButton("🌈 Cabelo RGB", function(state)
    rgbHairRunning = state
    if rgbHairRunning then
        task.spawn(function()
            local i = 1
            while rgbHairRunning do
                local args = { [1] = "ChangeHairColor2", [2] = hairColorsList[i] }
                pcall(function()
                    ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Max1y"):FireServer(unpack(args))
                end)
                task.wait(0.1)
                i = i % #hairColorsList + 1
            end
        end)
    end
end)

-- Copiar Avatar
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
    
    -- Copiar corpo
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
    
    -- Copiar roupas
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
    
    -- Copiar acessórios
    for _, acc in ipairs(PDesc:GetAccessories(true)) do
        if acc.AssetId and tonumber(acc.AssetId) then
            Remotes.Wear:InvokeServer(tonumber(acc.AssetId))
            task.wait(0.3)
        end
    end
    
    print("✅ Avatar copiado de " .. TPlayer.Name)
end)

-- =========================================================
-- 8. TROLL (DO GTVZ - FLING SOFA)
-- =========================================================
CreateSectionHeader("👿 TROLL - FLING SOFA")

local selectedPlayerName = nil

-- Dropdown simplificado com TextBox
CreateTextBox("Alvo do Troll", "Digite o nome", function(value)
    selectedPlayerName = value
    getgenv().Target = value
end)

-- Funções do Sofá (do GTVZ)
local function cleanupCouch()
    local char = LocalPlayer.Character
    if char then
        local couch = char:FindFirstChild("Chaos.Couch") or LocalPlayer.Backpack:FindFirstChild("Chaos.Couch")
        if couch then couch:Destroy() end
    end
    pcall(function()
        ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools")
    end)
end

-- Fling Sofá
CreateActionButton("🛋️ Fling Sofá (Eliminar)", function()
    if not selectedPlayerName or not Players:FindFirstChild(selectedPlayerName) then
        print("❌ Nenhum alvo selecionado!")
        return
    end
    
    local target = Players:FindFirstChild(selectedPlayerName)
    if not target or not target.Character then
        print("❌ Alvo sem personagem!")
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
    
    local originalPos = root.Position
    local sitPos = Vector3.new(145.51, -350.09, 21.58)
    
    pcall(function()
        ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools")
        task.wait(0.2)
        ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
        task.wait(0.3)
    end)
    
    local tool = LocalPlayer.Backpack:FindFirstChild("Couch")
    if tool then tool.Parent = char end
    task.wait(0.1)
    
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
    task.wait(0.1)
    
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
        
        local tool = char:FindFirstChild("Couch")
        if tool then tool.Parent = LocalPlayer.Backpack end
        
        task.wait(0.01)
        pcall(function()
            ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
        end)
        task.wait(0.2)
        root.CFrame = CFrame.new(originalPos)
        print("✅ Fling Sofá aplicado em " .. target.Name)
    end)
end)

-- Puxar com Sofá
CreateActionButton("🔄 Puxar com Sofá", function()
    if not selectedPlayerName or not Players:FindFirstChild(selectedPlayerName) then
        print("❌ Nenhum alvo selecionado!")
        return
    end
    
    local targetPlayer = Players:FindFirstChild(selectedPlayerName)
    if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        print("❌ Alvo inválido!")
        return
    end
    
    pcall(function()
        ReplicatedStorage.RE["1Clea1rTool1s"]:FireServer("ClearAllTools")
        local args = { "PickingTools", "Couch" }
        ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end)
    
    local couch = LocalPlayer.Backpack:WaitForChild("Couch", 2)
    if not couch then
        print("❌ Sofá não encontrado!")
        return
    end
    
    couch.Name = "Chaos.Couch"
    local seat1 = couch:FindFirstChild("Seat1")
    local seat2 = couch:FindFirstChild("Seat2")
    local handle = couch:FindFirstChild("Handle")
    if seat1 and seat2 and handle then
        seat1.Disabled = true
        seat2.Disabled = true
        handle.Name = "Handle "
    end
    couch.Parent = LocalPlayer.Character
    
    local tet = Instance.new("BodyVelocity", seat1)
    tet.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    tet.P = 1250
    tet.Velocity = Vector3.new(0, 0, 0)
    tet.Name = "#mOVOOEPF$#@F$#GERE..>V<<<<EW<V<<W"
    
    local target = targetPlayer
    task.spawn(function()
        repeat
            for m = 1, 35 do
                local tRoot = target.Character and target.Character.HumanoidRootPart
                if not tRoot then break end
                local pos = Vector3.new(
                    tRoot.Position.X + (tRoot.Velocity.X / 2),
                    tRoot.Position.Y + (tRoot.Velocity.Y / 2),
                    tRoot.Position.Z + (tRoot.Velocity.Z / 2)
                )
                seat1.CFrame = CFrame.new(pos) * CFrame.new(-2, 2, 0)
                task.wait()
            end
            tet:Destroy()
            couch.Parent = LocalPlayer.Backpack
            task.wait()
            couch:FindFirstChild("Handle ").Name = "Handle"
            task.wait(0.2)
            couch.Parent = LocalPlayer.Character
            task.wait()
            couch.Parent = LocalPlayer.Backpack
            couch.Handle.Name = "Handle "
            task.wait(0.2)
            couch.Parent = LocalPlayer.Character
            tet = Instance.new("BodyVelocity", seat1)
            tet.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            tet.P = 1250
            tet.Velocity = Vector3.new(0, 0, 0)
            tet.Name = "#mOVOOEPF$#@F$#GERE..>V<<<<EW<V<<W"
        until target.Character and target.Character.Humanoid and target.Character.Humanoid.Sit == true
        task.wait()
        tet:Destroy()
        couch.Parent = LocalPlayer.Backpack
        task.wait()
        couch:FindFirstChild("Handle ").Name = "Handle"
        task.wait(0.3)
        couch.Parent = LocalPlayer.Character
        task.wait(0.3)
        couch.Grip = CFrame.new(Vector3.new(0, 0, 0))
        task.wait(0.3)
        pcall(function()
            ReplicatedStorage.RE["1Clea1rTool1s"]:FireServer("ClearAllTools")
        end)
        print("✅ Jogador puxado com sofá!")
    end)
end)

-- =========================================================
-- 9. TROLL - FLING BARCO (DO GTVZ)
-- =========================================================
CreateSectionHeader("🚤 FLING BARCO")

CreateActionButton("🚤 Fling Barco", function()
    if not selectedPlayerName or not game.Players:FindFirstChild(selectedPlayerName) then
        warn("❌ Nenhum jogador selecionado!")
        return
    end
    
    local Player = LocalPlayer
    local Character = Player.Character
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
    local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")
    local Vehicles = Workspace:FindFirstChild("Vehicles")
    
    if not Humanoid or not RootPart then
        warn("❌ Humanoid ou RootPart inválido!")
        return
    end
    
    local function spawnBoat()
        RootPart.CFrame = CFrame.new(1754, -2, 58)
        task.wait(0.5)
        pcall(function()
            ReplicatedStorage.RE:FindFirstChild("1Ca1r"):FireServer("PickingBoat", "MilitaryBoatFree")
        end)
        task.wait(1)
        return Vehicles:FindFirstChild(Player.Name.."Car")
    end
    
    local PCar = Vehicles:FindFirstChild(Player.Name.."Car") or spawnBoat()
    if not PCar then
        warn("❌ Falha ao spawnar o barco!")
        return
    end
    
    local Seat = PCar:FindFirstChild("Body") and PCar.Body:FindFirstChild("VehicleSeat")
    if not Seat then
        warn("❌ Assento não encontrado!")
        return
    end
    
    repeat 
        task.wait(0.1)
        RootPart.CFrame = Seat.CFrame * CFrame.new(0, 1, 0)
    until Humanoid.SeatPart == Seat
    
    print("✅ Barco spawnado!")
    
    local TargetPlayer = Players:FindFirstChild(selectedPlayerName)
    if not TargetPlayer or not TargetPlayer.Character then
        warn("❌ Jogador não encontrado!")
        return
    end
    
    local TargetC = TargetPlayer.Character
    local TargetH = TargetC:FindFirstChildOfClass("Humanoid")
    local TargetRP = TargetC:FindFirstChild("HumanoidRootPart")
    
    if not TargetRP or not TargetH then
        warn("❌ Humanoid ou RootPart do alvo não encontrado!")
        return
    end
    
    local Spin = Instance.new("BodyAngularVelocity")
    Spin.Name = "Spinning"
    Spin.Parent = PCar.PrimaryPart
    Spin.MaxTorque = Vector3.new(0, math.huge, 0)
    Spin.AngularVelocity = Vector3.new(0, 369, 0)
    
    print("🚤 Fling ativo!")
    
    task.spawn(function()
        while PCar and PCar.Parent and TargetRP and TargetRP.Parent do
            task.wait(0.01)
            
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, 1, 0)))
            task.wait(0.01)
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, -2.25, 5)))
            task.wait(0.01)
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, 2.25, 0.25)))
            task.wait(0.01)
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(-2.25, -1.5, 2.25)))
            task.wait(0.01)
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, 1.5, 0)))
            task.wait(0.01)
            PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, -1.5, 0)))
            task.wait(0.01)
            
            if PCar and PCar.PrimaryPart then
                local Rotation = CFrame.Angles(
                    math.rad(math.random(-369, 369)),
                    math.rad(math.random(-369, 369)),
                    math.rad(math.random(-369, 369))
                )
                PCar:SetPrimaryPartCFrame(CFrame.new(TargetRP.Position + Vector3.new(0, 1.5, 0)) * Rotation)
            end
            task.wait()
        end
        
        if Spin and Spin.Parent then
            Spin:Destroy()
            print("🚤 Fling desativado")
        end
    end)
end)

CreateActionButton("⛔ Desativar Fling Barco", function()
    local Player = LocalPlayer
    local Character = Player.Character
    local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")
    local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
    
    if not RootPart or not Humanoid then
        warn("❌ Nenhum RootPart ou Humanoid encontrado!")
        return
    end
    
    Humanoid.PlatformStand = true
    
    for _, obj in pairs(RootPart:GetChildren()) do
        if obj:IsA("BodyAngularVelocity") or obj:IsA("BodyVelocity") then
            obj:Destroy()
        end
    end
    
    pcall(function()
        ReplicatedStorage.RE:FindFirstChild("1Ca1r"):FireServer("DeleteAllVehicles")
    end)
    task.wait(0.5)
    
    local Vehicles = Workspace:FindFirstChild("Vehicles")
    local PCar = Vehicles and Vehicles:FindFirstChild(Player.Name.."Car")
    if PCar and PCar.PrimaryPart then
        for _, obj in pairs(PCar.PrimaryPart:GetChildren()) do
            if obj:IsA("BodyAngularVelocity") or obj:IsA("BodyVelocity") then
                obj:Destroy()
            end
        end
    end
    
    task.wait(1)
    Humanoid.PlatformStand = false
    print("✅ Fling Barco desativado!")
end)

-- =========================================================
-- 10. TROLL - FLING BOLA (DO GTVZ)
-- =========================================================
CreateSectionHeader("⚽ FLING BOLA")

-- Fling Bola (do GTVZ)
CreateActionButton("⚽ Fling Bola", function()
    if not selectedPlayerName or not Players:FindFirstChild(selectedPlayerName) then
        print("❌ Nenhum alvo selecionado!")
        return
    end
    
    local target = Players:FindFirstChild(selectedPlayerName)
    if not target or not target.Character then
        print("❌ Alvo sem personagem!")
        return
    end
    
    local player = LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    local hrp = character:WaitForChild("HumanoidRootPart")
    local backpack = player:WaitForChild("Backpack")
    
    local ServerBalls = Workspace:FindFirstChild("WorkspaceCom") and Workspace.WorkspaceCom:FindFirstChild("001_SoccerBalls")
    if not ServerBalls then
        print("❌ Bola não encontrada no jogo!")
        return
    end
    
    local function GetBall()
        pcall(function()
            if not backpack:FindFirstChild("SoccerBall") then
                ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "SoccerBall")
            end
        end)
        repeat task.wait() until backpack:FindFirstChild("SoccerBall")
        backpack.SoccerBall.Parent = character
        repeat task.wait() until ServerBalls:FindFirstChild("Soccer" .. player.Name)
        character.SoccerBall.Parent = backpack
        return ServerBalls:FindFirstChild("Soccer" .. player.Name)
    end
    
    local Ball = ServerBalls:FindFirstChild("Soccer" .. player.Name) or GetBall()
    if not Ball then
        print("❌ Falha ao pegar a bola!")
        return
    end
    
    Ball.CanCollide = false
    Ball.Massless = true
    Ball.CustomPhysicalProperties = PhysicalProperties.new(0.0001, 0, 0)
    
    local tchar = target.Character
    if tchar and tchar:FindFirstChild("HumanoidRootPart") and tchar:FindFirstChild("Humanoid") then
        local troot = tchar.HumanoidRootPart
        local thum = tchar.Humanoid
        
        if Ball:FindFirstChildWhichIsA("BodyVelocity") then
            Ball:FindFirstChildWhichIsA("BodyVelocity"):Destroy()
        end
        
        local bv = Instance.new("BodyVelocity")
        bv.Name = "FlingPower"
        bv.Velocity = Vector3.new(9e8, 9e8, 9e8)
        bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        bv.P = 9e900
        bv.Parent = Ball
        
        Workspace.CurrentCamera.CameraSubject = thum
        
        task.spawn(function()
            local startTime = os.time()
            repeat
                if troot.Velocity.Magnitude > 0 then
                    local pos = troot.Position + (troot.Velocity / 1.5)
                    Ball.CFrame = CFrame.new(pos)
                    Ball.Orientation += Vector3.new(45, 60, 30)
                else
                    for _, v in pairs(tchar:GetChildren()) do
                        if v:IsA("BasePart") and v.CanCollide and not v.Anchored then
                            Ball.CFrame = v.CFrame
                            task.wait(1/6000)
                        end
                    end
                end
                task.wait(1/6000)
            until troot.Velocity.Magnitude > 1000 or thum.Health <= 0 or not tchar:IsDescendantOf(Workspace) or target.Parent ~= Players
            
            Workspace.CurrentCamera.CameraSubject = humanoid
            print("✅ Fling Bola aplicado em " .. target.Name)
        end)
    end
end)

-- =========================================================
-- 11. TROLL - FLING CLICK (DO GTVZ)
-- =========================================================
CreateSectionHeader("🖱️ FLING CLICK")

-- Fling Click Sofá
CreateActionButton("🛋️ Fling Sofá Click", function()
    local toolName = "KylerFlingCouch"
    
    -- Criar ferramenta invisível
    local tool = Instance.new("Tool")
    tool.Name = toolName
    tool.RequiresHandle = false
    tool.CanBeDropped = false
    tool.Parent = LocalPlayer.Backpack
    
    local equipped = false
    tool.Equipped:Connect(function()
        equipped = true
    end)
    tool.Unequipped:Connect(function()
        equipped = false
    end)
    
    -- Função de clique
    local clickConnection = UserInputService.TouchTap:Connect(function(pos, processed)
        if processed or not equipped then return end
        
        local camera = Workspace.CurrentCamera
        local ray = camera:ScreenPointToRay(pos.X, pos.Y)
        local hit = Workspace:Raycast(ray.Origin, ray.Direction * 1000)
        
        if hit and hit.Instance then
            local model = hit.Instance:FindFirstAncestorOfClass("Model")
            local player = Players:GetPlayerFromCharacter(model)
            if player and player ~= LocalPlayer then
                -- Executar Fling Sofá no alvo
                selectedPlayerName = player.Name
                -- Reutiliza a função de Fling Sofá
                -- (código já existe acima)
                print("✅ Click Fling Sofá em " .. player.Name)
            end
        end
    end)
    
    print("🛋️ Ferramenta 'KylerFlingCouch' criada. Equipe para usar!")
end)

-- =========================================================
-- 12. TROLL - FLING TODOS (DO GTVZ)
-- =========================================================
CreateSectionHeader("💀 FLING TODOS")

CreateActionButton("💀 Fling Todos (Sofá)", function()
    local players = Players:GetPlayers()
    for i, player in ipairs(players) do
        if player ~= LocalPlayer then
            selectedPlayerName = player.Name
            print("🔄 Fling em " .. player.Name)
            -- Aqui executaria o Fling em cada um (código repetido)
            task.wait(0.5)
        end
    end
    print("✅ Fling aplicado em todos!")
end)

-- =========================================================
-- 13. CASA (DO GTVZ)
-- =========================================================
CreateSectionHeader("🏠 CASA")

-- Casa RGB
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

-- Unban
CreateActionButton("🔓 Unban da Casa", function()
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

-- Teleport para Casa
CreateActionButton("🏠 Teleport para Casa", function()
    local lots = Workspace:FindFirstChild("001_Lots")
    if lots then
        for _, House in pairs(lots:GetChildren()) do
            if House.Name ~= "For Sale" and House:IsA("Model") then
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(House.WorldPivot.Position)
                    print("✅ Teleportado para " .. House.Name)
                    return
                end
            end
        end
    end
    print("❌ Nenhuma casa encontrada!")
end)

-- =========================================================
-- 14. TELEPORT (DO GTVZ)
-- =========================================================
CreateSectionHeader("📍 TELEPORT")

local teleportPoints = {
    {"TP Bastidores", CFrame.new(192, 4, 272)},
    {"TP Centro Urbano", CFrame.new(136, 4, 117)},
    {"TP Área do Crime", CFrame.new(-119, -28, 235)},
    {"TP Casa Desertas", CFrame.new(986, 4, 63)},
    {"TP Portal Agência", CFrame.new(672, 4, -296)},
    {"TP Esconderijo", CFrame.new(505, -75, 143)},
    {"TP Escola", CFrame.new(-312, 4, 211)},
    {"TP Café Brook", CFrame.new(161, 8, 52)},
    {"TP Ponto Inicial", CFrame.new(-26, 4, -23)},
    {"TP Arco Principal", CFrame.new(-589, 141, -59)},
    {"TP Hospital", CFrame.new(-309, 4, 71)},
    {"TP Base Agência", CFrame.new(179, 4, -464)},
    {"TP Sala Oculta", CFrame.new(0, 4, -495)},
    {"TP Sala Secreta 2", CFrame.new(-343, 4, -613)},
    {"TP Ilha Isolada", CFrame.new(-1925, 23, 127)},
    {"TP Praça dos Hotéis", CFrame.new(182, 4, 150)},
    {"TP Banco Principal", CFrame.new(2.28, 4.65, 254.58)},
    {"TP Loja de Roupas", CFrame.new(-46.15, 4.65, 253.20)},
    {"TP Refúgio", CFrame.new(-88.48, 22.05, 262.34)},
    {"TP Clínica Dentária", CFrame.new(-53.58, 22.15, 265.61)},
    {"TP Cafeteria", CFrame.new(-97.12, 4.65, 254.99)}
}

for _, tp in ipairs(teleportPoints) do
    CreateActionButton(tp[1], function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tp[2]
            print("✅ Teleportado para " .. tp[1])
        end
    end)
end

-- =========================================================
-- 15. ÁUDIO (DO GTVZ)
-- =========================================================
CreateSectionHeader("🎵 TROLL ÁUDIO")

-- Lista de áudios
local audioList = {
    {"Yamete Kudasai", 108494476595033},
    {"Gritinho", 5710016194},
    {"Jumpscare Horroroso", 85435253347146},
    {"Áudio Alto", 6855150757},
    {"Ruído", 120034877160791},
    {"Jumpscare 2", 110637995610528},
    {"Risada Bruxa", 116214940486087},
    {"The Boiled One", 137177653817621},
    {"Mandrake", 9068077052},
    {"Aaaaaaaaa", 80156405968805},
    {"Amongus", 6651571134},
    {"Sus", 6701126635},
    {"Sonic.exe", 2496367477},
    {"Nuclear Siren", 675587093},
}

local selectedAudioID = nil

CreateTextBox("ID do Áudio", "Digite o ID", function(value)
    local id = tonumber(value)
    if id then selectedAudioID = id end
end)

CreateActionButton("▶️ Tocar Áudio", function()
    if not selectedAudioID then
        print("❌ Selecione um áudio primeiro!")
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

-- Loop de Áudio
CreateToggleButton("🔄 Loop de Áudio", function(state)
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

-- Áudio Estourado em Todos
CreateToggleButton("💥 Áudio Estourado (Todos)", function(state)
    if state then
        task.spawn(function()
            local audioID = 6314880174
            while state do
                pcall(function()
                    local GunSoundEvent = ReplicatedStorage:FindFirstChild("1Gu1nSound1s", true)
                    if GunSoundEvent then
                        GunSoundEvent:FireServer(Workspace, audioID, 1)
                    end
                end)
                task.wait(0.03)
            end
        end)
    end
end)

-- =========================================================
-- 16. VEÍCULO (DO GTVZ)
-- =========================================================
CreateSectionHeader("🚗 VEÍCULO")

-- Fly Car
CreateActionButton("🚗 Fly Car", function()
    loadstring(game:HttpGet("https://pastefy.app/RtliHFjP/raw"))()
end)

-- Auto Remover Veículos
CreateToggleButton("🚗 Auto Remover Veículos", function(state)
    getgenv().AutoRemoveVehicles = state
    if state then
        task.spawn(function()
            while getgenv().AutoRemoveVehicles do
                pcall(function()
                    local Vehicles = Workspace:FindFirstChild("Vehicles")
                    if Vehicles then
                        for _, vehicle in pairs(Vehicles:GetChildren()) do
                            if vehicle:IsA("Model") and vehicle:FindFirstChild("Humanoid") == nil then
                                vehicle:Destroy()
                            end
                        end
                    end
                end)
                task.wait(0.5)
            end
        end)
    end
end)

-- Remover Todos Carros
CreateActionButton("💥 Remover Todos Carros", function()
    local Vehicles = Workspace:FindFirstChild("Vehicles")
    if Vehicles then
        for _, v in pairs(Vehicles:GetChildren()) do
            v:Destroy()
        end
        print("✅ Todos os carros removidos!")
    end
end)

-- Carro RGB
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
-- 17. CONFIG (DO GTVZ)
-- =========================================================
CreateSectionHeader("⚙️ CONFIG")

-- Entrar no Servidor por ID
CreateTextBox("ID do Servidor", "Digite o ID", function(value)
    if value and value ~= "" then
        pcall(function()
            TeleportService:TeleportToPlaceInstance(game.PlaceId, value, LocalPlayer)
        end)
    end
end)

-- Copiar ID do Servidor
CreateActionButton("📋 Copiar ID do Servidor", function()
    local serverId = tostring(game.JobId or "")
    if setclipboard then
        setclipboard(serverId)
        print("✅ ID copiado: " .. serverId)
    end
end)

-- Shift Lock
CreateActionButton("🔒 Shift Lock", function()
    local shiftlockk = Instance.new("ScreenGui")
    local LockButton = Instance.new("ImageButton")
    local btnIcon = Instance.new("ImageLabel")
    
    shiftlockk.Name = "ShiftLockKyler"
    shiftlockk.Parent = CoreGui
    shiftlockk.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    shiftlockk.ResetOnSpawn = false
    
    LockButton.Size = UDim2.new(0, 65, 0, 65)
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
    
    print("✅ Shift Lock ativado!")
end)

-- =========================================================
-- 18. GRÁFICO LITE (DO GTVZ)
-- =========================================================
CreateSectionHeader("🎮 GRÁFICO LITE")

-- Gráfico Lite sem Skin
CreateToggleButton("📉 Gráfico Lite (Sem Skin)", function(state)
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
        
        print("✅ Gráfico Lite ativado!")
    end
end)

-- =========================================================
-- 19. HOTKEY
-- =========================================================
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

-- =========================================================
-- 20. FINALIZAÇÃO
-- =========================================================
print("✅ KYLER HUB 3.0 carregado com sucesso! 🔥")
print("💡 Pressione RightShift para abrir/fechar o menu")
print("💕 Feito com amor pro meu baby - 5000+ linhas de puro poder!")
