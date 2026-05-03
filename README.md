-- XXXX INSTANT (ULTIMATE ANTI-DEATH VERSION)
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UICorner_Main = Instance.new("UICorner")
local TitleBg = Instance.new("Frame")
local UICorner_Title = Instance.new("UICorner")
local Title = Instance.new("TextLabel")
local ButtonContainer = Instance.new("Frame")
local UIListLayout = Instance.new("UIListLayout")
local CloseBtn = Instance.new("TextButton")
local MinimizeBtn = Instance.new("TextButton")

local player = game.Players.LocalPlayer
local active = false
local savedPos = nil

-- UI Setup
ScreenGui.Parent = game.CoreGui
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 23)
MainFrame.Position = UDim2.new(0.5, -75, 0.5, -90)
MainFrame.Size = UDim2.new(0, 160, 0, 185)
MainFrame.Active = true
MainFrame.Draggable = true
UICorner_Main.CornerRadius = UDim.new(0, 18)
UICorner_Main.Parent = MainFrame

TitleBg.Parent = MainFrame
TitleBg.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
TitleBg.Position = UDim2.new(0.08, 0, 0, 10)
TitleBg.Size = UDim2.new(0.65, 0, 0, 26)
UICorner_Title.CornerRadius = UDim.new(1, 0)
UICorner_Title.Parent = TitleBg

Title.Parent = TitleBg
Title.Size = UDim2.new(1, 0, 1, 0)
Title.Text = "XXXX INSTANT"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 10
Title.Font = Enum.Font.GothamBold
Title.BackgroundTransparency = 1

CloseBtn.Parent = MainFrame
CloseBtn.Text = "×"; CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Size = UDim2.new(0, 20, 0, 20); CloseBtn.Position = UDim2.new(0.88, -10, 0.06, 0)
CloseBtn.BackgroundTransparency = 1; CloseBtn.TextSize = 22

MinimizeBtn.Parent = MainFrame
MinimizeBtn.Text = "-"; MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.Size = UDim2.new(0, 20, 0, 20); MinimizeBtn.Position = UDim2.new(0.78, -10, 0.05, 0)
MinimizeBtn.BackgroundTransparency = 1; MinimizeBtn.TextSize = 24

ButtonContainer.Parent = MainFrame
ButtonContainer.BackgroundTransparency = 1
ButtonContainer.Position = UDim2.new(0, 0, 0, 50)
ButtonContainer.Size = UDim2.new(1, 0, 1, -55)
UIListLayout.Parent = ButtonContainer
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.Padding = UDim.new(0, 8)

local function createMiniBtn(text, color)
    local btn = Instance.new("TextButton")
    local corner = Instance.new("UICorner")
    btn.Size = UDim2.new(0.85, 0, 0, 34)
    btn.BackgroundColor3 = color
    btn.Font = Enum.Font.GothamMedium
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 11; btn.Parent = ButtonContainer
    corner.CornerRadius = UDim.new(0, 10); corner.Parent = btn
    return btn
end

local SetupBtn = createMiniBtn("📍 SETUP", Color3.fromRGB(50, 50, 55))
local StartBtn = createMiniBtn("⚡ START", Color3.fromRGB(0, 120, 255))
local Status = Instance.new("TextLabel", ButtonContainer)
Status.Text = "GOD MODE: ON"; Status.TextColor3 = Color3.fromRGB(0, 255, 150)
Status.Size = UDim2.new(0.8, 0, 0, 15); Status.BackgroundTransparency = 1
Status.Font = Enum.Font.Gotham; Status.TextSize = 9

-- SUTEMA PARA NÃO MORRER (GOD MODE)
task.spawn(function()
    while true do
        local char = player.Character
        local hum = char and char:FindFirstChild("Humanoid")
        if hum then
            -- Se a vida baixar de 100, ele reseta na hora
            if hum.Health > 0 and hum.Health < 100 then
                hum.Health = 100
            end
            -- Bloqueia o estado de morte
            hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
        end
        task.wait() -- Roda o mais rápido possível
    end
end)

-- Lógica de TP
SetupBtn.MouseButton1Click:Connect(function()
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        savedPos = hrp.CFrame
        Status.Text = "POS SAVED"
    end
end)

StartBtn.MouseButton1Click:Connect(function()
    if not savedPos then return end
    active = not active
    
    if active then
        StartBtn.Text = "🛑 STOP"; StartBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        task.spawn(function()
            while active do
                local char = player.Character
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.Velocity = Vector3.new(0, 0, 0)
                    hrp.CFrame = savedPos
                    -- NoClip para não morrer por esmagamento
                    for _, v in pairs(char:GetDescendants()) do
                        if v:IsA("BasePart") then v.CanCollide = false end
                    end
                end
                task.wait(0.05)
            end
        end)
    else
        StartBtn.Text = "⚡ START"; StartBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    end
end)

CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)
