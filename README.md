# 839393939
Idk0
-- ТВОЙ УЛЬТРА-ЧЕТ MOBILE v3.1 (СПРЕЙ ДОБАВЛЕН)
-- Author: Yaa_35497
-- Дата создания: среда 6 ноября 2025

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Создаём ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GodPanelMobile"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- ИКОНКА (побольше для телефона)
local icon = Instance.new("ImageButton")
icon.Size = UDim2.new(0, 80, 0, 80)
icon.Position = UDim2.new(0, 20, 0, 20)
icon.BackgroundColor3 = Color3.fromRGB(255, 0, 100)
icon.BackgroundTransparency = 0.2
icon.BorderSizePixel = 0
icon.Image = "rbxassetid://3944680095"
icon.Parent = screenGui

-- Закруглённые углы
local iconCorner = Instance.new("UICorner")
iconCorner.CornerRadius = UDim.new(1, 0)
iconCorner.Parent = icon

-- ЧЁРНАЯ ПАНЕЛЬ
local panel = Instance.new("Frame")
panel.Size = UDim2.new(0.8, 0, 0.7, 0)
panel.Position = UDim2.new(0.1, 0, 1, 0)
panel.AnchorPoint = Vector2.new(0, 1)
panel.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
panel.BorderSizePixel = 0
panel.Visible = false
panel.Parent = screenGui

local panelCorner = Instance.new("UICorner")
panelCorner.CornerRadius = UDim.new(0, 12)
panelCorner.Parent = panel

-- КНОПКА ЗАКРЫТИЯ (X)
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 40, 0, 40)
closeBtn.Position = UDim2.new(1, -45, 0, 5)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 50, 50)
closeBtn.TextScaled = true
closeBtn.Font = Enum.Font.GothamBold
closeBtn.Parent = panel

-- Заголовок
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 0, 40)
title.Position = UDim2.new(0, 10, 0, 5)
title.BackgroundTransparency = 1
title.Text = "MOBILE PANEL"
title.TextColor3 = Color3.fromRGB(0, 255, 100)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.Parent = panel

-- Автор (под заголовком)
local author = Instance.new("TextLabel")
author.Size = UDim2.new(1, -50, 0, 20)
author.Position = UDim2.new(0, 10, 0, 45)
author.BackgroundTransparency = 1
author.Text = "by Yaa_35497 | среда 6 ноября 2025"
author.TextColor3 = Color3.fromRGB(150, 150, 150)
author.TextScaled = true
author.Font = Enum.Font.Gotham
author.TextXAlignment = Enum.TextXAlignment.Left
author.Parent = panel

-- SCROLLING FRAME
local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Size = UDim2.new(1, -20, 1, -90)
scrollFrame.Position = UDim2.new(0, 10, 0, 75)
scrollFrame.BackgroundTransparency = 1
scrollFrame.BorderSizePixel = 0
scrollFrame.ScrollBarThickness = 6
scrollFrame.ScrollBarImageColor3 = Color3.fromRGB(0, 255, 100)
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, 950)
scrollFrame.Parent = panel

-- КНОПКИ ФУНКЦИЙ
local function createBtn(name, posY, callback, bgColor)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0, 45)
    btn.Position = UDim2.new(0.05, 0, 0, posY)
    btn.BackgroundColor3 = bgColor or Color3.fromRGB(40, 40, 40)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(0, 255, 200)
    btn.TextScaled = true
    btn.Font = Enum.Font.Gotham
    btn.Parent = scrollFrame

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- ФУНКЦИЯ КОПИРОВАНИЯ В БУФЕР
local function copyToClipboard(text)
    local setclipboard = setclipboard or toclipboard or set_clipboard or (Clipboard and Clipboard.set)
    if setclipboard then
        setclipboard(text)
        game.StarterGui:SetCore("SendNotification", {
            Title = "📋 Буфер обмена",
            Text = "Ссылка скопирована!",
            Duration = 3
        })
    else
        game.StarterGui:SetCore("SendNotification", {
            Title = "❌ Ошибка",
            Text = "Не удалось скопировать",
            Duration = 3
        })
    end
