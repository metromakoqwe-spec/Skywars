-- SkyWars Minecraft-Style GUI | Precise Ores (Emerald, Diamond, Gold, Iron, Coal) + Infinite Jump & NoClip
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

if CoreGui:FindFirstChild("SkyWarsMinecraftGUI") then
    CoreGui.SkyWarsMinecraftGUI:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SkyWarsMinecraftGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = CoreGui

local function makeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

local OpenButton = Instance.new("TextButton")
OpenButton.Size = UDim2.new(0, 50, 0, 50)
OpenButton.Position = UDim2.new(0.05, 0, 0.2, 0)
OpenButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
OpenButton.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenButton.Text = "⚔️"
OpenButton.TextSize = 24
OpenButton.Parent = ScreenGui

local UICornerIcon = Instance.new("UICorner")
UICornerIcon.CornerRadius = UDim.new(1, 0)
UICornerIcon.Parent = OpenButton
makeDraggable(OpenButton)

local MainMenu = Instance.new("Frame")
MainMenu.Size = UDim2.new(0, 450, 0, 320)
MainMenu.Position = UDim2.new(0.3, 0, 0.3, 0)
MainMenu.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainMenu.Visible = false
MainMenu.Active = true
MainMenu.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainMenu
makeDraggable(MainMenu)

local TopBar = Instance.new("TextLabel")
TopBar.Size = UDim2.new(1, 0, 0, 35)
TopBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
TopBar.Text = "  SkyWars Client | Pro Edition"
TopBar.TextColor3 = Color3.fromRGB(255, 215, 0)
TopBar.TextSize = 14
TopBar.Font = Enum.Font.SourceSansBold
TopBar.TextXAlignment = Enum.TextXAlignment.Left
TopBar.Parent = MainMenu

local TopCorner = Instance.new("UICorner")
TopCorner.CornerRadius = UDim.new(0, 12)
TopCorner.Parent = TopBar

local Sidebar = Instance.new("Frame")
Sidebar.Size = UDim2.new(0, 120, 1, -35)
Sidebar.Position = UDim2.new(0, 0, 0, 35)
Sidebar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Sidebar.BorderSizePixel = 0
Sidebar.Parent = MainMenu

local ContentContainer = Instance.new("Frame")
ContentContainer.Size = UDim2.new(1, -120, 1, -35)
ContentContainer.Position = UDim2.new(0, 120, 0, 35)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainMenu

local Pages = {}
local function createPage(name)
    local page = Instance.new("ScrollingFrame")
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.Visible = false
    page.CanvasSize = UDim2.new(0, 0, 0, 150)
    page.ScrollBarThickness = 4
    page.Parent = ContentContainer
    Pages[name] = page
    return page
end

local pageHome = createPage("Home")
local pageVisuals = createPage("Visuals")
local pageSettings = createPage("Settings")
pageHome.Visible = true

