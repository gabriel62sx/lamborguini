--[[
    ╔═══════════════════════════════════════════════╗
    ║        LAMBORGHINI HUB - ULTRA PREMIUM        ║
    ║      Script Profissional Completo Roblox       ║
    ╚═══════════════════════════════════════════════╝
]]

-- ══════════════════════════════════════════
-- SERVIÇOS
-- ══════════════════════════════════════════
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportService = game:GetService("TeleportService")
local SoundService = game:GetService("SoundService")
local Workspace = game:GetService("Workspace")

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local Camera = Workspace.CurrentCamera

-- ══════════════════════════════════════════
-- CONFIGURAÇÕES DO TEMA - LAMBORGHINI
-- ══════════════════════════════════════════
local Theme = {
    -- Amarelo Lamborghini como cor principal
    Primary = Color3.fromRGB(255, 200, 0),
    PrimaryDark = Color3.fromRGB(200, 150, 0),
    PrimaryLight = Color3.fromRGB(255, 220, 60),
    PrimaryGlow = Color3.fromRGB(255, 210, 30),
    -- Preto/Cinza carbono como secundário
    Secondary = Color3.fromRGB(180, 180, 180),
    Background = Color3.fromRGB(8, 8, 8),
    BackgroundAlt = Color3.fromRGB(13, 13, 13),
    BackgroundCard = Color3.fromRGB(20, 20, 20),
    Surface = Color3.fromRGB(28, 28, 28),
    SurfaceHover = Color3.fromRGB(38, 38, 38),
    SurfaceBorder = Color3.fromRGB(50, 45, 10),
    TextPrimary = Color3.fromRGB(255, 255, 255),
    TextSecondary = Color3.fromRGB(200, 195, 160),
    TextMuted = Color3.fromRGB(120, 115, 80),
    Success = Color3.fromRGB(72, 222, 120),
    Warning = Color3.fromRGB(255, 200, 0),
    Error = Color3.fromRGB(255, 75, 75),
    Accent = Color3.fromRGB(255, 200, 0),
    Pink = Color3.fromRGB(255, 100, 180),
    Orange = Color3.fromRGB(255, 150, 50),
}

local Config = {
    ToggleKey = Enum.KeyCode.RightControl,
    HubName = "🏎️ Lamborghini Hub",
    Version = "🏎️ @gabriel.futury 🏎️",
    AnimSpeed = 0.4,
}

-- ══════════════════════════════════════════
-- ESTADOS
-- ══════════════════════════════════════════
local S = {
    Speed = false, SpeedVal = 10000,
    JumpPower = false, JumpVal = 10000,
    InfiniteJump = false,
    Fly = false, FlySpeed = 100000,
    Noclip = false,
    GodMode = false,
    AntiVoid = false,
    SpinBot = false, SpinSpeed = 60,
    TinyCharacter = false,
    GiantCharacter = false,
    Invisible = false,
    LowGravity = false,
    HighJump = false,
    AutoJump = false,
    FreezeSelf = false,
    KillAura = false, KillAuraRange = 60,
    AutoAttack = false,
    HitboxExpand = false, HitboxSize = 10000,
    AutoParry = false,
    SpamClick = false,
    Reach = false, ReachVal = 20,
    AimBot = false, AimSmooth = 0.5,
    AutoCombo = false,
    CriticalHit = false,
    ESP = false,
    Fullbright = false,
    NoFog = false,
    Chams = false,
    Tracers = false,
    PlayerNames = false,
    ItemESP = false,
    NoParticles = false,
    LowGraphics = false,
    Xray = false,
    Rainbow = false,
    FlingPlayer = false,
    SpinTarget = false,
    AnnoyTarget = false,
    FakeKick = false,
    OrbitTarget = false, OrbitRadius = 39, OrbitSpeed = 38,
    AttachTarget = false,
    HeadlessTarget = false,
    CopyTarget = false,
    BlockSpam = false,
    TargetPlayer = nil,
    AntiAFK = false,
    ChatSpam = false, ChatMsg = "Lamborghini Hub 🏎️",
    ClickTP = false,
    ClickDelete = false,
    InfYield = false,
    AutoFarm = false,
    AntiKick = false,
    UnlockAll = false,
    HubVisible = true,
    CurrentTab = "Home",
    SavedPos = nil,
    SavedPositions = {},
}

-- ══════════════════════════════════════════
-- FUNÇÕES UTILITÁRIAS
-- ══════════════════════════════════════════
local function GetChar()
    return Player.Character or Player.CharacterAdded:Wait()
end

local function GetHum()
    local c = GetChar()
    return c and c:FindFirstChildOfClass("Humanoid")
end

local function GetRoot()
    local c = GetChar()
    return c and (c:FindFirstChild("HumanoidRootPart") or c:FindFirstChild("Torso"))
end

local function GetHead()
    local c = GetChar()
    return c and c:FindFirstChild("Head")
end

local function Tween(obj, props, dur, style, dir)
    if not obj or not obj.Parent then return end
    local t = TweenService:Create(obj,
        TweenInfo.new(dur or Config.AnimSpeed, style or Enum.EasingStyle.Quint, dir or Enum.EasingDirection.Out),
        props
    )
    t:Play()
    return t
end

local function MakeCorner(p, r)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, r or 8)
    c.Parent = p
    return c
end

local function MakeStroke(p, col, th, tr)
    local s = Instance.new("UIStroke")
    s.Color = col or Theme.Primary
    s.Thickness = th or 1
    s.Transparency = tr or 0.7
    s.Parent = p
    return s
end

local function MakePadding(p, t, b, l, r)
    local pd = Instance.new("UIPadding")
    pd.PaddingTop = UDim.new(0, t or 8)
    pd.PaddingBottom = UDim.new(0, b or 8)
    pd.PaddingLeft = UDim.new(0, l or 8)
    pd.PaddingRight = UDim.new(0, r or 8)
    pd.Parent = p
    return pd
end

local function MakeGradient(p, c1, c2, rot)
    local g = Instance.new("UIGradient")
    g.Color = ColorSequence.new(c1 or Theme.Primary, c2 or Theme.PrimaryDark)
    g.Rotation = rot or 45
    g.Parent = p
    return g
end

local function MakeShadow(p)
    local sh = Instance.new("ImageLabel")
    sh.Name = "Shadow"
    sh.BackgroundTransparency = 1
    sh.Image = "rbxassetid://6014261993"
    sh.ImageColor3 = Color3.fromRGB(0, 0, 0)
    sh.ImageTransparency = 0.4
    sh.Size = UDim2.new(1, 40, 1, 40)
    sh.Position = UDim2.new(0, -20, 0, -20)
    sh.ScaleType = Enum.ScaleType.Slice
    sh.SliceCenter = Rect.new(49, 49, 450, 450)
    sh.ZIndex = p.ZIndex - 1
    sh.Parent = p
    return sh
end

-- ══════════════════════════════════════════
-- LIMPAR E CRIAR GUI
-- ══════════════════════════════════════════
pcall(function()
    if CoreGui:FindFirstChild("NovaHubV3") then CoreGui.NovaHubV3:Destroy() end
end)
if Player.PlayerGui:FindFirstChild("NovaHubV3") then Player.PlayerGui.NovaHubV3:Destroy() end

local gui = Instance.new("ScreenGui")
gui.Name = "NovaHubV3"
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
pcall(function() gui.Parent = CoreGui end)
if not gui.Parent then gui.Parent = Player.PlayerGui end

-- ══════════════════════════════════════════
-- SISTEMA DE NOTIFICAÇÃO
-- ══════════════════════════════════════════
local notifHolder = Instance.new("Frame")
notifHolder.Name = "Notifs"
notifHolder.BackgroundTransparency = 1
notifHolder.Size = UDim2.new(0, 320, 1, 0)
notifHolder.Position = UDim2.new(1, -330, 0, 0)
notifHolder.ZIndex = 200
notifHolder.Parent = gui

local nLayout = Instance.new("UIListLayout")
nLayout.SortOrder = Enum.SortOrder.LayoutOrder
nLayout.Padding = UDim.new(0, 6)
nLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom
nLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
nLayout.Parent = notifHolder
MakePadding(notifHolder, 0, 25, 5, 5)

local function Notify(title, msg, dur, ntype)
    local colors = {info = Theme.Primary, success = Theme.Success, warning = Theme.Warning, error = Theme.Error}
    local icons = {info = "🏎️", success = "✅", warning = "⚠️", error = "❌"}
    local col = colors[ntype or "info"] or Theme.Primary
    local ico = icons[ntype or "info"] or "🏎️"

    local n = Instance.new("Frame")
    n.BackgroundColor3 = Theme.BackgroundCard
    n.Size = UDim2.new(1, 0, 0, 72)
    n.ZIndex = 201
    n.ClipsDescendants = true
    n.Parent = notifHolder
    MakeCorner(n, 10)
    MakeStroke(n, col, 1.5, 0.2)
    MakeShadow(n)

    local bar = Instance.new("Frame")
    bar.BackgroundColor3 = col
    bar.Size = UDim2.new(0, 4, 1, -8)
    bar.Position = UDim2.new(0, 4, 0, 4)
    bar.ZIndex = 202
    bar.BorderSizePixel = 0
    bar.Parent = n
    MakeCorner(bar, 2)

    local tl = Instance.new("TextLabel")
    tl.Text = ico .. " " .. (title or "Notificação")
    tl.Font = Enum.Font.GothamBold
    tl.TextSize = 14
    tl.TextColor3 = Theme.TextPrimary
    tl.TextXAlignment = Enum.TextXAlignment.Left
    tl.BackgroundTransparency = 1
    tl.Size = UDim2.new(1, -24, 0, 22)
    tl.Position = UDim2.new(0, 18, 0, 10)
    tl.ZIndex = 202
    tl.Parent = n

    local ml = Instance.new("TextLabel")
    ml.Text = msg or ""
    ml.Font = Enum.Font.Gotham
    ml.TextSize = 12
    ml.TextColor3 = Theme.TextSecondary
    ml.TextXAlignment = Enum.TextXAlignment.Left
    ml.TextWrapped = true
    ml.BackgroundTransparency = 1
    ml.Size = UDim2.new(1, -24, 0, 28)
    ml.Position = UDim2.new(0, 18, 0, 34)
    ml.ZIndex = 202
    ml.Parent = n

    local prog = Instance.new("Frame")
    prog.BackgroundColor3 = col
    prog.Size = UDim2.new(1, 0, 0, 3)
    prog.Position = UDim2.new(0, 0, 1, -3)
    prog.ZIndex = 202
    prog.BorderSizePixel = 0
    prog.Parent = n

    n.Position = UDim2.new(1, 50, 0, 0)
    Tween(n, {Position = UDim2.new(0, 0, 0, 0)}, 0.35)

    local d = dur or 3
    Tween(prog, {Size = UDim2.new(0, 0, 0, 3)}, d, Enum.EasingStyle.Linear)

    task.delay(d, function()
        local tw = Tween(n, {Position = UDim2.new(1, 60, 0, 0), BackgroundTransparency = 0.5}, 0.35)
        if tw then tw.Completed:Wait() end
        pcall(function() n:Destroy() end)
    end)
end