end

-- ПЕРЕМЕННЫЕ ДЛЯ ФУНКЦИЙ
local flying = false
local bodyVelocity
local noclip = false
local noclipConnection
local espOn = false
local espBoxes = {}
local espNames = {}

-- ИСПРАВЛЕННЫЙ НОКЛИП
local function toggleNoclip()
    noclip = not noclip
    
    if noclip then
        game.StarterGui:SetCore("SendNotification", {
            Title = "NoClip", 
            Text = "Включен! Прохожу сквозь стены",
            Duration = 3
        })
        
        if noclipConnection then
            noclipConnection:Disconnect()
        end
        
        noclipConnection = RunService.Stepped:Connect(function()
            if not noclip then
                noclipConnection:Disconnect()
                return
            end
            
            local character = player.Character
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
        
    else
        if noclipConnection then
            noclipConnection:Disconnect()
            noclipConnection = nil
        end
        
        local character = player.Character
        if character then
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
        
        game.StarterGui:SetCore("SendNotification", {
            Title = "NoClip", 
            Text = "Выключен!",
            Duration = 2
        })
    end
end

-- ПРОСТОЙ ФЛАЙ
local function toggleFly()
    if flying then
        if bodyVelocity then
            bodyVelocity:Destroy()
            bodyVelocity = nil
        end
        flying = false
        game.StarterGui:SetCore("SendNotification", {
            Title = "Fly", 
            Text = "Выключен!",
            Duration = 2
        })
    else
        local character = player.Character
        if not character then return end
        
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        if not humanoidRootPart then return end
        
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 5, 0)
        bodyVelocity.MaxForce = Vector3.new(0, 40000, 0)
        bodyVelocity.Parent = humanoidRootPart
        
        flying = true
        game.StarterGui:SetCore("SendNotification", {
            Title = "Fly", 
            Text = "Летим вверх! Нажми ещё раз чтобы остановиться",
            Duration = 3
        })
    end
end

-- АВТОПОЛЁТ
local function autoFly()
    local character = player.Character
    if not character then return end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end
    
    local fly = Instance.new("BodyVelocity")
    fly.Velocity = humanoidRootPart.CFrame.LookVector * 50
    fly.MaxForce = Vector3.new(40000, 0, 40000)
    fly.Parent = humanoidRootPart
    
    game.StarterGui:SetCore("SendNotification", {
        Title = "AutoFly", 
        Text = "Летим вперёд!",
        Duration = 3
    })
    
    wait(5)
    fly:Destroy()
end

-- ESP ВКЛ/ВЫКЛ
local function toggleESP()
    espOn = not espOn
    if espOn then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("Head") then
                -- РАМКА
                local box = Instance.new("BoxHandleAdornment")
                box.Size = Vector3.new(2, 5, 2)
                box.Color3 = Color3.fromRGB(0, 255, 0)
                box.Transparency = 0.6
                box.AlwaysOnTop = true
                box.ZIndex = 10
                box.Adornee = plr.Character
                box.Parent = screenGui
                espBoxes[plr] = box

                -- ИМЯ
                local nameTag = Instance.new("BillboardGui")
                nameTag.Size = UDim2.new(0, 100, 0, 30)
                nameTag.StudsOffset = Vector3.new(0, 3, 0)
                nameTag.AlwaysOnTop = true
                nameTag.Adornee = plr.Character.Head
                nameTag.Parent = screenGui

                local label = Instance.new("TextLabel", nameTag)
                label.Size = UDim2.new(1,0,1,0)
                label.BackgroundTransparency = 1
                label.Text = plr.Name
                label.TextColor3 = Color3.fromRGB(0, 255, 255)
                label.TextScaled = true
                label.Font = Enum.Font.GothamBold
                espNames[plr] = nameTag
            end
        end
        game.StarterGui:SetCore("SendNotification",{Title="ESP",Text="ВКЛЮЧЕН",Duration=2})
    else
        for _, v in pairs(espBoxes) do if v then v:Destroy() end end
        for _, v in pairs(espNames) do if v then v:Destroy() end end
        espBoxes = {}
        espNames = {}
        game.StarterGui:SetCore("SendNotification",{Title="ESP",Text="ВЫКЛЮЧЕН",Duration=2})
    end
