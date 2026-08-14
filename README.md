-- =========================================================
-- ⚡ KYLER HUB (Edição Drip Solutions) - Roblox GUI [FIXED]
-- =========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Destrói instâncias anteriores
if PlayerGui:FindFirstChild("KylerHubGui") then
    PlayerGui.KylerHubGui:Destroy()
end

-- ---------------------------------------------------------
-- 1. ESTRUTURA DA INTERFACE (GUI)
-- ---------------------------------------------------------
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "KylerHubGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 420, 0, 530)
MainFrame.Position = UDim2.new(0.5, -210, 0.5, -265)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 22)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 8)
MainCorner.Parent = MainFrame

-- Barra de Título
local TitleBar = Instance.new("Frame")
TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1, 0, 0, 40)
TitleBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleCorner = Instance.new("UICorner")
TitleCorner.CornerRadius = UDim.new(0, 8)
TitleCorner.Parent = TitleBar

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, -90, 1, 0)
TitleText.Position = UDim2.new(0, 10, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.Text = "⚡ KYLER HUB | Drip Client Solutions"
TitleText.TextColor3 = Color3.fromRGB(170, 85, 255)
TitleText.TextSize = 18
TitleText.Font = Enum.Font.SourceSansBold
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Parent = TitleBar

local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -70, 0, 5)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinimizeButton.Text = "➖"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 14
MinimizeButton.Parent = TitleBar

-- FIX: Minimizar só esconde, não destrói
MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14
CloseButton.Parent = TitleBar

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Container com ROLAGEM
local ScrollFrame = Instance.new("ScrollingFrame")
ScrollFrame.Name = "ScrollFrame"
ScrollFrame.Size = UDim2.new(1, -20, 1, -50)
ScrollFrame.Position = UDim2.new(0, 10, 0, 45)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.BorderSizePixel = 0
ScrollFrame.ScrollBarThickness = 6
ScrollFrame.CanvasSize = UDim2.new(0, 0, 0, 2000)
ScrollFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ScrollFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 8)

-- ---------------------------------------------------------
-- 2. FUNÇÕES GERADORAS DE COMPONENTES
-- ---------------------------------------------------------
local function CreateToggleButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -10, 0, 35)
    button.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    button.Text = text .. " [OFF]"
    button.TextColor3 = Color3.fromRGB(200, 200, 200)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 15
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
    frame.Size = UDim2.new(1, -10, 0, 45)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.Text = text .. ": " .. tostring(default)
    label.TextColor3 = Color3.fromRGB(220, 220, 220)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 14
    label.Parent = frame

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -20, 0, 10)
    button.Position = UDim2.new(0, 10, 0, 25)
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
    label.Size = UDim2.new(1, -10, 0, 25)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(180, 120, 255)
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 16
    label.Parent = ScrollFrame
end

local function CreateActionButton(text, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, -10, 0, 32)
    button.BackgroundColor3 = Color3.fromRGB(40, 35, 45)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(240, 240, 240)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 14
    button.Parent = ScrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = button

    button.MouseButton1Click:Connect(callback)
    return button
end

-- ---------------------------------------------------------
-- 3. CAMPO DE SELEÇÃO DE JOGADOR ALVO
-- ---------------------------------------------------------
CreateSectionHeader("🎯 SELEÇÃO DE ALVO")

local TargetInput = Instance.new("TextBox")
TargetInput.Size = UDim2.new(1, -10, 0, 35)
TargetInput.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
TargetInput.PlaceholderText = "Digite o nome do jogador alvo..."
TargetInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetInput.Font = Enum.Font.SourceSans
TargetInput.TextSize = 14
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

-- ---------------------------------------------------------
-- 4. FUNÇÕES DO SISTEMA "TROLAR" - FLING REESCRITO
-- ---------------------------------------------------------
CreateSectionHeader("🔥 TROLAR (Fling & Orbitar)")