-- ══════════════════════════════════════════
-- TELA DE INTRO - TEMA LAMBORGHINI
-- ══════════════════════════════════════════
local function PlayIntro()
    local introFrame = Instance.new("Frame")
    introFrame.Name = "Intro"
    introFrame.BackgroundColor3 = Color3.fromRGB(5, 4, 0)
    introFrame.BackgroundTransparency = 0.3
    introFrame.Size = UDim2.new(1, 0, 1, 0)
    introFrame.ZIndex = 500
    introFrame.Parent = gui

    -- Blur suave
    local blur = Instance.new("BlurEffect")
    blur.Name = "LamboBlur"
    blur.Size = 0
    blur.Parent = Lighting
    Tween(blur, {Size = 5}, 0.5)

    -- ══ FUNDO COM GRADIENTE PRETO/AMARELO ══
    local bgGrad = Instance.new("Frame")
    bgGrad.BackgroundColor3 = Color3.fromRGB(10, 8, 0)
    bgGrad.BackgroundTransparency = 0.4
    bgGrad.Size = UDim2.new(1, 0, 1, 0)
    bgGrad.ZIndex = 500
    bgGrad.Parent = introFrame

    local bgGradEffect = Instance.new("UIGradient")
    bgGradEffect.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 0, 0)),
        ColorSequenceKeypoint.new(0.4, Color3.fromRGB(15, 12, 0)),
        ColorSequenceKeypoint.new(0.7, Color3.fromRGB(25, 18, 0)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0)),
    })
    bgGradEffect.Rotation = 135
    bgGradEffect.Parent = bgGrad

    -- ══ LINHAS DE VELOCIDADE (efeito racing) ══
    for i = 1, 12 do
        task.spawn(function()
            while task.wait(math.random(15, 40) / 100) do
                if not introFrame or not introFrame.Parent then break end
                local line = Instance.new("Frame")
                line.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
                line.BackgroundTransparency = math.random(60, 90) / 100
                local h = math.random(1, 3)
                line.Size = UDim2.new(0, math.random(80, 300), 0, h)
                line.Position = UDim2.new(-0.5, 0, math.random(0, 100) / 100, 0)
                line.ZIndex = 501
                line.BorderSizePixel = 0
                line.Parent = introFrame
                MakeCorner(line, 1)
                Tween(line, {Position = UDim2.new(1.5, 0, line.Position.Y.Scale, 0), BackgroundTransparency = 1}, math.random(4, 8) / 10, Enum.EasingStyle.Linear)
                task.delay(0.9, function() pcall(function() line:Destroy() end) end)
            end
        end)
    end

    -- ══ PARTÍCULAS DOURADAS ══
    for i = 1, 30 do
        task.spawn(function()
            local p = Instance.new("Frame")
            local sz = math.random(2, 5)
            p.BackgroundColor3 = Color3.fromRGB(255, math.random(180, 220), 0)
            p.BackgroundTransparency = math.random(50, 85) / 100
            p.Size = UDim2.new(0, sz, 0, sz)
            p.Position = UDim2.new(math.random(0, 100)/100, 0, math.random(0, 100)/100, 0)
            p.ZIndex = 502
            p.Parent = introFrame
            MakeCorner(p, sz)
            while p and p.Parent do
                Tween(p, {
                    Position = UDim2.new(math.random(0, 100)/100, 0, math.random(0, 100)/100, 0),
                    BackgroundTransparency = math.random(40, 90)/100,
                }, math.random(30, 80)/10, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut)
                task.wait(math.random(30, 80)/10)
            end
        end)
    end

    -- ══ LOGO LAMBORGHINI REMOVIDO A PEDIDO ══
    task.wait(1.4)

    -- ══ TEXTO DO TÍTULO ══
    local title = Instance.new("TextLabel")
    title.Text = "🏎️ LAMBORGHINI HUB"
    title.Font = Enum.Font.GothamBold
    title.TextSize = 38
    title.TextColor3 = Color3.fromRGB(255, 200, 0)
    title.TextTransparency = 1
    title.BackgroundTransparency = 1
    title.Size = UDim2.new(1, 0, 0, 50)
    title.Position = UDim2.new(0, 0, 0.56, 0)
    title.ZIndex = 507
    title.Parent = introFrame

    -- Sombra do título
    local titleShadow = Instance.new("TextLabel")
    titleShadow.Text = "🏎️ LAMBORGHINI HUB"
    titleShadow.Font = Enum.Font.GothamBold
    titleShadow.TextSize = 38
    titleShadow.TextColor3 = Color3.fromRGB(150, 100, 0)
    titleShadow.TextTransparency = 1
    titleShadow.BackgroundTransparency = 1
    titleShadow.Size = UDim2.new(1, 0, 0, 50)
    titleShadow.Position = UDim2.new(0, 2, 0.562, 0)
    titleShadow.ZIndex = 506
    titleShadow.Parent = introFrame

    -- Linha dourada sob o título
    local titleLine = Instance.new("Frame")
    titleLine.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    titleLine.Size = UDim2.new(0, 0, 0, 2)
    titleLine.Position = UDim2.new(0.5, 0, 0.618, 0)
    titleLine.AnchorPoint = Vector2.new(0.5, 0)
    titleLine.ZIndex = 507
    titleLine.BorderSizePixel = 0
    titleLine.Parent = introFrame
    local tlGrad = Instance.new("UIGradient")
    tlGrad.Color = ColorSequence.new(Color3.fromRGB(200, 145, 0), Color3.fromRGB(255, 220, 50))
    tlGrad.Rotation = 0
    tlGrad.Parent = titleLine

    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "ULTRA PREMIUM SCRIPT HUB  •  " .. Config.Version
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = 13
    subtitle.TextColor3 = Color3.fromRGB(180, 140, 30)
    subtitle.TextTransparency = 1
    subtitle.BackgroundTransparency = 1
    subtitle.Size = UDim2.new(1, 0, 0, 22)
    subtitle.Position = UDim2.new(0, 0, 0.645, 0)
    subtitle.ZIndex = 507
    subtitle.Parent = introFrame

    -- Slogan Lamborghini
    local slogan = Instance.new("TextLabel")
    slogan.Text = '"Expect the Unexpected" 🐂'
    slogan.Font = Enum.Font.GothamBold
    slogan.TextSize = 12
    slogan.TextColor3 = Color3.fromRGB(255, 200, 0)
    slogan.TextTransparency = 1
    slogan.BackgroundTransparency = 1
    slogan.Size = UDim2.new(1, 0, 0, 20)
    slogan.Position = UDim2.new(0, 0, 0.675, 0)
    slogan.ZIndex = 507
    slogan.Parent = introFrame

    -- ══ BARRA DE PROGRESSO ══
    local progressBG = Instance.new("Frame")
    progressBG.BackgroundColor3 = Color3.fromRGB(30, 25, 0)
    progressBG.BackgroundTransparency = 1
    progressBG.Size = UDim2.new(0, 370, 0, 8)
    progressBG.Position = UDim2.new(0.5, -185, 0, 0)
    progressBG.AnchorPoint = Vector2.new(0, 0)
    progressBG.ZIndex = 507
    progressBG.Parent = introFrame
    MakeCorner(progressBG, 4)
    MakeStroke(progressBG, Color3.fromRGB(80, 60, 0), 1, 0.3)

    -- Posição Y via Position
    progressBG.Position = UDim2.new(0.5, -185, 0.745, 0)

    local progressFill = Instance.new("Frame")
    progressFill.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    progressFill.Size = UDim2.new(0, 0, 1, 0)
    progressFill.ZIndex = 508
    progressFill.Parent = progressBG
    MakeCorner(progressFill, 4)

    local progressFillGrad = Instance.new("UIGradient")
    progressFillGrad.Color = ColorSequence.new(
        Color3.fromRGB(200, 145, 0),
        Color3.fromRGB(255, 230, 60)
    )
    progressFillGrad.Rotation = 0
    progressFillGrad.Parent = progressFill

    -- Brilho na ponta da barra
    local progressGlow = Instance.new("Frame")
    progressGlow.BackgroundColor3 = Color3.fromRGB(255, 255, 200)
    progressGlow.BackgroundTransparency = 0.2
    progressGlow.Size = UDim2.new(0, 4, 1, 6)
    progressGlow.Position = UDim2.new(1, -4, 0, -3)
    progressGlow.ZIndex = 509
    progressGlow.Parent = progressFill
    MakeCorner(progressGlow, 2)

    local percentTxt = Instance.new("TextLabel")
    percentTxt.Text = "0%"
    percentTxt.Font = Enum.Font.GothamBold
    percentTxt.TextSize = 16
    percentTxt.TextColor3 = Color3.fromRGB(255, 200, 0)
    percentTxt.TextTransparency = 1
    percentTxt.BackgroundTransparency = 1
    percentTxt.Size = UDim2.new(1, 0, 0, 22)
    percentTxt.Position = UDim2.new(0, 0, 0.71, 0)
    percentTxt.ZIndex = 507
    percentTxt.Parent = introFrame

    local statusTxt = Instance.new("TextLabel")
    statusTxt.Text = "Iniciando motores..."
    statusTxt.Font = Enum.Font.Gotham
    statusTxt.TextSize = 12
    statusTxt.TextColor3 = Color3.fromRGB(150, 115, 20)
    statusTxt.TextTransparency = 1
    statusTxt.BackgroundTransparency = 1
    statusTxt.Size = UDim2.new(1, 0, 0, 20)
    statusTxt.Position = UDim2.new(0, 0, 0.785, 0)
    statusTxt.ZIndex = 507
    statusTxt.Parent = introFrame

    -- Velocímetro decorativo (detalhe Lambo)
    local speedoFrame = Instance.new("Frame")
    speedoFrame.BackgroundColor3 = Color3.fromRGB(15, 12, 0)
    speedoFrame.BackgroundTransparency = 0.4
    speedoFrame.Size = UDim2.new(0, 160, 0, 24)
    speedoFrame.Position = UDim2.new(0.5, -80, 0.84, 0)
    speedoFrame.ZIndex = 507
    speedoFrame.Parent = introFrame
    MakeCorner(speedoFrame, 4)
    MakeStroke(speedoFrame, Color3.fromRGB(255, 200, 0), 1, 0.5)

    local speedoTxt = Instance.new("TextLabel")
    speedoTxt.Text = "🏁 0 km/h"
    speedoTxt.Font = Enum.Font.GothamBold
    speedoTxt.TextSize = 12
    speedoTxt.TextColor3 = Color3.fromRGB(255, 200, 0)
    speedoTxt.BackgroundTransparency = 1
    speedoTxt.Size = UDim2.new(1, 0, 1, 0)
    speedoTxt.ZIndex = 508
    speedoTxt.TextTransparency = 1
    speedoTxt.Parent = speedoFrame

    local bottomText = Instance.new("TextLabel")
    bottomText.Text = "Right Ctrl para toggle  •  Criado por @gabriel.futury  🏎️"
    bottomText.Font = Enum.Font.Gotham
    bottomText.TextSize = 11
    bottomText.TextColor3 = Color3.fromRGB(100, 80, 20)
    bottomText.TextTransparency = 1
    bottomText.BackgroundTransparency = 1
    bottomText.Size = UDim2.new(1, 0, 0, 20)
    bottomText.Position = UDim2.new(0, 0, 0.93, 0)
    bottomText.ZIndex = 507
    bottomText.Parent = introFrame

    -- ══ APARECER TEXTOS ══
    task.wait(0.55)
    Tween(title, {TextTransparency = 0}, 0.5)
    Tween(titleShadow, {TextTransparency = 0.4}, 0.5)
    task.wait(0.15)
    Tween(titleLine, {Size = UDim2.new(0, 220, 0, 2)}, 0.5, Enum.EasingStyle.Quint)
    task.wait(0.1)
    Tween(subtitle, {TextTransparency = 0}, 0.4)
    task.wait(0.1)
    Tween(slogan, {TextTransparency = 0}, 0.4)
    task.wait(0.15)

    Tween(progressBG, {BackgroundTransparency = 0}, 0.3)
    Tween(percentTxt, {TextTransparency = 0}, 0.3)
    Tween(statusTxt, {TextTransparency = 0}, 0.3)
    Tween(speedoTxt, {TextTransparency = 0}, 0.4)
    Tween(bottomText, {TextTransparency = 0.5}, 0.5)

    -- ══ LOADING COM TEMÁTICA LAMBO ══
    local steps = {
        {p = 0.08,  s = "🔑 Girando a chave...",               t = 0.25, km = 50},
        {p = 0.16,  s = "⚙️  Aquecendo o motor V12...",         t = 0.30, km = 150},
        {p = 0.25,  s = "🏎️  Acelerando sistemas...",           t = 0.35, km = 280},
        {p = 0.35,  s = "💨  Injetando combustível...",         t = 0.28, km = 380},
        {p = 0.45,  s = "🔧  Calibrando suspensão...",          t = 0.32, km = 450},
        {p = 0.55,  s = "🎯  Carregando modos de ataque...",    t = 0.30, km = 520},
        {p = 0.65,  s = "👁️  Ativando câmeras ESP...",          t = 0.28, km = 600},
        {p = 0.73,  s = "🌍  Carregando GPS e teleport...",     t = 0.25, km = 680},
        {p = 0.82,  s = "😈  Modo troll desbloqueado...",       t = 0.30, km = 750},
        {p = 0.90,  s = "⚡  Turbo boost ativado...",           t = 0.22, km = 850},
        {p = 0.96,  s = "🏁  Cruzando a linha de chegada...",   t = 0.20, km = 950},
        {p = 1.00,  s = "✅  LAMBORGHINI HUB PRONTO!",          t = 0.15, km = 1000},
    }

    for _, step in ipairs(steps) do
        Tween(progressFill, {Size = UDim2.new(step.p, 0, 1, 0)}, step.t, Enum.EasingStyle.Quad)
        statusTxt.Text = step.s
        percentTxt.Text = math.floor(step.p * 100) .. "%"
        speedoTxt.Text = "🏁 " .. step.km .. " km/h"
        if step.p >= 1 then
            Tween(statusTxt, {TextColor3 = Theme.Success}, 0.3)
            Tween(speedoTxt, {TextColor3 = Theme.Success}, 0.3)
            percentTxt.TextColor3 = Theme.Success
        end
        task.wait(step.t + 0.04)
    end

    task.wait(0.7)

    -- ══ SISTEMA DE VERIFICAÇÃO DE SENHA ══
    local passPassed = false
    local validPasswords = {
        ["lambo057"] = true,
        ["lambo506"] = true,
        ["lambo641"] = true,
        ["lambo321"] = true,
        ["lambo630"] = true,
        ["lambo703"] = true
    }

    local passFrame = Instance.new("Frame")
    passFrame.Size = UDim2.new(0, 320, 0, 140)
    passFrame.Position = UDim2.new(0.5, -160, 0.5, -30)
    passFrame.BackgroundColor3 = Color3.fromRGB(15, 12, 0)
    passFrame.ZIndex = 600
    passFrame.Parent = introFrame
    MakeCorner(passFrame, 8)
    MakeStroke(passFrame, Color3.fromRGB(255, 200, 0), 2, 0)
    
    local passTitle = Instance.new("TextLabel")
    passTitle.Text = "🔒 LOGIN NECESSÁRIO"
    passTitle.Font = Enum.Font.GothamBold
    passTitle.TextSize = 16
    passTitle.TextColor3 = Color3.fromRGB(255, 200, 0)
    passTitle.BackgroundTransparency = 1
    passTitle.Size = UDim2.new(1, 0, 0, 30)
    passTitle.Position = UDim2.new(0, 0, 0, 10)
    passTitle.ZIndex = 601
    passTitle.Parent = passFrame

    local passInput = Instance.new("TextBox")
    passInput.PlaceholderText = "Digite a Senha..."
    passInput.Text = ""
    passInput.Font = Enum.Font.Gotham
    passInput.TextSize = 14
    passInput.TextColor3 = Color3.fromRGB(255, 255, 255)
    passInput.BackgroundColor3 = Color3.fromRGB(30, 25, 0)
    passInput.Size = UDim2.new(0, 280, 0, 35)
    passInput.Position = UDim2.new(0.5, -140, 0, 50)
    passInput.ZIndex = 601
    passInput.Parent = passFrame
    MakeCorner(passInput, 6)
    MakeStroke(passInput, Color3.fromRGB(80, 60, 0), 1, 0)

    local passBtn = Instance.new("TextButton")
    passBtn.Text = "ACESSAR HUB"
    passBtn.Font = Enum.Font.GothamBold
    passBtn.TextSize = 14
    passBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
    passBtn.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    passBtn.Size = UDim2.new(0, 280, 0, 35)
    passBtn.Position = UDim2.new(0.5, -140, 0, 95)
    passBtn.ZIndex = 601
    passBtn.Parent = passFrame
    MakeCorner(passBtn, 6)

    passBtn.MouseButton1Click:Connect(function()
        if validPasswords[passInput.Text] then
            passBtn.Text = "✅ ACESSO CONCEDIDO"
            passBtn.BackgroundColor3 = Color3.fromRGB(72, 222, 120)
            task.wait(1)
            Tween(passFrame, {BackgroundTransparency = 1}, 0.5)
            Tween(passTitle, {TextTransparency = 1}, 0.5)
            Tween(passInput, {BackgroundTransparency = 1, TextTransparency = 1}, 0.5)
            Tween(passBtn, {BackgroundTransparency = 1, TextTransparency = 1}, 0.5)
            task.wait(0.5)
            passFrame:Destroy()
            passPassed = true
        else
            passBtn.Text = "❌ SENHA INCORRETA"
            passBtn.BackgroundColor3 = Color3.fromRGB(255, 75, 75)
            task.wait(1.5)
            passBtn.Text = "ACESSAR HUB"
            passBtn.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
        end
    end)

    statusTxt.Text = "Aguardando autenticação..."
    statusTxt.TextColor3 = Color3.fromRGB(255, 200, 0)

    -- Hide titles to focus on password
    Tween(title, {TextTransparency = 1}, 0.3)
    Tween(titleShadow, {TextTransparency = 1}, 0.3)
    Tween(titleLine, {BackgroundTransparency = 1}, 0.3)
    Tween(subtitle, {TextTransparency = 1}, 0.3)
    Tween(slogan, {TextTransparency = 1}, 0.3)
    Tween(logoContainer, {Position = UDim2.new(0.5, 0, 0.25, 0)}, 0.5, Enum.EasingStyle.Quint)

    repeat task.wait(0.1) until passPassed

    -- ══ SAÍDA CINEMATOGRÁFICA ══
    -- Escudo "lança" para cima com velocidade
    Tween(logoContainer, {
        Size = UDim2.new(0, 20, 0, 20),
        Position = UDim2.new(0.5, 0, -0.2, 0)
    }, 0.4, Enum.EasingStyle.Back, Enum.EasingDirection.In)

    Tween(title, {TextTransparency = 1, Position = UDim2.new(0, 0, 0.48, 0)}, 0.35)
    Tween(titleShadow, {TextTransparency = 1}, 0.35)
    Tween(subtitle, {TextTransparency = 1}, 0.30)
    Tween(slogan, {TextTransparency = 1}, 0.25)
    Tween(titleLine, {Size = UDim2.new(0, 0, 0, 2)}, 0.3)
    Tween(percentTxt, {TextTransparency = 1}, 0.25)
    Tween(statusTxt, {TextTransparency = 1}, 0.25)
    Tween(speedoTxt, {TextTransparency = 1}, 0.25)
    Tween(bottomText, {TextTransparency = 1}, 0.25)
    Tween(progressBG, {BackgroundTransparency = 1}, 0.25)

    task.wait(0.2)

    -- Flash amarelo Lambo antes de abrir
    local flash = Instance.new("Frame")
    flash.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    flash.BackgroundTransparency = 0.1
    flash.Size = UDim2.new(1, 0, 1, 0)
    flash.ZIndex = 520
    flash.Parent = introFrame
    Tween(flash, {BackgroundTransparency = 1}, 0.45, Enum.EasingStyle.Quad)

    Tween(introFrame, {BackgroundTransparency = 1}, 0.5)
    Tween(bgGrad, {BackgroundTransparency = 1}, 0.5)
    Tween(blur, {Size = 0}, 0.5)

    task.wait(0.55)
    pcall(function() blur:Destroy() end)
    pcall(function() introFrame:Destroy() end)
end

-- ══════════════════════════════════════════
-- INTERFACE PRINCIPAL
-- ══════════════════════════════════════════
local mainFrame, sidebarFrame, contentFrame
local tabBtns = {}
local tabPages = {}

