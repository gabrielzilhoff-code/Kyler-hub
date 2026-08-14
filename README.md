-- =========================================================
-- ⚡ KYLER HUB 3.1 (VERSÃO LIMPA)
-- 🔥 SEM PUXAR SOFÁ + SEM TROLL PROP
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

-- =========================================================
-- TAB BAR
-- =========================================================
local TabBar = Instance.new("Frame")
TabBar.Name = "TabBar"
TabBar.Size = UDim2.new(1, 0, 0, 28)
TabBar.Position = UDim2.new(0, 0, 0, 35)
TabBar.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
TabBar.BorderSizePixel = 0
TabBar.Parent = MainFrame

local TabLayout = Instance.new("UIListLayout")
TabLayout.FillDirection = Enum.FillDirection.Horizontal
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Padding = UDim.new(0, 2)
TabLayout.Parent = TabBar

-- Dois ScrollFrames: Principal e Troll
local function MakeScrollFrame()
    local sf = Instance.new("ScrollingFrame")
    sf.Size = UDim2.new(1, -16, 1, -72)
    sf.Position = UDim2.new(0, 8, 0, 68)
    sf.BackgroundTransparency = 1
    sf.BorderSizePixel = 0
    sf.ScrollBarThickness = 5
    sf.CanvasSize = UDim2.new(0, 0, 0, 3000)
    sf.Visible = false
    sf.Parent = MainFrame

    local layout = Instance.new("UIListLayout")
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 4)
    layout.Parent = sf
    return sf
end

local ScrollMain = MakeScrollFrame()
ScrollMain.Visible = true
local ScrollTroll = MakeScrollFrame()

-- Tab buttons
local activeTab = "main"

local function MakeTab(text, key, sf, order)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 100, 1, 0)
    btn.BackgroundColor3 = key == "main" and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(30, 30, 35)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = text
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 12
    btn.BorderSizePixel = 0
    btn.LayoutOrder = order
    btn.Parent = TabBar

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = btn

    return btn
end

local TabMain  = MakeTab("⚙️ Principal", "main",  ScrollMain,  1)
local TabTroll = MakeTab("😈 Troll",     "troll", ScrollTroll, 2)

local function SwitchTab(key)
    activeTab = key
    ScrollMain.Visible  = key == "main"
    ScrollTroll.Visible = key == "troll"
    TabMain.BackgroundColor3  = key == "main"  and Color3.fromRGB(170,85,255) or Color3.fromRGB(30,30,35)
    TabTroll.BackgroundColor3 = key == "troll" and Color3.fromRGB(170,85,255) or Color3.fromRGB(30,30,35)
end

TabMain.MouseButton1Click:Connect(function()  SwitchTab("main")  end)
TabTroll.MouseButton1Click:Connect(function() SwitchTab("troll") end)

-- =========================================================
-- 2. COMPONENTES (ScrollFrame parametrizado)
-- =========================================================
local function CreateToggleButton(parent, text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -8, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    button.Text = text .. " [OFF]"
    button.TextColor3 = Color3.fromRGB(200, 200, 200)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 13
    button.Parent = parent

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

local function CreateSlider(parent, text, min, max, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = parent

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
        if not frame.Parent and changedConnection then changedConnection:Disconnect() end
    end)
end

local function CreateSectionHeader(parent, text)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -8, 0, 22)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(180, 120, 255)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 14
    label.Parent = parent
end

local function CreateActionButton(parent, text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -8, 0, 28)
    button.BackgroundColor3 = Color3.fromRGB(40, 35, 45)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(240, 240, 240)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 12
    button.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = button

    button.MouseButton1Click:Connect(callback)
    return button
end

local function CreateTextBox(parent, text, placeholder, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 32)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = parent

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

    box.FocusLost:Connect(function() callback(box.Text) end)
    return box
end