-- FLING - VERSÃO REESCRITA COM UserInputService
local clickFling = false
local flingConnection = nil
local ballObject = nil
local isFlinging = false

local function FindBallInWorkspace()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and (obj.Name:lower():find("ball") or obj.Name:lower():find("bola")) then
            return obj
        end
        if obj:IsA("Tool") and obj.Name:lower():find("ball") then
            local handle = obj:FindFirstChild("Handle")
            if handle then return handle end
            return obj
        end
    end
    return nil
end

local function CreateBall()
    local newBall = Instance.new("Part")
    newBall.Name = "KylerFlingBall"
    newBall.Size = Vector3.new(2, 2, 2)
    newBall.Shape = Enum.PartType.Ball
    newBall.BrickColor = BrickColor.new("Bright red")
    newBall.Material = Enum.Material.SmoothPlastic
    newBall.Anchored = false
    newBall.CanCollide = true
    newBall.Transparency = 0
    newBall.Position = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(0, 10, 0) or Vector3.new(0, 10, 0)
    newBall.Parent = Workspace
    
    local highlight = Instance.new("Highlight")
    highlight.Name = "BallHighlight"
    highlight.FillColor = Color3.fromRGB(255, 0, 0)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 0)
    highlight.Parent = newBall
    
    return newBall
end

-- FUNÇÃO QUE EXECUTA O FLING
local function ExecuteFling()
    if isFlinging then return end
    isFlinging = true
    
    local targetPlayer = GetTargetPlayer()
    if not targetPlayer then
        print("❌ Nenhum alvo selecionado!")
        isFlinging = false
        return
    end
    
    local targetChar = targetPlayer.Character
    if not targetChar then
        print("❌ Alvo sem personagem!")
        isFlinging = false
        return
    end
    
    local targetHrp = targetChar:FindFirstChild("HumanoidRootPart")
    if not targetHrp then
        print("❌ Alvo sem HumanoidRootPart!")
        isFlinging = false
        return
    end
    
    local humanoid = targetChar:FindFirstChildOfClass("Humanoid")
    if not humanoid then
        print("❌ Alvo sem Humanoid!")
        isFlinging = false
        return
    end
    
    if not ballObject or not ballObject.Parent then
        ballObject = CreateBall()
    end
    
    task.spawn(function()
        if not targetHrp or not targetHrp.Parent then
            isFlinging = false
            return
        end
        
        ballObject.CFrame = targetHrp.CFrame * CFrame.new(0, 1, 0)
        ballObject.AssemblyLinearVelocity = Vector3.zero
        ballObject.AssemblyAngularVelocity = Vector3.zero
        ballObject.Transparency = 0
        ballObject.CanCollide = true
        
        local weld = Instance.new("Weld")
        weld.Part0 = ballObject
        weld.Part1 = targetHrp
        weld.C0 = CFrame.new(0, 1, 0)
        weld.Parent = ballObject
        
        for i = 1, 20 do
            if not targetHrp or not targetHrp.Parent then
                break
            end
            
            local randomDir = Vector3.new(
                math.random(-25000, 25000),
                math.random(5000, 35000),
                math.random(-25000, 25000)
            )
            
            targetHrp.AssemblyLinearVelocity = randomDir
            targetHrp.AssemblyAngularVelocity = Vector3.new(
                math.random(-20000, 20000),
                math.random(-20000, 20000),
                math.random(-20000, 20000)
            )
            
            if ballObject and ballObject.Parent then
                ballObject.AssemblyLinearVelocity = randomDir
                ballObject.AssemblyAngularVelocity = Vector3.new(
                    math.random(-20000, 20000),
                    math.random(-20000, 20000),
                    math.random(-20000, 20000)
                )
            end
            
            if i % 5 == 0 and humanoid and humanoid.Parent then
                humanoid.Health = humanoid.Health - 15
            end
            
            RunService.RenderStepped:Wait()
        end
        
        if weld and weld.Parent then
            weld:Destroy()
        end
        
        if ballObject and ballObject.Parent then
            ballObject.AssemblyLinearVelocity = Vector3.zero
            ballObject.AssemblyAngularVelocity = Vector3.zero
            if targetHrp and targetHrp.Parent then
                ballObject.CFrame = targetHrp.CFrame * CFrame.new(0, -5, 0)
            end
            ballObject.Transparency = 0.5
        end
        
        if targetHrp and targetHrp.Parent then
            targetHrp.AssemblyLinearVelocity = Vector3.zero
            targetHrp.AssemblyAngularVelocity = Vector3.zero
        end
        
        if humanoid and humanoid.Parent then
            humanoid.Health = humanoid.Health - 30
            if humanoid.Health > 0 then
                humanoid.Health = humanoid.Health - 20
            end
        end
        
        print("✅ Fling aplicado em " .. targetPlayer.Name)
        isFlinging = false
    end)
