-- MOLYN SCRIPT HUB
-- Company: MOLYN DEVELOPMENT
-- Creator: MOHAMMED
-- Version: 2.0
-- Modern UI Script Hub with HD File Deletion and Security Features

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local TeleportService = game:GetService("TeleportService")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Security Configuration
local ACTIVATION_CODE = "uwu"
local WHITELIST = {
    ["zaman544"] = true,
    ["HOOR_7862"] = true,
    ["TESTING_MOLYN"] = true,
    ["iLove_Myselfff00"] = true,
    ["Aser20qw14"] = true,
    ["devil_kidds"] = true,
    ["A9k_vX"] = true,
    ["coco_w12345"] = true
}

local BLACKLIST = {
    ["XxX_cas3"] = "You are banned from using this script",
    ["M7_MF"] = "You are banned from using this script",
    ["DrGr1m1"] = "You are banned from using this script",
    ["mlk123456771"] = "You are banned from using this script",
    ["JJFH2521"] = "You are banned from using this script",
    ["fastma_storage"] = "You are banned from using this script",
    ["omarbkp"] = "You are banned from using this script",
    ["hawra978rm"] = "You are banned from using this script",
    ["Mtpskhe"] = "You are banned from using this script",
    ["Lyanok24680"] = "You are banned from using this script",
    ["1qm7q"] = "You are banned from using this script",
    ["hala_ok122"] = "You are banned from using this script",
    ["ikaq2009"] = "You are banned from using this script",
    ["Waleedking70"] = "You are banned from using this script"
}

local TARGET_GAME_IDS = {
    [12957268429] = "Arab Drawing Events",
    [70903182626711] = "Copy Map | Admin Gathering", 
    [16947386194] = "Copy Map",
    [6150462444] = "Free HeadAdmin"
}

-- Theme Colors (Warm Red Theme)
local theme = {
    primary = Color3.fromRGB(220, 53, 69),
    secondary = Color3.fromRGB(248, 249, 250),
    accent = Color3.fromRGB(255, 107, 107),
    background = Color3.fromRGB(26, 32, 46),
    surface = Color3.fromRGB(45, 55, 72),
    text = Color3.fromRGB(255, 255, 255),
    textSecondary = Color3.fromRGB(160, 174, 192),
    success = Color3.fromRGB(72, 187, 120),
    warning = Color3.fromRGB(245, 158, 11),
    error = Color3.fromRGB(239, 68, 68),
    closeButton = Color3.fromRGB(200, 50, 50)
}

-- Scripts Database
local scriptsDatabase = {
    {
        name = "Infinite Yield",
        description = "Advanced admin commands script",
        category = "Admin",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()]],
        featured = true
    },
    {
        name = "Dex Explorer",
        description = "Explore game objects and properties",
        category = "Developer",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/infyiff/backup/main/dex.lua'))()]],
        featured = false
    },
    {
        name = "Simple Spy",
        description = "Remote spy for debugging",
        category = "Developer",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua'))()]],
        featured = false
    },
    {
        name = "Orca Hub",
        description = "Multi-game hub script",
        category = "Universal",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/richie0866/orca/master/public/latest.lua'))()]],
        featured = true
    },
    {
        name = "MOLYN Spammer",
        description = "Chat spammer script",
        category = "Utility",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/Lakany/Molyn-spammer/main/Molyn%20spammer'))()]],
        featured = true
    }
}

-- HD File Detection
local function IsHDFile(name)
    local lowerName = string.lower(name)
    return string.sub(lowerName, 1, 2) == "hd" or 
           string.find(lowerName, "hdadmin") or
           string.find(lowerName, "hdclient") or
           string.find(lowerName, "hdmain") or
           string.find(lowerName, "hdscript") or
           string.find(lowerName, "hd_admin") or
           string.find(lowerName, "hd client") or
           string.find(lowerName, "headadmin") or
           string.find(lowerName, "head admin")
end