local function BuildUI()
    mainFrame = Instance.new("Frame")
    mainFrame.Name = "Main"
    mainFrame.BackgroundColor3 = Theme.Background
    mainFrame.Size = UDim2.new(0, 720, 0, 490)
    mainFrame.Position = UDim2.new(0.5, -360, 0.5, -245)
    mainFrame.ZIndex = 10
    mainFrame.ClipsDescendants = true
    mainFrame.Parent = gui
    MakeCorner(mainFrame, 14)
    MakeStroke(mainFrame, Theme.Primary, 1.5, 0.4)
    MakeShadow(mainFrame)

    mainFrame.Size = UDim2.new(0, 0, 0, 0)
    mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    mainFrame.BackgroundTransparency = 1
    Tween(mainFrame, {
        Size = UDim2.new(0, 720, 0, 490),
        Position = UDim2.new(0.5, -360, 0.5, -245),
        BackgroundTransparency = 0
    }, 0.5, Enum.EasingStyle.Back)

    local topBar = Instance.new("Frame")
    topBar.BackgroundColor3 = Theme.BackgroundAlt
    topBar.Size = UDim2.new(1, 0, 0, 42)
    topBar.ZIndex = 11
    topBar.BorderSizePixel = 0
    topBar.Parent = mainFrame
    MakeCorner(topBar, 14)

    local topFill = Instance.new("Frame")
    topFill.BackgroundColor3 = Theme.BackgroundAlt
    topFill.Size = UDim2.new(1, 0, 0, 16)
    topFill.Position = UDim2.new(0, 0, 1, -16)
    topFill.ZIndex = 11
    topFill.BorderSizePixel = 0
    topFill.Parent = topBar

    -- Faixa amarela Lamborghini no topo
    local topAccent = Instance.new("Frame")
    topAccent.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    topAccent.Size = UDim2.new(1, 0, 0, 2)
    topAccent.Position = UDim2.new(0, 0, 0, 0)
    topAccent.ZIndex = 15
    topAccent.BorderSizePixel = 0
    topAccent.Parent = topBar
    local tacGrad = Instance.new("UIGradient")
    tacGrad.Color = ColorSequence.new(Color3.fromRGB(200, 145, 0), Color3.fromRGB(255, 230, 60))
    tacGrad.Rotation = 0
    tacGrad.Parent = topAccent

    local topTitle = Instance.new("TextLabel")
    topTitle.Text = "  🏎️ " .. Config.HubName .. "  " .. Config.Version
    topTitle.Font = Enum.Font.GothamBold
    topTitle.TextSize = 14
    topTitle.TextColor3 = Color3.fromRGB(255, 200, 0)
    topTitle.TextXAlignment = Enum.TextXAlignment.Left
    topTitle.BackgroundTransparency = 1
    topTitle.Size = UDim2.new(0.6, 0, 1, 0)
    topTitle.Position = UDim2.new(0, 8, 0, 0)
    topTitle.ZIndex = 12
    topTitle.Parent = topBar

    local btnConfigs = {
        {text = "✕", color = Theme.Error, offset = -40, action = "close"},
        {text = "─", color = Theme.Warning, offset = -78, action = "minimize"},
    }

    for _, bc in ipairs(btnConfigs) do
        local btn = Instance.new("TextButton")
        btn.Text = bc.text
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = bc.text == "✕" and 16 or 14
        btn.TextColor3 = Theme.TextMuted
        btn.BackgroundColor3 = bc.color
        btn.BackgroundTransparency = 1
        btn.Size = UDim2.new(0, 32, 0, 32)
        btn.Position = UDim2.new(1, bc.offset, 0, 5)
        btn.ZIndex = 13
        btn.Parent = topBar
        MakeCorner(btn, 8)

        btn.MouseEnter:Connect(function()
            Tween(btn, {BackgroundTransparency = 0.15, TextColor3 = Theme.TextPrimary}, 0.2)
        end)
        btn.MouseLeave:Connect(function()
            Tween(btn, {BackgroundTransparency = 1, TextColor3 = Theme.TextMuted}, 0.2)
        end)
        btn.MouseButton1Click:Connect(function()
            if bc.action == "close" then
                S.HubVisible = false
                Tween(mainFrame, {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0), BackgroundTransparency = 1}, 0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In)
                task.wait(0.35)
                mainFrame.Visible = false
            elseif bc.action == "minimize" then
                S.HubVisible = false
                Tween(mainFrame, {Size = UDim2.new(0, 720, 0, 0), Position = UDim2.new(0.5, -360, 1, 20)}, 0.3)
                task.wait(0.35)
                mainFrame.Visible = false
            end
        end)
    end

    local drag, dragIn, dragSt, startP
    topBar.InputBegan:Connect(function(inp)
        if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
            drag = true
            dragSt = inp.Position
            startP = mainFrame.Position
            inp.Changed:Connect(function()
                if inp.UserInputState == Enum.UserInputState.End then drag = false end
            end)
        end
    end)
    topBar.InputChanged:Connect(function(inp)
        if inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch then
            dragIn = inp
        end
    end)
    UserInputService.InputChanged:Connect(function(inp)
        if inp == dragIn and drag then
            local d = inp.Position - dragSt
            mainFrame.Position = UDim2.new(startP.X.Scale, startP.X.Offset + d.X, startP.Y.Scale, startP.Y.Offset + d.Y)
        end
    end)

    sidebarFrame = Instance.new("Frame")
    sidebarFrame.BackgroundColor3 = Theme.BackgroundAlt
    sidebarFrame.Size = UDim2.new(0, 165, 1, -42)
    sidebarFrame.Position = UDim2.new(0, 0, 0, 42)
    sidebarFrame.ZIndex = 11
    sidebarFrame.BorderSizePixel = 0
    sidebarFrame.Parent = mainFrame

    local sidebarSep = Instance.new("Frame")
    sidebarSep.BackgroundColor3 = Color3.fromRGB(80, 60, 0)
    sidebarSep.BackgroundTransparency = 0.3
    sidebarSep.Size = UDim2.new(0, 1, 1, 0)
    sidebarSep.Position = UDim2.new(1, 0, 0, 0)
    sidebarSep.ZIndex = 12
    sidebarSep.BorderSizePixel = 0
    sidebarSep.Parent = sidebarFrame

    local avatarFrame = Instance.new("Frame")
    avatarFrame.BackgroundTransparency = 1
    avatarFrame.Size = UDim2.new(1, 0, 0, 65)
    avatarFrame.ZIndex = 12
    avatarFrame.Parent = sidebarFrame

    local avatarImg = Instance.new("ImageLabel")
    avatarImg.BackgroundColor3 = Theme.Surface
    avatarImg.Size = UDim2.new(0, 38, 0, 38)
    avatarImg.Position = UDim2.new(0, 14, 0.5, -19)
    avatarImg.ZIndex = 13
    avatarImg.Parent = avatarFrame
    MakeCorner(avatarImg, 19)
    MakeStroke(avatarImg, Color3.fromRGB(255, 200, 0), 2, 0.2)

    pcall(function()
        avatarImg.Image = Players:GetUserThumbnailAsync(Player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
    end)

    local nameLabel = Instance.new("TextLabel")
    nameLabel.Text = Player.DisplayName
    nameLabel.Font = Enum.Font.GothamBold
    nameLabel.TextSize = 12
    nameLabel.TextColor3 = Color3.fromRGB(255, 200, 0)
    nameLabel.TextXAlignment = Enum.TextXAlignment.Left
    nameLabel.TextTruncate = Enum.TextTruncate.AtEnd
    nameLabel.BackgroundTransparency = 1
    nameLabel.Size = UDim2.new(1, -65, 0, 16)
    nameLabel.Position = UDim2.new(0, 60, 0, 16)
    nameLabel.ZIndex = 13
    nameLabel.Parent = avatarFrame

    local statusL = Instance.new("TextLabel")
    statusL.Text = "🏎️ Premium"
    statusL.Font = Enum.Font.Gotham
    statusL.TextSize = 10
    statusL.TextColor3 = Theme.Success
    statusL.TextXAlignment = Enum.TextXAlignment.Left
    statusL.BackgroundTransparency = 1
    statusL.Size = UDim2.new(1, -65, 0, 14)
    statusL.Position = UDim2.new(0, 60, 0, 34)
    statusL.ZIndex = 13
    statusL.Parent = avatarFrame

    local sepLine = Instance.new("Frame")
    sepLine.BackgroundColor3 = Color3.fromRGB(100, 75, 0)
    sepLine.BackgroundTransparency = 0.4
    sepLine.Size = UDim2.new(0.8, 0, 0, 1)
    sepLine.Position = UDim2.new(0.1, 0, 0, 64)
    sepLine.ZIndex = 12
    sepLine.BorderSizePixel = 0
    sepLine.Parent = sidebarFrame

    local tabScroll = Instance.new("ScrollingFrame")
    tabScroll.BackgroundTransparency = 1
    tabScroll.Size = UDim2.new(1, 0, 1, -72)
    tabScroll.Position = UDim2.new(0, 0, 0, 72)
    tabScroll.ZIndex = 12
    tabScroll.ScrollBarThickness = 0
    tabScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    tabScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    tabScroll.Parent = sidebarFrame

    local tabLay = Instance.new("UIListLayout")
    tabLay.SortOrder = Enum.SortOrder.LayoutOrder
    tabLay.Padding = UDim.new(0, 3)
    tabLay.Parent = tabScroll
    MakePadding(tabScroll, 4, 4, 8, 8)

    contentFrame = Instance.new("Frame")
    contentFrame.BackgroundColor3 = Theme.Background
    contentFrame.Size = UDim2.new(1, -166, 1, -42)
    contentFrame.Position = UDim2.new(0, 166, 0, 42)
    contentFrame.ZIndex = 11
    contentFrame.BorderSizePixel = 0
    contentFrame.ClipsDescendants = true
    contentFrame.Parent = mainFrame

    local tabs = {
        {name = "Home",     icon = "🏠", order = 1},
        {name = "Player",   icon = "👤", order = 2},
        {name = "Combat",   icon = "⚔️", order = 3},
        {name = "Visuals",  icon = "👁️", order = 4},
        {name = "Teleport", icon = "🌍", order = 5},
        {name = "Troll",    icon = "😈", order = 6},
        {name = "Misc",     icon = "🔧", order = 7},
        {name = "Server",   icon = "🖥️", order = 8},
        {name = "Settings", icon = "⚙️", order = 9},
    }

    for _, ti in ipairs(tabs) do
        local btn = Instance.new("TextButton")
        btn.Name = ti.name
        btn.Text = "  " .. ti.icon .. "  " .. ti.name
        btn.Font = Enum.Font.Gotham
        btn.TextSize = 13
        btn.TextColor3 = Theme.TextMuted
        btn.TextXAlignment = Enum.TextXAlignment.Left
        btn.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
        btn.BackgroundTransparency = 1
        btn.Size = UDim2.new(1, 0, 0, 34)
        btn.ZIndex = 13
        btn.LayoutOrder = ti.order
        btn.Parent = tabScroll
        MakeCorner(btn, 8)

        local indicator = Instance.new("Frame")
        indicator.Name = "Indicator"
        indicator.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
        indicator.Size = UDim2.new(0, 3, 0, 0)
        indicator.Position = UDim2.new(0, 0, 0.5, 0)
        indicator.AnchorPoint = Vector2.new(0, 0.5)
        indicator.ZIndex = 14
        indicator.Parent = btn
        MakeCorner(indicator, 2)
        local indGrad = Instance.new("UIGradient")
        indGrad.Color = ColorSequence.new(Color3.fromRGB(255, 200, 0), Color3.fromRGB(200, 145, 0))
        indGrad.Rotation = 90
        indGrad.Parent = indicator

        local page = Instance.new("ScrollingFrame")
        page.Name = ti.name .. "Page"
        page.BackgroundTransparency = 1
        page.Size = UDim2.new(1, 0, 1, 0)
        page.ZIndex = 12
        page.Visible = (ti.name == "Home")
        page.ScrollBarThickness = 3
        page.ScrollBarImageColor3 = Color3.fromRGB(255, 200, 0)
        page.CanvasSize = UDim2.new(0, 0, 0, 0)
        page.AutomaticCanvasSize = Enum.AutomaticSize.Y
        page.Parent = contentFrame

        local pLayout = Instance.new("UIListLayout")
        pLayout.SortOrder = Enum.SortOrder.LayoutOrder
        pLayout.Padding = UDim.new(0, 8)
        pLayout.Parent = page
        MakePadding(page, 12, 12, 12, 12)

        tabBtns[ti.name] = btn
        tabPages[ti.name] = page

        btn.MouseEnter:Connect(function()
            if S.CurrentTab ~= ti.name then
                Tween(btn, {BackgroundTransparency = 0.6, TextColor3 = Color3.fromRGB(220, 170, 0)}, 0.2)
            end
        end)
        btn.MouseLeave:Connect(function()
            if S.CurrentTab ~= ti.name then
                Tween(btn, {BackgroundTransparency = 1, TextColor3 = Theme.TextMuted}, 0.2)
            end
        end)

        btn.MouseButton1Click:Connect(function()
            if tabBtns[S.CurrentTab] then
                Tween(tabBtns[S.CurrentTab], {BackgroundTransparency = 1, TextColor3 = Theme.TextMuted}, 0.2)
                tabBtns[S.CurrentTab].Font = Enum.Font.Gotham
                local ind = tabBtns[S.CurrentTab]:FindFirstChild("Indicator")
                if ind then Tween(ind, {Size = UDim2.new(0, 3, 0, 0)}, 0.2) end
            end
            if tabPages[S.CurrentTab] then tabPages[S.CurrentTab].Visible = false end

            S.CurrentTab = ti.name
            Tween(btn, {BackgroundTransparency = 0.25, TextColor3 = Color3.fromRGB(255, 200, 0)}, 0.2)
            btn.Font = Enum.Font.GothamBold
            Tween(indicator, {Size = UDim2.new(0, 3, 0, 18)}, 0.3, Enum.EasingStyle.Back)
            page.Visible = true
        end)
    end

    if tabBtns["Home"] then
        tabBtns["Home"].Font = Enum.Font.GothamBold
        Tween(tabBtns["Home"], {BackgroundTransparency = 0.25, TextColor3 = Color3.fromRGB(255, 200, 0)}, 0.1)
        local ind = tabBtns["Home"]:FindFirstChild("Indicator")
        if ind then ind.Size = UDim2.new(0, 3, 0, 18) end
    end
end

-- ══════════════════════════════════════════
-- COMPONENTES REUTILIZÁVEIS
-- ══════════════════════════════════════════
local function Section(parent, title, order)
    local sec = Instance.new("Frame")
    sec.BackgroundColor3 = Theme.BackgroundCard
    sec.Size = UDim2.new(1, 0, 0, 0)
    sec.AutomaticSize = Enum.AutomaticSize.Y
    sec.ZIndex = 13
    sec.LayoutOrder = order or 0
    sec.Parent = parent
    MakeCorner(sec, 10)
    MakeStroke(sec, Color3.fromRGB(60, 45, 0), 1, 0.3)

    local lay = Instance.new("UIListLayout")
    lay.SortOrder = Enum.SortOrder.LayoutOrder
    lay.Padding = UDim.new(0, 3)
    lay.Parent = sec
    MakePadding(sec, 12, 12, 14, 14)

    if title then
        local tl = Instance.new("TextLabel")
        tl.Text = title
        tl.Font = Enum.Font.GothamBold
        tl.TextSize = 14
        tl.TextColor3 = Color3.fromRGB(255, 200, 0)
        tl.TextXAlignment = Enum.TextXAlignment.Left
        tl.BackgroundTransparency = 1
        tl.Size = UDim2.new(1, 0, 0, 24)
        tl.ZIndex = 14
        tl.LayoutOrder = 0
        tl.Parent = sec

        local sp = Instance.new("Frame")
        sp.BackgroundColor3 = Color3.fromRGB(80, 60, 0)
        sp.BackgroundTransparency = 0.4
        sp.Size = UDim2.new(1, 0, 0, 1)
        sp.ZIndex = 14
        sp.BorderSizePixel = 0
        sp.LayoutOrder = 1
        sp.Parent = sec
    end
    return sec
end

local function Toggle(parent, text, default, callback, order)
    local cont = Instance.new("Frame")
    cont.BackgroundColor3 = Theme.Surface
    cont.BackgroundTransparency = 0.5
    cont.Size = UDim2.new(1, 0, 0, 38)
    cont.ZIndex = 14
    cont.LayoutOrder = order or 10
    cont.Parent = parent
    MakeCorner(cont, 8)

    local lbl = Instance.new("TextLabel")
    lbl.Text = text
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 13
    lbl.TextColor3 = default and Theme.TextPrimary or Theme.TextSecondary
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.BackgroundTransparency = 1
    lbl.Size = UDim2.new(1, -58, 1, 0)
    lbl.Position = UDim2.new(0, 10, 0, 0)
    lbl.ZIndex = 15
    lbl.Parent = cont

    local bg = Instance.new("Frame")
    bg.BackgroundColor3 = default and Color3.fromRGB(255, 200, 0) or Theme.BackgroundCard
    bg.Size = UDim2.new(0, 42, 0, 22)
    bg.Position = UDim2.new(1, -52, 0.5, -11)
    bg.ZIndex = 15
    bg.Parent = cont
    MakeCorner(bg, 11)

    local circle = Instance.new("Frame")
    circle.BackgroundColor3 = default and Color3.fromRGB(0, 0, 0) or Color3.fromRGB(255, 255, 255)
    circle.Size = UDim2.new(0, 16, 0, 16)
    circle.Position = default and UDim2.new(1, -19, 0.5, -8) or UDim2.new(0, 3, 0.5, -8)
    circle.ZIndex = 16
    circle.Parent = bg
    MakeCorner(circle, 8)

    local enabled = default or false

    local btn = Instance.new("TextButton")
    btn.Text = ""
    btn.BackgroundTransparency = 1
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.ZIndex = 17
    btn.Parent = cont

    btn.MouseEnter:Connect(function() Tween(cont, {BackgroundTransparency = 0.2}, 0.2) end)
    btn.MouseLeave:Connect(function() Tween(cont, {BackgroundTransparency = 0.5}, 0.2) end)

    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            Tween(bg, {BackgroundColor3 = Color3.fromRGB(255, 200, 0)}, 0.2)
            Tween(circle, {Position = UDim2.new(1, -19, 0.5, -8), BackgroundColor3 = Color3.fromRGB(0, 0, 0)}, 0.2, Enum.EasingStyle.Back)
            lbl.TextColor3 = Theme.TextPrimary
        else
            Tween(bg, {BackgroundColor3 = Theme.BackgroundCard}, 0.2)
            Tween(circle, {Position = UDim2.new(0, 3, 0.5, -8), BackgroundColor3 = Color3.fromRGB(255, 255, 255)}, 0.2, Enum.EasingStyle.Back)
            lbl.TextColor3 = Theme.TextSecondary
        end
        if callback then callback(enabled) end
    end)

    return cont, function() return enabled end, function(val)
        enabled = val
        if enabled then
            bg.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
            circle.Position = UDim2.new(1, -19, 0.5, -8)
            circle.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
            lbl.TextColor3 = Theme.TextPrimary
        else
            bg.BackgroundColor3 = Theme.BackgroundCard
            circle.Position = UDim2.new(0, 3, 0.5, -8)
            circle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            lbl.TextColor3 = Theme.TextSecondary
        end
        if callback then callback(enabled) end
    end
