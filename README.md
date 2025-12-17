-- Fishing GUI Script with Fluent UI Design
-- Created by Claude

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Konfigurasi
local loopEnabled = false
local loopSpeed = 1

-- Buat ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FishingGUI"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- Background Blur (Fluent Effect)
local blurFrame = Instance.new("Frame")
blurFrame.Name = "BlurFrame"
blurFrame.Size = UDim2.new(0, 420, 0, 520)
blurFrame.Position = UDim2.new(0.5, -210, 0.5, -260)
blurFrame.BackgroundColor3 = Color3.fromRGB(32, 32, 32)
blurFrame.BackgroundTransparency = 0.3
blurFrame.BorderSizePixel = 0
blurFrame.Parent = screenGui

local blurCorner = Instance.new("UICorner")
blurCorner.CornerRadius = UDim.new(0, 8)
blurCorner.Parent = blurFrame

-- Acrylic Effect
local acrylicGradient = Instance.new("UIGradient")
acrylicGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(45, 45, 45)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(28, 28, 28))
}
acrylicGradient.Rotation = 90
acrylicGradient.Parent = blurFrame

-- Stroke Border (Fluent Style)
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(70, 70, 70)
stroke.Thickness = 1
stroke.Transparency = 0.5
stroke.Parent = blurFrame

-- Main Frame (Content)
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(1, -2, 1, -2)
mainFrame.Position = UDim2.new(0, 1, 0, 1)
mainFrame.BackgroundTransparency = 1
mainFrame.Parent = blurFrame

-- Header
local headerFrame = Instance.new("Frame")
headerFrame.Name = "Header"
headerFrame.Size = UDim2.new(1, 0, 0, 60)
headerFrame.BackgroundTransparency = 1
headerFrame.Parent = mainFrame

-- App Icon
local iconLabel = Instance.new("TextLabel")
iconLabel.Size = UDim2.new(0, 40, 0, 40)
iconLabel.Position = UDim2.new(0, 15, 0, 10)
iconLabel.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
iconLabel.Text = "🎣"
iconLabel.TextSize = 20
iconLabel.Font = Enum.Font.GothamBold
iconLabel.Parent = headerFrame

local iconCorner = Instance.new("UICorner")
iconCorner.CornerRadius = UDim.new(0, 8)
iconCorner.Parent = iconLabel

-- Title
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -120, 0, 30)
titleLabel.Position = UDim2.new(0, 65, 0, 10)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Fishing Automation"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 18
titleLabel.Font = Enum.Font.GothamSemibold
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = headerFrame

-- Subtitle
local subtitleLabel = Instance.new("TextLabel")
subtitleLabel.Size = UDim2.new(1, -120, 0, 16)
subtitleLabel.Position = UDim2.new(0, 65, 0, 34)
subtitleLabel.BackgroundTransparency = 1
subtitleLabel.Text = "Fluent Design System"
subtitleLabel.TextColor3 = Color3.fromRGB(160, 160, 160)
subtitleLabel.TextSize = 12
subtitleLabel.Font = Enum.Font.Gotham
subtitleLabel.TextXAlignment = Enum.TextXAlignment.Left
subtitleLabel.Parent = headerFrame

-- Close Button (Fluent Style)
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 46, 0, 32)
closeButton.Position = UDim2.new(1, -61, 0, 14)
closeButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
closeButton.BackgroundTransparency = 1
closeButton.Text = "✕"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 14
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = headerFrame

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 4)
closeCorner.Parent = closeButton

-- Divider
local divider = Instance.new("Frame")
divider.Size = UDim2.new(1, -30, 0, 1)
divider.Position = UDim2.new(0, 15, 0, 60)
divider.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
divider.BackgroundTransparency = 0.5
divider.BorderSizePixel = 0
divider.Parent = mainFrame

-- Content Container
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Name = "Content"
contentFrame.Size = UDim2.new(1, -30, 1, -80)
contentFrame.Position = UDim2.new(0, 15, 0, 70)
contentFrame.BackgroundTransparency = 1
contentFrame.BorderSizePixel = 0
contentFrame.ScrollBarThickness = 4
contentFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
contentFrame.CanvasSize = UDim2.new(0, 0, 0, 440)
contentFrame.Parent = mainFrame