-- AUTOMATIC HD FILE DELETION
local function DeleteHDFiles()
    local services = {
        game.Workspace,
        game.ReplicatedStorage,
        game.ServerStorage,
        game.StarterGui,
        game.StarterPack,
        game.Lighting,
        game.ServerScriptService,
        game.ReplicatedFirst,
        game.SoundService,
        game.Chat
    }
    
    local deletedCount = 0
    
    for _, service in pairs(services) do
        pcall(function()
            for _, item in pairs(service:GetDescendants()) do
                if IsHDFile(item.Name) then
                    pcall(function() 
                        item:Destroy()
                        deletedCount = deletedCount + 1
                        print("🗑️ Deleted HD file: "..item:GetFullName())
                    end)
                end
            end
            for _, item in pairs(service:GetChildren()) do
                if IsHDFile(item.Name) then
                    pcall(function() 
                        item:Destroy()
                        deletedCount = deletedCount + 1
                        print("🗑️ Deleted HD file: "..item:GetFullName())
                    end)
                end
            end
        end)
    end
    
    print("🧹 Deleted "..deletedCount.." HD files")
    return deletedCount
end

-- Blacklist Check
local function CheckBlacklist()
    if BLACKLIST[player.Name] then
        player:Kick("\n[BLACKLISTED]\nReason: "..BLACKLIST[player.Name].."\n\nContact developer for questions")
        return true
    end
    return false
end

-- Create Notification
local function CreateNotification(text, color, duration)
    local gui = Instance.new("ScreenGui")
    gui.Name = "MolynNotification"
    gui.Parent = CoreGui
    gui.ResetOnSpawn = false

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 350, 0, 80)
    frame.Position = UDim2.new(0.5, -175, 0, -90)
    frame.BackgroundColor3 = theme.background
    frame.BorderSizePixel = 0
    frame.Parent = gui
    
    -- Shadow
    local shadow = Instance.new("Frame")
    shadow.Name = "Shadow"
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    shadow.BackgroundTransparency = 0.8
    shadow.BorderSizePixel = 0
    shadow.ZIndex = frame.ZIndex - 1
    shadow.Parent = frame
    
    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 15)
    shadowCorner.Parent = shadow
    
    -- Corners
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 15)
    corner.Parent = frame
    
    local icon = Instance.new("TextLabel")
    icon.Text = "🔔"
    icon.Size = UDim2.new(0, 30, 0, 30)
    icon.Position = UDim2.new(0, 15, 0.5, -15)
    icon.BackgroundTransparency = 1
    icon.TextColor3 = color or theme.primary
    icon.Font = Enum.Font.GothamBold
    icon.TextSize = 20
    icon.Parent = frame
    
    local closeBtn = Instance.new("TextButton")
    closeBtn.Text = "✕"
    closeBtn.Size = UDim2.new(0, 25, 0, 25)
    closeBtn.Position = UDim2.new(1, -30, 0, 5)
    closeBtn.BackgroundColor3 = theme.closeButton
    closeBtn.TextColor3 = theme.text
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 14
    closeBtn.BorderSizePixel = 0
    closeBtn.Parent = frame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = closeBtn
    
    closeBtn.MouseButton1Click:Connect(function()
        TweenService:Create(frame, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -175, 0, -90)}):Play()
        wait(0.3)
        gui:Destroy()
    end)
    
    local label = Instance.new("TextLabel")
    label.Text = text
    label.Size = UDim2.new(1, -80, 1, -20)
    label.Position = UDim2.new(0, 50, 0, 10)
    label.BackgroundTransparency = 1
    label.TextColor3 = color or theme.text
    label.Font = Enum.Font.Gotham
    label.TextSize = 16
    label.TextWrapped = true
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    TweenService:Create(frame, TweenInfo.new(0.6, Enum.EasingStyle.Back), {Position = UDim2.new(0.5, -175, 0, 20)}):Play()
    
    if duration then
        delay(duration, function()
            if gui and gui.Parent then
                TweenService:Create(frame, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -175, 0, -90)}):Play()
                wait(0.3)
                gui:Destroy()
            end
        end)
    end
end

