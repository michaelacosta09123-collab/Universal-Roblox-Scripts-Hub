-- ELITE UNIVERSAL V14: RE-ENGINEERED INITIALIZATION SYSTEM + SCROLLABLE MENU
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Ensure the LocalPlayer is fully loaded before progressing
local LocalPlayer = Players.LocalPlayer
if not LocalPlayer then
    Players:GetPropertyChangedSignal("LocalPlayer"):Wait()
    LocalPlayer = Players.LocalPlayer
end

-- Safely resolve the target UI repository container
local TargetContainer = nil
local success, err = pcall(function()
    TargetContainer = LocalPlayer:WaitForChild("PlayerGui", 10)
end)

if not TargetContainer or not success then
    pcall(function()
        TargetContainer = game:GetService("CoreGui")
    end)
end

if not TargetContainer then
    warn("EliteScriptUI Error: Failed to find an interface parent container.")
    return
end

-- Remove any pre-existing instances to avoid object duplication conflicts
local existingUI = TargetContainer:FindFirstChild("EliteScriptUI")
if existingUI then existingUI:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "EliteScriptUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = TargetContainer

-- GLOBAL NEO-RAINBOW ACCENT DRIVER
local rainbowColor = Color3.fromRGB(0, 255, 255)
RunService.RenderStepped:Connect(function()
    local hue = (tick() % 4) / 4
    rainbowColor = Color3.fromHSV(hue, 1, 1)
end)

-- SMOOTH ENGINE DRAG MANAGEMENT SYSTEM
local function makeDraggable(frame)
    local UserInputService = game:GetService("UserInputService")
    local dragging, dragInput, dragStart, startPos

    local function update(input)
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end

-- OPEN TOGGLE ACTIVATOR
local OpenBtn = Instance.new("TextButton", ScreenGui)
OpenBtn.Visible = false
OpenBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
OpenBtn.Position = UDim2.new(0, 15, 0.5, -20)
OpenBtn.Size = UDim2.new(0, 110, 0, 40)
OpenBtn.Text = "OPEN ELITE"
OpenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenBtn.Font = Enum.Font.GothamBold
OpenBtn.TextSize = 12

local OpenStroke = Instance.new("UIStroke", OpenBtn)
OpenStroke.Width = 2
Instance.new("UICorner", OpenBtn)
makeDraggable(OpenBtn)

RunService.RenderStepped:Connect(function()
    OpenStroke.Color = rainbowColor
end)

-- WINDOW CONFIGURATION CONTROLS
local function addControls(parent, targetFrame)
    local CloseBtn = Instance.new("TextButton", parent)
    CloseBtn.Text = "X"
    CloseBtn.Size = UDim2.new(0, 25, 0, 25)
    CloseBtn.Position = UDim2.new(1, -30, 0, 8)
    CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", CloseBtn)
    
    local MinBtn = Instance.new("TextButton", parent)
    MinBtn.Text = "—"
    MinBtn.Size = UDim2.new(0, 25, 0, 25)
    MinBtn.Position = UDim2.new(1, -60, 0, 8)
    MinBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinBtn.Font = Enum.Font.GothamBold
    Instance.new("UICorner", MinBtn)

    CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)
    MinBtn.MouseButton1Click:Connect(function()
        targetFrame.Visible = false
        OpenBtn.Visible = true
    end)
end

-- VECTOR ART NATIVE INTERFACE LOGO
local function attachKnifeLogo(parentContainer)
    local LogoFrame = Instance.new("Frame", parentContainer)
    LogoFrame.BackgroundTransparency = 1
    LogoFrame.Position = UDim2.new(0, 15, 0, 12)
    LogoFrame.Size = UDim2.new(0, 30, 0, 30)

    local Blade = Instance.new("Frame", LogoFrame)
    Blade.Size = UDim2.new(0, 6, 0, 16)
    Blade.Position = UDim2.new(0.5, -3, 0, 2)
    Blade.BorderSizePixel = 0
    
    local Handle = Instance.new("Frame", LogoFrame)
    Handle.Size = UDim2.new(0, 4, 0, 8)
    Handle.Position = UDim2.new(0.5, -2, 0, 20)
    Handle.BackgroundColor3 = Color3.fromRGB(100, 50, 30)
    Handle.BorderSizePixel = 0

    local Guard = Instance.new("Frame", LogoFrame)
    Guard.Size = UDim2.new(0, 14, 0, 2)
    Guard.Position = UDim2.new(0.5, -7, 0, 18)
    Guard.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
    Guard.BorderSizePixel = 0

    RunService.RenderStepped:Connect(function()
        Blade.BackgroundColor3 = rainbowColor
    end)
end

