-- Delta uyumlu Key & GUI Script

local Delta = require(game.ReplicatedStorage:WaitForChild("Delta"))

-- Key Sistemi

local correctKey = "Benjamim Hub🤤"

-- GUI

local player = game.Players.LocalPlayer

local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")

screenGui.Name = "ProGui"

screenGui.Parent = playerGui

-- Key Girişi Frame

local keyFrame = Instance.new("Frame")

keyFrame.Size = UDim2.new(0, 300, 0, 150)

keyFrame.Position = UDim2.new(0.5, -150, 0.5, -75)

keyFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

keyFrame.Parent = screenGui

local keyBox = Instance.new("TextBox")

keyBox.PlaceholderText = "Enter Key..."

keyBox.Size = UDim2.new(0, 200, 0, 40)

keyBox.Position = UDim2.new(0.5, -100, 0.3, 0)

keyBox.Parent = keyFrame

local checkButton = Instance.new("TextButton")

checkButton.Text = "Check Key"

checkButton.Size = UDim2.new(0, 100, 0, 30)

checkButton.Position = UDim2.new(0.5, -50, 0.7, 0)

checkButton.Parent = keyFrame

local errorLabel = Instance.new("TextLabel")

errorLabel.Text = ""

errorLabel.Size = UDim2.new(1, 0, 0, 30)

errorLabel.Position = UDim2.new(0, 0, 0.85, 0)

errorLabel.TextColor3 = Color3.fromRGB(255, 0, 0)

errorLabel.TextScaled = true

errorLabel.BackgroundTransparency = 1

errorLabel.Parent = keyFrame

-- GUI Ana Frame (Info + Scripts)

local mainFrame = Instance.new("Frame")

mainFrame.Size = UDim2.new(0, 400, 0, 500)

mainFrame.Position = UDim2.new(0.5, -200, 0.5, -250)

mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

mainFrame.Visible = false

mainFrame.Parent = screenGui

-- GUI Drag

local dragging, dragInput, dragStart, startPos

local function update(input)

    local delta = input.Position - dragStart

    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,

        startPos.Y.Scale, startPos.Y.Offset + delta.Y)

end

mainFrame.InputBegan:Connect(function(input)

    if input.UserInputType == Enum.UserInputType.MouseButton1 then

        dragging = true

        dragStart = input.Position

        startPos = mainFrame.Position

        input.Changed:Connect(function()

            if input.UserInputState == Enum.UserInputState.End then

                dragging = false

            end

        end)

    end

end)

mainFrame.InputChanged:Connect(function(input)

    if input.UserInputType == Enum.UserInputType.MouseMovement then

        dragInput = input

    end

end)

game:GetService("UserInputService").InputChanged:Connect(function(input)

    if input == dragInput and dragging then

        update(input)

    end

end)

-- Info Section

local infoButton = Instance.new("TextButton")

infoButton.Text = "Info"

infoButton.Size = UDim2.new(0, 380, 0, 50)

infoButton.Position = UDim2.new(0, 10, 0, 10)

infoButton.Parent = mainFrame

local infoLabel = Instance.new("TextLabel")

infoLabel.Size = UDim2.new(0, 380, 0, 200)

infoLabel.Position = UDim2.new(0, 10, 0, 70)

infoLabel.TextWrapped = true

infoLabel.BackgroundColor3 = Color3.fromRGB(30,30,30)

infoLabel.TextColor3 = Color3.fromRGB(255,255,255)

infoLabel.Visible = false

infoLabel.Parent = mainFrame

infoButton.MouseButton1Click:Connect(function()

    infoLabel.Visible = not infoLabel.Visible

    infoLabel.Text = "Made By Proertugrul29 (Eski Nicktir Takmayınız)\nKurulum Yılı: 2025"

end)

-- Scripts Section

local scriptsFrame = Instance.new("Frame")

scriptsFrame.Size = UDim2.new(0, 380, 0, 200)

scriptsFrame.Position = UDim2.new(0, 10, 0, 280)

scriptsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

scriptsFrame.Parent = mainFrame

local scriptBox = Instance.new("TextBox")

scriptBox.PlaceholderText = "Script Yaz..."

scriptBox.Size = UDim2.new(0, 260, 0, 40)

scriptBox.Position = UDim2.new(0, 10, 0, 10)

scriptBox.Parent = scriptsFrame

local addButton = Instance.new("TextButton")

addButton.Text = "+"

addButton.Size = UDim2.new(0, 50, 0, 40)

addButton.Position = UDim2.new(0, 280, 0, 10)

addButton.Parent = scriptsFrame

local savedScripts = {}

-- Script Ekleme

addButton.MouseButton1Click:Connect(function()

    if scriptBox.Text ~= "" then

        local scriptLabel = Instance.new("TextLabel")

        scriptLabel.Text = scriptBox.Text

        scriptLabel.Size = UDim2.new(0, 360, 0, 30)

        scriptLabel.Position = UDim2.new(0, 10, 0, #savedScripts * 35 + 60)

        scriptLabel.TextWrapped = true

        scriptLabel.BackgroundColor3 = Color3.fromRGB(50,50,50)

        scriptLabel.TextColor3 = Color3.fromRGB(255,255,255)

        scriptLabel.Parent = scriptsFrame

        table.insert(savedScripts, scriptLabel)

        scriptBox.Text = ""

    end

end)

-- Key Kontrol

checkButton.MouseButton1Click:Connect(function()

    if keyBox.Text == correctKey then

        keyFrame.Visible = false

        mainFrame.Visible = true

    else

        errorLabel.Text = "Wrong Key!"

        task.wait(3)

        errorLabel.Text = ""

    end

end)