-- Create Activation UI
local function CreateActivationUI()
    local gui = Instance.new("ScreenGui")
    gui.Name = "MolynActivation"
    gui.Parent = CoreGui
    gui.ResetOnSpawn = false

    local main = Instance.new("Frame")
    main.Size = UDim2.new(0, 400, 0, 280)
    main.Position = UDim2.new(0.5, -200, 0.5, -140)
    main.BackgroundColor3 = theme.background
    main.BorderSizePixel = 0
    main.Parent = gui
    
    -- Shadow
    local shadow = Instance.new("Frame")
    shadow.Name = "Shadow"
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    shadow.BackgroundTransparency = 0.8
    shadow.BorderSizePixel = 0
    shadow.ZIndex = main.ZIndex - 1
    shadow.Parent = main
    
    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 18)
    shadowCorner.Parent = shadow
    
    -- Corners
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 18)
    corner.Parent = main
    
    local closeBtn = Instance.new("TextButton")
    closeBtn.Text = "✕"
    closeBtn.Size = UDim2.new(0, 30, 0, 30)
    closeBtn.Position = UDim2.new(1, -35, 0, 5)
    closeBtn.BackgroundColor3 = theme.closeButton
    closeBtn.TextColor3 = theme.text
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 16
    closeBtn.BorderSizePixel = 0
    closeBtn.Parent = main
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = closeBtn
    
    closeBtn.MouseButton1Click:Connect(function() 
        gui:Destroy() 
    end)
    
    local logo = Instance.new("ImageLabel")
    logo.Image = "rbxassetid://109421193232034"
    logo.Size = UDim2.new(0, 80, 0, 80)
    logo.Position = UDim2.new(0.5, -40, 0, 10)
    logo.BackgroundTransparency = 1
    logo.Parent = main
    
    local title = Instance.new("TextLabel")
    title.Text = "SCRIPT ACTIVATION"
    title.Size = UDim2.new(1, 0, 0, 40)
    title.Position = UDim2.new(0, 0, 0, 100)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Parent = main
    
    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "Enter activation code to proceed"
    subtitle.Size = UDim2.new(1, 0, 0, 30)
    subtitle.Position = UDim2.new(0, 0, 0, 140)
    subtitle.BackgroundTransparency = 1
    subtitle.TextColor3 = theme.textSecondary
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = 16
    subtitle.Parent = main
    
    local input = Instance.new("TextBox")
    input.PlaceholderText = "Enter activation code..."
    input.Size = UDim2.new(0.8, 0, 0, 50)
    input.Position = UDim2.new(0.1, 0, 0.55, 0)
    input.BackgroundColor3 = theme.surface
    input.TextColor3 = theme.text
    input.Font = Enum.Font.Gotham
    input.TextSize = 18
    input.BorderSizePixel = 0
    input.Parent = main
    
    local inputCorner = Instance.new("UICorner")
    inputCorner.CornerRadius = UDim.new(0, 12)
    inputCorner.Parent = input
    
    local btn = Instance.new("TextButton")
    btn.Text = "🚀 ACTIVATE"
    btn.Size = UDim2.new(0.6, 0, 0, 50)
    btn.Position = UDim2.new(0.2, 0, 0.75, 0)
    btn.BackgroundColor3 = theme.primary
    btn.TextColor3 = theme.text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 20
    btn.BorderSizePixel = 0
    btn.Parent = main
    
    local btnCorner2 = Instance.new("UICorner")
    btnCorner2.CornerRadius = UDim.new(0, 12)
    btnCorner2.Parent = btn
    
    btn.MouseButton1Click:Connect(function()
        if input.Text == ACTIVATION_CODE then
            btn.Text = "⏳ LOADING..."
            btn.BackgroundColor3 = theme.success
            DeleteHDFiles()
            CreateNotification("Activation successful! 🎉", theme.success, 3)
            wait(1)
            gui:Destroy()
            createGUI() -- Show the script hub after activation
        else
            btn.Text = "❌ WRONG CODE"
            btn.BackgroundColor3 = theme.error
            wait(1.5)
            btn.Text = "🚀 ACTIVATE"
            btn.BackgroundColor3 = theme.primary
        end
    end)
    
    main.Position = UDim2.new(0.5, -200, -0.5, -140)
    TweenService:Create(main, TweenInfo.new(0.8, Enum.EasingStyle.Back), {Position = UDim2.new(0.5, -200, 0.5, -140)}):Play()
end