end

-- ТЕЛЕПОРТ К ИГРОКУ
local function teleportToPlayer()
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0.6,0,0,50)
    box.Position = UDim2.new(0.2,0,0.4,0)
    box.BackgroundColor3 = Color3.fromRGB(30,30,30)
    box.TextColor3 = Color3.fromRGB(0,255,200)
    box.PlaceholderText = "Имя игрока..."
    box.Text = ""
    box.TextScaled = true
    box.Font = Enum.Font.GothamBold
    box.Parent = screenGui
    local c = Instance.new("UICorner",box); c.CornerRadius = UDim.new(0,10)

    local go = Instance.new("TextButton")
    go.Size = UDim2.new(0,80,0,50)
    go.Position = UDim2.new(0.82,0,0.4,0)
    go.BackgroundColor3 = Color3.fromRGB(0,200,0)
    go.Text = "GO"
    go.TextColor3 = Color3.new(1,1,1)
    go.TextScaled = true
    go.Parent = screenGui
    local gc = Instance.new("UICorner",go); gc.CornerRadius = UDim.new(0,10)

    go.MouseButton1Click:Connect(function()
        local name = box.Text:lower()
        for _, p in pairs(Players:GetPlayers()) do
            if p.Name:lower():find(name) or p.DisplayName:lower():find(name) then
                if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    player.Character.HumanoidRootPart.CFrame = p.Character.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
                    game.StarterGui:SetCore("SendNotification",{Title="Телепорт",Text="Ты у "..p.Name.."!",Duration=3})
                end
                box:Destroy()
                go:Destroy()
                return
            end
        end
        game.StarterGui:SetCore("SendNotification",{Title="Ошибка",Text="Игрок не найден!",Duration=2})
    end)
end

-- ЖЕЛТЫЙ КУБ
local function giveYellowCube()
    local character = player.Character
    if not character then return end
    
    local tool = Instance.new("Tool")
    tool.Name = "ЖелтыйКуб"
    tool.Parent = character
    
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1.2, 1.2, 1.2)
    handle.BrickColor = BrickColor.new("Bright yellow")
    handle.Material = Enum.Material.Neon
    handle.Shape = Enum.PartType.Block
    handle.Parent = tool
    
    tool.GripPos = Vector3.new(0, 0, 0)
    tool.GripForward = Vector3.new(0, 0, -1)
    tool.GripRight = Vector3.new(1, 0, 0)
    tool.GripUp = Vector3.new(0, 1, 0)
    
    game.StarterGui:SetCore("SendNotification", {
        Title = "Инвентарь", 
        Text = "Желтый куб добавлен!",
        Duration = 3
    })
end

