local player = game.Players.LocalPlayer
local playerGui = player.PlayerGui

-- Create the main frame for the GUI
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.Position = UDim2.new(0.25, 0, 0.25, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 139)  -- Dark Blue
mainFrame.BorderSizePixel = 0
mainFrame.Parent = playerGui

-- Create a title label at the top
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0.1, 0)
titleLabel.Text = "RC7 Remake"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.BackgroundTransparency = 1
titleLabel.TextSize = 24
titleLabel.TextAlign = Enum.TextAnchor.MiddleCenter
titleLabel.Parent = mainFrame

-- Create a text box at the top half of the GUI
local textBox = Instance.new("TextBox")
textBox.Size = UDim2.new(1, 0, 0.4, 0)
textBox.Position = UDim2.new(0, 0, 0.1, 0)
textBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
textBox.PlaceholderText = "Enter Script Here"
textBox.TextColor3 = Color3.fromRGB(0, 0, 0)
textBox.TextSize = 18
textBox.ClearTextOnFocus = true
textBox.Parent = mainFrame

-- Create a function to create buttons
local function createButton(name, positionY, onClick)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.8, 0, 0.1, 0)
    button.Position = UDim2.new(0.1, 0, positionY, 0)
    button.Text = name
    button.TextSize = 18
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    button.Parent = mainFrame

    button.MouseButton1Click:Connect(onClick)
end

-- Functionality for each button
local function attach()
    -- Show the other buttons and show "Attached!"
    local attachedLabel = Instance.new("TextLabel")
    attachedLabel.Size = UDim2.new(1, 0, 0.1, 0)
    attachedLabel.Position = UDim2.new(0, 0, 0.5, 0)
    attachedLabel.Text = "Attached!"
    attachedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    attachedLabel.BackgroundTransparency = 1
    attachedLabel.TextSize = 20
    attachedLabel.TextAlign = Enum.TextAnchor.MiddleCenter
    attachedLabel.Parent = mainFrame

    -- Show all buttons
    createButton("Execute", 0.6, function()
        local scriptText = textBox.Text
        local success, result = pcall(loadstring(scriptText))
        if not success then
            print("Error executing script:", result)
        end
    end)
    createButton("Detach", 0.7, function()
        -- Remove all buttons except Attach
        mainFrame:ClearAllChildren()
        mainFrame.Parent = playerGui
        mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 139)
        mainFrame.Size = UDim2.new(0.5, 0, 0.5, 0)
        mainFrame.Position = UDim2.new(0.25, 0, 0.25, 0)
        textBox.Parent = mainFrame
        titleLabel.Parent = mainFrame
        createButton("Attach", 0.5, attach)
    end)
    createButton("Infinite Yield", 0.8, function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end)
    createButton("W33P GUI", 0.9, function()
        -- W33P GUI script
        local jumpTextBox = Instance.new("TextBox")
        jumpTextBox.Position = UDim2.new(0, 0, 0, 150)
        jumpTextBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        jumpTextBox.BorderColor3 = Color3.fromRGB(255, 0, 0)
        jumpTextBox.TextColor3 = Color3.fromRGB(255, 0, 0)
        jumpTextBox.TextSize = 18
        jumpTextBox.PlaceholderText = "Enter Jump Power"
        jumpTextBox.TextStrokeTransparency = 0
        jumpTextBox.TextStrokeColor3 = Color3.fromRGB(255, 0, 0)
        jumpTextBox.Parent = mainFrame

        createButton("Jump Set", 200, function()
            local jumpPower = tonumber(jumpTextBox.Text)
            if jumpPower then
                player.Character.Humanoid.JumpPower = jumpPower
            end
        end)

        -- Fire All button
        createButton("Fire All", 250, function()
            for _, obj in pairs(workspace:GetChildren()) do
                if obj:IsA("BasePart") then
                    local fire = Instance.new("Fire")
                    fire.Parent = obj
                    fire.Heat = 10
                    fire.Size = 10
                end
            end
        end)

        -- Disco Fog button
        createButton("Disco Fog", 300, function()
            local fog = Instance.new("FogEnd")
            fog.Parent = workspace
            game.Lighting.FogColor = Color3.fromRGB(255, 0, 0)

            while true do
                wait(1)
                game.Lighting.FogColor = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
            end
        end)

        -- Give Sword button
        createButton("Give Sword", 350, function()
            local sword = Instance.new("Tool")
            sword.Name = "Sword"
            sword.Parent = player.Backpack
        end)

        -- Flood button
        createButton("Flood", 400, function()
            for _, obj in pairs(workspace:GetChildren()) do
                if obj:IsA("BasePart") then
                    obj.Position = Vector3.new(obj.Position.X, 145, obj.Position.Z)
                end
            end
        end)

        -- Ultimate Destruction button
        createButton("Ultimate Destruction", 450, function()
            for i = 1, 2000 do
                local part = Instance.new("Part")
                part.Size = Vector3.new(4, 4, 4)
                part.Position = Vector3.new(math.random(-1000, 1000), 100, math.random(-1000, 1000))
                part.Color = Color3.fromRGB(255, 0, 0)
                part.Anchored = true
                part.CanCollide = false
                part.Parent = workspace
                local fire = Instance.new("Fire")
                fire.Parent = part
            end
        end)

        -- Heal button
        createButton("Heal", 500, function()
            player.Character.Humanoid.Health = 100
        end)
    end)
end

-- Attach button, initial button before any actions
createButton("Attach", 0.5, attach)

-- Make the GUI draggable
local dragging = false
local dragInput, dragStart, startPos

local function updateInput(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
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
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        updateInput(input)
    end
end)