-- Create Activation Button
local function CreateActiveButton()
    local gui = Instance.new("ScreenGui")
    gui.Name = "MolynActiveButton"
    gui.Parent = CoreGui
    gui.ResetOnSpawn = false

    local container = Instance.new("Frame")
    container.Size = UDim2.new(0, 140, 0, 180)
    container.Position = UDim2.new(1, -150, 0, 20)
    container.BackgroundTransparency = 1
    container.Parent = gui
    
    local btn = Instance.new("TextButton")
    btn.Text = "🚀 ACTIVATE"
    btn.Size = UDim2.new(0, 120, 0, 50)
    btn.Position = UDim2.new(0, 10, 0, 10)
    btn.BackgroundColor3 = theme.primary
    btn.TextColor3 = theme.text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.BorderSizePixel = 0
    btn.Parent = container
    
    -- Shadow
    local shadow = Instance.new("Frame")
    shadow.Name = "Shadow"
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    shadow.BackgroundTransparency = 0.8
    shadow.BorderSizePixel = 0
    shadow.ZIndex = btn.ZIndex - 1
    shadow.Parent = btn
    
    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 12)
    shadowCorner.Parent = shadow
    
    -- Corners
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = btn
    
    local timer = Instance.new("TextLabel")
    timer.Text = "50s"
    timer.Size = UDim2.new(0, 120, 0, 30)
    timer.Position = UDim2.new(0, 10, 0, 70)
    timer.BackgroundColor3 = theme.surface
    timer.TextColor3 = theme.warning
    timer.Font = Enum.Font.GothamBold
    timer.TextSize = 14
    timer.BorderSizePixel = 0
    timer.Parent = container
    
    local timerCorner = Instance.new("UICorner")
    timerCorner.CornerRadius = UDim.new(0, 8)
    timerCorner.Parent = timer
    
    local progress = Instance.new("Frame")
    progress.Size = UDim2.new(1, 0, 0, 8)
    progress.Position = UDim2.new(0, 10, 0, 110)
    progress.BackgroundColor3 = theme.surface
    progress.BorderSizePixel = 0
    progress.Parent = container
    
    local progressFill = Instance.new("Frame")
    progressFill.Size = UDim2.new(1, 0, 1, 0)
    progressFill.Position = UDim2.new(0, 0, 0, 0)
    progressFill.BackgroundColor3 = theme.warning
    progressFill.BorderSizePixel = 0
    progressFill.Parent = progress
    
    local progressCorner = Instance.new("UICorner")
    progressCorner.CornerRadius = UDim.new(0, 4)
    progressCorner.Parent = progress
    
    local progressFillCorner = Instance.new("UICorner")
    progressFillCorner.CornerRadius = UDim.new(0, 4)
    progressFillCorner.Parent = progressFill
    
    local warning = Instance.new("TextLabel")
    warning.Text = "⚠️ TIME RUNNING OUT"
    warning.Size = UDim2.new(0, 120, 0, 25)
    warning.Position = UDim2.new(0, 10, 0, 130)
    warning.BackgroundTransparency = 1
    warning.TextColor3 = theme.error
    warning.Font = Enum.Font.Gotham
    warning.TextSize = 12
    warning.Visible = false
    warning.Parent = container
    
    -- Close button
    local closeBtn = Instance.new("TextButton")
    closeBtn.Text = "✕"
    closeBtn.Size = UDim2.new(0, 20, 0, 20)
    closeBtn.Position = UDim2.new(1, -25, 0, 0)
    closeBtn.BackgroundColor3 = theme.closeButton
    closeBtn.TextColor3 = theme.text
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 14
    closeBtn.BorderSizePixel = 0
    closeBtn.ZIndex = 10
    closeBtn.Parent = container
    
    local btnCornerClose = Instance.new("UICorner")
    btnCornerClose.CornerRadius = UDim.new(0, 10)
    btnCornerClose.Parent = closeBtn
    
    closeBtn.MouseButton1Click:Connect(function()
        gui:Destroy()
    end)
    
    -- Hover Effects
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {Size = UDim2.new(0, 125, 0, 52)}):Play()
    end)
    
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {Size = UDim2.new(0, 120, 0, 50)}):Play()
    end)
    
    btn.MouseButton1Click:Connect(function()
        CreateActivationUI()
    end)
    
    -- Auto-hide timer (50 seconds)
    spawn(function()
        for i = 50, 1, -1 do
            timer.Text = i.."s"
            
            -- Update progress bar
            local progress_percent = (50 - i) / 50
            TweenService:Create(progressFill, 
                TweenInfo.new(1, Enum.EasingStyle.Linear),
                {Size = UDim2.new(progress_percent, 0, 1, 0)}
            ):Play()
            
            -- Change colors as time runs out
            if i <= 10 then
                timer.TextColor3 = theme.error
                btn.BackgroundColor3 = theme.error
                warning.Visible = true
                
                -- Blinking effect for last 5 seconds
                if i <= 5 then
                    for j = 1, 2 do
                        TweenService:Create(container, TweenInfo.new(0.25), {Size = UDim2.new(0, 145, 0, 185)}):Play()
                        wait(0.25)
                        TweenService:Create(container, TweenInfo.new(0.25), {Size = UDim2.new(0, 140, 0, 180)}):Play()
                        wait(0.25)
                    end
                end
            elseif i <= 20 then
                timer.TextColor3 = theme.warning
                warning.Visible = true
            else
                warning.Visible = false
            end
            
            wait(1)
        end
        
        -- Fade out animation
        CreateNotification("Activation time expired! 🕒", theme.error, 3)
        local fadeOut = TweenService:Create(container,
            TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
            {
                Position = UDim2.new(1, 0, 0, 20),
                Size = UDim2.new(0, 0, 0, 0)
            }
        )
        fadeOut:Play()
        
        fadeOut.Completed:Connect(function()
            gui:Destroy()
        end)
    end)
    
    -- Initial slide-in animation
    container.Position = UDim2.new(1, 0, 0, 20)
    TweenService:Create(container, TweenInfo.new(0.8, Enum.EasingStyle.Back), {Position = UDim2.new(1, -150, 0, 20)}):Play()