-- Helper function untuk membuat Fluent Input
local function createFluentInput(parent, labelText, placeholder, defaultValue, yPos)
    local container = Instance.new("Frame")
    container.Size = UDim2.new(1, 0, 0, 60)
    container.Position = UDim2.new(0, 0, 0, yPos)
    container.BackgroundTransparency = 1
    container.Parent = parent
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 18)
    label.BackgroundTransparency = 1
    label.Text = labelText
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.TextSize = 13
    label.Font = Enum.Font.GothamMedium
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = container
    
    local inputBg = Instance.new("Frame")
    inputBg.Size = UDim2.new(1, 0, 0, 32)
    inputBg.Position = UDim2.new(0, 0, 0, 22)
    inputBg.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    inputBg.BorderSizePixel = 0
    inputBg.Parent = container
    
    local inputCorner = Instance.new("UICorner")
    inputCorner.CornerRadius = UDim.new(0, 4)
    inputCorner.Parent = inputBg
    
    local inputStroke = Instance.new("UIStroke")
    inputStroke.Color = Color3.fromRGB(80, 80, 80)
    inputStroke.Thickness = 1
    inputStroke.Transparency = 0.5
    inputStroke.Parent = inputBg
    
    local input = Instance.new("TextBox")
    input.Size = UDim2.new(1, -16, 1, 0)
    input.Position = UDim2.new(0, 8, 0, 0)
    input.BackgroundTransparency = 1
    input.PlaceholderText = placeholder
    input.PlaceholderColor3 = Color3.fromRGB(120, 120, 120)
    input.Text = defaultValue
    input.TextColor3 = Color3.fromRGB(255, 255, 255)
    input.TextSize = 13
    input.Font = Enum.Font.Gotham
    input.TextXAlignment = Enum.TextXAlignment.Left
    input.ClearTextOnFocus = false
    input.Parent = inputBg
    
    -- Hover effect
    input.MouseEnter:Connect(function()
        TweenService:Create(inputStroke, TweenInfo.new(0.2), {Color = Color3.fromRGB(0, 120, 215), Transparency = 0}):Play()
    end)
    
    input.MouseLeave:Connect(function()
        if input:IsFocused() then return end
        TweenService:Create(inputStroke, TweenInfo.new(0.2), {Color = Color3.fromRGB(80, 80, 80), Transparency = 0.5}):Play()
    end)
    
    input.Focused:Connect(function()
        TweenService:Create(inputStroke, TweenInfo.new(0.2), {Color = Color3.fromRGB(0, 120, 215), Thickness = 2, Transparency = 0}):Play()
    end)
    
    input.FocusLost:Connect(function()
        TweenService:Create(inputStroke, TweenInfo.new(0.2), {Color = Color3.fromRGB(80, 80, 80), Thickness = 1, Transparency = 0.5}):Play()
    end)
    
    return input
end

-- Helper function untuk membuat Fluent Button
local function createFluentButton(parent, text, color, icon, yPos, width)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(width or 1, 0, 0, 40)
    button.Position = UDim2.new(0, 0, 0, yPos)
    button.BackgroundColor3 = color
    button.BorderSizePixel = 0
    button.Text = ""
    button.AutoButtonColor = false
    button.Parent = parent
    
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 4)
    buttonCorner.Parent = button
    
    local buttonLabel = Instance.new("TextLabel")
    buttonLabel.Size = UDim2.new(1, 0, 1, 0)
    buttonLabel.BackgroundTransparency = 1
    buttonLabel.Text = (icon or "") .. " " .. text
    buttonLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    buttonLabel.TextSize = 14
    buttonLabel.Font = Enum.Font.GothamMedium
    buttonLabel.Parent = button
    
    -- Hover effect
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.new(
            math.min(color.R * 1.2, 1),
            math.min(color.G * 1.2, 1),
            math.min(color.B * 1.2, 1)
        )}):Play()
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = color}):Play()
    end)
    
    -- Click effect
    button.MouseButton1Down:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.new(
            color.R * 0.8,
            color.G * 0.8,
            color.B * 0.8
        )}):Play()
    end)
    
    button.MouseButton1Up:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = color}):Play()
    end)
    
    return button