-- LOG IN ENGINE FRAME
local KeyFrame = Instance.new("Frame", ScreenGui)
KeyFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
KeyFrame.Position = UDim2.new(0.5, -140, 0.5, -145)
KeyFrame.Size = UDim2.new(0, 280, 0, 290)
Instance.new("UICorner", KeyFrame)

local KeyStroke = Instance.new("UIStroke", KeyFrame)
KeyStroke.Width = 2
addControls(KeyFrame, KeyFrame)
attachKnifeLogo(KeyFrame)
makeDraggable(KeyFrame)

local Title = Instance.new("TextLabel", KeyFrame)
Title.Text = "SD UNIVERSAL"
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.TextSize = 20
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Center

RunService.RenderStepped:Connect(function()
    KeyStroke.Color = rainbowColor
    Title.TextColor3 = rainbowColor
end)

local KeyInput = Instance.new("TextBox", KeyFrame)
KeyInput.PlaceholderText = "ENTER KEY"
KeyInput.Position = UDim2.new(0.1, 0, 0.28, 0)
KeyInput.Size = UDim2.new(0.8, 0, 0, 40)
KeyInput.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
KeyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyInput.Font = Enum.Font.Gotham
Instance.new("UICorner", KeyInput)

local SubmitBtn = Instance.new("TextButton", KeyFrame)
SubmitBtn.Text = "SUBMIT"
SubmitBtn.Position = UDim2.new(0.1, 0, 0.48, 0)
SubmitBtn.Size = UDim2.new(0.8, 0, 0, 40)
SubmitBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
SubmitBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", SubmitBtn)
local SubmitStroke = Instance.new("UIStroke", SubmitBtn)

local GetKeyBtn = Instance.new("TextButton", KeyFrame)
GetKeyBtn.Text = "GET KEY (DISCORD)"
GetKeyBtn.Position = UDim2.new(0.1, 0, 0.72, 0)
GetKeyBtn.Size = UDim2.new(0.8, 0, 0, 40)
GetKeyBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
GetKeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
GetKeyBtn.Font = Enum.Font.GothamBold
Instance.new("UICorner", GetKeyBtn)

RunService.RenderStepped:Connect(function()
    SubmitStroke.Color = rainbowColor
    SubmitBtn.TextColor3 = rainbowColor
end)

-- DASHBOARD MAIN HUB FRAME
local MainHub = Instance.new("Frame", ScreenGui)
MainHub.Visible = false
MainHub.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainHub.Position = UDim2.new(0.5, -150, 0.5, -140)
MainHub.Size = UDim2.new(0, 300, 0, 280)
Instance.new("UICorner", MainHub)

local HubStroke = Instance.new("UIStroke", MainHub)
HubStroke.Width = 2
addControls(MainHub, MainHub)
attachKnifeLogo(MainHub)
makeDraggable(MainHub)

local HubTitle = Instance.new("TextLabel", MainHub)
HubTitle.Text = "ELITE HUB"
HubTitle.Size = UDim2.new(1, 0, 0, 50)
HubTitle.Position = UDim2.new(0, 20, 0, 0)
HubTitle.BackgroundTransparency = 1
HubTitle.TextSize = 20
HubTitle.Font = Enum.Font.GothamBold
HubTitle.TextXAlignment = Enum.TextXAlignment.Center

RunService.RenderStepped:Connect(function()
    HubStroke.Color = rainbowColor
    HubTitle.TextColor3 = rainbowColor
end)

-- SCROLLING FRAME FOR THE BUTTON ROW
local ButtonContainer = Instance.new("ScrollingFrame", MainHub)
ButtonContainer.BackgroundTransparency = 1
ButtonContainer.Position = UDim2.new(0, 5, 0, 55)
ButtonContainer.Size = UDim2.new(1, -10, 1, -65)
ButtonContainer.CanvasSize = UDim2.new(0, 0, 0, 0)
ButtonContainer.ScrollBarThickness = 0
ButtonContainer.AutomaticCanvasSize = Enum.AutomaticCanvasSize.Y
ButtonContainer.ScrollingDirection = Enum.ScrollingDirection.Y

local layout = Instance.new("UIListLayout", ButtonContainer)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
layout.VerticalAlignment = Enum.VerticalAlignment.Top
layout.Padding = UDim.new(0, 12)

local function createBtn(name)
    local b = Instance.new("TextButton", ButtonContainer)
    b.Text = name
    b.Size = UDim2.new(0.9, 0, 0, 45)
    b.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    b.TextColor3 = Color3.fromRGB(240, 240, 240)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 14
    Instance.new("UICorner", b)
    local btnStroke = Instance.new("UIStroke", b)
    btnStroke.Color = Color3.fromRGB(50, 50, 50)
    return b, btnStroke
end

local ESPBtn, espStroke = createBtn("ENABLE ESP")
local AimBtn, aimStroke = createBtn("AIMBOT: OFF")
local TPBtn, tpStroke = createBtn("TELEPORT TO GUN")

