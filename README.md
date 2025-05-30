-- Copiador visual de skin (Brookhaven compatível)
-- Interface corrigida para funcionar no PlayerGui

local Players = game:GetService("Players")
local InsertService = game:GetService("InsertService")
local LocalPlayer = Players.LocalPlayer

-- Função para aplicar roupas e acessórios
local function applySkinVisual(username)
    local success, userId = pcall(function()
        return Players:GetUserIdFromNameAsync(username)
    end)

    if not success then
        warn("Usuário inválido.")
        return
    end

    local successDesc, desc = pcall(function()
        return Players:GetHumanoidDescriptionFromUserId(userId)
    end)

    if not successDesc then
        warn("Erro ao obter descrição.")
        return
    end

    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        humanoid:RemoveAccessories()

        -- Escalas (opcional)
        humanoid.HipHeight = desc.HipHeight or 2
        humanoid.BodyTypeScale = desc.BodyTypeScale
        humanoid.DepthScale = desc.DepthScale
        humanoid.HeadScale = desc.HeadScale
        humanoid.HeightScale = desc.HeightScale
        humanoid.ProportionScale = desc.ProportionScale
        humanoid.WidthScale = desc.WidthScale

        -- Roupas
        for _, obj in pairs(character:GetChildren()) do
            if obj:IsA("Shirt") or obj:IsA("Pants") then
                obj:Destroy()
            end
        end

        local shirt = Instance.new("Shirt")
        shirt.ShirtTemplate = "rbxassetid://" .. desc.Shirt
        shirt.Parent = character

        local pants = Instance.new("Pants")
        pants.PantsTemplate = "rbxassetid://" .. desc.Pants
        pants.Parent = character

        -- Acessórios
        local function addAccessory(id)
            pcall(function()
                local acc = InsertService:LoadAsset(id)
                local accessory = acc:FindFirstChildOfClass("Accessory")
                if accessory then
                    accessory.Parent = character
                end
                acc:Destroy()
            end)
        end

        local ids = {
            desc.HatAccessory,
            desc.HairAccessory,
            desc.FaceAccessory,
            desc.BackAccessory,
            desc.FrontAccessory,
            desc.NeckAccessory,
            desc.ShouldersAccessory,
            desc.WaistAccessory
        }

        for _, accStr in pairs(ids) do
            for id in string.gmatch(accStr or "", "%d+") do
                addAccessory(tonumber(id))
            end
        end

        print("Skin visual aplicada.")
    end
end

-- Interface corrigida (inserida no PlayerGui)
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
local Gui = Instance.new("ScreenGui", PlayerGui)
Gui.Name = "SkinVisualUI"
Gui.ResetOnSpawn = false

local Frame = Instance.new("Frame", Gui)
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
Frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", Frame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Text = "Copiar Skin (Visual)"
Title.BackgroundTransparency = 1
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

local corner1 = Instance.new("UICorner", TextBox)
corner1.CornerRadius = UDim.new(0, 6)

local ApplyBtn = Instance.new("TextButton", Frame)
ApplyBtn.Position = UDim2.new(0.25, 0, 0.65, 0)
ApplyBtn.Size = UDim2.new(0.5, 0, 0, 30)
ApplyBtn.Text = "Aplicar Skin"
ApplyBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ApplyBtn.TextColor3 = Color3.new(1, 1, 1)
ApplyBtn.Font = Enum.Font.GothamBold
ApplyBtn.TextSize = 14

local corner2 = Instance.new("UICorner", ApplyBtn)
corner2.CornerRadius = UDim.new(0, 6)

ApplyBtn.MouseButton1Click:Connect(function()
    local username = TextBox.Text
    if username ~= "" then
        applySkinVisual(username)
    else
        warn("Nome inválido.")
    end
end)