-- ПИСТОЛЕТ
local function givePistol()
    local character = player.Character
    if not character then return end
    
    local tool = Instance.new("Tool")
    tool.Name = "Пистолет"
    tool.Parent = character
    
    -- Основная ручка
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(0.3, 0.4, 0.8)
    handle.BrickColor = BrickColor.new("Black")
    handle.Material = Enum.Material.Metal
    handle.Parent = tool
    
    -- Ствол
    local barrel = Instance.new("Part")
    barrel.Name = "Barrel"
    barrel.Size = Vector3.new(0.2, 0.2, 1.2)
    barrel.BrickColor = BrickColor.new("Dark stone grey")
    barrel.Material = Enum.Material.Metal
    barrel.Parent = tool
    
    -- Курок
    local trigger = Instance.new("Part")
    trigger.Name = "Trigger"
    trigger.Size = Vector3.new(0.1, 0.15, 0.1)
    trigger.BrickColor = BrickColor.new("Really black")
    trigger.Material = Enum.Material.Metal
    trigger.Parent = tool
    
    -- Прицел
    local sight = Instance.new("Part")
    sight.Name = "Sight"
    sight.Size = Vector3.new(0.15, 0.05, 0.15)
    sight.BrickColor = BrickColor.new("White")
    sight.Material = Enum.Material.Metal
    sight.Parent = tool
    
    -- Рукоятка
    local grip = Instance.new("Part")
    grip.Name = "Grip"
    grip.Size = Vector3.new(0.25, 0.6, 0.3)
    grip.BrickColor = BrickColor.new("Black")
    grip.Material = Enum.Material.Plastic
    grip.Parent = tool
    
    -- Соединяем все части с ручкой
    local weld1 = Instance.new("Weld")
    weld1.Part0 = handle
    weld1.Part1 = barrel
    weld1.C0 = CFrame.new(0, 0, -0.8)
    weld1.Parent = handle
    
    local weld2 = Instance.new("Weld")
    weld2.Part0 = handle
    weld2.Part1 = trigger
    weld2.C0 = CFrame.new(0, -0.1, 0.3)
    weld2.Parent = handle
    
    local weld3 = Instance.new("Weld")
    weld3.Part0 = barrel
    weld3.Part1 = sight
    weld3.C0 = CFrame.new(0, 0.1, -0.5)
    weld3.Parent = handle
    
    local weld4 = Instance.new("Weld")
    weld4.Part0 = handle
    weld4.Part1 = grip
    weld4.C0 = CFrame.new(0, -0.4, 0)
    weld4.Parent = handle
    
    -- ФИКС ПОЗИЦИИ В РУКЕ
    tool.GripPos = Vector3.new(0.1, -0.2, 0.3)
    tool.GripForward = Vector3.new(0, 0, -1)
    tool.GripRight = Vector3.new(1, 0, 0)
    tool.GripUp = Vector3.new(0, 1, 0)
    
    -- Анимация выстрела с уроном
    tool.Activated:Connect(function()
        -- Анимация отдачи
        local originalCFrame = handle.CFrame
        
        local recoilTween = TweenService:Create(handle, TweenInfo.new(0.08, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            CFrame = originalCFrame * CFrame.new(0, 0.05, 0.1) * CFrame.Angles(math.rad(-5), 0, 0)
        })
        recoilTween:Play()
        
        -- ОГОНЬ из дула
        local fire = Instance.new("Part")
        fire.Name = "MuzzleFlash"
        fire.Size = Vector3.new(0.4, 0.4, 0.4)
        fire.BrickColor = BrickColor.new("Bright orange")
        fire.Material = Enum.Material.Neon
        fire.Anchored = true
        fire.CanCollide = false
        fire.Parent = workspace
        
        -- Позиция огня в дуле (стреляем ВПЕРЁД)
        local muzzlePos = barrel.Position + barrel.CFrame.LookVector * 1.8
        fire.CFrame = CFrame.new(muzzlePos)
        
        -- Эффект частиц для огня
        local particle = Instance.new("ParticleEmitter")
        particle.Texture = "rbxassetid://243664672"
        particle.Lifetime = NumberRange.new(0.2, 0.4)
        particle.Rate = 100
        particle.Speed = NumberRange.new(8, 15)
        particle.SpreadAngle = Vector2.new(25, 25)
        particle.Parent = fire
        
        -- Свет от выстрела
        local light = Instance.new("PointLight")
        light.Brightness = 8
        light.Range = 10
        light.Color = Color3.new(1, 0.3, 0)
        light.Parent = fire
        
        -- УРОН ИГРОКАМ
        local rayOrigin = barrel.Position
        local rayDirection = barrel.CFrame.LookVector * 1000
        
        local raycastParams = RaycastParams.new()
        raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
        raycastParams.FilterDescendantsInstances = {character}
        
        local raycastResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
        
        if raycastResult then
            local hit = raycastResult.Instance
            local model = hit:FindFirstAncestorOfClass("Model")
            if model then
                local humanoid = model:FindFirstChild("Humanoid")
                if humanoid and model ~= character then
                    humanoid.Health = humanoid.Health - 10
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "🔫 Попадание!", 
                        Text = "Нанесено 10 урона " .. model.Name,
                        Duration = 3
                    })
                end
            end
        end
        
        -- Возврат в исходное положение
        wait(0.08)
        local returnTween = TweenService:Create(handle, TweenInfo.new(0.15, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            CFrame = originalCFrame
        })
        returnTween:Play()
        
        -- Удаляем огонь
        wait(0.5)
        fire:Destroy()
    end)
    
    game.StarterGui:SetCore("SendNotification", {
        Title = "🔫 Пистолет", 
        Text = "Добавлен! Стреляй в игроков (-10 HP)",
        Duration = 4
    })
