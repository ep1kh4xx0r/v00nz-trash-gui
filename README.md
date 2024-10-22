-- Create the ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "v00nzTrashGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Create the main frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0.4, 0, 0.5, 0) -- Medium-large
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.BackgroundTransparency = 0.5 -- Transparent-grey
MainFrame.BackgroundColor3 = Color3.fromRGB(128, 128, 128) -- Grey color
MainFrame.BorderSizePixel = 2
MainFrame.BorderColor3 = Color3.fromRGB(255, 255, 255) -- White outline
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Title label
local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Text = "v00nz trash gui v1"
Title.Size = UDim2.new(1, 0, 0.15, 0)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.TextScaled = true
Title.TextColor3 = Color3.new(1, 1, 1) -- White text
Title.Parent = MainFrame

-- Button 1: Jumpscare
local JumpscareButton = Instance.new("TextButton")
JumpscareButton.Name = "JumpscareButton"
JumpscareButton.Text = "Jumpscare"
JumpscareButton.Size = UDim2.new(0.8, 0, 0.15, 0)
JumpscareButton.Position = UDim2.new(0.1, 0, 0.25, 0)
JumpscareButton.BackgroundColor3 = Color3.new(0, 0, 0) -- Black color
JumpscareButton.TextScaled = true
JumpscareButton.TextColor3 = Color3.fromRGB(255, 0, 0) -- Start as red
JumpscareButton.Parent = MainFrame

-- Button 2: Particles
local ParticleButton = Instance.new("TextButton")
ParticleButton.Name = "ParticleButton"
ParticleButton.Text = "Particles"
ParticleButton.Size = UDim2.new(0.8, 0, 0.15, 0)
ParticleButton.Position = UDim2.new(0.1, 0, 0.45, 0)
ParticleButton.BackgroundColor3 = Color3.new(0, 0, 0) -- Black color
ParticleButton.TextScaled = true
ParticleButton.TextColor3 = Color3.fromRGB(255, 0, 0) -- Start as red
ParticleButton.Parent = MainFrame

-- Button 3: Message
local MessageButton = Instance.new("TextButton")
MessageButton.Name = "MessageButton"
MessageButton.Text = "Message"
MessageButton.Size = UDim2.new(0.8, 0, 0.15, 0)
MessageButton.Position = UDim2.new(0.1, 0, 0.65, 0)
MessageButton.BackgroundColor3 = Color3.new(0, 0, 0) -- Black color
MessageButton.TextScaled = true
MessageButton.TextColor3 = Color3.fromRGB(255, 0, 0) -- Start as red
MessageButton.Parent = MainFrame

-- Button 4: Disco
local DiscoButton = Instance.new("TextButton")
DiscoButton.Name = "DiscoButton"
DiscoButton.Text = "Disco"
DiscoButton.Size = UDim2.new(0.8, 0, 0.15, 0)
DiscoButton.Position = UDim2.new(0.1, 0, 0.85, 0)
DiscoButton.BackgroundColor3 = Color3.new(0, 0, 0) -- Black color
DiscoButton.TextScaled = true
DiscoButton.TextColor3 = Color3.fromRGB(255, 0, 0) -- Start as red
DiscoButton.Parent = MainFrame

-- Function to create rainbow effect for text color
local function applyRainbowText(button)
    local hue = 0
    while true do
        hue = hue + 0.01
        if hue >= 1 then
            hue = 0
        end
        button.TextColor3 = Color3.fromHSV(hue, 1, 1) -- HSV for rainbow effect
        wait(0.05) -- Adjust speed of color change
    end
end

-- Apply the rainbow effect to the buttons
spawn(function() applyRainbowText(JumpscareButton) end)
spawn(function() applyRainbowText(ParticleButton) end)
spawn(function() applyRainbowText(MessageButton) end)
spawn(function() applyRainbowText(DiscoButton) end)

-- Function for jumpscare
local function jumpscare()
    -- Create the jumpscare image
    local jumpscareImage = Instance.new("ImageLabel")
    jumpscareImage.Size = UDim2.new(1, 0, 1, 0) -- Full screen
    jumpscareImage.Position = UDim2.new(0, 0, 0, 0)
    jumpscareImage.Image = "rbxassetid://94603582731783" -- Jumpscare image ID
    jumpscareImage.BackgroundTransparency = 1
    jumpscareImage.Parent = ScreenGui

    -- Play the jumpscare sound
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://6018028320" -- Jumpscare music ID
    sound.Volume = 1
    sound.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    sound:Play()

    -- Remove jumpscare image and sound when the music ends
    sound.Ended:Connect(function()
        jumpscareImage:Destroy()
        sound:Destroy()
    end)
end

JumpscareButton.MouseButton1Click:Connect(jumpscare)

-- Function for particles (emit particles around all players)
local function createParticles()
    local particleEmitter = Instance.new("ParticleEmitter")
    particleEmitter.Texture = "rbxassetid://94603582731783" -- Particle image ID
    particleEmitter.Size = NumberSequence.new(5) -- Large 2D particles
    particleEmitter.Lifetime = NumberRange.new(3) -- Particles last 3 seconds
    particleEmitter.Rate = 50 -- Emission rate
    particleEmitter.VelocitySpread = 180 -- Spread out

    -- Emit particles from every player’s HumanoidRootPart
    for _, player in pairs(game.Players:GetPlayers()) do
        local character = player.Character or player.CharacterAdded:Wait()
        local rootPart = character:WaitForChild("HumanoidRootPart")
        particleEmitter:Clone().Parent = rootPart
    end
end

ParticleButton.MouseButton1Click:Connect(createParticles)

-- Function for the message button to fill the screen
local function showMessage()
    for _, player in pairs(game.Players:GetPlayers()) do
        local playerGui = player:WaitForChild("PlayerGui")
        
        -- Create a full-screen message label for the player
        local messageLabel = Instance.new("TextLabel")
        messageLabel.Size = UDim2.new(1, 0, 1, 0) -- Full screen
        messageLabel.Position = UDim2.new(0, 0, 0, 0)
        messageLabel.BackgroundTransparency = 1
        messageLabel.Text = "tem v00nked and c0l_k1led j0in todiy!1!1!1!1"
        messageLabel.TextScaled = true
        messageLabel.TextColor3 = Color3.new(1, 0, 0) -- Red text
        messageLabel.Parent = playerGui

        -- Remove the message after 5 seconds
        wait(5)
        messageLabel:Destroy()
    end
end

MessageButton.MouseButton1Click:Connect(showMessage)

-- Function to create a continuous disco effect (looped color transition)
local function createDiscoEffect()
    -- Create a full-screen frame
    local discoFrame = Instance.new("Frame")
    discoFrame.Size = UDim2.new(1, 0, 1, 0)
    discoFrame.Position = UDim2.new(0, 0, 0, 0)
    discoFrame.BackgroundTransparency = 0.5 -- Transparent
    discoFrame.Parent = ScreenGui
    
    -- Continuously change the background color smoothly
    spawn(function()
        while discoFrame.Parent do
            for hue = 0, 1, 0.01 do
                discoFrame.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
                wait(0.05)
            end
        end
    end)
end

DiscoButton.MouseButton1Click:Connect(createDiscoEffect)