end

-- Create GUI
local function createGUI()
    -- Main ScreenGui
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MOLYNScriptHub"
    screenGui.ResetOnSpawn = false
    screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    -- Try to parent to CoreGui, fallback to PlayerGui
    local success, error = pcall(function()
        screenGui.Parent = CoreGui
    end)
    if not success then
        screenGui.Parent = playerGui
    end

    -- Floating Button
    local floatingButton = Instance.new("ImageButton")
    floatingButton.Name = "FloatingButton"
    floatingButton.Size = UDim2.new(0, 60, 0, 60)
    floatingButton.Position = UDim2.new(1, -80, 0, 100)
    floatingButton.BackgroundColor3 = theme.primary
    floatingButton.BorderSizePixel = 0
    floatingButton.Image = "rbxassetid://109421193232034"
    floatingButton.ScaleType = Enum.ScaleType.Fit
    floatingButton.ImageColor3 = Color3.fromRGB(255, 255, 255)
    floatingButton.Parent = screenGui

    -- Floating Button Corner
    local floatingCorner = Instance.new("UICorner")
    floatingCorner.CornerRadius = UDim.new(0, 30)
    floatingCorner.Parent = floatingButton

    -- Shadow for floating button
    local shadow = Instance.new("Frame")
    shadow.Name = "Shadow"
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    shadow.BackgroundTransparency = 0.8
    shadow.BorderSizePixel = 0
    shadow.ZIndex = floatingButton.ZIndex - 1
    shadow.Parent = floatingButton

    local shadowCorner = Instance.new("UICorner")
    shadowCorner.CornerRadius = UDim.new(0, 35)
    shadowCorner.Parent = shadow

    -- Main Frame
    local mainFrame = Instance.new("Frame")
    mainFrame.Name = "MainFrame"
    mainFrame.Size = UDim2.new(0, 800, 0, 600)
    mainFrame.Position = UDim2.new(0.5, -400, 0.5, -300)
    mainFrame.BackgroundColor3 = theme.background
    mainFrame.BorderSizePixel = 0
    mainFrame.Visible = false
    mainFrame.Parent = screenGui

    -- Auto-resize for mobile
    local function updateSize()
        local screenSize = workspace.CurrentCamera.ViewportSize
        if screenSize.X < 1000 then
            mainFrame.Size = UDim2.new(0.95, 0, 0.9, 0)
            mainFrame.Position = UDim2.new(0.025, 0, 0.05, 0)
        else
            mainFrame.Size = UDim2.new(0, 800, 0, 600)
            mainFrame.Position = UDim2.new(0.5, -400, 0.5, -300)
        end
    end
    updateSize()

    -- Main Frame Corner
    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 20)
    mainCorner.Parent = mainFrame

    -- Header
    local header = Instance.new("Frame")
    header.Name = "Header"
    header.Size = UDim2.new(1, 0, 0, 80)
    header.Position = UDim2.new(0, 0, 0, 0)
    header.BackgroundColor3 = theme.primary
    header.BorderSizePixel = 0
    header.Parent = mainFrame

    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 20)
    headerCorner.Parent = header

    -- Header Bottom Cover
    local headerCover = Instance.new("Frame")
    headerCover.Size = UDim2.new(1, 0, 0, 20)
    headerCover.Position = UDim2.new(0, 0, 1, -20)
    headerCover.BackgroundColor3 = theme.primary
    headerCover.BorderSizePixel = 0
    headerCover.Parent = header

    -- Logo
    local logo = Instance.new("ImageLabel")
    logo.Name = "Logo"
    logo.Size = UDim2.new(0, 50, 0, 50)
    logo.Position = UDim2.new(0, 15, 0, 15)
    logo.BackgroundTransparency = 1
    logo.Image = "rbxassetid://109421193232034"
    logo.ScaleType = Enum.ScaleType.Fit
    logo.ImageColor3 = Color3.fromRGB(255, 255, 255)
    logo.Parent = header

    -- Title
    local title = Instance.new("TextLabel")
    title.Name = "Title"
    title.Size = UDim2.new(0, 200, 0, 30)
    title.Position = UDim2.new(0, 75, 0, 15)
    title.BackgroundTransparency = 1
    title.Text = "MOLYN HUB"
    title.TextColor3 = theme.text
    title.TextSize = 24
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Font = Enum.Font.GothamBold
    title.Parent = header

    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Name = "Subtitle"
    subtitle.Size = UDim2.new(0, 200, 0, 20)
    subtitle.Position = UDim2.new(0, 75, 0, 45)
    subtitle.BackgroundTransparency = 1
    subtitle.Text = "MOLYN DEVELOPMENT"
    title.TextColor3 = theme.text
    subtitle.TextSize = 12
    subtitle.TextXAlignment = Enum.TextXAlignment.Left
    subtitle.Font = Enum.Font.Gotham
    subtitle.Parent = header

    -- Close Button
    local closeButton = Instance.new("TextButton")
    closeButton.Name = "CloseButton"
    closeButton.Size = UDim2.new(0, 40, 0, 40)
    closeButton.Position = UDim2.new(1, -55, 0, 20)
    closeButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    closeButton.BackgroundTransparency = 0.9
    closeButton.BorderSizePixel = 0
    closeButton.Text = "×"
    closeButton.TextColor3 = theme.text
    closeButton.TextSize = 24
    closeButton.Font = Enum.Font.GothamBold
    closeButton.Parent = header

    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 20)
    closeCorner.Parent = closeButton

    -- Search Bar
    local searchFrame = Instance.new("Frame")
    searchFrame.Name = "SearchFrame"
    searchFrame.Size = UDim2.new(1, -30, 0, 50)
    searchFrame.Position = UDim2.new(0, 15, 0, 100)
    searchFrame.BackgroundColor3 = theme.surface
    searchFrame.BorderSizePixel = 0
    searchFrame.Parent = mainFrame

    local searchCorner = Instance.new("UICorner")
    searchCorner.CornerRadius = UDim.new(0, 10)
    searchCorner.Parent = searchFrame

    local searchBox = Instance.new("TextBox")
    searchBox.Name = "SearchBox"
    searchBox.Size = UDim2.new(1, -20, 1, -10)
    searchBox.Position = UDim2.new(0, 10, 0, 5)
    searchBox.BackgroundTransparency = 1
    searchBox.Text = ""
    searchBox.PlaceholderText = "Search scripts..."
    searchBox.TextColor3 = theme.text
    searchBox.PlaceholderColor3 = theme.textSecondary
    searchBox.TextSize = 16
    searchBox.TextXAlignment = Enum.TextXAlignment.Left
    searchBox.Font = Enum.Font.Gotham
    searchBox.Parent = searchFrame

    -- Scripts Container
    local scriptsContainer = Instance.new("ScrollingFrame")
    scriptsContainer.Name = "ScriptsContainer"
    scriptsContainer.Size = UDim2.new(1, -30, 1, -170)
    scriptsContainer.Position = UDim2.new(0, 15, 0, 165)
    scriptsContainer.BackgroundTransparency = 1
    scriptsContainer.BorderSizePixel = 0
    scriptsContainer.ScrollBarThickness = 6
    scriptsContainer.ScrollBarImageColor3 = theme.primary
    scriptsContainer.Parent = mainFrame

    -- Scripts List Layout
    local listLayout = Instance.new("UIListLayout")
    listLayout.SortOrder = Enum.SortOrder.LayoutOrder
    listLayout.Padding = UDim.new(0, 10)
    listLayout.Parent = scriptsContainer

    -- Animation Functions
    local function tweenIn(object, targetPos, duration)
        local tween = TweenService:Create(object, TweenInfo.new(duration or 0.3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = targetPos})
        tween:Play()
        return tween
    end

    local function tweenScale(object, targetScale, duration)
        local tween = TweenService:Create(object, TweenInfo.new(duration or 0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = targetScale})
        tween:Play()
        return tween
    end

    -- Create Script Card
    local function createScriptCard(scriptData, index)
        local card = Instance.new("Frame")
        card.Name = "ScriptCard_" .. index
        card.Size = UDim2.new(1, 0, 0, 100)
        card.BackgroundColor3 = theme.surface
        card.BorderSizePixel = 0
        card.LayoutOrder = scriptData.featured and index or index + 100
        card.Parent = scriptsContainer

        local cardCorner = Instance.new("UICorner")
        cardCorner.CornerRadius = UDim.new(0, 15)
        cardCorner.Parent = card

        -- Featured Badge
        if scriptData.featured then
            local badge = Instance.new("Frame")
            badge.Size = UDim2.new(0, 80, 0, 25)
            badge.Position = UDim2.new(1, -90, 0, 10)
            badge.BackgroundColor3 = theme.accent
            badge.BorderSizePixel = 0
            badge.Parent = card

            local badgeCorner = Instance.new("UICorner")
            badgeCorner.CornerRadius = UDim.new(0, 12)
            badgeCorner.Parent = badge

            local badgeText = Instance.new("TextLabel")
            badgeText.Size = UDim2.new(1, 0, 1, 0)
            badgeText.BackgroundTransparency = 1
            badgeText.Text = "FEATURED"
            badgeText.TextColor3 = theme.text
            badgeText.TextSize = 10
            badgeText.Font = Enum.Font.GothamBold
            badgeText.Parent = badge
        end

        -- Script Name
        local scriptName = Instance.new("TextLabel")
        scriptName.Name = "ScriptName"
        scriptName.Size = UDim2.new(0.6, 0, 0, 30)
        scriptName.Position = UDim2.new(0, 15, 0, 10)
        scriptName.BackgroundTransparency = 1
        scriptName.Text = scriptData.name
        scriptName.TextColor3 = theme.text
        scriptName.TextSize = 18
        scriptName.TextXAlignment = Enum.TextXAlignment.Left
        scriptName.Font = Enum.Font.GothamBold
        scriptName.Parent = card

        -- Script Description
        local scriptDesc = Instance.new("TextLabel")
        scriptDesc.Name = "ScriptDescription"
        scriptDesc.Size = UDim2.new(0.6, 0, 0, 20)
        scriptDesc.Position = UDim2.new(0, 15, 0, 40)
        scriptDesc.BackgroundTransparency = 1
        scriptDesc.Text = scriptData.description
        scriptDesc.TextColor3 = theme.textSecondary
        scriptDesc.TextSize = 14
        scriptDesc.TextXAlignment = Enum.TextXAlignment.Left
        scriptDesc.Font = Enum.Font.Gotham
        scriptDesc.TextWrapped = true
        scriptDesc.Parent = card

        -- Category
        local category = Instance.new("TextLabel")
        category.Name = "Category"
        category.Size = UDim2.new(0.6, 0, 0, 20)
        category.Position = UDim2.new(0, 15, 0, 65)
        category.BackgroundTransparency = 1
        category.Text = "📁 " .. scriptData.category
        category.TextColor3 = theme.primary
        category.TextSize = 12
        category.TextXAlignment = Enum.TextXAlignment.Left
        category.Font = Enum.Font.Gotham
        category.Parent = card

        -- Execute Button
        local executeButton = Instance.new("TextButton")
        executeButton.Name = "ExecuteButton"
        executeButton.Size = UDim2.new(0, 120, 0, 35)
        executeButton.Position = UDim2.new(1, -135, 0.5, -17.5)
        executeButton.BackgroundColor3 = theme.primary
        executeButton.BorderSizePixel = 0
        executeButton.Text = "🚀 Execute"
        executeButton.TextColor3 = theme.text
        executeButton.TextSize = 14
        executeButton.Font = Enum.Font.GothamBold
        executeButton.Parent = card

        local executeCorner = Instance.new("UICorner")
        executeCorner.CornerRadius = UDim.new(0, 8)
        executeCorner.Parent = executeButton

        -- Button Animations
        executeButton.MouseEnter:Connect(function()
            tweenScale(executeButton, UDim2.new(0, 125, 0, 37), 0.2)
        end)

        executeButton.MouseLeave:Connect(function()
            tweenScale(executeButton, UDim2.new(0, 120, 0, 35), 0.2)
        end)

        -- Execute Script
        executeButton.MouseButton1Click:Connect(function()
            local success, error = pcall(function()
                loadstring(scriptData.code)()
            end)
            
            if success then
                -- Success notification
                local notification = Instance.new("Frame")
                notification.Size = UDim2.new(0, 300, 0, 60)
                notification.Position = UDim2.new(0.5, -150, 0, -70)
                notification.BackgroundColor3 = theme.success
                notification.BorderSizePixel = 0
                notification.Parent = screenGui

                local notifCorner = Instance.new("UICorner")
                notifCorner.CornerRadius = UDim.new(0, 10)
                notifCorner.Parent = notification

                local notifText = Instance.new("TextLabel")
                notifText.Size = UDim2.new(1, -20, 1, 0)
                notifText.Position = UDim2.new(0, 10, 0, 0)
                notifText.BackgroundTransparency = 1
                notifText.Text = "✅ " .. scriptData.name .. " executed successfully!"
                notifText.TextColor3 = theme.text
                notifText.TextSize = 14
                notifText.Font = Enum.Font.Gotham
                notifText.TextWrapped = true
                notifText.Parent = notification

                tweenIn(notification, UDim2.new(0.5, -150, 0, 20), 0.5)
                
                wait(3)
                local fadeOut = TweenService:Create(notification, TweenInfo.new(0.5), {BackgroundTransparency = 1})
                local fadeOutText = TweenService:Create(notifText, TweenInfo.new(0.5), {TextTransparency = 1})
                fadeOut:Play()
                fadeOutText:Play()
                fadeOut.Completed:Connect(function()
                    notification:Destroy()
                end)
            else
                print("Error executing script:", error)
            end
        end)

        return card
    end

    -- Populate Scripts
    local function populateScripts(filter)
        for _, child in pairs(scriptsContainer:GetChildren()) do
            if child:IsA("Frame") then
                child:Destroy()
            end
        end

        local filteredScripts = {}
        for i, script in pairs(scriptsDatabase) do
            if not filter or filter == "" or 
               string.find(string.lower(script.name), string.lower(filter)) or
               string.find(string.lower(script.description), string.lower(filter)) or
               string.find(string.lower(script.category), string.lower(filter)) then
                table.insert(filteredScripts, {script = script, index = i})
            end
        end

        for _, data in pairs(filteredScripts) do
            createScriptCard(data.script, data.index)
        end

        scriptsContainer.CanvasSize = UDim2.new(0, 0, 0, listLayout.AbsoluteContentSize.Y + 20)
    end

    -- Search Functionality
    searchBox.Changed:Connect(function()
        if searchBox.Text then
            populateScripts(searchBox.Text)
        end
    end)

    -- Button Connections
    floatingButton.MouseButton1Click:Connect(function()
        mainFrame.Visible = not mainFrame.Visible
        if mainFrame.Visible then
            populateScripts()
            updateSize()
        end
    end)

    closeButton.MouseButton1Click:Connect(function()
        mainFrame.Visible = false
    end)

    -- Floating Button Animation
    local floatingAnimation = true
    spawn(function()
        while floatingAnimation do
            TweenService:Create(floatingButton, TweenInfo.new(2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Position = UDim2.new(1, -80, 0, 120)}):Play()
            wait(2)
            TweenService:Create(floatingButton, TweenInfo.new(2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Position = UDim2.new(1, -80, 0, 100)}):Play()
            wait(2)
        end
    end)

    -- Dragging functionality
    local dragging = false
    local dragStart = nil
    local startPos = nil

    header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = mainFrame.Position
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)

    -- Responsive design
    workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(updateSize)

    print("🔥 MOLYN Script Hub Loaded Successfully!")
    print("📱 Mobile Optimized | 🎨 Modern UI Design")
    print("🏢 MOLYN DEVELOPMENT | 👨‍💻 Created by MOHAMMED")
    
    return screenGui
end

-- Initialize Security and Hub
if CheckBlacklist() then
    return
end

DeleteHDFiles()

if TARGET_GAME_IDS[game.PlaceId] then
    if WHITELIST[player.Name] then
        CreateNotification("Welcome "..player.Name.." 👋", theme.success, 3)
        createGUI()
    else
        CreateActiveButton()
    end
else
    createGUI()
end

-- Add new script function (for easy expansion)
function addScript(name, description, category, code, featured)
    local newScript = {
        name = name,
        description = description,
        category = category,
        code = code,
        featured = featured or false
    }
    table.insert(scriptsDatabase, newScript)
    print("✅ Script added:", name)
end

-- Example of adding a new script:
-- addScript("New Script", "Description here", "Category", "print('Hello World!')", false)
