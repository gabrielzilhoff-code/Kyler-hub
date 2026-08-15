-- =========================================================
-- ⚡ KYLER HUB 3.1 (VERSÃO LIMPA)
-- 💕 Feito com amor pro meu baby
-- =========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local VirtualInputManager = game:GetService("VirtualInputManager")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("KylerHubGui") then
    PlayerGui.KylerHubGui:Destroy()
end

-- =========================================================
-- GUI ROOT
-- =========================================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KylerHubGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 420, 0, 500)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -250)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 22)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 35)
TitleBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame
Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 12)

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

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Position = UDim2.new(1, -60, 0, 5)
MinBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinBtn.Text = "➖"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.TextSize = 14
MinBtn.Parent = TitleBar
MinBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(1, -32, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 14
CloseBtn.Parent = TitleBar
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- =========================================================
-- TAB BAR
-- =========================================================
local TabBar = Instance.new("Frame")
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

local function MakeScrollFrame()
    local sf = Instance.new("ScrollingFrame")
    sf.Size = UDim2.new(1, -16, 1, -72)
    sf.Position = UDim2.new(0, 8, 0, 68)
    sf.BackgroundTransparency = 1
    sf.BorderSizePixel = 0
    sf.ScrollBarThickness = 5
    sf.CanvasSize = UDim2.new(0, 0, 0, 3200)
    sf.Visible = false
    sf.Parent = MainFrame
    local layout = Instance.new("UIListLayout")
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 4)
    layout.Parent = sf
    return sf
end

local ScrollMain  = MakeScrollFrame()
local ScrollTroll = MakeScrollFrame()
ScrollMain.Visible = true

local function MakeTab(text, key, order)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 100, 1, 0)
    btn.BackgroundColor3 = (key == "main") and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(30, 30, 35)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = text
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 12
    btn.BorderSizePixel = 0
    btn.LayoutOrder = order
    btn.Parent = TabBar
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 4)
    return btn
end

local TabMain  = MakeTab("⚙️ Principal", "main",  1)
local TabTroll = MakeTab("😈 Troll",     "troll", 2)

local function SwitchTab(key)
    ScrollMain.Visible  = (key == "main")
    ScrollTroll.Visible = (key == "troll")
    TabMain.BackgroundColor3  = (key == "main")  and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(30, 30, 35)
    TabTroll.BackgroundColor3 = (key == "troll") and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(30, 30, 35)
end
TabMain.MouseButton1Click:Connect(function()  SwitchTab("main")  end)
TabTroll.MouseButton1Click:Connect(function() SwitchTab("troll") end)

-- =========================================================
-- COMPONENTES
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
    Instance.new("UICorner", button).CornerRadius = UDim.new(0, 6)
    local state = false
    button.MouseButton1Click:Connect(function()
        state = not state
        button.Text = text .. (state and " [ON]" or " [OFF]")
        button.TextColor3 = state and Color3.fromRGB(170, 85, 255) or Color3.fromRGB(200, 200, 200)
        button.BackgroundColor3 = state and Color3.fromRGB(45, 35, 55) or Color3.fromRGB(30, 30, 35)
        callback(state)
    end)
    return button
end

local function CreateSlider(parent, text, minVal, maxVal, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 40)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 18)
    label.BackgroundTransparency = 1
    label.Text = text .. ": " .. tostring(default)
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 12
    label.Parent = frame

    local track = Instance.new("TextButton")
    track.Size = UDim2.new(1, -16, 0, 10)
    track.Position = UDim2.new(0, 8, 0, 22)
    track.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    track.Text = ""
    track.Parent = frame

    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((default - minVal) / (maxVal - minVal), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(170, 85, 255)
    fill.BorderSizePixel = 0
    fill.Parent = track

    local dragging = false
    local function update(input)
        if not track.AbsoluteSize then return end
        local pos = math.clamp(
            (input.Position.X - track.AbsolutePosition.X) / track.AbsoluteSize.X, 0, 1
        )
        fill.Size = UDim2.new(pos, 0, 1, 0)
        local value = math.floor(minVal + (maxVal - minVal) * pos)
        label.Text = text .. ": " .. tostring(value)
        callback(value)
    end

    track.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            update(i)
        end
    end)
    track.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    local c = UserInputService.InputChanged:Connect(function(i)
        if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then
            update(i)
        end
    end)
    frame.AncestryChanged:Connect(function()
        if not frame.Parent and c then c:Disconnect() end
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
    Instance.new("UICorner", button).CornerRadius = UDim.new(0, 6)
    button.MouseButton1Click:Connect(callback)
    return button
end

local function CreateTextBox(parent, text, placeholder, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 32)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = parent
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)

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
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    box.FocusLost:Connect(function() callback(box.Text) end)
    return box
