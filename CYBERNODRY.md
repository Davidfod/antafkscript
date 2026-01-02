local Cores = {
    Fundo = Color3.fromRGB(15, 15, 15),
    Topo = Color3.fromRGB(25, 25, 25),
    Destaque = Color3.fromRGB(0, 170, 255),
    Texto = Color3.fromRGB(240, 240, 240),
    BotaoOff = Color3.fromRGB(40, 40, 40),
    BotaoOn = Color3.fromRGB(0, 170, 255)
}

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local VirtualUser = game:GetService("VirtualUser")
local TweenService = game:GetService("TweenService")

local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "EliteScript_V3"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "Main"
MainFrame.Size = UDim2.new(0, 240, 0, 195)
MainFrame.Position = UDim2.new(0.5, -120, 0.5, -97)
MainFrame.BackgroundColor3 = Cores.Fundo
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 12)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Thickness = 1.5
MainStroke.Color = Color3.fromRGB(45, 45, 45)
MainStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
MainStroke.Parent = MainFrame

local Header = Instance.new("Frame")
Header.Name = "Barra"
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Cores.Topo
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 12)
HeaderCorner.Parent = Header

local Line = Instance.new("Frame")
Line.Size = UDim2.new(1, 0, 0, 2)
Line.Position = UDim2.new(0, 0, 1, -2)
Line.BackgroundColor3 = Cores.Destaque
Line.BorderSizePixel = 0
Line.Parent = Header

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "ANT AFK"
Title.TextColor3 = Cores.Texto
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -34, 0, 6)
CloseBtn.BackgroundColor3 = Cores.Fundo
CloseBtn.Text = "×"
CloseBtn.TextColor3 = Cores.Texto
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20
CloseBtn.Parent = Header

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 28, 0, 28)
MinBtn.Position = UDim2.new(1, -68, 0, 6)
MinBtn.BackgroundColor3 = Cores.Fundo
MinBtn.Text = "−"
MinBtn.TextColor3 = Cores.Texto
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 18
MinBtn.Parent = Header

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, 0, 1, -40)
Content.Position = UDim2.new(0, 0, 0, 40)
Content.BackgroundTransparency = 1
Content.Parent = MainFrame

local StatusDot = Instance.new("Frame")
StatusDot.Size = UDim2.new(0, 8, 0, 8)
StatusDot.Position = UDim2.new(0, 20, 0, 25)
StatusDot.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
StatusDot.Parent = Content

local DotCorner = Instance.new("UICorner")
DotCorner.CornerRadius = UDim.new(1, 0)
DotCorner.Parent = StatusDot

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -40, 0, 20)
StatusLabel.Position = UDim2.new(0, 35, 0, 19)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "Status: Off"
StatusLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 12
StatusLabel.TextXAlignment = Enum.TextXAlignment.Left
StatusLabel.Parent = Content

local ActionBtn = Instance.new("TextButton")
ActionBtn.Name = "Toggle"
ActionBtn.Size = UDim2.new(0.85, 0, 0, 45)
ActionBtn.Position = UDim2.new(0.075, 0, 0.45, 0)
ActionBtn.BackgroundColor3 = Cores.BotaoOff
ActionBtn.Text = "LIGAR ANT AFK"
ActionBtn.TextColor3 = Cores.Texto
ActionBtn.Font = Enum.Font.GothamBold
ActionBtn.TextSize = 13
ActionBtn.AutoButtonColor = false
ActionBtn.Parent = Content

local BtnCorner = Instance.new("UICorner")
BtnCorner.CornerRadius = UDim.new(0, 8)
BtnCorner.Parent = ActionBtn

local Credits = Instance.new("TextLabel")
Credits.Size = UDim2.new(1, 0, 0, 20)
Credits.Position = UDim2.new(0, 0, 1, -22)
Credits.BackgroundTransparency = 1
Credits.Text = "Direitos Reservados Cybernodry"
Credits.TextColor3 = Color3.fromRGB(80, 80, 80)
Credits.Font = Enum.Font.Gotham
Credits.TextSize = 10
Credits.TextXAlignment = Enum.TextXAlignment.Center
Credits.Parent = Content

local Active = false
local Minimized = false

local dragToggle, dragStart, startPos
Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragToggle = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragToggle and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragToggle = false
    end
end)

local function cycleTools()
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum then return end

    local backpackTool = LocalPlayer.Backpack:FindFirstChildOfClass("Tool")
    local equippedTool = char:FindFirstChildOfClass("Tool")

    if backpackTool then
        hum:EquipTool(backpackTool)
        StatusLabel.Text = "Status: Equipando " .. backpackTool.Name
        task.wait(1)
        hum:UnequipTools()
        StatusLabel.Text = "Status: Ativo"
    elseif equippedTool then
        hum:UnequipTools()
        task.wait(0.5)
        hum:EquipTool(equippedTool)
        StatusLabel.Text = "Status: Ciclando " .. equippedTool.Name
    else
        StatusLabel.Text = "Status: Sem tools"
    end
end

task.spawn(function()
    while true do
        if Active then
            cycleTools()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new(0,0))
        end
        task.wait(3)
    end
end)

ActionBtn.MouseButton1Click:Connect(function()
    Active = not Active
    if Active then
        TweenService:Create(ActionBtn, TweenInfo.new(0.3), {BackgroundColor3 = Cores.BotaoOn}):Play()
        TweenService:Create(StatusDot, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(50, 255, 100)}):Play()
        ActionBtn.Text = "ANT AFK: ON"
    else
        TweenService:Create(ActionBtn, TweenInfo.new(0.3), {BackgroundColor3 = Cores.BotaoOff}):Play()
        TweenService:Create(StatusDot, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(200, 50, 50)}):Play()
        ActionBtn.Text = "LIGAR ANT AFK"
        StatusLabel.Text = "Status: Off"
    end
end)

MinBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    if Minimized then
        Content.Visible = false
        MainFrame:TweenSize(UDim2.new(0, 240, 0, 40), "Out", "Quad", 0.3, true)
        MinBtn.Text = "+"
    else
        MainFrame:TweenSize(UDim2.new(0, 240, 0, 195), "Out", "Quad", 0.3, true)
        task.wait(0.3)
        Content.Visible = true
        MinBtn.Text = "−"
    end
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

LocalPlayer.Idled:Connect(function()
    if Active then
        VirtualUser:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
        task.wait(1)
        VirtualUser:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
    end
end)
