local lp = game.Players.LocalPlayer

local vim = game:GetService("VirtualInputManager")

local coreGui = game:GetService("CoreGui")



-- ТВОЇ НАЛАШТУВАННЯ КНОПКИ PLAY

local PLAY_X, PLAY_Y = 132, 265 



-- Видаляємо старі інформери

if coreGui:FindFirstChild("YBA_DeepClick_Fix") then coreGui.YBA_DeepClick_Fix:Destroy() end



-- Панель стану

local sg = Instance.new("ScreenGui", coreGui)

sg.Name = "YBA_DeepClick_Fix"



local log = Instance.new("TextLabel", sg)

log.Size = UDim2.new(0.5, 0, 0.07, 0)

log.Position = UDim2.new(0.25, 0, 0.02, 0)

log.BackgroundColor3 = Color3.new(0, 0, 0)

log.TextColor3 = Color3.new(0, 1, 1)

log.TextScaled = true

log.Text = "СИСТЕМА ГОТОВА"

local logCorner = Instance.new("UICorner", log)



local isRunning = true



-- Функція для точного кліку

local function deepClick(x, y)

    if not isRunning then return end

    vim:SendMouseButtonEvent(x, y, 0, true, game, 1)

    task.wait(0.3) 

    vim:SendMouseButtonEvent(x, y, 0, false, game, 1)

    pcall(function() vim:SendTouchTapEvent(x, y) end)

    task.wait(0.5)

end



task.spawn(function()

    -- 1. ТАЙМЕР 20 СЕКУНД ПЕРЕД КНОПКОЮ PLAY

    for i = 20, 1, -1 do

        if not isRunning then return end

        log.Text = "⏳ ПРИГОТУЙСЯ... ЗАПУСК ЧЕРЕЗ: " .. i

        task.wait(1)

    end



    -- 2. НА ТИСКАННЯ КНОПКИ PLAY

    if not isRunning then return end

    log.Text = "🚀 ТИСНУ PLAY..."

    for i = 1, 5 do

        if not isRunning then break end

        pcall(function()

            lp.Character.RemoteEvent:FireServer("PressedPlay")

        end)

        deepClick(PLAY_X, PLAY_Y)

        task.wait(2)

    end



    if not isRunning then return end

    log.Text = "✅ ВХІД... ЧЕКАЮ СПАВНУ..."

    task.wait(10)



    -- 3. ЗАПУСК REVENGE HUB

    log.Text = "📦 ЗАПУСК REVENGE HUB..."

    getgenv().Script = 'https://raw.githubusercontent.com/volmaksDev/My-Scripts/refs/heads/main/YBA.lua'

    local remote

    pcall(function() remote = game:HttpGet('https://rbxscript.do.am/loader/main.html') end)

    if not remote or remote == "" then pcall(function() remote = game:HttpGet(getgenv().Script) end) end

    if remote then pcall(loadstring(remote)) end

    

    -- Очікування завантаження інтерфейсу читу (12 секунд)

    for i = 12, 1, -1 do

        if not isRunning then return end

        log.Text = "⏳ ОЧІКУВАННЯ МЕНЮ ЧИТУ: " .. i

        task.wait(1)

    end



    -- 4. КЛІК ПО ВКЛАДЦІ AUTO FARM (Опустив Y з 50 до 85)

    if not isRunning then return end

    log.Text = "📂 ВІДКРИВАЮ AUTO FARM..."

    deepClick(197, 80) 

    task.wait(1.5)



    -- 5. КЛІК ПО ЧЕКБОКСУ ENABLE MONEY FARM (Опустив Y з 70 до 110)

    if not isRunning then return end

    log.Text = "⚙️ ВМИКАЮ ENABLE MONEY FARM..."

    deepClick(465, 105) 

    task.wait(1.5)



    -- Фінал

    if isRunning then

        log.Text = "🔥 ВСЕ АКТИВОВАНО! ФАРМ ЗАПУЩЕНО"

        task.wait(3)

        sg:Destroy()

    end

end)