end

-- =========================================================
-- ALVO COMPARTILHADO
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
Instance.new("UICorner", TargetInput).CornerRadius = UDim.new(0, 6)

local selectedPlayer = nil

local function FindPlayerByPartialName(partial)
    if partial == "" then return nil end
    local pl = partial:lower()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer then
            if p.Name:lower():sub(1, #pl) == pl or
               p.DisplayName:lower():sub(1, #pl) == pl then
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

local function GetTarget() return selectedPlayer end

-- =========================================================
-- STATUS
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
        local e = math.floor(tick() - startTime)
        playTimeLabel.Text = string.format("⏱️ %02d:%02d", math.floor(e / 60), e % 60)
    end
end)

local playersOnlineLabel = Instance.new("TextLabel")
playersOnlineLabel.Size = UDim2.new(1, -8, 0, 20)
playersOnlineLabel.BackgroundTransparency = 1
playersOnlineLabel.Text = "👥 0/" .. Players.MaxPlayers
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

local executorLabel = Instance.new("TextLabel")
executorLabel.Size = UDim2.new(1, -8, 0, 20)
executorLabel.BackgroundTransparency = 1
executorLabel.Text = "⚡ Executor: " .. (identifyexecutor and identifyexecutor() or "Desconhecido")
executorLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
executorLabel.Font = Enum.Font.SourceSans
executorLabel.TextSize = 12
executorLabel.Parent = ScrollMain

CreateActionButton(ScrollMain, "🔄 Reconectar", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId)
end)

CreateActionButton(ScrollMain, "👥 Server Cheio", function()
    local PlaceId = game.PlaceId
    local Api = "https://games.roblox.com/v1/games/" .. PlaceId .. "/servers/Public?sortOrder=Desc&limit=100"
    local function FindServer()
        local cursor
        repeat
            local data = HttpService:JSONDecode(game:HttpGet(Api .. (cursor and "&cursor=" .. cursor or "")))
            for _, s in pairs(data.data) do
                if s.playing < s.maxPlayers and s.playing >= 21 then return s.id end
            end
            cursor = data.nextPageCursor
        until not cursor
    end
    local id = FindServer()
    if id then TeleportService:TeleportToPlaceInstance(PlaceId, id, LocalPlayer) end
end)

CreateActionButton(ScrollMain, "🔄 Mudar Server", function()
    local _place = game.PlaceId
    local _url = "https://games.roblox.com/v1/games/" .. _place .. "/servers/Public?sortOrder=Asc&limit=100"
    local Server, Next
    repeat
        local data = HttpService:JSONDecode(game:HttpGet(_url .. (Next and "&cursor=" .. Next or "")))
        Server = data.data[1]
        Next = data.nextPageCursor
    until Server
    TeleportService:TeleportToPlaceInstance(_place, Server.id, LocalPlayer)
end)

-- =========================================================
-- JOGADOR
-- =========================================================
CreateSectionHeader(ScrollMain, "🏃 JOGADOR")

CreateActionButton(ScrollMain, "🕊️ Fly", function()
    loadstring(game:HttpGet("https://pastebin.com/raw/QjjQvMsE"))()
end)

local noclipActive = false
local noclipConn = nil
CreateToggleButton(ScrollMain, "🚪 Noclip", function(state)
    noclipActive = state
    if noclipConn then noclipConn:Disconnect() noclipConn = nil end
    if noclipActive then
        noclipConn = RunService.Stepped:Connect(function()
            if not LocalPlayer.Character then return end
            for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end
        end)
    end
end)

CreateSlider(ScrollMain, "Velocidade", 1, 200, 16, function(val)
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.WalkSpeed = val end
end)

CreateSlider(ScrollMain, "Pulo", 1, 500, 50, function(val)
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.UseJumpPower = true hum.JumpPower = val end
end)

CreateSlider(ScrollMain, "Gravidade", 0, 500, 196, function(val)
    Workspace.Gravity = val
end)