end

-- BOTÃO DO FLING (AGORA USA UserInputService)
CreateToggleButton("⚡ Fling Bola (Visível para Todos)", function(state)
    clickFling = state
    
    if clickFling then
        ballObject = FindBallInWorkspace()
        if not ballObject then
            ballObject = CreateBall()
            print("✅ Bola criada no Workspace!")
        else
            print("✅ Bola encontrada no Workspace!")
        end
        
        -- USA UserInputService EM VEZ DE GetMouse()
        flingConnection = UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if gameProcessed then return end
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                -- Verifica se NÃO clicou na GUI
                local mouse = LocalPlayer:GetMouse()
                if mouse and mouse.Target then
                    local targetGui = mouse.Target:FindFirstAncestorOfClass("ScreenGui")
                    if targetGui == ScreenGui then
                        return -- Ignora clique na GUI
                    end
                end
                ExecuteFling()
            end
        end)
    else
        if flingConnection then
            flingConnection:Disconnect()
            flingConnection = nil
        end
        
        if ballObject and ballObject.Parent and ballObject.Name == "KylerFlingBall" then
            ballObject:Destroy()
        end
        ballObject = nil
        isFlinging = false
    end
end)

-- Sistema de Orbitar Jogador
local orbitSpeed = 19
local orbiting = false
local orbitConnection = nil

CreateSlider("Velocidade <Tween>", 1, 50, 19, function(val)
    orbitSpeed = val
end)

CreateToggleButton("🪐 Órbita Jogador", function(state)
    if orbitConnection then
        orbitConnection:Disconnect()
        orbitConnection = nil
    end
    
    orbiting = state
    
    if orbiting then
        local angle = 0
        orbitConnection = RunService.RenderStepped:Connect(function()
            if not orbiting then
                orbitConnection:Disconnect()
                orbitConnection = nil
                return
            end
            
            local target = GetTargetPlayer()
            if not target or not target.Character or not target.Character:FindFirstChild("HumanoidRootPart") then
                print("❌ Alvo inválido ou morto, desativando órbita...")
                orbiting = false
                if orbitConnection then
                    orbitConnection:Disconnect()
                    orbitConnection = nil
                end
                return
            end
            
            if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                return
            end
            
            angle = angle + math.rad(orbitSpeed)
            local targetHrp = target.Character.HumanoidRootPart
            local offset = Vector3.new(math.cos(angle) * 10, 2, math.sin(angle) * 10)
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(targetHrp.Position + offset, targetHrp.Position)
        end)
    end
end)

-- ---------------------------------------------------------
-- 5. MÓDULO DE CARROS
-- ---------------------------------------------------------
CreateSectionHeader("🚗 CONTROLE DE CARROS")

local vehicleSpeed = 100
local vehicleTurbo = 50

CreateSlider("Definir Velocidade (Carro)", 10, 500, 100, function(val)
    vehicleSpeed = val
end)

CreateSlider("Definir Turbo (Carro)", 0, 300, 50, function(val)
    vehicleTurbo = val
end)