end

-- НОЖ (для куб-аватара с очень длинной ручкой)
local function giveKnife()
    local character = player.Character
    if not character then return end
    
    local tool = Instance.new("Tool")
    tool.Name = "Нож"
    tool.Parent = character
    
    -- Ручка ножа (очень длинная для куб-аватара)
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(0.3, 0.3, 2.0) -- Очень длинная ручка для куба
    handle.BrickColor = BrickColor.new("Brown")
    handle.Material = Enum.Material.Wood
    handle.Parent = tool
    
    -- Лезвие ножа
    local blade = Instance.new("Part")
    blade.Name = "Blade"
    blade.Size = Vector3.new(0.4, 0.1, 1.5) -- Увеличил лезвие
    blade.BrickColor = BrickColor.new("Medium stone grey")
    blade.Material = Enum.Material.Metal
    blade.Parent = tool
    
    -- Соединяем лезвие с ручкой
    local weld = Instance.new("Weld")
    weld.Part0 = handle
    weld.Part1 = blade
    weld.C0 = CFrame.new(0, 0, -1.8) -- Позиция для очень длинной ручки
    weld.Parent = handle
    
    -- Хват для куб-аватара (настроил позицию)
    tool.GripPos = Vector3.new(0, 0, 0.5) -- Сдвинул хват ближе к центру
    tool.GripForward = Vector3.new(0, 0, -1)
    tool.GripRight = Vector3.new(1, 0, 0)
    tool.GripUp = Vector3.new(0, 1, 0)
    
    -- Анимация замаха с уроном 30 HP
    tool.Activated:Connect(function()
        -- Сохраняем оригинальное положение
        local originalCFrame = handle.CFrame
        
        -- Анимация замаха
        for i = 1, 2 do
            -- Взмах вперед
            local swingForward = TweenService:Create(handle, TweenInfo.new(0.1, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
                CFrame = originalCFrame * CFrame.new(0, 0, -1.0) -- Увеличил амплитуду
            })
            
            -- Возврат
            local swingBack = TweenService:Create(handle, TweenInfo.new(0.1, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
                CFrame = originalCFrame
            })
            
            swingForward:Play()
            wait(0.1)
            swingBack:Play()
            wait(0.1)
        end
        
        -- УРОН 30 HP ближайшему игроку
        local closestPlayer = nil
        local closestDistance = 15 -- Увеличил дистанцию для длинного ножа
        
        for _, targetPlayer in pairs(Players:GetPlayers()) do
            if targetPlayer ~= player and targetPlayer.Character then
                local targetChar = targetPlayer.Character
                local humanoid = targetChar:FindFirstChild("Humanoid")
                local head = targetChar:FindFirstChild("Head")
                
                if humanoid and head then
                    local distance = (head.Position - handle.Position).Magnitude
                    if distance < closestDistance then
                        closestDistance = distance
                        closestPlayer = targetPlayer
                    end
                end
            end
        end
        
        -- Наносим урон ближайшему игроку
        if closestPlayer and closestPlayer.Character then
            local humanoid = closestPlayer.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = humanoid.Health - 30
                game.StarterGui:SetCore("SendNotification", {
                    Title = "🔪 Нож", 
                    Text = "Нанесено 30 урона " .. closestPlayer.Name,
                    Duration = 3
                })
            end
        end
    end)
    
    game.StarterGui:SetCore("SendNotification", {
        Title = "🔪 Нож", 
        Text = "Добавлен! Наносит 30 урона (версия для куб-аватара)",
        Duration = 3
    })