end

-- Create Inputs
local nameInput = createFluentInput(contentFrame, "Fish Name", "Enter fish name...", "Golden Fish", 0)
local rarityInput = createFluentInput(contentFrame, "Rarity", "Enter rarity...", "Legendary", 70)
local weightInput = createFluentInput(contentFrame, "Weight", "Enter weight...", "50", 140)
local speedInput = createFluentInput(contentFrame, "Loop Speed (seconds)", "Enter speed...", "1", 210)

-- Section Label
local actionLabel = Instance.new("TextLabel")
actionLabel.Size = UDim2.new(1, 0, 0, 24)
actionLabel.Position = UDim2.new(0, 0, 0, 290)
actionLabel.BackgroundTransparency = 1
actionLabel.Text = "Quick Actions"
actionLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
actionLabel.TextSize = 12
actionLabel.Font = Enum.Font.GothamMedium
actionLabel.TextXAlignment = Enum.TextXAlignment.Left
actionLabel.Parent = contentFrame

-- Money Buttons Row
local moneyContainer = Instance.new("Frame")
moneyContainer.Size = UDim2.new(1, 0, 0, 40)
moneyContainer.Position = UDim2.new(0, 0, 0, 320)
moneyContainer.BackgroundTransparency = 1
moneyContainer.Parent = contentFrame

local infMoneyButton = createFluentButton(moneyContainer, "INF MONEY", Color3.fromRGB(255, 185, 0), "💰", 0, 0.48)
infMoneyButton.Position = UDim2.new(0, 0, 0, 0)

local fixMoneyButton = createFluentButton(moneyContainer, "FIX MONEY", Color3.fromRGB(16, 124, 16), "🔧", 0, 0.48)
fixMoneyButton.Position = UDim2.new(0.52, 0, 0, 0)

-- Control Buttons Row
local controlContainer = Instance.new("Frame")
controlContainer.Size = UDim2.new(1, 0, 0, 40)
controlContainer.Position = UDim2.new(0, 0, 0, 370)
controlContainer.BackgroundTransparency = 1
controlContainer.Parent = contentFrame

local executeButton = createFluentButton(controlContainer, "EXECUTE", Color3.fromRGB(0, 120, 215), "▶", 0, 0.48)
executeButton.Position = UDim2.new(0, 0, 0, 0)

local loopButton = createFluentButton(controlContainer, "LOOP: OFF", Color3.fromRGB(90, 90, 90), "🔄", 0, 0.48)
loopButton.Position = UDim2.new(0.52, 0, 0, 0)

-- Status Label
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 20)
statusLabel.Position = UDim2.new(0, 0, 0, 420)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "Ready"
statusLabel.TextColor3 = Color3.fromRGB(120, 120, 120)
statusLabel.TextSize = 11
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextXAlignment = Enum.TextXAlignment.Center
statusLabel.Parent = contentFrame

-- Functions
local function updateStatus(text, color)
    statusLabel.Text = text
    statusLabel.TextColor3 = color or Color3.fromRGB(120, 120, 120)
end

local function executeFixMoney()
    updateStatus("Fixing money...", Color3.fromRGB(16, 124, 16))
    
    local args = {
        {
            hookPosition = Vector3.new(1984.92919921875, 450.6968688964844, 205.5204620361328),
            name = "Trash Fish",
            rarity = "Common",
            weight = 1
        }
    }
    
    for i = 1, 5 do
        local success, err = pcall(function()
            game:GetService("ReplicatedStorage"):WaitForChild("FishingSystem"):WaitForChild("FishGiver"):FireServer(unpack(args))
        end)
        
        if success then
            updateStatus("Fix Money: " .. i .. " / 5", Color3.fromRGB(16, 124, 16))
        else
            warn("✗ Error:", err)
        end
        wait(0.5)
    end
    
    updateStatus("Money fixed!", Color3.fromRGB(16, 124, 16))
    wait(2)
    updateStatus("Ready", Color3.fromRGB(120, 120, 120))
