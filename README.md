-- Benjamim Hub - LocalScript

-- Delta (caso você queira usar depois, senão pode remover)
local Delta = require(game.ReplicatedStorage:WaitForChild("Delta"))

-- Key System
local correctKey = "Gumball123"

-- Player e GUI
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BenjamimHub"
screenGui.Parent = playerGui

-- Key Entry Frame
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

-- Main GUI Frame
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 500)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -250)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.Visible = false
mainFrame.Parent = screenGui

-- Hub Title
local hubTitle = Instance.new("TextLabel")
hubTitle.Text = "Benjamim Hub"
hubTitle.Size = UDim2.new(0, 380, 0, 40)
hubTitle.Position = UDim2.new(0, 10, 0, 10)
hubTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
hubTitle.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
hubTitle.TextScaled = true
hubTitle.Parent = mainFrame

-- Drag Function
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
infoButton.Position = UDim2.new(0, 10, 0, 60)
infoButton.Parent = mainFrame

local infoLabel = Instance.new("TextLabel")
infoLabel.Size = UDim2.new(0, 380, 0, 200)
infoLabel.Position = UDim2.new(0, 10, 0, 120)
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
scriptsFrame.Position = UDim2.new(0, 10, 0, 330)
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

-- Key Control + Load Script do GitHub
checkButton.MouseButton1Click:Connect(function()
    if keyBox.Text == correctKey then
        keyFrame.Visible = false
        mainFrame.Visible = true
        
        -- Executa script externo do GitHub
        local success, err = pcall(function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/Benjamim-b/Teste/main/README.md"))()
        end)
        if not success then
            warn("Erro ao executar script do GitHub: "..tostring(err))
        end
    else
        errorLabel.Text = "Wrong Key!"
        task.wait(3)
        errorLabel.Text = ""
    end
end)