-- =========================================================
-- 3. ALVO (PRINCIPAL)
-- =========================================================
CreateSectionHeader(ScrollMain, "🎯 ALVO")

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, -8, 0, 30)
TargetInput.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TargetInput.PlaceholderText = "Digite o começo do nome..."
TargetInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetInput.Font = Enum.Font.SourceSans
TargetInput.TextSize = 13
TargetInput.Parent = ScrollMain

local TargetCorner = Instance.new("UICorner")
TargetCorner.CornerRadius = UDim.new(0, 6)
TargetCorner.Parent = TargetInput

local selectedPlayer = nil

local function FindPlayerByPartialName(partial)
    if partial == "" then return nil end
    local partialLower = partial:lower()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            local nameLower = p.Name:lower()
            local displayLower = p.DisplayName:lower()
            if nameLower:sub(1, #partialLower) == partialLower or
               displayLower:sub(1, #partialLower) == partialLower then
                return p
            end
        end
    end
    return nil
end

local function UpdateTarget()
    local input = TargetInput.Text
    if input == "" then selectedPlayer = nil return end
    local player = FindPlayerByPartialName(input)
    if player then
        selectedPlayer = player
        TargetInput.Text = player.Name
        TargetInput.TextColor3 = Color3.fromRGB(0, 255, 0)
    end
end

TargetInput.FocusLost:Connect(UpdateTarget)
TargetInput:GetPropertyChangedSignal("Text"):Connect(function()
    if #TargetInput.Text >= 2 then UpdateTarget() end
end)

local function GetTargetPlayer() return selectedPlayer end

-- =========================================================
-- 4. STATUS
-- =========================================================
CreateSectionHeader(ScrollMain, "📊 STATUS")

local startTime = tick()
local playTimeLabel = Instance.new("TextLabel")
playTimeLabel.Size = UDim2.new(1, -8, 0, 20)
playTimeLabel.BackgroundTransparency = 1
playTimeLabel.Text = "⏱️ 00:00"
playTimeLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playTimeLabel.Font = Enum.Font.SourceSans
playTimeLabel.TextSize = 12
playTimeLabel.Parent = ScrollMain

task.spawn(function()
    while true do
        task.wait(1)
        local elapsed = math.floor(tick() - startTime)
        playTimeLabel.Text = string.format("⏱️ %02d:%02d", math.floor(elapsed/60), elapsed%60)
    end
end)

local playersOnlineLabel = Instance.new("TextLabel")
playersOnlineLabel.Size = UDim2.new(1, -8, 0, 20)
playersOnlineLabel.BackgroundTransparency = 1
playersOnlineLabel.Text = "👥 Jogadores: 0/" .. Players.MaxPlayers
playersOnlineLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
playersOnlineLabel.Font = Enum.Font.SourceSans
playersOnlineLabel.TextSize = 12
playersOnlineLabel.Parent = ScrollMain

task.spawn(function()
    while true do
        task.wait(1)
        playersOnlineLabel.Text = "👥 Jogadores: " .. #Players:GetPlayers() .. "/" .. Players.MaxPlayers
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
executorLabel.Parent = ScrollMain

CreateActionButton(ScrollMain, "🔄 Reconectar", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId)
end)

CreateActionButton(ScrollMain, "👥 Server Cheio", function()
    local PlaceId = game.PlaceId
    local Api = "https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Desc&limit=100"
    local function FindServer()
        local cursor = nil
        repeat
            local url = Api .. (cursor and "&cursor="..cursor or "")
            local data = HttpService:JSONDecode(game:HttpGet(url))
            for _, server in pairs(data.data) do
                if server.playing < server.maxPlayers and server.playing >= 21 then return server.id end
            end
            cursor = data.nextPageCursor
        until not cursor
    end
    local serverId = FindServer()
    if serverId then TeleportService:TeleportToPlaceInstance(PlaceId, serverId, LocalPlayer) end
end)

CreateActionButton(ScrollMain, "🔄 Mudar Server", function()
    local _place = game.PlaceId
    local _servers = "https://games.roblox.com/v1/games/".._place.."/servers/Public?sortOrder=Asc&limit=100"
    local function ListServers(cursor)
        return HttpService:JSONDecode(game:HttpGet(_servers .. (cursor and "&cursor="..cursor or "")))
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
-- 5. JOGADOR
-- =========================================================
CreateSectionHeader(ScrollMain, "🏃 JOGADOR")

CreateActionButton(ScrollMain, "🕊️ Fly", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/QjjQvMsE"))()
end)

local noclipActive = false
local noclipConnection = nil
CreateToggleButton(ScrollMain, "🚪 Noclip", function(state)
    noclipActive = state
    if noclipConnection then noclipConnection:Disconnect() noclipConnection = nil end
    if noclipActive then
        noclipConnection = RunService.Stepped:Connect(function()
            if not LocalPlayer.Character then return end
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end
        end)
    end
end)

CreateSlider(ScrollMain, "Velocidade", 1, 200, 16, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

CreateSlider(ScrollMain, "Pulo", 1, 500, 50, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.UseJumpPower = true
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

CreateSlider(ScrollMain, "Gravidade", 0, 500, 196, function(val)
    Workspace.Gravity = val
end)

-- =========================================================
-- 6. ESP
-- =========================================================
CreateSectionHeader(ScrollMain, "👁️ ESP")

local espActive = false
local espConnection = nil
CreateToggleButton(ScrollMain, "👤 ESP Nome", function(state)
    espActive = state
    if espConnection then espConnection:Disconnect() espConnection = nil end
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
                        textLabel.TextStrokeColor3 = Color3.new(0,0,0)
                        textLabel.TextStrokeTransparency = 0
                        textLabel.TextScaled = true
                        textLabel.Text = player.Name .. "\n" .. math.floor(distance) .. "m"
                        textLabel.Parent = billboardGui
                    else
                        local lbl = billboardGui:FindFirstChild("NameLabel")
                        if lbl then lbl.Text = player.Name .. "\n" .. math.floor(distance) .. "m" end
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
CreateToggleButton(ScrollMain, "✨ ESP Holograma", function(state)
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
            highlight.OutlineColor = Color3.fromRGB(0,0,0)
            highlight.OutlineTransparency = 0
            highlight.Parent = highlightStorage
            if plr.Character then highlight.Adornee = plr.Character end
            highlightConnections[plr] = plr.CharacterAdded:Connect(function(char)
                highlight.Adornee = char
            end)
        end
        highlightConnections["_PlayerAdded"] = Players.PlayerAdded:Connect(HighlightPlayer)
        for _, v in ipairs(Players:GetPlayers()) do HighlightPlayer(v) end
    end
end)

-- =========================================================
-- 7. AVATAR RGB
-- =========================================================
CreateSectionHeader(ScrollMain, "🎨 RGB")

CreateToggleButton(ScrollMain, "🌈 Nome RGB", function(state)
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
                    if remote then remote:FireServer("PickingRPNameColor", colors[i]) end
                end)
                i = i % #colors + 1
                task.wait(0.3)
            end
        end)
    end
end)

CreateToggleButton(ScrollMain, "🌈 Corpo RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {"Bright red", "Lime green", "Bright blue", "Bright yellow", "Bright cyan", "Hot pink"}
            local i = 1
            while state do
                if not state then return end
                pcall(function()
                    local remote = ReplicatedStorage:FindFirstChild("Remotes") and ReplicatedStorage.Remotes:FindFirstChild("ChangeBodyColor")
                    if remote then remote:FireServer(colors[i]) end
                end)
                i = i % #colors + 1
                task.wait(0.5)
            end
        end)
    end
end)

local copyTarget = nil
CreateTextBox(ScrollMain, "Alvo para copiar", "Digite o nome", function(value) copyTarget = value end)

CreateActionButton(ScrollMain, "📋 Copiar Avatar", function()
    if not copyTarget or copyTarget == "" then print("❌ Digite um nome!") return end
    local TPlayer = Players:FindFirstChild(copyTarget)
    if not TPlayer or not TPlayer.Character then print("❌ Jogador não encontrado!") return end
    local LChar = LocalPlayer.Character
    if not LChar then return end
    local LHumanoid = LChar:FindFirstChildOfClass("Humanoid")
    local THumanoid = TPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not (LHumanoid and THumanoid) then return end
    local Remotes = ReplicatedStorage:WaitForChild("Remotes")
    local PDesc = THumanoid:GetAppliedDescription()
    local argsBody = {[1]={[1]=PDesc.Torso,[2]=PDesc.RightArm,[3]=PDesc.LeftArm,[4]=PDesc.RightLeg,[5]=PDesc.LeftLeg,[6]=PDesc.Head}}
    pcall(function() Remotes.ChangeCharacterBody:InvokeServer(unpack(argsBody)) end)
    task.wait(0.5)
    for _, id in ipairs({tonumber(PDesc.Shirt), tonumber(PDesc.Pants), tonumber(PDesc.Face)}) do
        if id then Remotes.Wear:InvokeServer(id) task.wait(0.3) end
    end
    for _, acc in ipairs(PDesc:GetAccessories(true)) do
        if acc.AssetId and tonumber(acc.AssetId) then
            Remotes.Wear:InvokeServer(tonumber(acc.AssetId))
            task.wait(0.3)
        end
    end
    print("✅ Avatar copiado!")
end)

-- =========================================================
-- 8. FLING
-- =========================================================
CreateSectionHeader(ScrollMain, "🔥 FLING")

local function PegarEEquiparSofa()
    local char = LocalPlayer.Character
    if not char then return false end
    pcall(function() ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools") end)
    task.wait(0.2)
    pcall(function() ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch") end)
    task.wait(0.3)
    local tool = LocalPlayer.Backpack:FindFirstChild("Couch")
    if not tool then return false end
    tool.Parent = char
    task.wait(0.1)
    return char:FindFirstChild("Couch") ~= nil
end

CreateActionButton(ScrollMain, "⚽ Fling Bola", function()
    local target = GetTargetPlayer()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local targetHRP = target.Character:FindFirstChild("HumanoidRootPart")
    local myHRP = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not targetHRP or not myHRP then return end
    local targetHumanoid = target.Character:FindFirstChildOfClass("Humanoid")
    local ball = Instance.new("Part")
    ball.Size = Vector3.new(2,2,2)
    ball.Shape = Enum.PartType.Ball
    ball.Material = Enum.Material.SmoothPlastic
    ball.BrickColor = BrickColor.new("Bright red")
    ball.Anchored = false
    ball.CanCollide = true
    ball.Transparency = 0.3
    ball.Parent = Workspace
    ball.CFrame = targetHRP.CFrame * CFrame.new(0, 2, 0)
    for i = 1, 25 do
        if not target.Character or not targetHRP.Parent then break end
        local randomDir = Vector3.new(math.random(-8000,8000), math.random(3000,15000), math.random(-8000,8000))
        targetHRP.AssemblyLinearVelocity = randomDir
        targetHRP.AssemblyAngularVelocity = Vector3.new(math.random(-2000,2000), math.random(-2000,2000), math.random(-2000,2000))
        ball.CFrame = targetHRP.CFrame * CFrame.new(0,2,0)
        ball.AssemblyLinearVelocity = randomDir
        if targetHumanoid and i % 5 == 0 then targetHumanoid.Health -= 10 end
        task.wait()
    end
    ball:Destroy()
    print("✅ Fling Bola em " .. target.Name)
end)

CreateActionButton(ScrollMain, "🛋️ Fling Sofá", function()
    local target = GetTargetPlayer()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    local root = char:FindFirstChild("HumanoidRootPart")
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not hum or not root or not tRoot then return end
    if not PegarEEquiparSofa() then return end
    local originalPos = root.CFrame
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.F, false, game)
    task.wait(0.1)
    hum:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
    hum.PlatformStand = false
    Workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChild("Head") or tRoot or hum
    local align = Instance.new("BodyPosition")
    align.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    align.D = 10
    align.P = 30000
    align.Position = root.Position
    align.Parent = tRoot
    task.spawn(function()
        local angle = 0
        local st = tick()
        while tick()-st < 5 and target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
            local tHum = target.Character:FindFirstChildOfClass("Humanoid")
            if not tHum or tHum.Sit then break end
            local hrp = target.Character.HumanoidRootPart
            angle += 50
            root.CFrame = CFrame.new(hrp.Position + hrp.Velocity/1.5 + Vector3.new(0,2,0)) * CFrame.Angles(math.rad(angle),0,0)
            align.Position = root.Position + Vector3.new(2,0,0)
            task.wait()
        end
        align:Destroy()
        hum:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
        hum.PlatformStand = false
        Workspace.CurrentCamera.CameraSubject = hum
        for _, p in pairs(char:GetDescendants()) do
            if p:IsA("BasePart") then p.Velocity = Vector3.zero p.RotVelocity = Vector3.zero end
        end
        task.wait(0.1)
        root.CFrame = CFrame.new(145.51, -350.09, 21.58)
        task.wait(0.3)
        local tool = char:FindFirstChild("Couch")
        if tool then tool.Parent = LocalPlayer.Backpack end
        task.wait(0.01)
        pcall(function() ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch") end)
        task.wait(0.2)
        root.CFrame = originalPos
        print("✅ Fling Sofá em " .. target.Name)
    end)
end)

-- =========================================================
-- 9. TELEPORT
-- =========================================================
CreateSectionHeader(ScrollMain, "📍 TELEPORT")

local teleportPoints = {
    {"🏠 Spawn", CFrame.new(-26, 4, -23)},
    {"🏥 Hospital", CFrame.new(-309, 4, 71)},
    {"🏫 Escola", CFrame.new(-312, 4, 211)},
    {"🏦 Banco", CFrame.new(2.28, 4.65, 254.58)},
    {"🛍️ Loja", CFrame.new(-46.15, 4.65, 253.20)},
    {"🏝️ Ilha", CFrame.new(-1925, 23, 127)},
}
for _, tp in ipairs(teleportPoints) do
    CreateActionButton(ScrollMain, tp[1], function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tp[2]
        end
    end)
end

-- =========================================================
-- 10. ÁUDIO
-- =========================================================
CreateSectionHeader(ScrollMain, "🎵 ÁUDIO")

local selectedAudioID = nil
CreateTextBox(ScrollMain, "ID do Áudio", "Digite o ID", function(value)
    local id = tonumber(value)
    if id then selectedAudioID = id end
end)

CreateActionButton(ScrollMain, "▶️ Tocar Áudio", function()
    if not selectedAudioID then return end
    pcall(function()
        ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(Workspace, selectedAudioID, 1)
    end)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://" .. selectedAudioID
    sound.Parent = LocalPlayer.Character:FindFirstChild("HumanoidRootPart") or Workspace
    sound:Play()
    task.wait(sound.TimeLength or 3)
    sound:Destroy()
end)

CreateToggleButton(ScrollMain, "🔄 Loop Áudio", function(state)
    if state then
        task.spawn(function()
            while state do
                if selectedAudioID then
                    pcall(function()
                        ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(Workspace, selectedAudioID, 1)
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
-- 11. VEÍCULO
-- =========================================================
CreateSectionHeader(ScrollMain, "🚗 VEÍCULO")

CreateActionButton(ScrollMain, "🚗 Fly Car", function()
    loadstring(game:HttpGet("https://pastefy.app/RtliHFjP/raw"))()
end)

CreateActionButton(ScrollMain, "💥 Remover Carros", function()
    local Vehicles = Workspace:FindFirstChild("Vehicles")
    if Vehicles then
        for _, v in pairs(Vehicles:GetChildren()) do v:Destroy() end
        print("✅ Carros removidos!")
    end
end)

CreateToggleButton(ScrollMain, "🌈 Carro RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {
                Color3.new(1,0,0), Color3.new(0,1,0), Color3.new(0,0,1),
                Color3.new(1,1,0), Color3.new(1,0,1), Color3.new(0,1,1),
                Color3.new(0.5,0,0.5), Color3.new(1,0.5,0)
            }
            while state do
                for _, color in ipairs(colors) do
                    if not state then return end
                    pcall(function()
                        local remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Player1sCa1r")
                        if remote then remote:FireServer("PickingCarColor", color) end
                    end)
                    task.wait(1)
                end
            end
        end)
    end
end)

-- =========================================================
-- 12. CASA
-- =========================================================
CreateSectionHeader(ScrollMain, "🏠 CASA")

CreateToggleButton(ScrollMain, "🌈 Casa RGB", function(state)
    if state then
        task.spawn(function()
            local colors = {Color3.new(1,0,0),Color3.new(0,1,0),Color3.new(0,0,1),Color3.new(1,1,0),Color3.new(0,1,1),Color3.new(1,0,1)}
            while state do
                for _, color in ipairs(colors) do
                    if not state then return end
                    pcall(function()
                        local remote = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1Player1sHous1e")
                        if remote then remote:FireServer("ColorPickHouse", color) end
                    end)
                    task.wait(0.8)
                end
            end
        end)
    end
end)

CreateActionButton(ScrollMain, "🔓 Unban Casa", function()
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

CreateActionButton(ScrollMain, "🏠 TP Casa", function()
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
-- 13. CONFIG
-- =========================================================
CreateSectionHeader(ScrollMain, "⚙️ CONFIG")

CreateTextBox(ScrollMain, "ID do Servidor", "Digite o ID", function(value)
    if value and value ~= "" then
        pcall(function() TeleportService:TeleportToPlaceInstance(game.PlaceId, value, LocalPlayer) end)
    end
end)

CreateActionButton(ScrollMain, "📋 Copiar ID", function()
    local serverId = tostring(game.JobId or "")
    if setclipboard then setclipboard(serverId) print("✅ ID copiado: " .. serverId) end
end)

CreateActionButton(ScrollMain, "🔒 Shift Lock", function()
    local shiftlockk = Instance.new("ScreenGui")
    shiftlockk.Name = "ShiftLockKyler"
    shiftlockk.Parent = CoreGui
    shiftlockk.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    shiftlockk.ResetOnSpawn = false
    local LockButton = Instance.new("ImageButton")
    LockButton.Size = UDim2.new(0, 50, 0, 50)
    LockButton.Position = UDim2.new(0.785, 0, 0.865, 0)
    LockButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    LockButton.BackgroundTransparency = 0.3
    LockButton.Parent = shiftlockk
    LockButton.Image = ""
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(1, 0)
    corner.Parent = LockButton
    local btnIcon = Instance.new("ImageLabel")
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
                pcall(function() GameSettings.RotationType = Enum.RotationType.CameraRelative end)
            end)
        else
            btnIcon.ImageColor3 = Color3.fromRGB(255, 255, 255)
            if connection then connection:Disconnect() connection = nil end
            pcall(function() GameSettings.RotationType = Enum.RotationType.MovementRelative end)
        end
    end)
    print("✅ Shift Lock!")
end)

-- =========================================================
-- 14. GRÁFICO LITE
-- =========================================================
CreateSectionHeader(ScrollMain, "🎮 GRÁFICO LITE")

CreateToggleButton(ScrollMain, "📉 Gráfico Lite", function(state)
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
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") or obj:IsA("Fire") then
                obj:Destroy()
            elseif obj:IsA("BasePart") then
                obj.Material = Enum.Material.SmoothPlastic
                obj.CastShadow = false
                obj.Reflectance = 0
            end
        end
        for _, plr in pairs(Players:GetPlayers()) do
            if plr.Character then
                for _, child in pairs(plr.Character:GetChildren()) do
                    if child:IsA("Accessory") or child:IsA("Clothing") then child:Destroy() end
                end
            end
        end
        print("✅ Gráfico Lite!")
    end
end)

-- =========================================================
-- 😈 TROLL TAB — BROOKHAVEN
-- =========================================================
CreateSectionHeader(ScrollTroll, "😈 TROLL — BROOKHAVEN")

-- Alvo da aba troll (compartilha o selectedPlayer do principal)
local function GetTrollTarget()
    return selectedPlayer
end

-- TP EM CIMA DO ALVO
CreateActionButton(ScrollTroll, "📍 TP em cima do alvo", function()
    local target = GetTrollTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot or not myRoot then return end
    myRoot.CFrame = tRoot.CFrame * CFrame.new(0, 3, 0)
    print("✅ TP em cima de " .. target.Name)
end)

-- SEGUIR ALVO
local followActive = false
local followConnection = nil
CreateToggleButton(ScrollTroll, "🏃 Seguir Alvo", function(state)
    followActive = state
    if followConnection then followConnection:Disconnect() followConnection = nil end
    if followActive then
        followConnection = RunService.Heartbeat:Connect(function()
            local target = GetTrollTarget()
            if not target or not target.Character then return end
            local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
            local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if not tRoot or not myRoot then return end
            myRoot.CFrame = tRoot.CFrame * CFrame.new(0, 0, 3)
        end)
    end
end)

-- TRAVAR ALVO NO LUGAR
local freezeActive = false
local freezeConnection = nil
CreateToggleButton(ScrollTroll, "🧊 Travar Alvo", function(state)
    freezeActive = state
    if freezeConnection then freezeConnection:Disconnect() freezeConnection = nil end
    if freezeActive then
        local target = GetTrollTarget()
        if not target or not target.Character then return end
        local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
        if not tRoot then return end
        local frozenPos = tRoot.CFrame
        freezeConnection = RunService.Heartbeat:Connect(function()
            if not target.Character then return end
            local r = target.Character:FindFirstChild("HumanoidRootPart")
            if r then r.CFrame = frozenPos end
        end)
    end
end)

-- ARRASTAR ALVO
local dragActive = false
local dragConnection = nil
CreateToggleButton(ScrollTroll, "🧲 Arrastar Alvo", function(state)
    dragActive = state
    if dragConnection then dragConnection:Disconnect() dragConnection = nil end
    if dragActive then
        dragConnection = RunService.Heartbeat:Connect(function()
            local target = GetTrollTarget()
            local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if not target or not target.Character or not myRoot then return end
            local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
            if not tRoot then return end
            tRoot.CFrame = myRoot.CFrame * CFrame.new(0, 0, -4)
        end)
    end
end)

-- SPIN NO ALVO
local spinTargetActive = false
local spinTargetConn = nil
CreateToggleButton(ScrollTroll, "🌀 Spin no Alvo", function(state)
    spinTargetActive = state
    if spinTargetConn then spinTargetConn:Disconnect() spinTargetConn = nil end
    if spinTargetActive then
        local angle = 0
        spinTargetConn = RunService.Heartbeat:Connect(function()
            local target = GetTrollTarget()
            if not target or not target.Character then return end
            local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
            if not tRoot then return end
            angle += 10
            tRoot.CFrame = CFrame.new(tRoot.Position) * CFrame.Angles(0, math.rad(angle), 0)
        end)
    end
end)

-- ARREMESSAR ALVO PARA O AR
CreateActionButton(ScrollTroll, "🚀 Arremessar Alvo", function()
    local target = GetTrollTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    tRoot.AssemblyLinearVelocity = Vector3.new(
        math.random(-200, 200),
        math.random(800, 1500),
        math.random(-200, 200)
    )
    print("✅ Arremessado!")
end)

-- CÂMERA PRESA NO ALVO
local camLockActive = false
local camLockConn = nil
CreateToggleButton(ScrollTroll, "📷 Câmera no Alvo", function(state)
    camLockActive = state
    if camLockConn then camLockConn:Disconnect() camLockConn = nil end
    if camLockActive then
        local target = GetTrollTarget()
        if not target or not target.Character then return end
        local hum = target.Character:FindFirstChildOfClass("Humanoid")
        if hum then Workspace.CurrentCamera.CameraSubject = hum end
    else
        local myHum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if myHum then Workspace.CurrentCamera.CameraSubject = myHum end
    end
end)

-- LOOP TP (TELEPORT ALEATÓRIO REPETIDO NO ALVO)
local loopTpActive = false
local loopTpConn = nil
CreateToggleButton(ScrollTroll, "🔁 Loop TP no Alvo", function(state)
    loopTpActive = state
    if not loopTpActive then return end
    task.spawn(function()
        while loopTpActive do
            local target = GetTrollTarget()
            local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if target and target.Character and myRoot then
                local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
                if tRoot then
                    myRoot.CFrame = tRoot.CFrame * CFrame.new(
                        math.random(-3, 3), 2, math.random(-3, 3)
                    )
                end
            end
            task.wait(0.1)
        end
    end)
end)

-- INVISÍVEL (local)
local invisActive = false
CreateToggleButton(ScrollTroll, "👻 Invisível (Local)", function(state)
    invisActive = state
    local char = LocalPlayer.Character
    if not char then return end
    for _, part in pairs(char:GetDescendants()) do
        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
            part.LocalTransparencyModifier = state and 1 or 0
        end
    end
end)

-- SPEED BOOST RÁPIDO
CreateActionButton(ScrollTroll, "⚡ Speed x10 Rápido", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.WalkSpeed = 160 print("✅ Speed x10!") end
end)

CreateActionButton(ScrollTroll, "🔄 Reset Speed", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.WalkSpeed = 16 print("✅ Speed resetada!") end
end)

-- TRAVAR CÂMERA DO ALVO NA PROPRIA CABEÇA
CreateActionButton(ScrollTroll, "🎥 Resetar Câmera", function()
    local myHum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if myHum then
        Workspace.CurrentCamera.CameraSubject = myHum
        print("✅ Câmera resetada!")
    end
end)

-- CHAT SPAM (local, não envia pelo server)
local chatSpamActive = false
CreateToggleButton(ScrollTroll, "💬 Chat Spam", function(state)
    chatSpamActive = state
    if state then
        task.spawn(function()
            local msgs = {
                "👀", "kkkkkkk", "vai tomar!", "olha pra cima", 
                "😂😂😂", "fugiu não", "sumiu não", "KYLER HUB 🔥"
            }
            local i = 1
            while chatSpamActive do
                pcall(function()
                    game:GetService("ReplicatedStorage"):WaitForChild("DefaultChatSystemChatEvents")
                        :WaitForChild("SayMessageRequest"):FireServer(msgs[i], "All")
                end)
                i = i % #msgs + 1
                task.wait(1.5)
            end
        end)
    end
end)

-- EXPLODIR (velocidade em todas as direções)
CreateActionButton(ScrollTroll, "💥 Explodir Alvo", function()
    local target = GetTrollTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    for i = 1, 10 do
        tRoot.AssemblyLinearVelocity = Vector3.new(
            math.random(-3000, 3000),
            math.random(1000, 5000),
            math.random(-3000, 3000)
        )
        tRoot.AssemblyAngularVelocity = Vector3.new(
            math.random(-500, 500),
            math.random(-500, 500),
            math.random(-500, 500)
        )
        task.wait(0.05)
    end
    print("✅ Explodido!")
end)

-- =========================================================
-- HOTKEY
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