end

local function executeInfMoney()
    updateStatus("Executing INF MONEY...", Color3.fromRGB(255, 185, 0))
    
    local args = {
        {
            hookPosition = Vector3.new(1984.92919921875, 450.6968688964844, 205.5204620361328),
            name = "purple Kraken",
            rarity = "Rare",
            weight = 100000000
        }
    }
    
    local success, err = pcall(function()
        game:GetService("ReplicatedStorage"):WaitForChild("FishingSystem"):WaitForChild("FishGiver"):FireServer(unpack(args))
    end)
    
    if success then
        updateStatus("INF MONEY executed!", Color3.fromRGB(255, 185, 0))
    else
        updateStatus("Error: " .. tostring(err), Color3.fromRGB(255, 50, 50))
    end
    
    wait(2)
    updateStatus("Ready", Color3.fromRGB(120, 120, 120))
end

local function executeFishing()
    updateStatus("Executing fishing...", Color3.fromRGB(0, 120, 215))
    
    local fishName = nameInput.Text
    local fishRarity = rarityInput.Text
    local fishWeight = tonumber(weightInput.Text) or 50
    
    local args = {
        {
            hookPosition = Vector3.new(2071.37890625, 450.6968688964844, 179.6163787841797),
            name = fishName,
            rarity = fishRarity,
            weight = fishWeight
        }
    }
    
    local success, err = pcall(function()
        game:GetService("ReplicatedStorage"):WaitForChild("FishingSystem"):WaitForChild("FishGiver"):FireServer(unpack(args))
    end)
    
    if success then
        updateStatus("Fishing executed: " .. fishName, Color3.fromRGB(0, 120, 215))
    else
        updateStatus("Error: " .. tostring(err), Color3.fromRGB(255, 50, 50))
    end
end

local loopCoroutine
local function startLoop()
    loopCoroutine = coroutine.create(function()
        while loopEnabled do
            executeFishing()
            wait(loopSpeed)
        end
    end)
    coroutine.resume(loopCoroutine)
end

-- Event Handlers
fixMoneyButton.MouseButton1Click:Connect(function()
    spawn(executeFixMoney)
end)

infMoneyButton.MouseButton1Click:Connect(function()
    spawn(executeInfMoney)
end)

executeButton.MouseButton1Click:Connect(function()
    executeFishing()
end)

loopButton.MouseButton1Click:Connect(function()
    loopEnabled = not loopEnabled
    
    if loopEnabled then
        loopButton.TextLabel.Text = "🔄 LOOP: ON"
        loopButton.BackgroundColor3 = Color3.fromRGB(16, 124, 16)
        loopSpeed = tonumber(speedInput.Text) or 1
        updateStatus("Loop started", Color3.fromRGB(16, 124, 16))
        startLoop()
    else
        loopButton.TextLabel.Text = "🔄 LOOP: OFF"
        loopButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
        updateStatus("Loop stopped", Color3.fromRGB(255, 185, 0))
        if loopCoroutine then
            loopCoroutine = nil
        end
        wait(1)
        updateStatus("Ready", Color3.fromRGB(120, 120, 120))
    end
end)

closeButton.MouseButton1Click:Connect(function()
    loopEnabled = false
    screenGui:Destroy()
end)

closeButton.MouseEnter:Connect(function()
    closeButton.BackgroundTransparency = 0
    closeButton.BackgroundColor3 = Color3.fromRGB(196, 43, 28)
end)

closeButton.MouseLeave:Connect(function()
    closeButton.BackgroundTransparency = 1
end)

speedInput.FocusLost:Connect(function()
    loopSpeed = tonumber(speedInput.Text) or 1
    if loopSpeed < 0.1 then loopSpeed = 0.1 end
    speedInput.Text = tostring(loopSpeed)
end)

-- Draggable
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    blurFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

headerFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = blurFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

headerFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

print("🎣 Fishing GUI (Fluent Design) loaded successfully!")