end

local function Slider(parent, text, min, max, default, callback, order)
    local cont = Instance.new("Frame")
    cont.BackgroundColor3 = Theme.Surface
    cont.BackgroundTransparency = 0.5
    cont.Size = UDim2.new(1, 0, 0, 52)
    cont.ZIndex = 14
    cont.LayoutOrder = order or 10
    cont.Parent = parent
    MakeCorner(cont, 8)

    local lbl = Instance.new("TextLabel")
    lbl.Text = text
    lbl.Font = Enum.Font.Gotham
    lbl.TextSize = 12
    lbl.TextColor3 = Theme.TextSecondary
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.BackgroundTransparency = 1
    lbl.Size = UDim2.new(1, -60, 0, 18)
    lbl.Position = UDim2.new(0, 10, 0, 5)
    lbl.ZIndex = 15
    lbl.Parent = cont

    local valLbl = Instance.new("TextLabel")
    valLbl.Text = tostring(default or min)
    valLbl.Font = Enum.Font.GothamBold
    valLbl.TextSize = 12
    valLbl.TextColor3 = Color3.fromRGB(255, 200, 0)
    valLbl.TextXAlignment = Enum.TextXAlignment.Right
    valLbl.BackgroundTransparency = 1
    valLbl.Size = UDim2.new(0, 50, 0, 18)
    valLbl.Position = UDim2.new(1, -60, 0, 5)
    valLbl.ZIndex = 15
    valLbl.Parent = cont

    local slBg = Instance.new("Frame")
    slBg.BackgroundColor3 = Theme.BackgroundCard
    slBg.Size = UDim2.new(1, -20, 0, 6)
    slBg.Position = UDim2.new(0, 10, 0, 33)
    slBg.ZIndex = 15
    slBg.Parent = cont
    MakeCorner(slBg, 3)

    local initPct = ((default or min) - min) / math.max(max - min, 1)
    local slFill = Instance.new("Frame")
    slFill.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    slFill.Size = UDim2.new(initPct, 0, 1, 0)
    slFill.ZIndex = 16
    slFill.Parent = slBg
    MakeCorner(slFill, 3)
    local slGrad = Instance.new("UIGradient")
    slGrad.Color = ColorSequence.new(Color3.fromRGB(200, 145, 0), Color3.fromRGB(255, 230, 60))
    slGrad.Rotation = 0
    slGrad.Parent = slFill

    local knob = Instance.new("Frame")
    knob.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    knob.Size = UDim2.new(0, 14, 0, 14)
    knob.Position = UDim2.new(initPct, -7, 0.5, -7)
    knob.ZIndex = 17
    knob.Parent = slBg
    MakeCorner(knob, 7)
    MakeStroke(knob, Color3.fromRGB(0, 0, 0), 2, 0.5)

    local sliding = false

    local inputArea = Instance.new("TextButton")
    inputArea.Text = ""
    inputArea.BackgroundTransparency = 1
    inputArea.Size = UDim2.new(1, 0, 0, 22)
    inputArea.Position = UDim2.new(0, 0, 0, 25)
    inputArea.ZIndex = 18
    inputArea.Parent = cont

    local function Update(x)
        local ap = slBg.AbsolutePosition.X
        local as = slBg.AbsoluteSize.X
        local pct = math.clamp((x - ap) / as, 0, 1)
        local val = math.floor(min + (max - min) * pct)
        slFill.Size = UDim2.new(pct, 0, 1, 0)
        knob.Position = UDim2.new(pct, -7, 0.5, -7)
        valLbl.Text = tostring(val)
        if callback then callback(val) end
    end

    inputArea.InputBegan:Connect(function(inp)
        if inp.UserInputType == Enum.UserInputType.MouseButton1 then
            sliding = true
            Update(inp.Position.X)
        end
    end)
    UserInputService.InputChanged:Connect(function(inp)
        if sliding and inp.UserInputType == Enum.UserInputType.MouseMovement then
            Update(inp.Position.X)
        end
    end)
    UserInputService.InputEnded:Connect(function(inp)
        if inp.UserInputType == Enum.UserInputType.MouseButton1 then sliding = false end
    end)

    return cont
end

local function Button(parent, text, callback, order, color)
    local btn = Instance.new("TextButton")
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    btn.TextColor3 = color and Theme.TextPrimary or Color3.fromRGB(0, 0, 0)
    btn.BackgroundColor3 = color or Color3.fromRGB(255, 200, 0)
    btn.Size = UDim2.new(1, 0, 0, 36)
    btn.ZIndex = 15
    btn.LayoutOrder = order or 10
    btn.Parent = parent
    MakeCorner(btn, 8)
    if not color then
        local bGrad = Instance.new("UIGradient")
        bGrad.Color = ColorSequence.new(Color3.fromRGB(255, 220, 40), Color3.fromRGB(200, 145, 0))
        bGrad.Rotation = 90
        bGrad.Parent = btn
    end

    btn.MouseEnter:Connect(function() Tween(btn, {BackgroundTransparency = 0.15}, 0.2) end)
    btn.MouseLeave:Connect(function() Tween(btn, {BackgroundTransparency = 0}, 0.2) end)
    btn.MouseButton1Click:Connect(function()
        Tween(btn, {Size = UDim2.new(1, -6, 0, 33)}, 0.08)
        task.wait(0.08)
        Tween(btn, {Size = UDim2.new(1, 0, 0, 36)}, 0.08)
        if callback then callback() end
    end)
    return btn
end

local function Label(parent, text, order)
    local l = Instance.new("TextLabel")
    l.Text = text
    l.Font = Enum.Font.Gotham
    l.TextSize = 12
    l.TextColor3 = Theme.TextSecondary
    l.TextXAlignment = Enum.TextXAlignment.Left
    l.TextWrapped = true
    l.BackgroundTransparency = 1
    l.Size = UDim2.new(1, 0, 0, 0)
    l.AutomaticSize = Enum.AutomaticSize.Y
    l.ZIndex = 15
    l.LayoutOrder = order or 10
    l.Parent = parent
    return l
end

local function TextInput(parent, placeholder, callback, order)
    local cont = Instance.new("Frame")
    cont.BackgroundColor3 = Theme.Surface
    cont.BackgroundTransparency = 0.3
    cont.Size = UDim2.new(1, 0, 0, 36)
    cont.ZIndex = 14
    cont.LayoutOrder = order or 10
    cont.Parent = parent
    MakeCorner(cont, 8)

    local input = Instance.new("TextBox")
    input.PlaceholderText = placeholder or "Digite aqui..."
    input.PlaceholderColor3 = Theme.TextMuted
    input.Text = ""
    input.Font = Enum.Font.Gotham
    input.TextSize = 13
    input.TextColor3 = Theme.TextPrimary
    input.BackgroundTransparency = 1
    input.Size = UDim2.new(1, -16, 1, 0)
    input.Position = UDim2.new(0, 8, 0, 0)
    input.ZIndex = 15
    input.ClearTextOnFocus = false
    input.Parent = cont

    input.Focused:Connect(function()
        MakeStroke(cont, Color3.fromRGB(255, 200, 0), 1.5, 0.1).Name = "FStroke"
    end)
    input.FocusLost:Connect(function(enter)
        local fs = cont:FindFirstChild("FStroke")
        if fs then fs:Destroy() end
        if enter and callback then callback(input.Text) end
    end)

    return cont, input
end

local function Dropdown(parent, text, options, callback, order)
    local cont = Instance.new("Frame")
    cont.BackgroundColor3 = Theme.Surface
    cont.BackgroundTransparency = 0.5
    cont.Size = UDim2.new(1, 0, 0, 38)
    cont.AutomaticSize = Enum.AutomaticSize.Y
    cont.ZIndex = 14
    cont.LayoutOrder = order or 10
    cont.ClipsDescendants = true
    cont.Parent = parent
    MakeCorner(cont, 8)

    local lay = Instance.new("UIListLayout")
    lay.SortOrder = Enum.SortOrder.LayoutOrder
    lay.Padding = UDim.new(0, 2)
    lay.Parent = cont
    MakePadding(cont, 0, 4, 0, 0)

    local header = Instance.new("TextButton")
    header.Text = "  " .. text .. "  ▼"
    header.Font = Enum.Font.Gotham
    header.TextSize = 13
    header.TextColor3 = Theme.TextSecondary
    header.TextXAlignment = Enum.TextXAlignment.Left
    header.BackgroundTransparency = 1
    header.Size = UDim2.new(1, 0, 0, 34)
    header.ZIndex = 15
    header.LayoutOrder = 0
    header.Parent = cont

    local isOpen = false
    local optionFrames = {}

    for i, opt in ipairs(options) do
        local optBtn = Instance.new("TextButton")
        optBtn.Text = "    " .. opt
        optBtn.Font = Enum.Font.Gotham
        optBtn.TextSize = 12
        optBtn.TextColor3 = Theme.TextMuted
        optBtn.TextXAlignment = Enum.TextXAlignment.Left
        optBtn.BackgroundColor3 = Theme.BackgroundCard
        optBtn.BackgroundTransparency = 0.5
        optBtn.Size = UDim2.new(1, -8, 0, 30)
        optBtn.Position = UDim2.new(0, 4, 0, 0)
        optBtn.ZIndex = 16
        optBtn.LayoutOrder = i
        optBtn.Visible = false
        optBtn.Parent = cont
        MakeCorner(optBtn, 6)
        table.insert(optionFrames, optBtn)

        optBtn.MouseEnter:Connect(function()
            Tween(optBtn, {BackgroundTransparency = 0.2, TextColor3 = Color3.fromRGB(255, 200, 0)}, 0.15)
        end)
        optBtn.MouseLeave:Connect(function()
            Tween(optBtn, {BackgroundTransparency = 0.5, TextColor3 = Theme.TextMuted}, 0.15)
        end)
        optBtn.MouseButton1Click:Connect(function()
            header.Text = "  " .. text .. ": " .. opt .. "  ▼"
            if callback then callback(opt, i) end
            isOpen = false
            for _, of in pairs(optionFrames) do of.Visible = false end
        end)
    end

    header.MouseButton1Click:Connect(function()
        isOpen = not isOpen
        for _, of in pairs(optionFrames) do of.Visible = isOpen end
    end)

    return cont
end

local function PlayerSelector(parent, title, callback, order)
    local sec = Section(parent, title, order)

    local refreshBtn = Button(sec, "🔄 Atualizar Lista", nil, 3)

    local playerList = Instance.new("Frame")
    playerList.BackgroundTransparency = 1
    playerList.Size = UDim2.new(1, 0, 0, 0)
    playerList.AutomaticSize = Enum.AutomaticSize.Y
    playerList.ZIndex = 15
    playerList.LayoutOrder = 5
    playerList.Parent = sec

    local pLay = Instance.new("UIListLayout")
    pLay.SortOrder = Enum.SortOrder.LayoutOrder
    pLay.Padding = UDim.new(0, 3)
    pLay.Parent = playerList

    local function Refresh()
        for _, c in pairs(playerList:GetChildren()) do
            if c:IsA("TextButton") then c:Destroy() end
        end
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Player then
                local b = Instance.new("TextButton")
                b.Text = "  🏎️ " .. plr.DisplayName .. " (@" .. plr.Name .. ")"
                b.Font = Enum.Font.Gotham
                b.TextSize = 12
                b.TextColor3 = Theme.TextSecondary
                b.TextXAlignment = Enum.TextXAlignment.Left
                b.BackgroundColor3 = Theme.Surface
                b.BackgroundTransparency = 0.5
                b.Size = UDim2.new(1, 0, 0, 30)
                b.ZIndex = 16
                b.Parent = playerList
                MakeCorner(b, 6)

                b.MouseEnter:Connect(function()
                    Tween(b, {BackgroundTransparency = 0.2, TextColor3 = Color3.fromRGB(255, 200, 0)}, 0.15)
                end)
                b.MouseLeave:Connect(function()
                    Tween(b, {BackgroundTransparency = 0.5, TextColor3 = Theme.TextSecondary}, 0.15)
                end)
                b.MouseButton1Click:Connect(function()
                    if callback then callback(plr) end
                end)
            end
        end
    end

    refreshBtn.MouseButton1Click:Connect(Refresh)
    Refresh()
    Players.PlayerAdded:Connect(function() task.wait(0.5); Refresh() end)
    Players.PlayerRemoving:Connect(function() task.wait(0.5); Refresh() end)

    return sec
end

-- ══════════════════════════════════════════
-- CONTEÚDO DAS TABS
-- ══════════════════════════════════════════