local function createTabButton(name, text, targetPage)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -12, 0, 36)
    btn.Position = UDim2.new(0, 6, 0, 8 + (#Sidebar:GetChildren() * 44))
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = text
    btn.TextSize = 14
    btn.Font = Enum.Font.SourceSansBold
    btn.Parent = Sidebar

    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 8)
    c.Parent = btn

    btn.MouseButton1Click:Connect(function()
        for _, p in pairs(Pages) do p.Visible = false end
        targetPage.Visible = true
    end)
end

createTabButton("Home", "Главная", pageHome)
createTabButton("Visuals", "Визуалы", pageVisuals)
createTabButton("Settings", "Настройки", pageSettings)

-- Функция NoClip
local noclipEnabled = false
RunService.Stepped:Connect(function()
    if noclipEnabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Функция Infinite Jump (Бесконечный прыжок по воздуху)
local infJumpEnabled = false
UserInputService.JumpRequest:Connect(function()
    if infJumpEnabled and LocalPlayer.Character then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Кнопка NoClip на главной
local NoclipBtn = Instance.new("TextButton")
NoclipBtn.Size = UDim2.new(0.9, 0, 0, 38)
NoclipBtn.Position = UDim2.new(0.05, 0, 0, 10)
NoclipBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
NoclipBtn.Text = "NoClip: ВЫКЛ"
NoclipBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
NoclipBtn.TextSize = 13
NoclipBtn.Font = Enum.Font.SourceSansBold
NoclipBtn.Parent = pageHome

local cN = Instance.new("UICorner") cN.CornerRadius = UDim.new(1, 0) cN.Parent = NoclipBtn
NoclipBtn.MouseButton1Click:Connect(function()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        NoclipBtn.Text = "NoClip: ВКЛ"
        NoclipBtn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
    else
        NoclipBtn.Text = "NoClip: ВЫКЛ"
        NoclipBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
    end
end)

-- Кнопка Infinite Jump на главной
local InfJumpBtn = Instance.new("TextButton")
InfJumpBtn.Size = UDim2.new(0.9, 0, 0, 38)
InfJumpBtn.Position = UDim2.new(0.05, 0, 0, 55)
InfJumpBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
InfJumpBtn.Text = "Прыжки по воздуху: ВЫКЛ"
InfJumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
InfJumpBtn.TextSize = 13
InfJumpBtn.Font = Enum.Font.SourceSansBold
InfJumpBtn.Parent = pageHome

local cIJ = Instance.new("UICorner") cIJ.CornerRadius = UDim.new(1, 0) cIJ.Parent = InfJumpBtn
InfJumpBtn.MouseButton1Click:Connect(function()
    infJumpEnabled = not infJumpEnabled
    if infJumpEnabled then
        InfJumpBtn.Text = "Прыжки по воздуху: ВКЛ"
        InfJumpBtn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
    else
        InfJumpBtn.Text = "Прыжки по воздуху: ВЫКЛ"
        InfJumpBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
    end
end)

-- ==================== СТРОГИЙ ESP ТОЛЬКО ДЛЯ 5 РУД ====================

local oreEspEnabled = false
local oreConnections = {}

local function applyOreESP(part)
    if part:IsA("BasePart") and not part:FindFirstChild("OreHighlight") then
        if part.Position.Y > 80 then return end -- Игры только внизу, лобби вверху
        if part:FindFirstAncestorWhichIsA("Model") and Players:GetPlayerFromCharacter(part.Parent) then return end

        local color = part.Color
        local r, g, b = color.R * 255, color.G * 255, color.B * 255
        local name = string.lower(part.Name)
        local targetColor = nil
        
        -- 1. ИЗУМРУД (Ярко-зеленый)
        if (g > 170 and r < 80 and b < 100) or string.find(name, "emerald") or string.find(name, "изумруд") then
            targetColor = Color3.fromRGB(0, 255, 100)     
        -- 2. АЛМАЗ (Ярко-синий / голубой)
        elseif (r < 70 and g > 130 and b > 170) or string.find(name, "diamond") or string.find(name, "алмаз") then
            targetColor = Color3.fromRGB(0, 170, 255)     
        -- 3. ЗОЛОТО (Желтый)
        elseif (r > 190 and g > 170 and b < 60) or string.find(name, "gold") or string.find(name, "золот") then
            targetColor = Color3.fromRGB(255, 215, 0)     
        -- 4. ЖЕЛЕЗО (Светлые пятна, строго исключая серый фон камня)
        elseif (r > 160 and g > 150 and b > 160) or string.find(name, "iron") or string.find(name, "желез") then
            if math.abs(r - g) > 4 or math.abs(g - b) > 4 then
                targetColor = Color3.fromRGB(200, 200, 200)   
            end
        -- 5. УГОЛЬ (Темно-серый / черный со светлым)
        elseif (r < 60 and g < 60 and b < 60) or string.find(name, "coal") or string.find(name, "уголь") then
            targetColor = Color3.fromRGB(60, 60, 60)
        end
        
        if targetColor then
            local hl = Instance.new("Highlight")
            hl.Name = "OreHighlight"
            hl.Adornee = part
            hl.FillColor = targetColor
            hl.FillTransparency = 0.4
            hl.OutlineColor = Color3.fromRGB(255, 255, 255)
            hl.OutlineTransparency = 0.2
            hl.Parent = part
        end
    end
end

local playerEspEnabled = false
local playerConnections = {}

local function applyPlayerESP(player)
    if player == LocalPlayer then return end
    local function setupCharacter(char)
        if not char:FindFirstChild("PlayerHighlight") then
            local hl = Instance.new("Highlight")
            hl.Name = "PlayerHighlight"
            hl.Adornee = char
            hl.FillColor = Color3.fromRGB(240, 240, 240)
            hl.FillTransparency = 0.5
            hl.OutlineColor = Color3.fromRGB(255, 255, 255)
            hl.OutlineTransparency = 0.2
            hl.Parent = char
        end
    end
    if player.Character then setupCharacter(player.Character) end
    table.insert(playerConnections, player.CharacterAdded:Connect(setupCharacter))
end

-- Кнопка ESP Руд
local OreEspBtn = Instance.new("TextButton")
OreEspBtn.Size = UDim2.new(0.9, 0, 0, 42)
OreEspBtn.Position = UDim2.new(0.05, 0, 0, 10)
OreEspBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
OreEspBtn.Text = "ESP Руд (5 видов): ВЫКЛ"
OreEspBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OreEspBtn.TextSize = 13
OreEspBtn.Font = Enum.Font.SourceSansBold
OreEspBtn.Parent = pageVisuals

local c1 = Instance.new("UICorner") c1.CornerRadius = UDim.new(1, 0) c1.Parent = OreEspBtn

OreEspBtn.MouseButton1Click:Connect(function()
    oreEspEnabled = not oreEspEnabled
    if oreEspEnabled then
        OreEspBtn.Text = "ESP Руд (5 видов): ВКЛ"
        OreEspBtn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
        for _, obj in pairs(workspace:GetDescendants()) do applyOreESP(obj) end
        table.insert(oreConnections, workspace.DescendantAdded:Connect(function(obj) applyOreESP(obj) end))
    else
        OreEspBtn.Text = "ESP Руд (5 видов): ВЫКЛ"
        OreEspBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name == "OreHighlight" then obj:Destroy() end
        end
        for _, conn in pairs(oreConnections) do conn:Disconnect() end
        oreConnections = {}
    end
end)

-- Кнопка ESP Игроков
local PlayerEspBtn = Instance.new("TextButton")
PlayerEspBtn.Size = UDim2.new(0.9, 0, 0, 42)
PlayerEspBtn.Position = UDim2.new(0.05, 0, 0, 65) 
PlayerEspBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
PlayerEspBtn.Text = "ESP Игроков (WH): ВЫКЛ"
PlayerEspBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
PlayerEspBtn.TextSize = 14
PlayerEspBtn.Font = Enum.Font.SourceSansBold
PlayerEspBtn.Parent = pageVisuals

local c2 = Instance.new("UICorner") c2.CornerRadius = UDim.new(1, 0) c2.Parent = PlayerEspBtn

PlayerEspBtn.MouseButton1Click:Connect(function()
    playerEspEnabled = not playerEspEnabled
    if playerEspEnabled then
        PlayerEspBtn.Text = "ESP Игроков (WH): ВКЛ"
        PlayerEspBtn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
        for _, p in pairs(Players:GetPlayers()) do applyPlayerESP(p) end
        table.insert(playerConnections, Players.PlayerAdded:Connect(applyPlayerESP))
    else
        PlayerEspBtn.Text = "ESP Игроков (WH): ВЫКЛ"
        PlayerEspBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
        for _, p in pairs(Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("PlayerHighlight") then
                p.Character.PlayerHighlight:Destroy()
            end
        end
        for _, conn in pairs(playerConnections) do conn:Disconnect() end
        playerConnections = {}
    end
end)

local isOpen = false
OpenButton.MouseButton1Click:Connect(function()
    isOpen = not isOpen
    if isOpen then
        MainMenu.Visible = true
        MainMenu.Size = UDim2.new(0, 0, 0, 0)
        TweenService:Create(MainMenu, TweenInfo.new(0.25, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 450, 0, 320)
        }):Play()
    else
        local tween = TweenService:Create(MainMenu, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 0, 0, 0)
        })
        tween:Play()
        tween.Completed:Connect(function()
            if not isOpen then MainMenu.Visible = false end
        end)
    end
end)