end

-- БАЛОНЧИК RAID (спрей с белым дымом на 10 секунд)
local function giveRaidSpray()
    local character = player.Character
    if not character then return end
    
    local tool = Instance.new("Tool")
    tool.Name = "RaidSpray"
    tool.Parent = character
    
    -- Основной баллончик (как у ножа по размеру)
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(0.3, 0.3, 2.0)
    handle.BrickColor = BrickColor.new("Bright red")
    handle.Material = Enum.Material.Plastic
    handle.Parent = tool
    
    -- Верхняя часть (распылитель)
    local sprayer = Instance.new("Part")
    sprayer.Name = "Sprayer"
    sprayer.Size = Vector3.new(0.2, 0.2, 0.4)
    sprayer.BrickColor = BrickColor.new("Really black")
    sprayer.Material = Enum.Material.Plastic
    sprayer.Parent = tool
    
    -- Соединяем распылитель с баллончиком
    local weld = Instance.new("Weld")
    weld.Part0 = handle
    weld.Part1 = sprayer
    weld.C0 = CFrame.new(0, 0, -1.2)
    weld.Parent = handle
    
    -- Хват для куб-аватара
    tool.GripPos = Vector3.new(0, 0, 0.5)
    tool.GripForward = Vector3.new(0, 0, -1)
    tool.GripRight = Vector3.new(1, 0, 0)
    tool.GripUp = Vector3.new(0, 1, 0)
    
    -- Переменная для отслеживания активного спрея
    local sprayActive = false
    local currentSpray = nil
    
    -- Анимация распыления
    tool.Activated:Connect(function()
        if sprayActive then
            -- Если спрей уже активен, выключаем его
            if currentSpray then
                currentSpray:Destroy()
                currentSpray = nil
            end
            sprayActive = false
            game.StarterGui:SetCore("SendNotification", {
                Title = "🦟 Raid Spray", 
                Text = "Спрей выключен",
                Duration = 2
            })
            return
        end
        
        -- Включаем спрей
        sprayActive = true
        
        -- Анимация нажатия
        local originalCFrame = handle.CFrame
        
        local pressTween = TweenService:Create(handle, TweenInfo.new(0.1, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            CFrame = originalCFrame * CFrame.new(0, 0, -0.3)
        })
        pressTween:Play()
        
        -- ЭФФЕКТ БЕЛОГО ДЫМА
        local spray = Instance.new("Part")
        spray.Name = "RaidSprayEffect"
        spray.Size = Vector3.new(4, 4, 6)
        spray.BrickColor = BrickColor.new("White")
        spray.Material = Enum.Material.Neon
        spray.Transparency = 0.8
        spray.Anchored = true
        spray.CanCollide = false
        spray.Parent = workspace
        
        currentSpray = spray
        
        -- Эффект частиц для БЕЛОГО дыма
        local particle = Instance.new("ParticleEmitter")
        particle.Texture = "rbxassetid://243664672"
        particle.Lifetime = NumberRange.new(1.0, 2.0)
        particle.Rate = 150
        particle.Speed = NumberRange.new(3, 8)
        particle.SpreadAngle = Vector2.new(45, 45)
        particle.Color = ColorSequence.new(Color3.new(1, 1, 1))
        particle.Transparency = NumberSequence.new(0.3, 0.8)
        particle.Size = NumberSequence.new(0.5, 2.0)
        particle.Parent = spray
        
        -- Постоянное обновление позиции дыма
        local smokeConnection
        smokeConnection = RunService.Heartbeat:Connect(function()
            if spray and spray.Parent and sprayActive then
                local smokePos = sprayer.Position + sprayer.CFrame.LookVector * -3
                spray.CFrame = CFrame.new(smokePos) * CFrame.Angles(0, math.rad(180), 0)
            else
                smokeConnection:Disconnect()
            end
        end)
        
        wait(0.2)
        local returnTween = TweenService:Create(handle, TweenInfo.new(0.2, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            CFrame = originalCFrame
        })
        returnTween:Play()
        
        game.StarterGui:SetCore("SendNotification", {
            Title = "🦟 Raid Spray", 
            Text = "Спрей активирован! Дым на 10 секунд",
            Duration = 3
        })
        
        -- АВТОМАТИЧЕСКОЕ ВЫКЛЮЧЕНИЕ ЧЕРЕЗ 10 СЕКУНД
        wait(10)
        if sprayActive then
            sprayActive = false
            if currentSpray then
                currentSpray:Destroy()
                currentSpray = nil
            end
            game.StarterGui:SetCore("SendNotification", {
                Title = "🦟 Raid Spray", 
                Text = "Спрей автоматически выключен",
                Duration = 2
            })
        end
    end)
    
    game.StarterGui:SetCore("SendNotification", {
        Title = "🦟 Raid Spray", 
        Text = "Добавлен! Нажми для активации белого дыма на 10 сек",
        Duration = 4
    })