local function SetupHome()
    local p = tabPages["Home"]
    if not p then return end

    -- Banner temático Lamborghini
    local banner = Instance.new("Frame")
    banner.BackgroundColor3 = Color3.fromRGB(20, 16, 0)
    banner.Size = UDim2.new(1, 0, 0, 110)
    banner.ZIndex = 13
    banner.LayoutOrder = 1
    banner.Parent = p
    MakeCorner(banner, 12)
    MakeStroke(banner, Color3.fromRGB(255, 200, 0), 1.5, 0.2)

    -- Gradiente amarelo/preto Lambo
    local banGrad = Instance.new("UIGradient")
    banGrad.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 30, 0)),
        ColorSequenceKeypoint.new(0.5, Color3.fromRGB(20, 15, 0)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(5, 4, 0)),
    })
    banGrad.Rotation = 135
    banGrad.Parent = banner

    -- Detalhe lateral amarelo
    local banStripe = Instance.new("Frame")
    banStripe.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    banStripe.Size = UDim2.new(0, 4, 1, -16)
    banStripe.Position = UDim2.new(0, 8, 0, 8)
    banStripe.ZIndex = 14
    banStripe.BorderSizePixel = 0
    banStripe.Parent = banner
    MakeCorner(banStripe, 2)
    local bsGrad = Instance.new("UIGradient")
    bsGrad.Color = ColorSequence.new(Color3.fromRGB(255, 230, 60), Color3.fromRGB(200, 145, 0))
    bsGrad.Rotation = 90
    bsGrad.Parent = banStripe

    local bTitle = Instance.new("TextLabel")
    bTitle.Text = "🏎️ Lamborghini Hub"
    bTitle.Font = Enum.Font.GothamBold
    bTitle.TextSize = 26
    bTitle.TextColor3 = Color3.fromRGB(255, 200, 0)
    bTitle.TextXAlignment = Enum.TextXAlignment.Left
    bTitle.BackgroundTransparency = 1
    bTitle.Size = UDim2.new(1, -30, 0, 35)
    bTitle.Position = UDim2.new(0, 22, 0, 14)
    bTitle.ZIndex = 14
    bTitle.Parent = banner

    local bSlogan = Instance.new("TextLabel")
    bSlogan.Text = '"Expect the Unexpected" 🐂'
    bSlogan.Font = Enum.Font.GothamBold
    bSlogan.TextSize = 11
    bSlogan.TextColor3 = Color3.fromRGB(180, 140, 30)
    bSlogan.TextXAlignment = Enum.TextXAlignment.Left
    bSlogan.BackgroundTransparency = 1
    bSlogan.Size = UDim2.new(1, -30, 0, 18)
    bSlogan.Position = UDim2.new(0, 22, 0, 46)
    bSlogan.ZIndex = 14
    bSlogan.Parent = banner

    local bDesc = Instance.new("TextLabel")
    bDesc.Text = "O hub mais veloz do Roblox. ESP, Fly, Troll, Combat, Teleport e muito mais!"
    bDesc.Font = Enum.Font.Gotham
    bDesc.TextSize = 12
    bDesc.TextColor3 = Color3.fromRGB(160, 130, 60)
    bDesc.TextXAlignment = Enum.TextXAlignment.Left
    bDesc.TextWrapped = true
    bDesc.BackgroundTransparency = 1
    bDesc.Size = UDim2.new(1, -30, 0, 35)
    bDesc.Position = UDim2.new(0, 22, 0, 66)
    bDesc.ZIndex = 14
    bDesc.Parent = banner

    local stats = Section(p, "📊 Estatísticas em Tempo Real", 2)

    local function Stat(parent, icon, label, val, ord)
        local f = Instance.new("Frame")
        f.BackgroundColor3 = Theme.Surface
        f.BackgroundTransparency = 0.3
        f.Size = UDim2.new(1, 0, 0, 32)
        f.ZIndex = 15
        f.LayoutOrder = ord
        f.Parent = parent
        MakeCorner(f, 6)

        local sl = Instance.new("TextLabel")
        sl.Text = icon .. "  " .. label
        sl.Font = Enum.Font.Gotham
        sl.TextSize = 12
        sl.TextColor3 = Theme.TextSecondary
        sl.TextXAlignment = Enum.TextXAlignment.Left
        sl.BackgroundTransparency = 1
        sl.Size = UDim2.new(0.65, 0, 1, 0)
        sl.Position = UDim2.new(0, 10, 0, 0)
        sl.ZIndex = 16
        sl.Parent = f

        local sv = Instance.new("TextLabel")
        sv.Name = "Val"
        sv.Text = val
        sv.Font = Enum.Font.GothamBold
        sv.TextSize = 12
        sv.TextColor3 = Color3.fromRGB(255, 200, 0)
        sv.TextXAlignment = Enum.TextXAlignment.Right
        sv.BackgroundTransparency = 1
        sv.Size = UDim2.new(0.3, 0, 1, 0)
        sv.Position = UDim2.new(0.65, 0, 0, 0)
        sv.ZIndex = 16
        sv.Parent = f

        return f
    end

    local fpsS = Stat(stats, "📈", "FPS", "60", 5)
    local pingS = Stat(stats, "📡", "Ping", "0ms", 6)
    local plrS = Stat(stats, "👥", "Jogadores", tostring(#Players:GetPlayers()), 7)

    pcall(function()
        local name = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name
        Stat(stats, "🎮", "Jogo", name, 8)
    end)
    Stat(stats, "🆔", "Place ID", tostring(game.PlaceId), 9)
    Stat(stats, "🖥️", "Job ID", string.sub(game.JobId, 1, 12) .. "...", 10)

    task.spawn(function()
        while task.wait(1) do
            pcall(function()
                local fps = math.floor(1 / RunService.RenderStepped:Wait())
                local ping = math.floor(Player:GetNetworkPing() * 1000)
                local fv = fpsS:FindFirstChild("Val")
                local pv = pingS:FindFirstChild("Val")
                local plv = plrS:FindFirstChild("Val")
                if fv then fv.Text = tostring(fps) end
                if pv then pv.Text = ping .. "ms" end
                if plv then plv.Text = tostring(#Players:GetPlayers()) end
            end)
        end
    end)

    local quick = Section(p, "⚡ Ações Rápidas", 3)
    Label(quick, "Ative features rapidamente:", 4)

    Button(quick, "🚀 Ativar Speed (100)", function()
        S.Speed = true; S.SpeedVal = 100
        local h = GetHum()
        if h then h.WalkSpeed = 100 end
        Notify("Speed", "Speed 100 ativado!", 2, "success")
    end, 5)

    Button(quick, "✈️ Ativar Fly", function()
        S.Fly = true
        Notify("Fly", "Fly ativado! WASD + Space/Shift", 2, "success")
    end, 6)

    Button(quick, "👁️ Ativar ESP", function()
        S.ESP = true
        Notify("ESP", "ESP ativado!", 2, "success")
    end, 7)

    local info = Section(p, "ℹ️ Informações", 4)
    Label(info, "🔑 Toggle: Right Control", 5)
    Label(info, "📌 9 abas com dezenas de funcionalidades", 6)
    Label(info, "💻 Executor: " .. (identifyexecutor and identifyexecutor() or "Desconhecido"), 7)
    Label(info, "🏎️ Lamborghini Hub — Premium Ativo ✓", 8)
end

local function SetupPlayer()
    local p = tabPages["Player"]
    if not p then return end

    local move = Section(p, "🏃 Movimento", 1)

    Toggle(move, "🚀 Speed Hack", false, function(v)
        S.Speed = v
        if not v then pcall(function() GetHum().WalkSpeed = 16 end) end
        Notify("Speed", v and "Ativado!" or "Desativado!", 2, v and "success" or "info")
    end, 5)

    Slider(move, "Velocidade", 16, 500, 50, function(v) S.SpeedVal = v end, 6)

    Toggle(move, "⬆️ Jump Power", false, function(v)
        S.JumpPower = v
        if not v then pcall(function() GetHum().JumpPower = 50; GetHum().UseJumpPower = true end) end
        Notify("Jump", v and "Ativado!" or "Desativado!", 2, v and "success" or "info")
    end, 7)

    Slider(move, "Força do Pulo", 50, 500, 100, function(v) S.JumpVal = v end, 8)

    Toggle(move, "♾️ Infinite Jump", false, function(v)
        S.InfiniteJump = v
        Notify("Inf Jump", v and "Pule infinitamente!" or "Desativado!", 2, v and "success" or "info")
    end, 9)

    Toggle(move, "🤸 Auto Jump", false, function(v)
        S.AutoJump = v
        pcall(function() GetHum().AutoJumpEnabled = v end)
    end, 10)

    local fly = Section(p, "✈️ Sistema de Voo", 2)

    Toggle(fly, "🕊️ Fly Mode", false, function(v)
        S.Fly = v
        Notify("Fly", v and "Use WASD + Space/Shift!" or "Desativado!", 2, v and "success" or "info")
    end, 5)

    Slider(fly, "Velocidade de Voo", 10, 300, 50, function(v) S.FlySpeed = v end, 6)

    local prot = Section(p, "🛡️ Proteção & Imortalidade", 3)

    Toggle(prot, "❤️ God Mode (Vida Infinita)", false, function(v)
        S.GodMode = v
        Notify("God", v and "Invencível!" or "Desativado!", 2, v and "success" or "info")
    end, 5)

    Toggle(prot, "👻 Noclip (Atravessar Paredes)", false, function(v)
        S.Noclip = v
        Notify("Noclip", v and "Atravesse tudo!" or "Desativado!", 2, v and "success" or "info")
    end, 6)

    Toggle(prot, "🌀 Anti-Void", false, function(v)
        S.AntiVoid = v
        Notify("Anti-Void", v and "Proteção ativada!" or "Desativado!", 2, v and "success" or "info")
    end, 7)

    Toggle(prot, "❄️ Congelar Posição", false, function(v)
        S.FreezeSelf = v
        if v then
            local r = GetRoot()
            if r then S.FreezePos = r.CFrame end
        end
        Notify("Freeze", v and "Posição congelada!" or "Descongelado!", 2, v and "warning" or "info")
    end, 8)

    local body = Section(p, "🧍 Modificações de Corpo", 4)

    Toggle(body, "🔄 SpinBot", false, function(v)
        S.SpinBot = v
        Notify("Spin", v and "Girando!" or "Parou!", 2, v and "warning" or "info")
    end, 5)

    Slider(body, "Velocidade do Spin", 1, 50, 10, function(v) S.SpinSpeed = v end, 6)

    Toggle(body, "🐜 Personagem Minúsculo", false, function(v)
        S.TinyCharacter = v
        pcall(function()
            local c = GetChar()
            if c then
                local hum = c:FindFirstChildOfClass("Humanoid")
                if hum then
                    if v then
                        hum.HeadScale.Value = 0.5
                        hum.BodyDepthScale.Value = 0.5
                        hum.BodyHeightScale.Value = 0.5
                        hum.BodyWidthScale.Value = 0.5
                    else
                        hum.HeadScale.Value = 1
                        hum.BodyDepthScale.Value = 1
                        hum.BodyHeightScale.Value = 1
                        hum.BodyWidthScale.Value = 1
                    end
                end
            end
        end)
    end, 7)

    Toggle(body, "🏔️ Low Gravity", false, function(v)
        S.LowGravity = v
        Workspace.Gravity = v and 50 or 196.2
        Notify("Gravity", v and "Gravidade baixa!" or "Normal!", 2, v and "success" or "info")
    end, 8)

    local acts = Section(p, "⚡ Ações do Player", 5)

    Button(acts, "🔄 Resetar Personagem", function()
        pcall(function() GetHum().Health = 0 end)
        Notify("Reset", "Personagem resetado!", 2, "info")
    end, 5)

    Button(acts, "💊 Curar Completo", function()
        pcall(function() GetHum().Health = GetHum().MaxHealth end)
        Notify("Heal", "Vida completa!", 2, "success")
    end, 6)

    Button(acts, "📍 Salvar Posição Atual", function()
        local r = GetRoot()
        if r then
            S.SavedPos = r.CFrame
            Notify("Salvo", "Posição salva com sucesso!", 2, "success")
        end
    end, 7)

    Button(acts, "📌 Voltar à Posição Salva", function()
        local r = GetRoot()
        if r and S.SavedPos then
            r.CFrame = S.SavedPos
            Notify("TP", "Voltou à posição salva!", 2, "success")
        else
            Notify("Erro", "Nenhuma posição salva!", 2, "error")
        end
    end, 8)

    Button(acts, "🧹 Remover Acessórios", function()
        pcall(function()
            for _, acc in pairs(GetChar():GetChildren()) do
                if acc:IsA("Accessory") then acc:Destroy() end
            end
        end)
        Notify("Acessórios", "Todos removidos!", 2, "info")
    end, 9)

    Button(acts, "🔦 Lanterna no Head", function()
        pcall(function()
            local head = GetHead()
            if head and not head:FindFirstChild("NovaLight") then
                local sl = Instance.new("SpotLight")
                sl.Name = "NovaLight"
                sl.Brightness = 5
                sl.Range = 60
                sl.Angle = 90
                sl.Parent = head
                Notify("Luz", "Lanterna ativada!", 2, "success")
            else
                local l = head:FindFirstChild("NovaLight")
                if l then l:Destroy() end
                Notify("Luz", "Lanterna removida!", 2, "info")
            end
        end)
    end, 10)
end

local function SetupCombat()
    local p = tabPages["Combat"]
    if not p then return end

    local aura = Section(p, "⚔️ Kill Aura & Auto Attack", 1)

    Toggle(aura, "💀 Kill Aura", false, function(v)
        S.KillAura = v
        Notify("Kill Aura", v and "Ativado!" or "Desativado!", 2, v and "warning" or "info")
    end, 5)

    Slider(aura, "Alcance da Aura", 5, 80, 15, function(v) S.KillAuraRange = v end, 6)

    Toggle(aura, "🗡️ Auto Attack (Click)", false, function(v)
        S.AutoAttack = v
        Notify("Auto Attack", v and "Atacando automaticamente!" or "Parou!", 2, v and "success" or "info")
    end, 7)

    Toggle(aura, "🔄 Auto Combo", false, function(v)
        S.AutoCombo = v
    end, 8)

    Toggle(aura, "⚡ Spam Click", false, function(v)
        S.SpamClick = v
        Notify("Spam Click", v and "Clicando rapidamente!" or "Parou!", 2, v and "warning" or "info")
    end, 9)

    local hitbox = Section(p, "🎯 Hitbox & Reach", 2)

    Toggle(hitbox, "📦 Hitbox Expander (Só Inimigos)", false, function(v)
        S.HitboxExpand = v
        if not v then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local root = plr.Character:FindFirstChild("HumanoidRootPart")
                    if root then
                        root.Size = Vector3.new(2, 2, 1)
                        root.Transparency = 1
                        root.Material = Enum.Material.Plastic
                    end
                end
            end
        end
        Notify("Hitbox", v and "Hitboxes expandidos (só inimigos)!" or "Restaurado!", 2, v and "warning" or "info")
    end, 5)

    Slider(hitbox, "Tamanho do Hitbox", 2, 50, 10, function(v) S.HitboxSize = v end, 6)

    Toggle(hitbox, "🦾 Reach Extendido", false, function(v)
        S.Reach = v
    end, 7)

    Slider(hitbox, "Distância do Reach", 5, 60, 20, function(v) S.ReachVal = v end, 8)

    local aim = Section(p, "🎯 AimBot", 3)

    Toggle(aim, "🎯 AimBot (Auto Aim)", false, function(v)
        S.AimBot = v
        Notify("AimBot", v and "Mira automática ativada!" or "Desativado!", 2, v and "success" or "info")
    end, 5)

    Slider(aim, "Suavidade da Mira", 1, 10, 5, function(v) S.AimSmooth = v / 10 end, 6)

    local cutil = Section(p, "🔥 Utilitários de Combate", 4)

    Button(cutil, "💥 TP para Todos os Players (Loop)", function()
        Notify("TP Kill", "Teleportando para cada player...", 3, "warning")
        task.spawn(function()
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local r = GetRoot()
                    local tr = plr.Character:FindFirstChild("HumanoidRootPart")
                    if r and tr then
                        r.CFrame = tr.CFrame * CFrame.new(0, 0, 2)
                        task.wait(0.5)
                    end
                end
            end
            Notify("Concluído", "Loop de TP finalizado!", 2, "success")
        end)
    end, 5)

    Button(cutil, "👊 Forçar Punch em Todos", function()
        Notify("Punch", "Tentando atacar todos...", 2, "warning")
        pcall(function()
            local tool = Player.Backpack:FindFirstChildOfClass("Tool") or GetChar():FindFirstChildOfClass("Tool")
            if tool then
                local char = GetChar()
                if tool.Parent ~= char then tool.Parent = char end
                for _, plr in pairs(Players:GetPlayers()) do
                    if plr ~= Player and plr.Character then
                        pcall(function() tool:Activate() end)
                    end
                end
            end
        end)
    end, 6)

    Button(cutil, "🧲 Trazer Todos para Você", function()
        Notify("Bring", "Tentando trazer...", 2, "warning")
        local r = GetRoot()
        if r then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local tr = plr.Character:FindFirstChild("HumanoidRootPart")
                    if tr then
                        pcall(function() tr.CFrame = r.CFrame * CFrame.new(0, 0, 3) end)
                    end
                end
            end
        end
    end, 7)
end

local function SetupVisuals()
    local p = tabPages["Visuals"]
    if not p then return end

    local esp = Section(p, "👁️ ESP (Visão Extra)", 1)

    Toggle(esp, "📍 ESP de Jogadores", false, function(v)
        S.ESP = v
        Notify("ESP", v and "ESP ativado! Veja todos." or "Desativado!", 2, v and "success" or "info")
    end, 5)

    Toggle(esp, "🔲 Chams (Highlight)", false, function(v)
        S.Chams = v
        if not v then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr.Character then
                    local h = plr.Character:FindFirstChild("NovaHL")
                    if h then h:Destroy() end
                end
            end
        end
        Notify("Chams", v and "Highlighting players!" or "Removido!", 2, v and "success" or "info")
    end, 6)

    Toggle(esp, "📏 Tracers (Linhas)", false, function(v)
        S.Tracers = v
    end, 7)

    Toggle(esp, "📦 Item ESP", false, function(v)
        S.ItemESP = v
        Notify("Item ESP", v and "Mostrando itens no mapa!" or "Desativado!", 2, v and "success" or "info")
    end, 8)

    local world = Section(p, "🌍 Mundo & Iluminação", 2)

    Toggle(world, "☀️ Fullbright", false, function(v)
        S.Fullbright = v
        if v then
            Lighting.Brightness = 3
            Lighting.ClockTime = 14
            Lighting.FogEnd = 999999
            Lighting.GlobalShadows = false
            Lighting.OutdoorAmbient = Color3.fromRGB(150, 150, 150)
            Lighting.Ambient = Color3.fromRGB(200, 200, 200)
        else
            Lighting.Brightness = 1
            Lighting.ClockTime = 14
            Lighting.FogEnd = 10000
            Lighting.GlobalShadows = true
            Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
            Lighting.Ambient = Color3.fromRGB(0, 0, 0)
        end
        Notify("Fullbright", v and "Tudo iluminado!" or "Normal!", 2, v and "success" or "info")
    end, 5)

    Toggle(world, "🌫️ Remover Fog (Névoa)", false, function(v)
        S.NoFog = v
        Lighting.FogEnd = v and 999999 or 10000
        Lighting.FogStart = v and 999999 or 0
    end, 6)

    Toggle(world, "🔎 X-Ray (Ver através de paredes)", false, function(v)
        S.Xray = v
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") and not Players:GetPlayerFromCharacter(obj.Parent) then
                if v then
                    if obj.Transparency < 0.5 then
                        obj:SetAttribute("OrigTransparency", obj.Transparency)
                        obj.Transparency = 0.7
                    end
                else
                    local orig = obj:GetAttribute("OrigTransparency")
                    if orig then obj.Transparency = orig end
                end
            end
        end
        Notify("X-Ray", v and "Paredes transparentes!" or "Normal!", 2, v and "success" or "info")
    end, 7)

    Toggle(world, "🎨 Modo Rainbow (Céu)", false, function(v)
        S.Rainbow = v
    end, 8)

    Toggle(world, "❌ Remover Partículas", false, function(v)
        S.NoParticles = v
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
                obj.Enabled = not v
            end
        end
    end, 9)

    Toggle(world, "📉 Gráficos Baixos", false, function(v)
        S.LowGraphics = v
        if v then
            settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
            for _, obj in pairs(Lighting:GetDescendants()) do
                if obj:IsA("BloomEffect") or obj:IsA("BlurEffect") or obj:IsA("SunRaysEffect") or obj:IsA("ColorCorrectionEffect") or obj:IsA("DepthOfFieldEffect") then
                    obj.Enabled = false
                end
            end
            Lighting.GlobalShadows = false
        else
            settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
            for _, obj in pairs(Lighting:GetDescendants()) do
                if obj:IsA("PostEffect") then obj.Enabled = true end
            end
            Lighting.GlobalShadows = true
        end
        Notify("Graphics", v and "Gráficos no mínimo!" or "Normal!", 2, v and "success" or "info")
    end, 10)

    local times = Section(p, "🕐 Controle de Horário", 3)
    Button(times, "🌅 Dia (14h)", function() Lighting.ClockTime = 14 end, 5)
    Button(times, "🌇 Pôr do Sol (18h)", function() Lighting.ClockTime = 18 end, 6)
    Button(times, "🌙 Noite (0h)", function() Lighting.ClockTime = 0 end, 7)
    Button(times, "🌄 Amanhecer (6h)", function() Lighting.ClockTime = 6 end, 8)

    local cam = Section(p, "📷 Câmera", 4)
    Button(cam, "🔍 FOV 120 (Máximo)", function() Camera.FieldOfView = 120 end, 5)
    Button(cam, "🔭 FOV 70 (Normal)", function() Camera.FieldOfView = 70 end, 6)
    Button(cam, "🔬 FOV 30 (Zoom)", function() Camera.FieldOfView = 30 end, 7)
    Slider(cam, "FOV Customizado", 10, 120, 70, function(v) Camera.FieldOfView = v end, 8)
end

local function SetupTeleport()
    local p = tabPages["Teleport"]
    if not p then return end

    PlayerSelector(p, "🎯 Teleportar para Jogador", function(plr)
        local r = GetRoot()
        local tc = plr.Character
        if r and tc then
            local tr = tc:FindFirstChild("HumanoidRootPart")
            if tr then
                r.CFrame = tr.CFrame * CFrame.new(0, 0, 3)
                Notify("TP", "Teleportado para " .. plr.DisplayName .. "!", 2, "success")
            end
        end
    end, 1)

    local clickSec = Section(p, "🖱️ Teleporte por Clique", 2)
    Toggle(clickSec, "🖱️ Click Teleport", false, function(v)
        S.ClickTP = v
        Notify("Click TP", v and "Clique para teleportar!" or "Off!", 2, v and "success" or "info")
    end, 5)
    Label(clickSec, "Ative e clique em qualquer lugar do mapa.", 6)

    local way = Section(p, "📌 Waypoints Rápidos", 3)
    Button(way, "🏠 Ir para o Spawn", function()
        local r = GetRoot()
        if r then
            local sp = Workspace:FindFirstChildOfClass("SpawnLocation")
            if sp then r.CFrame = sp.CFrame + Vector3.new(0, 5, 0)
            else r.CFrame = CFrame.new(0, 50, 0) end
            Notify("TP", "Teleportado ao Spawn!", 2, "success")
        end
    end, 5)
    Button(way, "⬆️ Ir para o Céu (Y=1000)", function()
        local r = GetRoot()
        if r then r.CFrame = CFrame.new(r.CFrame.X, 1000, r.CFrame.Z) end
        Notify("TP", "No céu!", 2, "success")
    end, 6)
    Button(way, "⬇️ Ir para o Chão (Y=5)", function()
        local r = GetRoot()
        if r then r.CFrame = CFrame.new(r.CFrame.X, 5, r.CFrame.Z) end
    end, 7)
    Button(way, "🎲 Posição Aleatória no Mapa", function()
        local r = GetRoot()
        if r then
            local x = math.random(-500, 500)
            local z = math.random(-500, 500)
            r.CFrame = CFrame.new(x, 100, z)
            Notify("TP", string.format("(%d, 100, %d)", x, z), 2, "info")
        end
    end, 8)

    local saveSec = Section(p, "💾 Waypoints Salvos", 4)
    local savedList = Instance.new("Frame")
    savedList.BackgroundTransparency = 1
    savedList.Size = UDim2.new(1, 0, 0, 0)
    savedList.AutomaticSize = Enum.AutomaticSize.Y
    savedList.ZIndex = 15
    savedList.LayoutOrder = 10
    savedList.Parent = saveSec
    local slLay = Instance.new("UIListLayout")
    slLay.Padding = UDim.new(0, 3)
    slLay.Parent = savedList

    local _, wpInput = TextInput(saveSec, "Nome do waypoint...", nil, 5)
    Button(saveSec, "💾 Salvar Posição Atual", function()
        local r = GetRoot()
        if r then
            local name = wpInput.Text ~= "" and wpInput.Text or ("WP-" .. #S.SavedPositions + 1)
            table.insert(S.SavedPositions, {name = name, cf = r.CFrame})
            local wb = Instance.new("TextButton")
            wb.Text = "  📍 " .. name
            wb.Font = Enum.Font.Gotham
            wb.TextSize = 12
            wb.TextColor3 = Theme.TextSecondary
            wb.TextXAlignment = Enum.TextXAlignment.Left
            wb.BackgroundColor3 = Theme.Surface
            wb.BackgroundTransparency = 0.5
            wb.Size = UDim2.new(1, 0, 0, 30)
            wb.ZIndex = 16
            wb.Parent = savedList
            MakeCorner(wb, 6)
            local idx = #S.SavedPositions
            wb.MouseButton1Click:Connect(function()
                local root = GetRoot()
                if root then
                    root.CFrame = S.SavedPositions[idx].cf
                    Notify("TP", "Teleportado para " .. name .. "!", 2, "success")
                end
            end)
            Notify("Salvo", "Waypoint '" .. name .. "' salvo!", 2, "success")
            wpInput.Text = ""
        end
    end, 6)

    local coord = Section(p, "🗺️ Teleporte por Coordenadas", 5)
    local _, xBox = TextInput(coord, "X (ex: 100)", nil, 5)
    local _, yBox = TextInput(coord, "Y (ex: 50)", nil, 6)
    local _, zBox = TextInput(coord, "Z (ex: 100)", nil, 7)
    Button(coord, "🚀 Teleportar para Coordenadas", function()
        local x = tonumber(xBox.Text) or 0
        local y = tonumber(yBox.Text) or 50
        local z = tonumber(zBox.Text) or 0
        local r = GetRoot()
        if r then
            r.CFrame = CFrame.new(x, y, z)
            Notify("TP", string.format("(%d, %d, %d)!", x, y, z), 2, "success")
        end
    end, 8)
    Button(coord, "📋 Copiar Posição Atual", function()
        local r = GetRoot()
        if r then
            local pos = r.Position
            local text = string.format("%.1f, %.1f, %.1f", pos.X, pos.Y, pos.Z)
            pcall(function() setclipboard(text) end)
            Notify("Copiado", "Posição: " .. text, 2, "success")
        end
    end, 9)

    local gameTp = Section(p, "🌐 Teleportar para Outros Jogos", 6)
    Label(gameTp, "Digite o Place ID do jogo:", 5)
    local _, gameIdBox = TextInput(gameTp, "Place ID (ex: 2753915549)", nil, 6)
    Button(gameTp, "🚀 Teleportar para o Jogo", function()
        local id = tonumber(gameIdBox.Text)
        if id then
            Notify("Teleport", "Teleportando para " .. id .. "...", 3, "info")
            task.wait(1)
            pcall(function() TeleportService:Teleport(id, Player) end)
        else
            Notify("Erro", "Place ID inválido!", 2, "error")
        end
    end, 7)
end

local function SetupTroll()
    local p = tabPages["Troll"]
    if not p then return end

    PlayerSelector(p, "😈 Selecionar Alvo do Troll", function(plr)
        S.TargetPlayer = plr
        Notify("Alvo", plr.DisplayName .. " selecionado como alvo!", 2, "warning")
    end, 1)

    local moveTroll = Section(p, "🌀 Trolls de Movimento", 2)
    Toggle(moveTroll, "🌪️ Orbitar Alvo", false, function(v)
        S.OrbitTarget = v
        Notify("Orbit", v and "Orbitando o alvo!" or "Parou!", 2, v and "warning" or "info")
    end, 5)
    Slider(moveTroll, "Raio da Órbita", 3, 30, 10, function(v) S.OrbitRadius = v end, 6)
    Slider(moveTroll, "Velocidade da Órbita", 1, 20, 5, function(v) S.OrbitSpeed = v end, 7)
    Toggle(moveTroll, "📎 Grudar no Alvo", false, function(v)
        S.AttachTarget = v
        Notify("Attach", v and "Grudado no alvo!" or "Soltou!", 2, v and "warning" or "info")
    end, 8)
    Toggle(moveTroll, "🔄 Girar o Alvo", false, function(v)
        S.SpinTarget = v
    end, 9)

    local actionTroll = Section(p, "💣 Trolls de Ação", 3)
    Button(actionTroll, "🚀 Fling o Alvo (Lançar)", function()
        if S.TargetPlayer and S.TargetPlayer.Character then
            local r = GetRoot()
            local tr = S.TargetPlayer.Character:FindFirstChild("HumanoidRootPart")
            if r and tr then
                Notify("Fling", "Flingando " .. S.TargetPlayer.DisplayName .. "!", 2, "warning")
                local oldPos = r.CFrame
                local bv = Instance.new("BodyVelocity")
                bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                bv.Velocity = Vector3.new(0, 0, 0)
                bv.Parent = r
                local bg = Instance.new("BodyAngularVelocity")
                bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                bg.AngularVelocity = Vector3.new(0, 100, 0)
                bg.Parent = r
                task.spawn(function()
                    for i = 1, 60 do
                        pcall(function()
                            if tr and tr.Parent then r.CFrame = tr.CFrame end
                        end)
                        task.wait(0.01)
                    end
                    pcall(function() bv:Destroy() end)
                    pcall(function() bg:Destroy() end)
                    task.wait(0.5)
                    pcall(function() r.CFrame = oldPos end)
                    Notify("Fling", "Fling concluído!", 2, "success")
                end)
            end
        else
            Notify("Erro", "Selecione um alvo primeiro!", 2, "error")
        end
    end, 5)

    Button(actionTroll, "📢 Anunciar no Chat sobre o Alvo", function()
        if S.TargetPlayer then
            pcall(function()
                local chatEvents = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
                if chatEvents then
                    local say = chatEvents:FindFirstChild("SayMessageRequest")
                    if say then say:FireServer("🏎️ " .. S.TargetPlayer.DisplayName .. " está sendo trollado pelo Lamborghini Hub!", "All") end
                end
            end)
            Notify("Chat", "Mensagem enviada!", 2, "warning")
        else
            Notify("Erro", "Selecione um alvo!", 2, "error")
        end
    end, 6)

    Button(actionTroll, "🏃 TP para Frente do Alvo Repetidamente", function()
        if S.TargetPlayer and S.TargetPlayer.Character then
            Notify("Annoy", "Incomodando " .. S.TargetPlayer.DisplayName .. "!", 3, "warning")
            task.spawn(function()
                for i = 1, 20 do
                    pcall(function()
                        local r = GetRoot()
                        local tr = S.TargetPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if r and tr then r.CFrame = tr.CFrame * CFrame.new(0, 0, -3) end
                    end)
                    task.wait(0.3)
                end
            end)
        else
            Notify("Erro", "Selecione um alvo!", 2, "error")
        end
    end, 7)

    Button(actionTroll, "🌀 Cercar Alvo com Personagem Girando", function()
        if S.TargetPlayer then
            S.OrbitTarget = true
            S.SpinBot = true
            Notify("Cerco", "Orbitando e girando ao redor do alvo!", 3, "warning")
        end
    end, 8)

    local allTroll = Section(p, "🌐 Trolls em Todos", 4)
    Button(allTroll, "📢 Spam Chat", function()
        pcall(function()
            local chatEvents = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
            if chatEvents then
                local say = chatEvents:FindFirstChild("SayMessageRequest")
                if say then say:FireServer("🏎️ Lamborghini Hub — O mais veloz do Roblox! 🐂", "All") end
            end
        end)
    end, 5)

    Button(allTroll, "🚀 Fling em Todos (Loop)", function()
        Notify("Fling All", "Flingando todos os jogadores...", 3, "error")
        task.spawn(function()
            local r = GetRoot()
            if not r then return end
            local oldPos = r.CFrame
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            bv.Velocity = Vector3.new(0, 0, 0)
            bv.Parent = r
            local bg = Instance.new("BodyAngularVelocity")
            bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
            bg.AngularVelocity = Vector3.new(0, 80, 0)
            bg.Parent = r
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local tr = plr.Character:FindFirstChild("HumanoidRootPart")
                    if tr then
                        for i = 1, 30 do
                            pcall(function() r.CFrame = tr.CFrame end)
                            task.wait(0.01)
                        end
                    end
                end
            end
            pcall(function() bv:Destroy() end)
            pcall(function() bg:Destroy() end)
            task.wait(0.5)
            pcall(function() r.CFrame = oldPos end)
            Notify("Fling All", "Concluído!", 2, "success")
        end)
    end, 6)

    Button(allTroll, "🏃 TP para Todos Rapidamente", function()
        task.spawn(function()
            local r = GetRoot()
            if r then
                for _, plr in pairs(Players:GetPlayers()) do
                    if plr ~= Player and plr.Character then
                        local tr = plr.Character:FindFirstChild("HumanoidRootPart")
                        if tr then
                            r.CFrame = tr.CFrame * CFrame.new(0, 0, 2)
                            task.wait(0.3)
                        end
                    end
                end
            end
        end)
        Notify("TP All", "Visitando todos!", 2, "info")
    end, 7)

    local visualTroll = Section(p, "🎨 Trolls Visuais", 5)
    Button(visualTroll, "🌈 Fazer Chão Rainbow", function()
        task.spawn(function()
            local baseplate = Workspace:FindFirstChild("Baseplate") or Workspace:FindFirstChild("Base")
            if baseplate then
                for i = 1, 50 do
                    baseplate.BrickColor = BrickColor.Random()
                    baseplate.Material = Enum.Material.Neon
                    task.wait(0.1)
                end
                baseplate.Material = Enum.Material.Plastic
                baseplate.BrickColor = BrickColor.new("Medium stone grey")
            else
                Notify("Erro", "Baseplate não encontrado!", 2, "error")
            end
        end)
    end, 5)

    Button(visualTroll, "🔦 Flash Bang (Tela branca)", function()
        local flash = Instance.new("Frame")
        flash.BackgroundColor3 = Color3.new(1, 1, 1)
        flash.Size = UDim2.new(1, 0, 1, 0)
        flash.ZIndex = 999
        flash.Parent = gui
        task.wait(0.3)
        Tween(flash, {BackgroundTransparency = 1}, 2)
        task.delay(2.1, function() flash:Destroy() end)
    end, 6)

    Button(visualTroll, "💀 Tela de Morte Falsa", function()
        local fakeD = Instance.new("Frame")
        fakeD.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
        fakeD.BackgroundTransparency = 0.3
        fakeD.Size = UDim2.new(1, 0, 1, 0)
        fakeD.ZIndex = 998
        fakeD.Parent = gui
        local deathTxt = Instance.new("TextLabel")
        deathTxt.Text = "☠️ YOU DIED ☠️"
        deathTxt.Font = Enum.Font.GothamBold
        deathTxt.TextSize = 60
        deathTxt.TextColor3 = Color3.fromRGB(255, 0, 0)
        deathTxt.BackgroundTransparency = 1
        deathTxt.Size = UDim2.new(1, 0, 1, 0)
        deathTxt.ZIndex = 999
        deathTxt.Parent = fakeD
        task.wait(2)
        Tween(fakeD, {BackgroundTransparency = 1}, 1)
        Tween(deathTxt, {TextTransparency = 1}, 1)
        task.delay(1.1, function() fakeD:Destroy() end)
    end, 7)
end

local function SetupMisc()
    local p = tabPages["Misc"]
    if not p then return end

    local utils = Section(p, "🔧 Utilidades Gerais", 1)

    Toggle(utils, "💤 Anti-AFK", false, function(v)
        S.AntiAFK = v
        Notify("Anti-AFK", v and "Nunca mais AFK!" or "Off!", 2, v and "success" or "info")
    end, 5)

    Toggle(utils, "🛡️ Anti-Kick (Tentativa)", false, function(v)
        S.AntiKick = v
        if v then
            pcall(function()
                local mt = getrawmetatable(game)
                if mt then
                    local old = mt.__namecall
                    setreadonly(mt, false)
                    mt.__namecall = newcclosure(function(self, ...)
                        if getnamecallmethod() == "Kick" then return end
                        return old(self, ...)
                    end)
                    setreadonly(mt, true)
                end
            end)
        end
        Notify("Anti-Kick", v and "Proteção ativada!" or "Off!", 2, v and "success" or "info")
    end, 6)

    Toggle(utils, "🖱️ Click Delete (Deletar Objetos)", false, function(v)
        S.ClickDelete = v
        Notify("Click Delete", v and "Clique em objetos para deletar!" or "Off!", 2, v and "warning" or "info")
    end, 7)

    Toggle(utils, "💬 Chat Spam", false, function(v)
        S.ChatSpam = v
    end, 8)

    local _, chatInp = TextInput(utils, "Mensagem do spam...", function(t) S.ChatMsg = t end, 9)

    local tools = Section(p, "🧰 Ferramentas & Itens", 2)
    Button(tools, "🎒 Pegar TODOS os Tools do Mapa", function()
        local c = 0
        for _, t in pairs(Workspace:GetDescendants()) do
            if t:IsA("Tool") then
                pcall(function() t.Parent = Player.Backpack; c = c + 1 end)
            end
        end
        Notify("Tools", c .. " ferramentas coletadas!", 2, "success")
    end, 5)
    Button(tools, "🗑️ Remover Todos os Tools", function()
        for _, t in pairs(Player.Backpack:GetChildren()) do
            if t:IsA("Tool") then t:Destroy() end
        end
        pcall(function()
            for _, t in pairs(GetChar():GetChildren()) do
                if t:IsA("Tool") then t:Destroy() end
            end
        end)
        Notify("Tools", "Todas removidas!", 2, "info")
    end, 6)
    Button(tools, "🔓 Desbloquear Todas as Portas", function()
        local c = 0
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                local name = obj.Name:lower()
                if name:find("door") or name:find("gate") or name:find("porta") then
                    pcall(function() obj.CanCollide = false; obj.Transparency = 0.5; c = c + 1 end)
                end
            end
        end
        Notify("Unlock", c .. " portas desbloqueadas!", 2, "success")
    end, 7)
    Button(tools, "🧱 Remover Todas as Barreiras", function()
        local c = 0
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                local name = obj.Name:lower()
                if name:find("barrier") or name:find("wall") or name:find("invisible") or name:find("barreira") then
                    pcall(function() obj:Destroy(); c = c + 1 end)
                end
            end
        end
        Notify("Barreiras", c .. " barreiras removidas!", 2, "success")
    end, 8)

    local charMisc = Section(p, "🧍 Personagem", 3)
    Button(charMisc, "🎭 Copiar Aparência Aleatória", function()
        local plrs = Players:GetPlayers()
        local target = plrs[math.random(1, #plrs)]
        if target and target ~= Player then
            pcall(function()
                local desc = Players:GetHumanoidDescriptionFromUserId(target.UserId)
                GetHum():ApplyDescription(desc)
            end)
            Notify("Aparência", "Copiou " .. target.DisplayName .. "!", 2, "success")
        end
    end, 5)
    Button(charMisc, "💀 Esqueleto Mode", function()
        pcall(function()
            for _, p in pairs(GetChar():GetDescendants()) do
                if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then
                    p.Transparency = p.Name == "Head" and 0 or 0.7
                    p.Material = Enum.Material.ForceField
                end
            end
        end)
        Notify("Esqueleto", "Modo esqueleto ativado!", 2, "info")
    end, 6)
    Button(charMisc, "🌟 Neon Mode", function()
        pcall(function()
            for _, part in pairs(GetChar():GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Material = Enum.Material.Neon
                    part.BrickColor = BrickColor.Random()
                end
            end
        end)
        Notify("Neon", "Personagem neon!", 2, "success")
    end, 7)
    Button(charMisc, "🏎️ Lambo Mode (Amarelo Neon)", function()
        pcall(function()
            for _, part in pairs(GetChar():GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Material = Enum.Material.Neon
                    part.Color = Color3.fromRGB(255, 200, 0)
                end
            end
        end)
        Notify("Lambo!", "Você virou uma Lamborghini! 🏎️", 2, "success")
    end, 8)
    Button(charMisc, "👤 Normal Mode", function()
        pcall(function()
            for _, part in pairs(GetChar():GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Material = Enum.Material.Plastic
                    part.Transparency = part.Name == "HumanoidRootPart" and 1 or 0
                end
            end
        end)
        Notify("Normal", "Aparência restaurada!", 2, "info")
    end, 9)
end

local function SetupServer()
    local p = tabPages["Server"]
    if not p then return end

    local serverActs = Section(p, "🖥️ Ações do Servidor", 1)
    Button(serverActs, "🔄 Rejoin (Reconectar)", function()
        Notify("Rejoin", "Reconectando...", 2, "info")
        task.wait(1)
        pcall(function() TeleportService:Teleport(game.PlaceId, Player) end)
    end, 5)
    Button(serverActs, "🌐 Server Hop (Trocar Servidor)", function()
        Notify("Server Hop", "Procurando servidor...", 2, "info")
        task.spawn(function()
            pcall(function()
                local url = "https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
                local data = HttpService:JSONDecode(game:HttpGet(url))
                if data and data.data then
                    for _, sv in pairs(data.data) do
                        if sv.playing < sv.maxPlayers and sv.id ~= game.JobId then
                            TeleportService:TeleportToPlaceInstance(game.PlaceId, sv.id, Player)
                            return
                        end
                    end
                    Notify("Server Hop", "Nenhum servidor encontrado!", 2, "error")
                end
            end)
        end)
    end, 6)
    Button(serverActs, "📋 Copiar Server ID (Job ID)", function()
        pcall(function() setclipboard(game.JobId) end)
        Notify("Copiado", "Job ID copiado!", 2, "success")
    end, 7)
    Button(serverActs, "📋 Copiar Link de Convite", function()
        pcall(function() setclipboard("https://www.roblox.com/games/" .. game.PlaceId) end)
        Notify("Copiado", "Link copiado!", 2, "success")
    end, 8)

    local sInfo = Section(p, "📊 Info do Servidor", 2)
    Label(sInfo, "🆔 Place ID: " .. game.PlaceId, 5)
    Label(sInfo, "🔗 Job ID: " .. game.JobId, 6)
    Label(sInfo, "👥 Players: " .. #Players:GetPlayers() .. "/" .. Players.MaxPlayers, 7)

    local consol = Section(p, "🖥️ Console & Debug", 3)
    Button(consol, "📊 Imprimir Info no Console (F9)", function()
        print("══════════════════════════════════════")
        print("🏎️ LAMBORGHINI HUB — INFORMAÇÕES")
        print("👤 Player: " .. Player.Name .. " (" .. Player.DisplayName .. ")")
        print("🆔 UserID: " .. Player.UserId)
        print("🎮 Game: " .. game.PlaceId)
        print("🔗 JobID: " .. game.JobId)
        print("👥 Players: " .. #Players:GetPlayers())
        print("📈 Ping: " .. math.floor(Player:GetNetworkPing() * 1000) .. "ms")
        print("══════════════════════════════════════")
        Notify("Console", "Info impressa! Abra F9.", 2, "info")
    end, 5)
    Button(consol, "📝 Listar Todos os Serviços", function()
        print("══ SERVIÇOS ══")
        for _, s in pairs(game:GetChildren()) do
            print("  • " .. s.ClassName .. " (" .. s.Name .. ")")
        end
        Notify("Console", "Serviços listados no F9!", 2, "info")
    end, 6)
    Button(consol, "📦 Listar RemoteEvents", function()
        print("══ REMOTE EVENTS ══")
        local c = 0
        for _, obj in pairs(ReplicatedStorage:GetDescendants()) do
            if obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction") then
                print("  • " .. obj:GetFullName())
                c = c + 1
            end
        end
        print("Total: " .. c)
        Notify("Console", c .. " remotes encontrados! F9", 2, "info")
    end, 7)
end

local function SetupSettings()
    local p = tabPages["Settings"]
    if not p then return end

    local uiSec = Section(p, "🎨 Interface & Tema", 1)
    Label(uiSec, "🔑 Tecla de Toggle: Right Control", 5)

    Dropdown(uiSec, "🎨 Cor do Tema", {"Amarelo Lambo (Padrão)", "Roxo", "Azul", "Verde", "Vermelho", "Rosa", "Cyan"}, function(opt)
        local colors = {
            ["Amarelo Lambo (Padrão)"] = Color3.fromRGB(255, 200, 0),
            ["Roxo"] = Color3.fromRGB(100, 70, 255),
            ["Azul"] = Color3.fromRGB(70, 130, 255),
            ["Verde"] = Color3.fromRGB(72, 222, 120),
            ["Vermelho"] = Color3.fromRGB(255, 75, 75),
            ["Rosa"] = Color3.fromRGB(255, 100, 180),
            ["Cyan"] = Color3.fromRGB(0, 220, 210),
        }
        Theme.Primary = colors[opt] or Theme.Primary
        Notify("Tema", "Cor alterada para " .. opt .. "!", 2, "success")
    end, 6)

    local credits = Section(p, "👥 Créditos & Info", 2)
    Label(credits, "🏎️ " .. Config.HubName, 5)
    Label(credits, "🔨 Script Hub Premium — Tema Lamborghini", 6)
    Label(credits, '🐂 "Expect the Unexpected"', 7)
    Label(credits, "📅 2025 — Todos os direitos reservados", 8)
    Label(credits, "❤️ Desenvolvido por @gabriel.futury", 9)

    local shortcuts = Section(p, "⌨️ Atalhos de Teclado", 3)
    Label(shortcuts, "Right Ctrl → Mostrar/Esconder Menu", 5)
    Label(shortcuts, "Click Esquerdo → Click TP (quando ativo)", 6)
    Label(shortcuts, "WASD + Space/Shift → Controles do Fly", 7)

    local danger = Section(p, "⚠️ Zona de Perigo", 4)
    Button(danger, "🔄 Resetar Todas as Configurações", function()
        S.Speed = false; S.Fly = false; S.Noclip = false
        S.GodMode = false; S.ESP = false; S.Chams = false
        S.KillAura = false; S.HitboxExpand = false
        S.AntiAFK = false; S.ChatSpam = false
        pcall(function()
            GetHum().WalkSpeed = 16
            GetHum().JumpPower = 50
            Workspace.Gravity = 196.2
            Camera.FieldOfView = 70
        end)
        Notify("Reset", "Tudo resetado!", 2, "warning")
    end, 5, Theme.Warning)

    Button(danger, "🗑️ Destruir Hub Completamente", function()
        Notify("Destruindo", "Adeus! 🏎️", 2, "error")
        task.wait(2)
        gui:Destroy()
    end, 6, Theme.Error)
end

-- ══════════════════════════════════════════
-- LOOPS DE FUNCIONALIDADE (idênticos ao original)
-- ══════════════════════════════════════════
local function StartLoops()
    RunService.Heartbeat:Connect(function()
        if S.Speed then pcall(function() GetHum().WalkSpeed = S.SpeedVal end) end
    end)
    RunService.Heartbeat:Connect(function()
        if S.JumpPower then pcall(function()
            local h = GetHum(); h.JumpPower = S.JumpVal; h.UseJumpPower = true
        end) end
    end)
    UserInputService.JumpRequest:Connect(function()
        if S.InfiniteJump then pcall(function() GetHum():ChangeState(Enum.HumanoidStateType.Jumping) end) end
    end)
    RunService.Heartbeat:Connect(function()
        if S.GodMode then pcall(function() GetHum().Health = GetHum().MaxHealth end) end
    end)
    RunService.Stepped:Connect(function()
        if S.Noclip then pcall(function()
            for _, part in pairs(GetChar():GetDescendants()) do
                if part:IsA("BasePart") then part.CanCollide = false end
            end
        end) end
    end)
    RunService.Heartbeat:Connect(function()
        if S.SpinBot then pcall(function()
            local r = GetRoot()
            if r then r.CFrame = r.CFrame * CFrame.Angles(0, math.rad(S.SpinSpeed), 0) end
        end) end
    end)
    RunService.Heartbeat:Connect(function()
        if S.AntiVoid then pcall(function()
            local r = GetRoot()
            if r and r.Position.Y < -50 then r.CFrame = CFrame.new(r.CFrame.X, 100, r.CFrame.Z) end
        end) end
    end)
    RunService.Heartbeat:Connect(function()
        if S.FreezeSelf and S.FreezePos then pcall(function() GetRoot().CFrame = S.FreezePos end) end
    end)

    local flyBV, flyBG, flyOn = nil, nil, false
    RunService.Heartbeat:Connect(function()
        if S.Fly then
            local r = GetRoot(); local h = GetHum()
            if r and h then
                if not flyOn then
                    flyOn = true
                    flyBV = Instance.new("BodyVelocity")
                    flyBV.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                    flyBV.Velocity = Vector3.zero; flyBV.Parent = r
                    flyBG = Instance.new("BodyGyro")
                    flyBG.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
                    flyBG.P = 9e4; flyBG.Parent = r
                    h.PlatformStand = true
                end
                local cam = Camera; local sp = S.FlySpeed; local dir = Vector3.zero
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then dir = dir + cam.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then dir = dir - cam.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then dir = dir - cam.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then dir = dir + cam.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.new(0, 1, 0) end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then dir = dir - Vector3.new(0, 1, 0) end
                flyBV.Velocity = dir * sp; flyBG.CFrame = cam.CFrame
            end
        else
            if flyOn then
                flyOn = false
                pcall(function() flyBV:Destroy() end)
                pcall(function() flyBG:Destroy() end)
                pcall(function() GetHum().PlatformStand = false end)
            end
        end
    end)

    RunService.Heartbeat:Connect(function()
        if S.ESP or S.Chams then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    if S.Chams then
                        if not plr.Character:FindFirstChild("NovaHL") then
                            local hl = Instance.new("Highlight")
                            hl.Name = "NovaHL"
                            hl.FillColor = Color3.fromRGB(255, 200, 0)
                            hl.FillTransparency = 0.65
                            hl.OutlineColor = Color3.fromRGB(255, 200, 0)
                            hl.OutlineTransparency = 0
                            hl.Adornee = plr.Character
                            hl.Parent = plr.Character
                        end
                    else
                        local h = plr.Character:FindFirstChild("NovaHL")
                        if h then h:Destroy() end
                    end
                    if S.ESP then
                        local head = plr.Character:FindFirstChild("Head")
                        if head then
                            if not head:FindFirstChild("NovaESP") then
                                local bb = Instance.new("BillboardGui")
                                bb.Name = "NovaESP"
                                bb.Size = UDim2.new(0, 200, 0, 55)
                                bb.StudsOffset = Vector3.new(0, 3.5, 0)
                                bb.AlwaysOnTop = true; bb.Parent = head
                                local nm = Instance.new("TextLabel")
                                nm.Name = "Name"; nm.BackgroundTransparency = 1
                                nm.Size = UDim2.new(1, 0, 0.4, 0)
                                nm.TextColor3 = Color3.fromRGB(255, 200, 0)
                                nm.TextStrokeTransparency = 0.4
                                nm.TextStrokeColor3 = Color3.new(0, 0, 0)
                                nm.Font = Enum.Font.GothamBold; nm.TextSize = 14
                                nm.Text = plr.DisplayName; nm.Parent = bb
                                local dt = Instance.new("TextLabel")
                                dt.Name = "Dist"; dt.BackgroundTransparency = 1
                                dt.Size = UDim2.new(1, 0, 0.25, 0)
                                dt.Position = UDim2.new(0, 0, 0.4, 0)
                                dt.TextColor3 = Theme.TextSecondary
                                dt.Font = Enum.Font.Gotham; dt.TextSize = 11; dt.Text = "0m"; dt.Parent = bb
                                local hbg = Instance.new("Frame")
                                hbg.Name = "HBar"; hbg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                                hbg.Size = UDim2.new(0.5, 0, 0, 5)
                                hbg.Position = UDim2.new(0.25, 0, 0.75, 0); hbg.Parent = bb
                                MakeCorner(hbg, 3)
                                local hfill = Instance.new("Frame")
                                hfill.Name = "Fill"; hfill.BackgroundColor3 = Theme.Success
                                hfill.Size = UDim2.new(1, 0, 1, 0); hfill.Parent = hbg
                                MakeCorner(hfill, 3)
                            end
                            local bb = head:FindFirstChild("NovaESP")
                            if bb then
                                local r = GetRoot()
                                if r then
                                    local dist = math.floor((r.Position - head.Position).Magnitude)
                                    local dl = bb:FindFirstChild("Dist")
                                    if dl then dl.Text = dist .. "m" end
                                end
                                local hum = plr.Character:FindFirstChildOfClass("Humanoid")
                                if hum then
                                    local hbar = bb:FindFirstChild("HBar")
                                    if hbar then
                                        local fill = hbar:FindFirstChild("Fill")
                                        if fill then
                                            local pct = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
                                            fill.Size = UDim2.new(pct, 0, 1, 0)
                                            fill.BackgroundColor3 = pct > 0.5 and Theme.Success or pct > 0.25 and Theme.Warning or Theme.Error
                                        end
                                    end
                                end
                            end
                        end
                    else
                        for _, d in pairs(plr.Character:GetDescendants()) do
                            if d.Name == "NovaESP" then d:Destroy() end
                        end
                    end
                end
            end
        else
            for _, plr in pairs(Players:GetPlayers()) do
                if plr.Character then
                    local h = plr.Character:FindFirstChild("NovaHL")
                    if h then h:Destroy() end
                    for _, d in pairs(plr.Character:GetDescendants()) do
                        if d.Name == "NovaESP" then d:Destroy() end
                    end
                end
            end
        end
    end)

    -- ★ HITBOX (só inimigos) ★
    RunService.Heartbeat:Connect(function()
        if S.HitboxExpand then
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local isTeammate = false
                    if Player.Team and plr.Team and Player.Team == plr.Team then
                        isTeammate = true
                    end
                    local r = plr.Character:FindFirstChild("HumanoidRootPart")
                    if r then
                        if not isTeammate then
                            r.Size = Vector3.new(S.HitboxSize, S.HitboxSize, S.HitboxSize)
                            r.Transparency = 0.7
                            r.BrickColor = BrickColor.new("Bright yellow")
                            r.Material = Enum.Material.ForceField
                            r.CanCollide = false
                        else
                            r.Size = Vector3.new(2, 2, 1)
                            r.Transparency = 1
                            r.Material = Enum.Material.Plastic
                        end
                    end
                end
            end
        end
    end)

    task.spawn(function()
        while true do
            if S.AutoAttack or S.SpamClick then
                pcall(function()
                    local tool = GetChar():FindFirstChildOfClass("Tool")
                    if tool then tool:Activate() end
                    Mouse:Click()
                end)
            end
            task.wait(S.SpamClick and 0.05 or 0.2)
        end
    end)

    local orbitAngle = 0
    RunService.Heartbeat:Connect(function(dt)
        if S.OrbitTarget and S.TargetPlayer then
            pcall(function()
                local r = GetRoot(); local tc = S.TargetPlayer.Character
                if r and tc then
                    local tr = tc:FindFirstChild("HumanoidRootPart")
                    if tr then
                        orbitAngle = orbitAngle + dt * S.OrbitSpeed
                        local x = math.cos(orbitAngle) * S.OrbitRadius
                        local z = math.sin(orbitAngle) * S.OrbitRadius
                        r.CFrame = CFrame.new(tr.Position.X + x, tr.Position.Y, tr.Position.Z + z) * CFrame.Angles(0, -orbitAngle + math.pi/2, 0)
                    end
                end
            end)
        end
    end)

    RunService.Heartbeat:Connect(function()
        if S.AttachTarget and S.TargetPlayer then
            pcall(function()
                local r = GetRoot(); local tc = S.TargetPlayer.Character
                if r and tc then
                    local tr = tc:FindFirstChild("HumanoidRootPart")
                    if tr then r.CFrame = tr.CFrame * CFrame.new(0, 0, -2) end
                end
            end)
        end
    end)

    Mouse.Button1Click:Connect(function()
        if S.ClickTP then
            pcall(function()
                local r = GetRoot()
                if r and Mouse.Hit then r.CFrame = Mouse.Hit + Vector3.new(0, 3, 0) end
            end)
        end
        if S.ClickDelete then
            pcall(function()
                local target = Mouse.Target
                if target and not Players:GetPlayerFromCharacter(target.Parent) then
                    target:Destroy()
                    Notify("Deletado", target.Name .. " removido!", 1, "info")
                end
            end)
        end
    end)

    task.spawn(function()
        while true do
            if S.AntiAFK then
                pcall(function()
                    local vu = game:GetService("VirtualUser")
                    vu:CaptureController(); vu:ClickButton2(Vector2.new())
                end)
            end
            task.wait(60)
        end
    end)

    task.spawn(function()
        while true do
            if S.ChatSpam then
                pcall(function()
                    local ce = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
                    if ce then
                        local say = ce:FindFirstChild("SayMessageRequest")
                        if say then say:FireServer(S.ChatMsg, "All") end
                    end
                end)
            end
            task.wait(2.5)
        end
    end)

    task.spawn(function()
        local hue = 0
        while true do
            if S.Rainbow then
                hue = (hue + 0.005) % 1
                pcall(function()
                    Lighting.Ambient = Color3.fromHSV(hue, 0.5, 0.5)
                    Lighting.OutdoorAmbient = Color3.fromHSV(hue, 0.3, 0.7)
                    Lighting.ColorShift_Top = Color3.fromHSV(hue, 0.4, 1)
                end)
            end
            task.wait(0.03)
        end
    end)

    RunService.RenderStepped:Connect(function()
        if S.AimBot then
            pcall(function()
                local closest = nil; local closestDist = math.huge
                local r = GetRoot(); if not r then return end
                for _, plr in pairs(Players:GetPlayers()) do
                    if plr ~= Player and plr.Character then
                        local head = plr.Character:FindFirstChild("Head")
                        if head then
                            local screenPos, onScreen = Camera:WorldToScreenPoint(head.Position)
                            if onScreen then
                                local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                                if dist < closestDist and dist < 300 then closestDist = dist; closest = head end
                            end
                        end
                    end
                end
                if closest then
                    Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, closest.Position), S.AimSmooth or 0.5)
                end
            end)
        end
    end)

    RunService.Heartbeat:Connect(function()
        if S.ItemESP then
            for _, obj in pairs(Workspace:GetDescendants()) do
                if obj:IsA("Tool") and obj.Parent == Workspace then
                    local handle = obj:FindFirstChild("Handle")
                    if handle and not handle:FindFirstChild("ItemESP") then
                        local bb = Instance.new("BillboardGui")
                        bb.Name = "ItemESP"; bb.Size = UDim2.new(0, 150, 0, 30)
                        bb.StudsOffset = Vector3.new(0, 2, 0)
                        bb.AlwaysOnTop = true; bb.Parent = handle
                        local tl = Instance.new("TextLabel")
                        tl.Text = "🏎️ " .. obj.Name
                        tl.BackgroundTransparency = 1; tl.Size = UDim2.new(1, 0, 1, 0)
                        tl.TextColor3 = Color3.fromRGB(255, 200, 0)
                        tl.Font = Enum.Font.GothamBold; tl.TextSize = 13
                        tl.TextStrokeTransparency = 0.5; tl.Parent = bb
                    end
                end
            end
        else
            for _, obj in pairs(Workspace:GetDescendants()) do
                if obj.Name == "ItemESP" then obj:Destroy() end
            end
        end
    end)

    Player.CharacterAdded:Connect(function(char)
        task.wait(1)
        if S.Speed then pcall(function() char:WaitForChild("Humanoid").WalkSpeed = S.SpeedVal end) end
        if S.JumpPower then pcall(function()
            local h = char:WaitForChild("Humanoid")
            h.JumpPower = S.JumpVal; h.UseJumpPower = true
        end) end
        if S.GodMode then pcall(function()
            local h = char:WaitForChild("Humanoid")
            h.Health = h.MaxHealth
        end) end
    end)
end

-- ══════════════════════════════════════════
-- WATERMARK LAMBORGHINI
-- ══════════════════════════════════════════
local function CreateWatermark()
    local wm = Instance.new("Frame")
    wm.BackgroundColor3 = Color3.fromRGB(10, 8, 0)
    wm.BackgroundTransparency = 0.1
    wm.Size = UDim2.new(0, 310, 0, 28)
    wm.Position = UDim2.new(0, 10, 0, 10)
    wm.ZIndex = 100
    wm.Parent = gui
    MakeCorner(wm, 6)
    MakeStroke(wm, Color3.fromRGB(255, 200, 0), 1.5, 0.2)

    -- Detalhe lateral Lambo
    local accent = Instance.new("Frame")
    accent.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
    accent.Size = UDim2.new(0, 3, 0.65, 0)
    accent.Position = UDim2.new(0, 3, 0.175, 0)
    accent.ZIndex = 101
    accent.Parent = wm
    MakeCorner(accent, 2)
    local acGrad = Instance.new("UIGradient")
    acGrad.Color = ColorSequence.new(Color3.fromRGB(255, 230, 60), Color3.fromRGB(200, 145, 0))
    acGrad.Rotation = 90
    acGrad.Parent = accent

    local wmTxt = Instance.new("TextLabel")
    wmTxt.Text = "🏎️ Lamborghini Hub │ FPS: -- │ Ping: --ms"
    wmTxt.Font = Enum.Font.GothamBold
    wmTxt.TextSize = 11
    wmTxt.TextColor3 = Color3.fromRGB(255, 200, 0)
    wmTxt.BackgroundTransparency = 1
    wmTxt.Size = UDim2.new(1, -12, 1, 0)
    wmTxt.Position = UDim2.new(0, 10, 0, 0)
    wmTxt.TextXAlignment = Enum.TextXAlignment.Left
    wmTxt.ZIndex = 101
    wmTxt.Parent = wm

    task.spawn(function()
        while task.wait(0.5) do
            pcall(function()
                local fps = math.floor(1 / RunService.RenderStepped:Wait())
                local ping = math.floor(Player:GetNetworkPing() * 1000)
                wmTxt.Text = string.format("🏎️ Lamborghini Hub │ FPS: %d │ Ping: %dms", fps, ping)
            end)
        end
    end)
end

-- ══════════════════════════════════════════
-- TOGGLE KEY
-- ══════════════════════════════════════════
UserInputService.InputBegan:Connect(function(inp, gp)
    if gp then return end
    if inp.KeyCode == Config.ToggleKey then
        if S.HubVisible then
            S.HubVisible = false
            Tween(mainFrame, {
                Size = UDim2.new(0, 720, 0, 0),
                Position = UDim2.new(0.5, -360, 0.5, 0)
            }, 0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In)
            task.wait(0.35)
            mainFrame.Visible = false
        else
            S.HubVisible = true
            mainFrame.Visible = true
            mainFrame.Size = UDim2.new(0, 0, 0, 0)
            mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
            Tween(mainFrame, {
                Size = UDim2.new(0, 720, 0, 490),
                Position = UDim2.new(0.5, -360, 0.5, -245)
            }, 0.4, Enum.EasingStyle.Back)
        end
    end
end)

-- ══════════════════════════════════════════
-- INICIALIZAÇÃO
-- ══════════════════════════════════════════
task.spawn(function()
    PlayIntro()
    task.wait(0.2)
    BuildUI()
    task.wait(0.4)
    SetupHome()
    SetupPlayer()
    SetupCombat()
    SetupVisuals()
    SetupTeleport()
    SetupTroll()
    SetupMisc()
    SetupServer()
    SetupSettings()
    CreateWatermark()
    StartLoops()
    task.wait(0.5)
    Notify("🏎️ Lamborghini Hub", "Carregado! Pressione RightCtrl para toggle.", 4, "success")
    Notify("🐂 Bem-vindo", "Olá, " .. Player.DisplayName .. "! Expect the Unexpected!", 3, "info")
end)

print("══════════════════════════════════════")
print("  🏎️ LAMBORGHINI HUB — Carregado!")
print("  👤 " .. Player.DisplayName .. " (@" .. Player.Name .. ")")
print("  🎮 Place: " .. game.PlaceId)
print("  🔑 Toggle: Right Control")
print("  🐂 Expect the Unexpected!")
print("══════════════════════════════════════") 