CreateActionButton("⚡ Mudar Velocidade e Turbo", function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        local seat = char.Humanoid.SeatPart
        if seat and seat:IsA("VehicleSeat") then
            seat.MaxSpeed = vehicleSpeed + vehicleTurbo
            seat.Torque = 100
            print("✅ Velocidade e Turbo ajustados!")
        end
    end
end)

CreateActionButton("🧲 Puxa Carros", function()
    local target = GetTargetPlayer()
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("VehicleSeat") and v.Occupant == nil and v.Parent then
                v.Parent.CFrame = target.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
                print("✅ Carro puxado!")
                break
            end
        end
    end
end)

CreateActionButton("🚀 Fly Carro Free", function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        local seat = char.Humanoid.SeatPart
        if seat and seat.Parent then
            local vehicle = seat.Parent
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(1e9, 1e9, 1e9)
            bv.Velocity = Workspace.CurrentCamera.CFrame.LookVector * 100
            bv.Parent = vehicle.PrimaryPart or seat
            task.wait(3)
            bv:Destroy()
            print("✅ Fly Carro ativado!")
        end
    end
end)

CreateActionButton("💥 Remover os Carros <Todos>", function()
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("VehicleSeat") and v.Parent then
            v.Parent:Destroy()
        end
    end
    print("✅ Todos os carros removidos!")
end)

CreateActionButton("🔧 Resetar Carros", function()
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("VehicleSeat") and v.Parent then
            v.Parent:SetPrimaryPartCFrame(CFrame.new(0, 10, 0))
        end
    end
    print("✅ Carros resetados!")
end)

local rainbowCar = false
local rainbowConnection = nil

CreateToggleButton("🌈 Carro Colorido <GamePass>", function(state)
    rainbowCar = state
    if rainbowConnection then
        rainbowConnection:Disconnect()
        rainbowConnection = nil
    end
    
    if rainbowCar then
        local hue = 0
        rainbowConnection = RunService.RenderStepped:Connect(function()
            if not rainbowCar then
                rainbowConnection:Disconnect()
                rainbowConnection = nil
                return
            end
            
            hue = (hue + 0.01) % 1
            local char = LocalPlayer.Character
            if char and char:FindFirstChildOfClass("Humanoid") then
                local seat = char.Humanoid.SeatPart
                if seat and seat.Parent then
                    for _, part in pairs(seat.Parent:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.Color = Color3.fromHSV(hue, 1, 1)
                        end
                    end
                end
            end
        end)
    end
end)

local focusCar = false
CreateToggleButton("🎥 Visualizar Carro (Foco Longe)", function(state)
    focusCar = state
    if not focusCar then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            Workspace.CurrentCamera.CameraSubject = humanoid
        end
    end
end)

CreateActionButton("🎯 Selecionar/Focar Carro Próximo", function()
    if focusCar then
        local foundCar = false
        for _, v in pairs(Workspace:GetDescendants()) do
            if v:IsA("VehicleSeat") and v.Occupant and v.Occupant == LocalPlayer then
                Workspace.CurrentCamera.CameraSubject = v
                print("✅ Focado no carro atual!")
                foundCar = true
                break
            end
        end
        
        if not foundCar then
            for _, v in pairs(Workspace:GetDescendants()) do
                if v:IsA("VehicleSeat") and v.Occupant then
                    Workspace.CurrentCamera.CameraSubject = v
                    print("✅ Focado em outro carro!")
                    foundCar = true
                    break
                end
            end
        end
        
        if not foundCar then
            print("❌ Nenhum carro com ocupante encontrado!")
        end
    else
        print("❌ Ative 'Visualizar Carro' primeiro!")
    end
end)

-- HOTKEY: RightShift pra abrir/fechar
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        MainFrame.Visible = not MainFrame.Visible
    end
end)

print("✅ KYLER HUB carregado com sucesso! | Drip Solutions 🔥")
print("💡 Pressione RightShift para abrir/fechar o menu")