end

-- ДОБАВЛЯЕМ ВСЕ ФУНКЦИИ В ПАНЕЛЬ
createBtn("FLY ВВЕРХ", 10, toggleFly)
createBtn("ЛЕТЕТЬ ВПЕРЁД", 65, autoFly)
createBtn("СКОРОСТЬ 300", 120, function()
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.WalkSpeed = 300
        game.StarterGui:SetCore("SendNotification", {
            Title = "Speed", 
            Text = "Скорость 300!",
            Duration = 2
        })
    end
end)
createBtn("СКОРОСТЬ 100", 175, function()
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.WalkSpeed = 100
        game.StarterGui:SetCore("SendNotification", {
            Title = "Speed", 
            Text = "Скорость 100!",
            Duration = 2
        })
    end
end)
createBtn("НОКЛИП ВКЛ/ВЫКЛ", 230, toggleNoclip)
createBtn("ТЕЛЕПОРТ ВВЕРХ", 285, function()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = character.HumanoidRootPart.CFrame + Vector3.new(0, 100, 0)
        game.StarterGui:SetCore("SendNotification", {
            Title = "Teleport", 
            Text = "Телепорт на 100 вверх!",
            Duration = 3
        })
    end
end)
createBtn("ТЕЛЕПОРТ ВПЕРЁД", 340, function()
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = character.HumanoidRootPart.CFrame + character.HumanoidRootPart.CFrame.LookVector * 50
        game.StarterGui:SetCore("SendNotification", {
            Title = "Teleport", 
            Text = "Телепорт на 50 вперёд!",
            Duration = 3
        })
    end
end)
createBtn("РЕСЕТ СКОРОСТИ", 395, function()
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.WalkSpeed = 16
        game.StarterGui:SetCore("SendNotification", {
            Title = "Speed", 
            Text = "Скорость сброшена!",
            Duration = 2
        })
    end
end)
createBtn("СИЛА ПРЫЖКА x3", 450, function()
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        character.Humanoid.JumpPower = 