-- INTERACTION ROUTINES
local aimActive = false
local espActive = false

GetKeyBtn.MouseButton1Click:Connect(function()
    if setclipboard then
        setclipboard("https://discord.gg/Tz9YX9SWR")
    end
    GetKeyBtn.Text = "COPIED!"
    task.wait(1)
    GetKeyBtn.Text = "GET KEY (DISCORD)"
end)

SubmitBtn.MouseButton1Click:Connect(function()
    if KeyInput.Text == "ClideSaltik" then
        KeyFrame.Visible = false
        MainHub.Visible = true
    else
        KeyInput.Text = ""
        KeyInput.PlaceholderText = "WRONG KEY!"
        task.wait(1.5)
        KeyInput.PlaceholderText = "ENTER KEY"
    end
end)

OpenBtn.MouseButton1Click:Connect(function()
    OpenBtn.Visible = false
    MainHub.Visible = true
end)

-- COMBAT & VISUAL FUNCTIONS
ESPBtn.MouseButton1Click:Connect(function()
    espActive = not espActive
    ESPBtn.Text = espActive and "ESP: ON" or "ENABLE ESP"
    espStroke.Color = espActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(50, 50, 50)
    
    if not espActive then
        for _, p in pairs(Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("Highlight") then
                p.Character.Highlight:Destroy()
            end
        end
    end
end)

RunService.Heartbeat:Connect(function()
    if not espActive then return end
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local h = p.Character:FindFirstChild("Highlight") or Instance.new("Highlight", p.Character)
            h.FillTransparency = 0.5
            h.OutlineTransparency = 0
            
            if p.Backpack:FindFirstChild("Knife") or p.Character:FindFirstChild("Knife") then
                h.FillColor = Color3.fromRGB(255, 0, 0)
            elseif p.Backpack:FindFirstChild("Gun") or p.Character:FindFirstChild("Gun") then
                h.FillColor = Color3.fromRGB(0, 100, 255)
            else
                h.FillColor = Color3.fromRGB(0, 255, 100)
            end
        end
    end
end)

AimBtn.MouseButton1Click:Connect(function()
    aimActive = not aimActive
    AimBtn.Text = aimActive and "AIMBOT: ON" or "AIMBOT: OFF"
    aimStroke.Color = aimActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(50, 50, 50)
end)

local function getClosestThreat()
    local target = nil
    local shortestDistance = math.huge
    local cam = workspace.CurrentCamera

    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            if p.Character:FindFirstChild("Knife") or p.Character:FindFirstChild("Gun") or p.Backpack:FindFirstChild("Knife") or p.Backpack:FindFirstChild("Gun") then
                local pos, onScreen = cam:WorldToViewportPoint(p.Character.HumanoidRootPart.Position)
                if onScreen then
                    local mousePos = Vector2.new(cam.ViewportSize.X/2, cam.ViewportSize.Y/2)
                    local dist = (Vector2.new(pos.X, pos.Y) - mousePos).Magnitude
                    if dist < shortestDistance then
                        shortestDistance = dist
                        target = p.Character.HumanoidRootPart
                    end
                end
            end
        end
    end
    return target
end

RunService.RenderStepped:Connect(function()
    if aimActive then
        local target = getClosestThreat()
        if target then
            local cam = workspace.CurrentCamera
            cam.CFrame = CFrame.lookAt(cam.CFrame.Position, target.Position)
        end
    end
end)

TPBtn.MouseButton1Click:Connect(function()
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    
    if hrp then
        local droppedGun = nil
        for _, obj in pairs(workspace:GetDescendants()) do
            if (obj:IsA("Tool") and string.find(string.lower(obj.Name), "gun")) or (obj.Name == "GunDrop") then
                droppedGun = obj
                break
            end
        end

        if droppedGun then
            local targetCFrame = nil
            if droppedGun:IsA("BasePart") then
                targetCFrame = droppedGun.CFrame
            elseif droppedGun:IsA("Model") and droppedGun.PrimaryPart then
                targetCFrame = droppedGun.PrimaryPart.CFrame
            elseif droppedGun:FindFirstChildWhichIsA("BasePart", true) then
                targetCFrame = droppedGun:FindFirstChildWhichIsA("BasePart", true).CFrame
            end

            if targetCFrame then
                hrp.CFrame = targetCFrame + Vector3.new(0, 3, 0)
                TPBtn.Text = "TELEPORTED!"
                task.wait(1)
                TPBtn.Text = "TELEPORT TO GUN"
            else
                TPBtn.Text = "PART NOT FOUND"
                task.wait(1)
                TPBtn.Text = "TELEPORT TO GUN"
            end
        else
            TPBtn.Text = "NO GUN FOUND"
            task.wait(1)
            TPBtn.Text = "TELEPORT TO GUN"
        end
    end
end)