-- =========================================================
-- ESP
-- =========================================================
CreateSectionHeader(ScrollMain, "👁️ ESP")

local espActive = false
local espConn = nil
CreateToggleButton(ScrollMain, "👤 ESP Nome", function(state)
    espActive = state
    if espConn then espConn:Disconnect() espConn = nil end
    if espActive then
        espConn = RunService.Heartbeat:Connect(function()
            local localChar = LocalPlayer.Character
            if not localChar or not localChar:FindFirstChild("Head") then return end
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
                    local head = player.Character.Head
                    local dist = (localChar.Head.Position - head.Position).Magnitude
                    local bg = head:FindFirstChild("KylerESP")
                    if not bg then
                        bg = Instance.new("BillboardGui")
                        bg.Name = "KylerESP"
                        bg.Adornee = head
                        bg.Size = UDim2.new(0, 100, 0, 40)
                        bg.StudsOffset = Vector3.new(0, 2, 0)
                        bg.AlwaysOnTop = true
                        bg.Parent = head
                        local lbl = Instance.new("TextLabel")
                        lbl.Name = "NameLabel"
                        lbl.Size = UDim2.new(1, 0, 1, 0)
                        lbl.BackgroundTransparency = 1
                        lbl.TextColor3 = Color3.fromRGB(170, 85, 255)
                        lbl.TextStrokeColor3 = Color3.new(0, 0, 0)
                        lbl.TextStrokeTransparency = 0
                        lbl.TextScaled = true
                        lbl.Text = player.Name .. "\n" .. math.floor(dist) .. "m"
                        lbl.Parent = bg
                    else
                        local lbl = bg:FindFirstChild("NameLabel")
                        if lbl then lbl.Text = player.Name .. "\n" .. math.floor(dist) .. "m" end
                    end
                end
            end
        end)
    else
        for _, p in ipairs(Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("Head") then
                local t = p.Character.Head:FindFirstChild("KylerESP")
                if t then t:Destroy() end
            end
        end
    end
end)

local hlStorage = nil
local hlConns = {}
CreateToggleButton(ScrollMain, "✨ ESP Holograma", function(state)
    if not state then
        if hlStorage then hlStorage:Destroy() hlStorage = nil end
        for _, c in pairs(hlConns) do if c and c.Disconnect then c:Disconnect() end end
        hlConns = {}
        return
    end
    hlStorage = Instance.new("Folder")
    hlStorage.Name = "KylerHighlightStorage"
    hlStorage.Parent = CoreGui
    local function HP(plr)
        if plr == LocalPlayer then return end
        local hl = Instance.new("Highlight")
        hl.Name = plr.Name
        hl.FillColor = Color3.fromRGB(170, 85, 255)
        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
        hl.FillTransparency = 0.5
        hl.OutlineColor = Color3.fromRGB(0, 0, 0)
        hl.OutlineTransparency = 0
        hl.Parent = hlStorage
        if plr.Character then hl.Adornee = plr.Character end
        hlConns[plr] = plr.CharacterAdded:Connect(function(c) hl.Adornee = c end)
    end
    hlConns["_pa"] = Players.PlayerAdded:Connect(HP)
    for _, v in ipairs(Players:GetPlayers()) do HP(v) end
end)

-- =========================================================
-- RGB
-- =========================================================
CreateSectionHeader(ScrollMain, "🎨 RGB")

CreateToggleButton(ScrollMain, "🌈 Nome RGB", function(state)
    if not state then return end
    task.spawn(function()
        local colors = {
            Color3.fromRGB(255,0,0), Color3.fromRGB(255,102,0), Color3.fromRGB(255,255,0),
            Color3.fromRGB(0,255,0), Color3.fromRGB(0,255,255), Color3.fromRGB(0,102,255),
            Color3.fromRGB(153,0,255), Color3.fromRGB(255,0,255)
        }
        local i = 1
        while state do
            pcall(function()
                local r = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1RPNam1eColo1r")
                if r then r:FireServer("PickingRPNameColor", colors[i]) end
            end)
            i = i % #colors + 1
            task.wait(0.3)
        end
    end)
end)

CreateToggleButton(ScrollMain, "🌈 Corpo RGB", function(state)
    if not state then return end
    task.spawn(function()
        local colors = {"Bright red","Lime green","Bright blue","Bright yellow","Bright cyan","Hot pink"}
        local i = 1
        while state do
            pcall(function()
                local r = ReplicatedStorage:FindFirstChild("Remotes") and ReplicatedStorage.Remotes:FindFirstChild("ChangeBodyColor")
                if r then r:FireServer(colors[i]) end
            end)
            i = i % #colors + 1
            task.wait(0.5)
        end
    end)
end)

local copyTarget = nil
CreateTextBox(ScrollMain, "Alvo copiar", "Nome do jogador", function(v) copyTarget = v end)

CreateActionButton(ScrollMain, "📋 Copiar Avatar", function()
    if not copyTarget or copyTarget == "" then return end
    local TPlayer = Players:FindFirstChild(copyTarget)
    if not TPlayer or not TPlayer.Character then return end
    local THum = TPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not THum then return end
    local Remotes = ReplicatedStorage:WaitForChild("Remotes")
    local PDesc = THum:GetAppliedDescription()
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
-- FLING
-- =========================================================
CreateSectionHeader(ScrollMain, "🔥 FLING")

local function PegarSofa()
    local char = LocalPlayer.Character
    if not char then return false end
    pcall(function()
        ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Clea1rTool1s"):FireServer("ClearAllTools")
    end)
    task.wait(0.2)
    pcall(function()
        ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
    end)
    task.wait(0.3)
    local tool = LocalPlayer.Backpack:FindFirstChild("Couch")
    if not tool then return false end
    tool.Parent = char
    task.wait(0.1)
    return char:FindFirstChild("Couch") ~= nil
end

CreateActionButton(ScrollMain, "⚽ Fling Bola", function()
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot or not myRoot then return end
    local tHum = target.Character:FindFirstChildOfClass("Humanoid")
    local ball = Instance.new("Part")
    ball.Size = Vector3.new(2, 2, 2)
    ball.Shape = Enum.PartType.Ball
    ball.Material = Enum.Material.SmoothPlastic
    ball.BrickColor = BrickColor.new("Bright red")
    ball.Anchored = false
    ball.CanCollide = true
    ball.Transparency = 0.3
    ball.Parent = Workspace
    ball.CFrame = tRoot.CFrame * CFrame.new(0, 2, 0)
    for i = 1, 25 do
        if not target.Character or not tRoot.Parent then break end
        local dir = Vector3.new(math.random(-8000,8000), math.random(3000,15000), math.random(-8000,8000))
        tRoot.AssemblyLinearVelocity = dir
        tRoot.AssemblyAngularVelocity = Vector3.new(math.random(-2000,2000), math.random(-2000,2000), math.random(-2000,2000))
        ball.CFrame = tRoot.CFrame * CFrame.new(0, 2, 0)
        ball.AssemblyLinearVelocity = dir
        if tHum and i % 5 == 0 then tHum.Health = tHum.Health - 10 end
        task.wait()
    end
    ball:Destroy()
    print("✅ Fling Bola em " .. target.Name)
end)

CreateActionButton(ScrollMain, "🛋️ Fling Sofá", function()
    local target = GetTarget()
    if not target or not target.Character then return end
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    local root = char:FindFirstChild("HumanoidRootPart")
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not hum or not root or not tRoot then return end
    if not PegarSofa() then return end
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
        while tick() - st < 5 and target and target.Character and target.Character:FindFirstChildOfClass("Humanoid") do
            local tHum = target.Character:FindFirstChildOfClass("Humanoid")
            if not tHum or tHum.Sit then break end
            local hrp = target.Character.HumanoidRootPart
            angle = angle + 50
            root.CFrame = CFrame.new(hrp.Position + hrp.Velocity / 1.5 + Vector3.new(0, 2, 0)) * CFrame.Angles(math.rad(angle), 0, 0)
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
        root.CFrame = CFrame.new(145.51, -350.09, 21.58)
        task.wait(0.3)
        local tool = char:FindFirstChild("Couch")
        if tool then tool.Parent = LocalPlayer.Backpack end
        task.wait(0.01)
        pcall(function()
            ReplicatedStorage.RE:FindFirstChild("1Too1l"):InvokeServer("PickingTools", "Couch")
        end)
        task.wait(0.2)
        root.CFrame = originalPos
        print("✅ Fling Sofá em " .. target.Name)
    end)
end)

-- =========================================================
-- TELEPORT
-- =========================================================
CreateSectionHeader(ScrollMain, "📍 TELEPORT")

for _, tp in ipairs({
    {"🏠 Spawn",    CFrame.new(-26, 4, -23)},
    {"🏥 Hospital", CFrame.new(-309, 4, 71)},
    {"🏫 Escola",   CFrame.new(-312, 4, 211)},
    {"🏦 Banco",    CFrame.new(2.28, 4.65, 254.58)},
    {"🛍️ Loja",    CFrame.new(-46.15, 4.65, 253.20)},
    {"🏝️ Ilha",    CFrame.new(-1925, 23, 127)},
}) do
    CreateActionButton(ScrollMain, tp[1], function()
        local r = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if r then r.CFrame = tp[2] end
    end)
end

-- =========================================================
-- ÁUDIO
-- =========================================================
CreateSectionHeader(ScrollMain, "🎵 ÁUDIO")

local selectedAudioID = nil
CreateTextBox(ScrollMain, "ID do Áudio", "Digite o ID", function(v)
    local id = tonumber(v)
    if id then selectedAudioID = id end
end)

CreateActionButton(ScrollMain, "▶️ Tocar Áudio", function()
    if not selectedAudioID then return end
    pcall(function()
        ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(Workspace, selectedAudioID, 1)
    end)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://" .. selectedAudioID
    sound.Parent = (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")) or Workspace
    sound:Play()
    task.wait(sound.TimeLength or 3)
    sound:Destroy()
end)

CreateToggleButton(ScrollMain, "🔄 Loop Áudio", function(state)
    if not state then return end
    task.spawn(function()
        while state do
            if selectedAudioID then
                pcall(function()
                    ReplicatedStorage.RE:FindFirstChild("1Gu1nSound1s"):FireServer(Workspace, selectedAudioID, 1)
                end)
                local sound = Instance.new("Sound")
                sound.SoundId = "rbxassetid://" .. selectedAudioID
                sound.Parent = (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")) or Workspace
                sound:Play()
                task.wait(0.5)
                sound:Destroy()
            end
            task.wait(0.5)
        end
    end)
end)

-- =========================================================
-- VEÍCULO
-- =========================================================
CreateSectionHeader(ScrollMain, "🚗 VEÍCULO")

CreateActionButton(ScrollMain, "🚗 Fly Car", function()
    loadstring(game:HttpGet("https://pastefy.app/RtliHFjP/raw"))()
end)

CreateActionButton(ScrollMain, "💥 Remover Carros", function()
    local v = Workspace:FindFirstChild("Vehicles")
    if v then for _, c in pairs(v:GetChildren()) do c:Destroy() end end
end)

CreateToggleButton(ScrollMain, "🌈 Carro RGB", function(state)
    if not state then return end
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
                    local r = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Player1sCa1r")
                    if r then r:FireServer("PickingCarColor", color) end
                end)
                task.wait(1)
            end
        end
    end)
end)

-- =========================================================
-- CASA
-- =========================================================
CreateSectionHeader(ScrollMain, "🏠 CASA")

CreateToggleButton(ScrollMain, "🌈 Casa RGB", function(state)
    if not state then return end
    task.spawn(function()
        local colors = {
            Color3.new(1,0,0), Color3.new(0,1,0), Color3.new(0,0,1),
            Color3.new(1,1,0), Color3.new(0,1,1), Color3.new(1,0,1)
        }
        while state do
            for _, color in ipairs(colors) do
                if not state then return end
                pcall(function()
                    local r = ReplicatedStorage:FindFirstChild("RE") and ReplicatedStorage.RE:FindFirstChild("1Player1sHous1e")
                    if r then r:FireServer("ColorPickHouse", color) end
                end)
                task.wait(0.8)
            end
        end
    end)
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
                if allow and allow:IsA("RemoteEvent") then allow:FireServer(LocalPlayer) end
            end
        end
    end
end)

CreateActionButton(ScrollMain, "🏠 TP Casa", function()
    local lots = Workspace:FindFirstChild("001_Lots")
    if lots then
        for _, House in pairs(lots:GetChildren()) do
            if House.Name ~= "For Sale" and House:IsA("Model") then
                local r = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                if r then r.CFrame = CFrame.new(House.WorldPivot.Position) return end
            end
        end
    end
end)

-- =========================================================
-- CONFIG
-- =========================================================
CreateSectionHeader(ScrollMain, "⚙️ CONFIG")

CreateTextBox(ScrollMain, "ID Servidor", "Digite o ID", function(value)
    if value and value ~= "" then
        pcall(function() TeleportService:TeleportToPlaceInstance(game.PlaceId, value, LocalPlayer) end)
    end
end)

CreateActionButton(ScrollMain, "📋 Copiar ID", function()
    if setclipboard then setclipboard(tostring(game.JobId or "")) end
end)

CreateActionButton(ScrollMain, "🔒 Shift Lock", function()
    if CoreGui:FindFirstChild("ShiftLockKyler") then return end
    local sg = Instance.new("ScreenGui")
    sg.Name = "ShiftLockKyler"
    sg.Parent = CoreGui
    sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    sg.ResetOnSpawn = false
    local lb = Instance.new("ImageButton")
    lb.Size = UDim2.new(0, 50, 0, 50)
    lb.Position = UDim2.new(0.785, 0, 0.865, 0)
    lb.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    lb.BackgroundTransparency = 0.3
    lb.Parent = sg
    lb.Image = ""
    Instance.new("UICorner", lb).CornerRadius = UDim.new(1, 0)
    local icon = Instance.new("ImageLabel")
    icon.Size = UDim2.new(0.7, 0, 0.7, 0)
    icon.Position = UDim2.new(0.15, 0, 0.15, 0)
    icon.BackgroundTransparency = 1
    icon.Image = "rbxasset://textures/ui/mouseLock_off.png"
    icon.ImageColor3 = Color3.fromRGB(255, 255, 255)
    icon.ScaleType = Enum.ScaleType.Fit
    icon.Parent = lb
    local active = false
    local GS = UserSettings():GetService("UserGameSettings")
    local conn = nil
    lb.MouseButton1Click:Connect(function()
        active = not active
        if active then
            icon.ImageColor3 = Color3.fromRGB(0, 170, 255)
            conn = RunService.RenderStepped:Connect(function()
                pcall(function() GS.RotationType = Enum.RotationType.CameraRelative end)
            end)
        else
            icon.ImageColor3 = Color3.fromRGB(255, 255, 255)
            if conn then conn:Disconnect() conn = nil end
            pcall(function() GS.RotationType = Enum.RotationType.MovementRelative end)
        end
    end)
end)

-- =========================================================
-- GRÁFICO LITE
-- =========================================================
CreateSectionHeader(ScrollMain, "🎮 GRÁFICO LITE")

CreateToggleButton(ScrollMain, "📉 Gráfico Lite", function(state)
    if not state then return end
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9
    Lighting.Brightness = 1
    Lighting.EnvironmentDiffuseScale = 0
    Lighting.EnvironmentSpecularScale = 0
    Lighting.Technology = Enum.Technology.Legacy
    for _, v in pairs(Lighting:GetDescendants()) do
        if v:IsA("PostEffect") or v:IsA("BloomEffect") or v:IsA("BlurEffect")
        or v:IsA("ColorCorrectionEffect") or v:IsA("Sky") then
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
end)

-- =========================================================
-- 😈 ABA TROLL — BROOKHAVEN
-- =========================================================
CreateSectionHeader(ScrollTroll, "😈 TROLL — BROOKHAVEN")

-- TP em cima do alvo
CreateActionButton(ScrollTroll, "📍 TP em cima do alvo", function()
    local target = GetTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot or not myRoot then return end
    myRoot.CFrame = tRoot.CFrame * CFrame.new(0, 4, 0)
    print("✅ TP em cima de " .. target.Name)
end)

-- Seguir alvo
local followActive = false
local followConn = nil
CreateToggleButton(ScrollTroll, "🏃 Seguir Alvo", function(state)
    followActive = state
    if followConn then followConn:Disconnect() followConn = nil end
    if not followActive then return end
    followConn = RunService.Heartbeat:Connect(function()
        local target = GetTarget()
        local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if not target or not target.Character or not myRoot then return end
        local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
        if not tRoot then return end
        myRoot.CFrame = tRoot.CFrame * CFrame.new(0, 0, 3)
    end)
end)

-- Arrastar alvo (BodyPosition no HRP do alvo)
local dragActive = false
local dragConn = nil
local dragBP = nil
CreateToggleButton(ScrollTroll, "🧲 Arrastar Alvo", function(state)
    dragActive = state
    if dragConn then dragConn:Disconnect() dragConn = nil end
    if dragBP then dragBP:Destroy() dragBP = nil end
    if not dragActive then return end
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    dragBP = Instance.new("BodyPosition")
    dragBP.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    dragBP.P = 50000
    dragBP.D = 500
    dragBP.Position = tRoot.Position
    dragBP.Parent = tRoot
    dragConn = RunService.Heartbeat:Connect(function()
        if not dragActive or not tRoot.Parent then
            if dragBP then dragBP:Destroy() dragBP = nil end
            if dragConn then dragConn:Disconnect() dragConn = nil end
            return
        end
        local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if myRoot then
            local targetPos = myRoot.CFrame * CFrame.new(0, 0, -4)
            dragBP.Position = targetPos.Position
        end
    end)
end)

-- Spin no alvo (BodyAngularVelocity)
local spinActive = false
local spinConn = nil
local spinBAV = nil
CreateToggleButton(ScrollTroll, "🌀 Spin no Alvo", function(state)
    spinActive = state
    if spinConn then spinConn:Disconnect() spinConn = nil end
    if spinBAV then spinBAV:Destroy() spinBAV = nil end
    if not spinActive then return end
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    spinBAV = Instance.new("BodyAngularVelocity")
    spinBAV.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
    spinBAV.AngularVelocity = Vector3.new(0, 50, 0)
    spinBAV.P = 100000
    spinBAV.Parent = tRoot
    spinConn = RunService.Heartbeat:Connect(function()
        if not spinActive or not tRoot.Parent then
            if spinBAV then spinBAV:Destroy() spinBAV = nil end
            if spinConn then spinConn:Disconnect() spinConn = nil end
        end
    end)
end)

-- Arremessar alvo
CreateActionButton(ScrollTroll, "🚀 Arremessar Alvo", function()
    local target = GetTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    task.spawn(function()
        for i = 1, 15 do
            if not tRoot.Parent then break end
            tRoot.AssemblyLinearVelocity = Vector3.new(
                math.random(-300, 300),
                math.random(600, 1200),
                math.random(-300, 300)
            )
            tRoot.AssemblyAngularVelocity = Vector3.new(
                math.random(-100, 100),
                math.random(-100, 100),
                math.random(-100, 100)
            )
            task.wait(0.05)
        end
    end)
    print("✅ Arremessado!")
end)

-- Explodir alvo
CreateActionButton(ScrollTroll, "💥 Explodir Alvo", function()
    local target = GetTarget()
    if not target or not target.Character then print("❌ Sem alvo!") return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    task.spawn(function()
        for i = 1, 20 do
            if not tRoot.Parent then break end
            tRoot.AssemblyLinearVelocity = Vector3.new(
                math.random(-5000, 5000),
                math.random(2000, 8000),
                math.random(-5000, 5000)
            )
            tRoot.AssemblyAngularVelocity = Vector3.new(
                math.random(-1000, 1000),
                math.random(-1000, 1000),
                math.random(-1000, 1000)
            )
            task.wait(0.03)
        end
    end)
    print("✅ Explodido!")
end)

-- Travar alvo (BodyPosition fixado)
local freezeActive = false
local freezeConn = nil
local freezeBP = nil
CreateToggleButton(ScrollTroll, "🧊 Travar Alvo", function(state)
    freezeActive = state
    if freezeConn then freezeConn:Disconnect() freezeConn = nil end
    if freezeBP then freezeBP:Destroy() freezeBP = nil end
    if not freezeActive then return end
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    freezeBP = Instance.new("BodyPosition")
    freezeBP.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    freezeBP.P = 100000
    freezeBP.D = 1000
    freezeBP.Position = tRoot.Position
    freezeBP.Parent = tRoot
    freezeConn = RunService.Heartbeat:Connect(function()
        if not freezeActive or not tRoot.Parent then
            if freezeBP then freezeBP:Destroy() freezeBP = nil end
            if freezeConn then freezeConn:Disconnect() freezeConn = nil end
        end
    end)
    print("✅ Alvo travado!")
end)

-- Câmera no alvo
local camActive = false
CreateToggleButton(ScrollTroll, "📷 Câmera no Alvo", function(state)
    camActive = state
    if state then
        local target = GetTarget()
        if not target or not target.Character then return end
        local hum = target.Character:FindFirstChildOfClass("Humanoid")
        if hum then Workspace.CurrentCamera.CameraSubject = hum end
    else
        local myHum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if myHum then Workspace.CurrentCamera.CameraSubject = myHum end
    end
end)

-- Loop TP no alvo
local loopTpActive = false
CreateToggleButton(ScrollTroll, "🔁 Loop TP no Alvo", function(state)
    loopTpActive = state
    if not state then return end
    task.spawn(function()
        while loopTpActive do
            local target = GetTarget()
            local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if target and target.Character and myRoot then
                local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
                if tRoot then
                    myRoot.CFrame = tRoot.CFrame * CFrame.new(math.random(-2, 2), 2, math.random(-2, 2))
                end
            end
            task.wait(0.08)
        end
    end)
end)

-- Carregar alvo (BodyPosition colada em você)
local carryActive = false
local carryConn = nil
local carryBP = nil
CreateToggleButton(ScrollTroll, "🤝 Carregar Alvo", function(state)
    carryActive = state
    if carryConn then carryConn:Disconnect() carryConn = nil end
    if carryBP then carryBP:Destroy() carryBP = nil end
    if not carryActive then return end
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    carryBP = Instance.new("BodyPosition")
    carryBP.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    carryBP.P = 50000
    carryBP.D = 500
    carryBP.Position = tRoot.Position
    carryBP.Parent = tRoot
    carryConn = RunService.Heartbeat:Connect(function()
        if not carryActive or not tRoot.Parent then
            if carryBP then carryBP:Destroy() carryBP = nil end
            if carryConn then carryConn:Disconnect() carryConn = nil end
            return
        end
        local myRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if myRoot then
            local cf = myRoot.CFrame * CFrame.new(0, 2, -3)
            carryBP.Position = cf.Position
        end
    end)
    print("✅ Carregando " .. target.Name)
end)

-- Vibrar alvo
local vibrateActive = false
local vibrateConn = nil
CreateToggleButton(ScrollTroll, "📳 Vibrar Alvo", function(state)
    vibrateActive = state
    if vibrateConn then vibrateConn:Disconnect() vibrateConn = nil end
    if not vibrateActive then return end
    local target = GetTarget()
    if not target or not target.Character then return end
    local tRoot = target.Character:FindFirstChild("HumanoidRootPart")
    if not tRoot then return end
    local dir = 1
    vibrateConn = RunService.Heartbeat:Connect(function()
        if not vibrateActive or not tRoot.Parent then
            if vibrateConn then vibrateConn:Disconnect() vibrateConn = nil end
            return
        end
        dir = -dir
        tRoot.AssemblyLinearVelocity = Vector3.new(dir * 80, 0, dir * 80)
    end)
end)

-- Speed x10
CreateActionButton(ScrollTroll, "⚡ Speed x10", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.WalkSpeed = 160 end
end)

CreateActionButton(ScrollTroll, "🔄 Reset Speed", function()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
    if hum then hum.WalkSpeed = 16 end
end)

-- Chat spam
local chatSpamActive = false
CreateToggleButton(ScrollTroll, "💬 Chat Spam", function(state)
    chatSpamActive = state
    if not state then return end
    task.spawn(function()
        local msgs = {
            "👀 olha pra cima", "kkkkkkkk", "fugiu não",
            "🔥 KYLER HUB", "sumiu não hein", "😂😂😂",
            "vai tomar!", "tá travado irmão"
        }
        local i = 1
        while chatSpamActive do
            pcall(function()
                local chatRemote = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
                if chatRemote then
                    local sayMsg = chatRemote:FindFirstChild("SayMessageRequest")
                    if sayMsg then sayMsg:FireServer(msgs[i], "All") end
                end
            end)
            pcall(function()
                game:GetService("Chat"):Chat(
                    LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head"),
                    msgs[i],
                    Enum.ChatColor.White
                )
            end)
            i = i % #msgs + 1
            task.wait(1.2)
        end
    end)
end)

-- Resetar câmera
CreateActionButton(ScrollTroll, "🎥 Resetar Câmera", function()
    local myHum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if myHum then Workspace.CurrentCamera.CameraSubject = myHum end
end)

-- =========================================================
-- HOTKEY
-- =========================================================
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

print("✅ KYLER HUB 3.1 CARREGADO! 🔥")
print("💡 RightShift para abrir/fechar")
print("💕 Feito com amor pro meu baby!")
