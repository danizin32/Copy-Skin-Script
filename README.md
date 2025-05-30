-- Skin Copier com Interface Profissional (Preta e Vermelha)
-- Autorizado para testes e detecção de uso indevido

local Players = game:GetService("Players")
local InsertService = game:GetService("InsertService")
local LocalPlayer = Players.LocalPlayer

-- Função segura para aplicar skin
local function applySkinFromUsername(username)
    local success, userId = pcall(function()
        return Players:GetUserIdFromNameAsync(username)
    end)

    if success then
        local successDesc, humanoidDescription = pcall(function()
            return Players:GetHumanoidDescriptionFromUserId(userId)
        end)

        if successDesc and humanoidDescription then
            local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            if character then
                character:FindFirstChildOfClass("Humanoid"):ApplyDescription(humanoidDescription)
                print("[✔] Skin aplicada com sucesso!")
            end
        else
            warn("[✘] Não foi possível obter a descrição do Humanoid.")
        end
    else
        warn("[✘] Usuário não encontrado.")
    end
end

-- Interface
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "SkinCopierUI"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
Frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "Copiador de Skin"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 18

local TextBox = Instance.new("TextBox", Frame)
TextBox.Position = UDim2.new(0.1, 0, 0.35, 0)
TextBox.Size = UDim2.new(0.8, 0, 0, 30)
TextBox.PlaceholderText = "Digite o nome do jogador"
TextBox.Text = ""
TextBox.Font = Enum.Font.Gotham
TextBox.TextSize = 14
TextBox.TextColor3 = Color3.new(1, 1, 1)
TextBox.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

local corner = Instance.new("UICorner", TextBox)
corner.CornerRadius = UDim.new(0, 6)

local ApplyButton = Instance.new("TextButton", Frame)
ApplyButton.Position = UDim2.new(0.25, 0, 0.65, 0)
ApplyButton.Size = UDim2.new(0.5, 0, 0, 30)
ApplyButton.Text = "Aplicar Skin"
ApplyButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ApplyButton.TextColor3 = Color3.new(1, 1, 1)
ApplyButton.Font = Enum.Font.GothamBold
ApplyButton.TextSize = 14

local corner2 = Instance.new("UICorner", ApplyButton)
corner2.CornerRadius = UDim.new(0, 6)

ApplyButton.MouseButton1Click:Connect(function()
    local username = TextBox.Text
    if username and username ~= "" then
        applySkinFromUsername(username)
    else
        warn("[✘] Nome inválido.")
    end
end)
