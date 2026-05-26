-- [[ Coelho Hub - Clean Template ]
-- Desenvolvido na pura força do tédio (e do pó de café)
-- Créditos: by mr by tedio

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = " Coelho Hub",
    SubTitle = "by mr",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.RightControl
})
local ScreenGui = Instance.new("ScreenGui")
local ToggleBtn = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

ToggleBtn.Parent = ScreenGui
ToggleBtn.Size = UDim2.new(0, 60, 0, 60)
ToggleBtn.Position = UDim2.new(0, 20, 0.5, 0)
ToggleBtn.BackgroundTransparency = 1
ToggleBtn.Image = "rbxassetid://94453919385793"

UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ToggleBtn

-- Abre/fecha igual RightControl
ToggleBtn.MouseButton1Click:Connect(function()
    game:GetService("VirtualInputManager"):SendKeyEvent(true, Enum.KeyCode.RightControl, false, game)
    task.wait(0.1)
    game:GetService("VirtualInputManager"):SendKeyEvent(false, Enum.KeyCode.RightControl, false, game)
end)

-- Arrastar a bolinha
local dragging, dragStart, startPos

ToggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = ToggleBtn.Position
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        ToggleBtn.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        ToggleBtn.Position = UDim2.new(
            startPos.X.Scale, 
            startPos.X.Offset + delta.X, 
            startPos.Y.Scale, 
            startPos.Y.Offset + delta.Y
        )
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
-- ========================================================
-- 1. PRIMEIRO: CRIA TODAS AS ABAS DO MENU
-- ========================================================
local Tabs = {
    ShopTab = Window:AddTab({ Title = "ShopTab", Icon = "coins" }),
    Race = Window:AddTab({ Title = "Race", Icon = "" }),
    Others = Window:AddTab({ Title = "Others", Icon = "" }),
    Status = Window:AddTab({ Title = "Status", Icon = "activity" }),
    Config = Window:AddTab({ Title = "config", Icon = "settings" }),
    Main = Window:AddTab({ Title = "Main", Icon = "sword" }),
    Items = Window:AddTab({ Title = "Items", Icon = "package" }),
    Fruit = Window:AddTab({ Title = "Fruit", Icon = "cherry" }),
    Teleport = Window:AddTab({ Title = "Teleport", Icon = "compass" }),
    Creditos = Window:AddTab({ Title = "Creditos", Icon = "info" })
}

-- [[ GERENCIADORES INTERNOS ]]
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
SaveManager:SetIgnoreIndexes({})


-- ========================================================
-- ABA CONFIG
-- ========================================================

local replicated = game:GetService("ReplicatedStorage")

local Time = Tabs.Status:AddParagraph({ Title = "Time Zone", Content = "" })
local Timmessss = Tabs.Status:AddParagraph({ Title = "Game Time", Content = "" })
local Miragecheck = Tabs.Status:AddParagraph({ Title = "Mirage Island", Content = "Status: " })
local Kitsunecheck = Tabs.Status:AddParagraph({ Title = "Kitsune Island", Content = "Status: " })
local CPrehistoriccheck = Tabs.Status:AddParagraph({ Title = "Prehistoric Island", Content = "Status: " })
local FrozenIsland = Tabs.Status:AddParagraph({ Title = "Frozen Dimension", Content = "Status: " })
local MobCakePrince = Tabs.Status:AddParagraph({ Title = "Cake Prince Kills", Content = "" })
local TyrantStatus = Tabs.Status:AddParagraph({ Title = "Tyrant of the Skies", Content = "Status: " })
local CheckRip = Tabs.Status:AddParagraph({ Title = "Rip_Indra", Content = "Status: " })
local CheckDoughKing = Tabs.Status:AddParagraph({ Title = "Dough King", Content = "Status: " })
local EliteHunter = Tabs.Status:AddParagraph({ Title = "Elite Hunter", Content = "Status: " })
local Pullever = Tabs.Status:AddParagraph({ Title = "Pull Lever", Content = "Status: " })
local FM = Tabs.Status:AddParagraph({ Title = "Full Moon", Content = "" })
local LegendarySword = Tabs.Status:AddParagraph({ Title = "Legendary Sword", Content = "Status: " })
local Bone = Tabs.Status:AddParagraph({ Title = "Bones", Content = "" })

task.spawn(function()
    while task.wait(1) do
        -- Time Zone
        pcall(function()
            local date = os.date("*t")
            local hour = date.hour % 24
            local ampm = hour < 12 and "AM" or "PM"
            local timezone = string.format("%02i:%02i:%02i %s", ((hour - 1) % 12) + 1, date.min, date.sec, ampm)
            local datetime = string.format("%02d/%02d/%04d", date.day, date.month, date.year)
            local ok, code = pcall(function()
                return game:GetService("LocalizationService"):GetCountryRegionForPlayerAsync(game.Players.LocalPlayer)
            end)
            Time:SetDesc(datetime .. " - " .. timezone .. " [ " .. (ok and code or "??") .. " ]")
        end)

        -- Game Time
        pcall(function()
            local GameTime = math.floor(workspace.DistributedGameTime + 0.5)
            local Hour = math.floor(GameTime / 3600) % 24
            local Minute = math.floor(GameTime / 60) % 60
            local Second = GameTime % 60
            Timmessss:SetDesc(Hour .. "h " .. Minute .. "m " .. Second .. "s")
        end)

        -- Mirage Island
        pcall(function()
            Miragecheck:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Mirage Island") and "✅" or "❌")
        end)

        -- Kitsune Island
        pcall(function()
            Kitsunecheck:SetDesc(workspace.Map:FindFirstChild("KitsuneIsland") and "✅" or "❌")
        end)

        -- Prehistoric Island
        pcall(function()
            CPrehistoriccheck:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Prehistoric Island") and "✅" or "❌")
        end)

        -- Frozen Dimension
        pcall(function()
            FrozenIsland:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Frozen Dimension") and "✅" or "❌")
        end)

        -- Cake Prince
        pcall(function()
            local response = replicated.Remotes.CommF_:InvokeServer("CakePrinceSpawner")
            local len = string.len(tostring(response))
            if len == 88 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 41))
            elseif len == 87 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 40))
            elseif len == 86 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 39))
            else
                MobCakePrince:SetDesc("Cake Prince : ✅")
            end
        end)

        -- Tyrant
        pcall(function()
            TyrantStatus:SetDesc(workspace.Enemies:FindFirstChild("Tyrant of the Skies") and "✅" or "❌")
        end)

        -- Rip Indra
        pcall(function()
            CheckRip:SetDesc((replicated:FindFirstChild("rip_indra True Form") or workspace.Enemies:FindFirstChild("rip_indra")) and "✅" or "❌")
        end)

        -- Dough King
        pcall(function()
            CheckDoughKing:SetDesc((replicated:FindFirstChild("Dough King") or workspace.Enemies:FindFirstChild("Dough King")) and "✅" or "❌")
        end)

        -- Elite Hunter
        pcall(function()
            local progress = replicated.Remotes.CommF_:InvokeServer("EliteHunter", "Progress")
            local eliteFound = replicated:FindFirstChild("Diablo") or replicated:FindFirstChild("Deandre") or replicated:FindFirstChild("Urban")
                or workspace.Enemies:FindFirstChild("Diablo") or workspace.Enemies:FindFirstChild("Deandre") or workspace.Enemies:FindFirstChild("Urban")
            EliteHunter:SetDesc((eliteFound and "✅" or "❌") .. " | Killed: " .. tostring(progress))
        end)

        -- Pull Lever
        pcall(function()
            Pullever:SetDesc(replicated.Remotes.CommF_:InvokeServer("CheckTempleDoor") and "✅" or "❌")
        end)

       pcall(function()
    local sky = game:GetService("Lighting"):FindFirstChildOfClass("Sky")
    if sky then
        local moonPhases = {
            ["http://www.roblox.com/asset/?id=9709149431"] = "Moon: 5/5 🌕",
            ["http://www.roblox.com/asset/?id=9709149052"] = "Moon: 4/5",
            ["http://www.roblox.com/asset/?id=9709143733"] = "Moon: 3/5",
            ["http://www.roblox.com/asset/?id=9709150401"] = "Moon: 2/5",
            ["http://www.roblox.com/asset/?id=9709149680"] = "Moon: 1/5",
        }
        FM:SetDesc(moonPhases[sky.MoonTextureId] or "Moon: 0/5")
    else
        FM:SetDesc("Sky não encontrado")
    end
end)
        -- Legendary Sword
        pcall(function()
            local rs = replicated.Remotes.CommF_
            if rs:InvokeServer("LegendarySwordDealer", "1") then
                LegendarySword:SetDesc("Shisui ✅")
            elseif rs:InvokeServer("LegendarySwordDealer", "2") then
                LegendarySword:SetDesc("Wando ✅")
            elseif rs:InvokeServer("LegendarySwordDealer", "3") then
                LegendarySword:SetDesc("Saddi ✅")
            else
                LegendarySword:SetDesc("Not Found ❌")
            end
        end)

        -- Bones
        pcall(function()
            local bones = replicated.Remotes.CommF_:InvokeServer("Bones", "Check")
            Bone:SetDesc("You Have : " .. tostring(bones) .. " Bones")
        end)
    end
end)

-- ========================================================
-- ABA CRÉDITOS
-- (A Fluent não suporta AddImageLabel nativamente,
--  então usamos Paragraph pra dar o crédito da imagem)
-- ========================================================
Tabs.Creditos:AddParagraph({
    Title = " Musa do Café (Fiscal de Código)",
    Content = "A guardiã que salvou o Coelho Hub de 60 erros fatais.\nID da imagem: 108660186329467"
})

Tabs.Creditos:AddParagraph({
    Title = "Agradecimentos Especiais",
    Content = "Criado por: by mr."
})

-- ========================================================
-- ATALHO PARA MINIMIZAR (RightAlt)
-- ========================================================
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightAlt then
        Fluent:ToggleVisibility()
    end
end)
-- =====================================================
-- Auto Attack - Corrigido pra parar de dar erro no Studio
-- Funções de executor (getupvalue, setupvalue, etc)
-- funcionam normal no executor, aqui só silenciamos o Luau
-- =====================================================

-- Fallbacks pra o Studio não surtar com funções de executor
local getupvalue   = getupvalue   or function() return nil end
local setupvalue   = setupvalue   or function() end
local getgc        = getgc        or function() return {} end
local hookfunction = hookfunction or function() end
local iscclosure   = iscclosure   or function() return false end
local getsenv      = getsenv      or function() return {} end
 
-- =====================================================
loadstring(game:HttpGet("https://raw.githubusercontent.com/AnhDzaiScript/Setting/refs/heads/main/FastMax.lua"))()
 
local function GetBladeHits()
    local targets = {}
    local function GetDistance(v)
        return (v.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
    end
 
    for _, part in pairs({game.Workspace.Enemies, game.Workspace.Characters}) do
        for _, v in pairs(part:GetChildren()) do
            if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") then
                if GetDistance(v.HumanoidRootPart) < 60 then
                    table.insert(targets, v)
                end
            end
        end
    end
 
    return targets
end
 
local function AttackAll()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if not character then return end
 
    local equippedWeapon = character:FindFirstChild("EquippedWeapon")
    if not equippedWeapon then return end
 
    local enemies = GetBladeHits()
    if #enemies > 0 then
        local netModule = game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("Net")
        netModule:WaitForChild("RE/RegisterAttack"):FireServer(-math.huge)
 
        local args = {nil, {}}
        for i, v in pairs(enemies) do
            if not args[1] then
                args[1] = v.Head
            end
            args[2][i] = {v, v.HumanoidRootPart}
        end
 
        netModule:WaitForChild("RE/RegisterHit"):FireServer(unpack(args))
    end
end
 
spawn(function()
    while task.wait() do AttackAll() end
end)
 
-- =====================================================
 
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local VirtualInputManager = game:GetService("VirtualInputManager")
 
local Player = Players.LocalPlayer
local Modules = ReplicatedStorage:WaitForChild("Modules")
local Net = Modules:WaitForChild("Net")
local RegisterAttack = Net:WaitForChild("RE/RegisterAttack")
local RegisterHit = Net:WaitForChild("RE/RegisterHit")
local ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent")
local GunValidator = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Validator2")
 
local Config = {
    AttackDistance = 90,
    AttackMobs = true,
    AttackPlayers = true,
    AttackCooldown = 0.2,
    ComboResetTime = 0.3,
    MaxCombo = 4,
    HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"},
    AutoClickEnabled = true
}
 
local FastAttack = {}
FastAttack.__index = FastAttack
 
function FastAttack.new()
    local self = setmetatable({
        Debounce = 0,
        ComboDebounce = 0,
        ShootDebounce = 0,
        M1Combo = 0,
        EnemyRootPart = nil,
        Connections = {},
        Overheat = {Dragonstorm = {MaxOverheat = 3, Cooldown = 0, TotalOverheat = 0, Distance = 350, Shooting = false}},
        ShootsPerTarget = {["Dual Flintlock"] = 2},
        SpecialShoots = {["Skull Guitar"] = "TAP", ["Bazooka"] = "Position", ["Cannon"] = "Position", ["Dragonstorm"] = "Overheat"}
    }, FastAttack)
 
    pcall(function()
        self.CombatFlags = require(Modules.Flags).COMBAT_REMOTE_THREAD
        self.ShootFunction = getupvalue(require(ReplicatedStorage.Controllers.CombatController).Attack, 9)
        local LocalScript = Player:WaitForChild("PlayerScripts"):FindFirstChildOfClass("LocalScript")
        if LocalScript and getsenv then
            self.HitFunction = getsenv(LocalScript)._G.SendHitsToServer
        end
    end)
 
    return self
end
 
function FastAttack:IsEntityAlive(entity)
    local humanoid = entity and entity:FindFirstChild("Humanoid")
    return humanoid and humanoid.Health > 0
end
 
function FastAttack:CheckStun(Character, Humanoid, ToolTip)
    local Stun = Character:FindFirstChild("Stun")
    local Busy = Character:FindFirstChild("Busy")
    if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Blox Fruit") then
        return false
    elseif Stun and Stun.Value > 0 or Busy and Busy.Value then
        return false
    end
    return true
end
 
function FastAttack:GetBladeHits(Character, Distance)
    local Position = Character:GetPivot().Position
    local BladeHits = {}
    Distance = Distance or Config.AttackDistance
 
    local function ProcessTargets(Folder)
        for _, Enemy in ipairs(Folder:GetChildren()) do
            if Enemy ~= Character and self:IsEntityAlive(Enemy) then
                local BasePart = Enemy:FindFirstChild(Config.HitboxLimbs[math.random(#Config.HitboxLimbs)]) or Enemy:FindFirstChild("HumanoidRootPart")
                if BasePart and (Position - BasePart.Position).Magnitude <= Distance then
                    if not self.EnemyRootPart then
                        self.EnemyRootPart = BasePart
                    else
                        table.insert(BladeHits, {Enemy, BasePart})
                    end
                end
            end
        end
    end
 
    if Config.AttackMobs then ProcessTargets(Workspace.Enemies) end
    if Config.AttackPlayers then ProcessTargets(Workspace.Characters) end
 
    return BladeHits
end
 
function FastAttack:GetClosestEnemy(Character, Distance)
    local BladeHits = self:GetBladeHits(Character, Distance)
    local Closest, MinDistance = nil, math.huge
 
    for _, Hit in ipairs(BladeHits) do
        local Magnitude = (Character:GetPivot().Position - Hit[2].Position).Magnitude
        if Magnitude < MinDistance then
            MinDistance = Magnitude
            Closest = Hit[2]
        end
    end
    return Closest
end
 
function FastAttack:GetCombo()
    local Combo = (tick() - self.ComboDebounce) <= Config.ComboResetTime and self.M1Combo or 0
    Combo = Combo >= Config.MaxCombo and 1 or Combo + 1
    self.ComboDebounce = tick()
    self.M1Combo = Combo
    return Combo
end
 
function FastAttack:ShootInTarget(TargetPosition)
    local Character = Player.Character
    if not self:IsEntityAlive(Character) then return end
 
    local Equipped = Character:FindFirstChildOfClass("Tool")
    if not Equipped or Equipped.ToolTip ~= "Gun" then return end
 
    local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or 0.3
    if (tick() - self.ShootDebounce) < Cooldown then return end
 
    local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"
    if ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
        Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
        GunValidator:FireServer(self:GetValidator2())
 
        if ShootType == "TAP" then
            Equipped.RemoteEvent:FireServer("TAP", TargetPosition)
        else
            ShootGunEvent:FireServer(TargetPosition)
        end
        self.ShootDebounce = tick()
    else
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
        task.wait(0.05)
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
        self.ShootDebounce = tick()
    end
end
 
function FastAttack:GetValidator2()
    local v1 = getupvalue(self.ShootFunction, 15)
    local v2 = getupvalue(self.ShootFunction, 13)
    local v3 = getupvalue(self.ShootFunction, 16)
    local v4 = getupvalue(self.ShootFunction, 17)
    local v5 = getupvalue(self.ShootFunction, 14)
    local v6 = getupvalue(self.ShootFunction, 12)
    local v7 = getupvalue(self.ShootFunction, 18)
 
    local v8 = v6 * v2
    local v9 = (v5 * v2 + v6 * v1) % v3
    v9 = (v9 * v3 + v8) % v4
    v5 = math.floor(v9 / v3)
    v6 = v9 - v5 * v3
    v7 = v7 + 1
 
    setupvalue(self.ShootFunction, 15, v1)
    setupvalue(self.ShootFunction, 13, v2)
    setupvalue(self.ShootFunction, 16, v3)
    setupvalue(self.ShootFunction, 17, v4)
    setupvalue(self.ShootFunction, 14, v5)
    setupvalue(self.ShootFunction, 12, v6)
    setupvalue(self.ShootFunction, 18, v7)
 
    return math.floor(v9 / v4 * 16777215), v7
end
 
function FastAttack:UseNormalClick(Character, Humanoid, Cooldown)
    self.EnemyRootPart = nil
    local BladeHits = self:GetBladeHits(Character)
 
    if self.EnemyRootPart then
        RegisterAttack:FireServer(Cooldown)
        if self.CombatFlags and self.HitFunction then
            self.HitFunction(self.EnemyRootPart, BladeHits)
        else
            RegisterHit:FireServer(self.EnemyRootPart, BladeHits)
        end
    end
end
 
function FastAttack:UseFruitM1(Character, Equipped, Combo)
    local Targets = self:GetBladeHits(Character)
    if not Targets[1] then return end
 
    local Direction = (Targets[1][2].Position - Character:GetPivot().Position).Unit
    Equipped.LeftClickRemote:FireServer(Direction, Combo)
end
 
function FastAttack:Attack()
    if not Config.AutoClickEnabled or (tick() - self.Debounce) < Config.AttackCooldown then return end
    local Character = Player.Character
    if not Character or not self:IsEntityAlive(Character) then return end
 
    local Humanoid = Character.Humanoid
    local Equipped = Character:FindFirstChildOfClass("Tool")
    if not Equipped then return end
 
    local ToolTip = Equipped.ToolTip
    if not table.find({"Melee", "Blox Fruit", "Sword", "Gun"}, ToolTip) then return end
 
    local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or Config.AttackCooldown
    if not self:CheckStun(Character, Humanoid, ToolTip) then return end
 
    local Combo = self:GetCombo()
    Cooldown = Cooldown + (Combo >= Config.MaxCombo and 0.05 or 0)
    self.Debounce = Combo >= Config.MaxCombo and ToolTip ~= "Gun" and (tick() + 0.05) or tick()
 
    if ToolTip == "Blox Fruit" and Equipped:FindFirstChild("LeftClickRemote") then
        self:UseFruitM1(Character, Equipped, Combo)
    elseif ToolTip == "Gun" then
        local Target = self:GetClosestEnemy(Character, 120)
        if Target then
            self:ShootInTarget(Target.Position)
        end
    else
        self:UseNormalClick(Character, Humanoid, Cooldown)
    end
end
 
local AttackInstance = FastAttack.new()
table.insert(AttackInstance.Connections, RunService.Stepped:Connect(function()
    AttackInstance:Attack()
end))
 
-- getgc só existe no executor, no Studio retorna {} então o loop não faz nada
for _, v in pairs(getgc(true)) do
    if typeof(v) == "function" and iscclosure(v) then
        local name = debug.getinfo(v).name
        if name == "Attack" or name == "attack" or name == "RegisterHit" then
            hookfunction(v, function(...)
                AttackInstance:Attack()
                return v(...)
            end)
        end
    end
end
 
-- =====================================================
-- Fast 2
-- =====================================================
local Modules2 = game.ReplicatedStorage.Modules
local Net2 = Modules2.Net
local Register_Hit = Net2:WaitForChild("RE/RegisterHit")
local Register_Attack = Net2:WaitForChild("RE/RegisterAttack")
local Funcs = {}
 
local function GetAllBladeHits()
    local bladehits = {}
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
 
local function Getplayerhit()
    local bladehits = {}
    for _, v in pairs(workspace.Characters:GetChildren()) do
        if v.Name ~= game.Players.LocalPlayer.Name and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
 
function Funcs:Attack()
    local bladehits = {}
    for _, v in pairs(GetAllBladeHits()) do
        table.insert(bladehits, v)
    end
    for _, v in pairs(Getplayerhit()) do
        table.insert(bladehits, v)
    end
    if #bladehits == 0 then return end
 
    local args = {
        [1] = nil,
        [2] = {},
        [4] = "078da341"
    }
    for r, v in pairs(bladehits) do
        Register_Attack:FireServer(0)
        if not args[1] then
            args[1] = v.Head
        end
        args[2][r] = {
            [1] = v,
            [2] = v.HumanoidRootPart
        }
    end
    Register_Hit:FireServer(unpack(args))
end

local TeamDropdown

TeamDropdown = Tabs.Config:AddDropdown("TeamDropdown", {
    Title = "Selecionar Time",
    Values = {"---", "Marines", "Pirates"},
    Default = "---",
    Callback = function(Value)
        if Value == "-- Selecionar --" then return end
        local replicated = game:GetService("ReplicatedStorage")
        local success, err = pcall(function()
            replicated.Remotes.CommF_:InvokeServer("SetTeam", Value)
        end)
        if success then
            print("Entrou no time: " .. Value)
            task.wait(0.2)
            TeamDropdown:SetValue("---")
        else
            warn("Erro ao entrar no time: " .. tostring(err))
        end
    end
})

Tabs.Fruit:AddToggle("CollectFruitsBtn", {
    Title = "Collect Fruits",
    Default = false,
    Callback = function(Value)
        _G.CollectFruits = Value
        if Value then
            task.spawn(function()
                while _G.CollectFruits do
                    local _success, _err = pcall(function()
                        local plr = game.Players.LocalPlayer
                        local char = plr.Character
                        if char then
                            local hrp = char:FindFirstChild("HumanoidRootPart")
                            local hum = char:FindFirstChildOfClass("Humanoid")
                            if hum then hum.WalkSpeed = 325 end

                            for _, K in pairs(workspace:GetChildren()) do
                                if string.find(K.Name, "Fruit") and K:FindFirstChild("Handle") then
                                    while _G.CollectFruits and K and K.Parent do
                                        local dist = (K.Handle.Position - hrp.Position).Magnitude
                                        if dist < 5 then break end

                                        -- noclip em todos os parts do char
                                        for _, part in pairs(char:GetDescendants()) do
                                            if part:IsA("BasePart") then
                                                part.CanCollide = false
                                            end
                                        end

                                        local bv = Instance.new("BodyVelocity")
                                        bv.Velocity = (K.Handle.Position - hrp.Position).Unit * 325
                                        bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                                        bv.Parent = hrp
                                        task.wait(0.05)
                                        bv:Destroy()
                                    end
                                end
                            end

                            -- volta colisão normal depois de pegar
                            for _, part in pairs(char:GetDescendants()) do
                                if part:IsA("BasePart") then
                                    part.CanCollide = true
                                end
                            end
                        end
                    end)
                    task.wait(0.1)
                end
            end)
        else
            -- desligou o toggle, volta tudo ao normal
            local char = game.Players.LocalPlayer.Character
            if char then
                local hum = char:FindFirstChildOfClass("Humanoid")
                if hum then hum.WalkSpeed = 16 end
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end
})
local selectedStats = {}
local plr = game.Players.LocalPlayer
local replicated = game:GetService("ReplicatedStorage")

Tabs.Config:AddDropdown("StatDropdown", {
    Title = "Stats ",
    Values = {"Melee", "Defense", "Sword", "Gun", "Devil"},
    Default = {},
    Multi = true,
    Callback = function(Value)
        selectedStats = {}
        for stat, enabled in pairs(Value) do
            if enabled then
                table.insert(selectedStats, stat)
            end
        end
    end
})

Tabs.Config:AddToggle("AutoStatToggle", {
    Title = "Auto Distribuir Stats",
    Default = false,
    Callback = function(Value)
        _G.AutoStat = Value
        if Value then
            task.spawn(function()
                while _G.AutoStat do
                    pcall(function()
                        if plr.Data.Points.Value ~= 0 then
                            local hasMelee = table.find(selectedStats, "Melee")
                            local hasDefense = table.find(selectedStats, "Defense")

                            if hasMelee and hasDefense then
                                -- prioridade melee mas ainda da pra defense
                                replicated.Remotes.CommF_:InvokeServer("AddPoint", "Melee", 2)
                                replicated.Remotes.CommF_:InvokeServer("AddPoint", "Defense", 1)
                            else
                                for _, stat in pairs(selectedStats) do
                                    local statName = stat == "Devil" and "Demon Fruit" or stat
                                    replicated.Remotes.CommF_:InvokeServer("AddPoint", statName, 1)
                                end
                            end
                        end
                    end)
                    task.wait(0)
                end
            end)
        end
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 1",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelMain")
        end)
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 2",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelDressrosa")
        end)
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 3",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelZou")
        end)
    end
})

Tabs.Items:AddToggle("AutoRengokuToggle", {
    Title = "Auto Rengoku",
    Default = false,
    Callback = function(Value)
        _G.AutoRengoku = Value
        if Value then
            task.spawn(function()
                while _G.AutoRengoku do
                    pcall(function()
                        local plr = game.Players.LocalPlayer
                        local char = plr.Character
                        local backpack = plr.Backpack

                        if backpack:FindFirstChild("Hidden Key") or char:FindFirstChild("Hidden Key") then
                            -- tem a key, vai pro local do rengoku
                            EquipWeapon("Hidden Key")
                            TweenPlayer(CFrame.new(6571.12, 299.23, -6967.84))

                        elseif workspace.Enemies:FindFirstChild("Snow Lurker") or workspace.Enemies:FindFirstChild("Arctic Warrior") then
                            -- mata os inimigos pra dropar a key
                            for _, v in pairs(workspace.Enemies:GetChildren()) do
                                if (v.Name == "Snow Lurker" or v.Name == "Arctic Warrior") and v.Humanoid.Health > 0 then
                                    repeat
                                        game:GetService("RunService").Heartbeat:Wait()
                                        EquipWeapon(_G.SelectWeapon)
                                        AutoHaki()
                                        v.HumanoidRootPart.Size = Vector3.new(1, 1, 1)
                                        TweenPlayer(v.HumanoidRootPart.CFrame * Pos)
                                        Attack()
                                    until backpack:FindFirstChild("Hidden Key") or not _G.AutoRengoku or not v.Parent or v.Humanoid.Health <= 0
                                end
                            end
                        else
                            -- vai pro spawn dos inimigos
                            TweenPlayer(CFrame.new(5439.71, 84.42, -6715.16))
                        end
                    end)
                    task.wait(0.2)
                end
            end)
        end
    end
})

Tabs.Items:AddToggle("AutogettushitaToggle", {
    Title = "Auto Get Tushita",
    Default = false,
    Callback = function(Value)
        _G.Auto_Tushita = Value
        if Value then
            task.spawn(function()
                while _G.Auto_Tushita do
                    pcall(function()
                        local plr = game.Players.LocalPlayer
                        local replicated = game:GetService("ReplicatedStorage")

                        if workspace.Map.Turtle:FindFirstChild("TushitaGate") then
                            if not GetBP("Holy Torch") then
                                -- vai buscar a Holy Torch
                                _tp(CFrame.new(5148.03, 162.35, 910.54))
                                task.wait(0.7)
                            else
                                -- tem a torch, faz o percurso
                                EquipWeapon("Holy Torch")
                                task.wait(1)

                                local checkpoints = {
                                    CFrame.new(-10752, 417, -9366),
                                    CFrame.new(-11672, 334, -9474),
                                    CFrame.new(-12132, 521, -10655),
                                    CFrame.new(-13336, 486, -6985),
                                    CFrame.new(-13489, 332, -7925),
                                }

                                for _, cf in pairs(checkpoints) do
                                    repeat
                                        task.wait()
                                        _tp(cf)
                                    until not _G.Auto_Tushita or (cf.Position - plr.Character.HumanoidRootPart.Position).Magnitude <= 10
                                    task.wait(0.7)
                                    if not _G.Auto_Tushita then return end
                                end
                            end
                        else
                            -- mata o Longma
                            local enemy = GetConnectionEnemies("Longma")
                            if enemy then
                                repeat
                                    task.wait()
                                    G.Kill(enemy, _G.Auto_Tushita)
                                until enemy.Humanoid.Health <= 0 or not _G.Auto_Tushita or not enemy.Parent
                            else
                                if replicated:FindFirstChild("Longma") then
                                    _tp(replicated:FindFirstChild("Longma").HumanoidRootPart.CFrame * CFrame.new(0, 40, 0))
                                end
                            end
                        end
                    end)
                    task.wait(0.1)
                end
            end)
        end
    end
})

Tabs.Items:AddToggle("AutoBuddySwordToggle", {
    Title = "Auto Buddy Sword", 
    Default = false,
    Callback = function(Value)
        _G.AutoBuddySword = Value
        if Value then
            task.spawn(function()
                while _G.AutoBuddySword do
                    pcall(function()
                        if workspace.Enemies:FindFirstChild("Cake Queen") then
                            for _, v in pairs(workspace.Enemies:GetChildren()) do
                                if v.Name == "Cake Queen" and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                                    repeat
                                        game:GetService("RunService").Heartbeat:Wait()
                                        AutoHaki()
                                        EquipWeapon(_G.SelectWeapon)
                                        v.Humanoid.WalkSpeed = 0
                                        v.HumanoidRootPart.Size = Vector3.new(1, 1, 1)
                                        TweenPlayer(v.HumanoidRootPart.CFrame * Pos)
                                        Attack()
                                    until not _G.AutoBuddySword or not v.Parent or v.Humanoid.Health <= 0
                                end
                            end
                        else
                            UnEquipWeapon(_G.SelectWeapon)
                            TweenPlayer(CFrame.new(-731.20, 381.56, -11198.49))
                        end
                    end)
                    task.wait(0.2)
                end
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("Auto Dragon Talon",{
    Title = "Auto Dragon Talon",
    Default = false,
    Callback = function(Value)
        _G.AutoDragonTalon = Value
        if Value then
            task.spawn(function()
                local replicated = game:GetService("ReplicatedStorage")
                local plr = game.Players.LocalPlayer
                while _G.AutoDragonTalon do
                    pcall(function()
                        if plr:FindFirstChild("WeaponAssetCache") then
                            if not GetBP("Dragon Claw") then
                                replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
                            end

                            local dragonClaw = GetBP("Dragon Claw")
                            if dragonClaw and dragonClaw.Level.Value >= 400 then
                                replicated.Remotes.CommF_:InvokeServer("Bones", "Buy", 1, 1)
                                replicated.Remotes.CommF_:InvokeServer("BuyDragonTalon")
                            elseif dragonClaw and dragonClaw.Level.Value < 400 then
                                repeat
                                    _G.AutoFarm_Bone = true
                                    task.wait()
                                until not _G.AutoDragonTalon or GetBP("Dragon Talon")
                                _G.AutoFarm_Bone = false
                            end
                        end
                    end)
                    task.wait(0.1)
                end
            end)
        end
    end
})

Tabs.Config:AddToggle("Anti AFK",{
    Title = "Anti AFK",
    Default = true,
    Callback = function(Value)
        _G.AntiAFK = Value
    end
})

print("✅ SISTEMA UNIVERSAL DE EQUIP WEAPON CARREGADO!")
print("📌 O script vai equipar a arma automaticamente para TODAS as funções")

game.Players.LocalPlayer.Idled:Connect(function()
    if _G.AntiAFK then
        local mouse = game.Players.LocalPlayer:GetMouse()
        mouse:Button2Down()
        task.wait(1)
        mouse:Button2Up()
    end
end)

Tabs.Config:AddDropdown("WeaponDropdown", {
    Title = "Choose Weapon",
    Values = {"---","Melee", "Sword", "Fruit", "Gun",},
    Default = "---",
    Callback = function(Value)
        _G.ChooseWP = Value
    end
})

task.spawn(function()
    while task.wait(0.3) do
        pcall(function()
            local plr = game.Players.LocalPlayer
            local char = plr.Character
            if not char then return end

            -- verifica o que tá equipado na mão
            local equipped = char:FindFirstChildOfClass("Tool")

            -- se tiver uma fruta na mão não faz nada
            if equipped and equipped.ToolTip == "Blox Fruit" then return end

            local tooltip = (_G.ChooseWP == "Fruit") and "Blox Fruit" or (_G.ChooseWP or "Melee")
            for _, v in pairs(plr.Backpack:GetChildren()) do
                if v.ToolTip == tooltip then
                    _G.SelectWeapon = v.Name
                    if not char:FindFirstChild(v.Name) then
                        char.Humanoid:EquipTool(v)
                    end
                end
            end
        end)
    end
end)

local replicated = game:GetService("ReplicatedStorage")

Tabs.ShopTab:AddParagraph({
    Title = "Fighting Style",
    Content = "Compre estilos de luta abaixo"
})

Tabs.ShopTab:AddButton({
    Title = "Black Leg",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyBlackLeg") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Fishman Karate",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyFishmanKarate") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Electro",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyElectro") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Dragon Breath",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "SuperHuman",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuySuperhuman") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Death Step",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyDeathStep") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Sharkman Karate",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BuySharkmanKarate", true)
            replicated.Remotes.CommF_:InvokeServer("BuySharkmanKarate")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Electric Claw",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyElectricClaw") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Dragon Talon",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyDragonTalon") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "God Human",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyGodhuman") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Sanguine Art",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BuySanguineArt", true)
            replicated.Remotes.CommF_:InvokeServer("BuySanguineArt")
        end)
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Swords",
    Content = "Compre espadas abaixo"
})

Tabs.ShopTab:AddButton({
    Title = "Cutlass [ 1,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Cutlass") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Katana [ 1,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Katana") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Iron Mace [ 25,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Iron Mace") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Dual Katana [ 12,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Duel Katana") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Triple Katana [ 60,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Triple Katana") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Pipe [ 100,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Pipe") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Dual-Headed Blade [ 400,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Dual-Headed Blade") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Bisento [ 1,200,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Bisento") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Soul Cane [ 750,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Soul Cane") end)
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Guns",
    Content = "Compre armas de fogo abaixo"
})

Tabs.ShopTab:AddButton({
    Title = "Slingshot [ 5,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Slingshot") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Musket [ 8,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Musket") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Flintlock [ 10,500 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Flintlock") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Refined Flintlock [ 65,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Refined Flintlock") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Cannon [ 100,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyItem", "Cannon") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Kabucha [ 1,500 Fragments ]",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Slingshot", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Slingshot", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Bizarre Rifle [ 250 Ectoplasm ]",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("Ectoplasm", "Buy", 1)
            replicated.Remotes.CommF_:InvokeServer("Ectoplasm", "Buy", 1)
        end)
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Abilities",
    Content = "Compre habilidades abaixo"
})

Tabs.ShopTab:AddButton({
    Title = "Skyjump [ 10,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Geppo") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buso Haki [ 25,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Buso") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Observation Haki [ 750,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("KenTalk", "Buy") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Soru [ 100,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Soru") end)
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Misc",
    Content = "Outros itens"
})

Tabs.ShopTab:AddButton({
    Title = "Buy Refund Stat (2500F)",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Refund", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Refund", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Reroll Race (3000F)",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Reroll", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Reroll", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Draco",
    Callback = function()
        pcall(function()
            local plr = game.Players.LocalPlayer
            local char = plr.Character
            _tp(CFrame.new(5814.42, 1208.32, 884.57))
            repeat task.wait() until (char.HumanoidRootPart.Position - Vector3.new(5814.42, 1208.32, 884.57)).Magnitude < 1
            game:GetService("ReplicatedStorage").Modules.Net:FindFirstChild("RF/InteractDragonQuest"):InvokeServer({
                ["NPC"] = "Dragon Wizard",
                ["Command"] = "DragonRace"
            })
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Ghoul Race",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("Ectoplasm", "Change", 4) end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Cyborg Race (2500F)",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("CyborgTrainer", "Buy") end)
    end
})

Tabs.Others:AddToggle("AutoFactoryRaidToggle", {
    Title = "Auto Factory Raid",
    Default = false,
    Callback = function(Value)
        _G.AutoFactory = Value
        if Value then
            task.spawn(function()
                local FactoryPos = Vector3.new(430.98, 238.41, -433.16)
                while _G.AutoFactory do
                    pcall(function()
                        local char = game.Players.LocalPlayer.Character
                        if not char then return end
                        local hrp = char:FindFirstChild("HumanoidRootPart")
                        if not hrp then return end

                        for _, part in pairs(char:GetDescendants()) do
                            if part:IsA("BasePart") then
                                part.CanCollide = false
                            end
                        end

                        local bv = Instance.new("BodyVelocity")
                        if (FactoryPos - hrp.Position).Magnitude > 5 then
                            bv.Velocity = (FactoryPos - hrp.Position).Unit * 200
                        else
                            bv.Velocity = Vector3.new(0, 0, 0)
                        end
                        bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
                        bv.Parent = hrp
                        task.wait(0.05)
                        bv:Destroy()
                    end)
                    task.wait(0.05)
                end

                local char = game.Players.LocalPlayer.Character
                if char then
                    for _, part in pairs(char:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = true
                        end
                    end
                end
            end)
        else
            local char = game.Players.LocalPlayer.Character
            if char then
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end
})

Tabs.Others:AddToggle("AutoPirateRaidToggle", {
    Title = "Auto Pirate Raid",
    Default = false,
    Callback = function(Value)
        _G.AutoRaidCastle = Value
        if Value then
            task.spawn(function()
                local replicated = game:GetService("ReplicatedStorage")
                local plr = game.Players.LocalPlayer
                local TargetCFrame = CFrame.new(-5496.17, 313.76, -2841.53)
                local CheckCFrame = CFrame.new(-5539.31, 313.80, -2972.37)
                local EnemyList = {
                    "Galley Pirate", "Galley Captain", "Raider", "Mercenary",
                    "Vampire", "Zombie", "Snow Trooper", "Winter Warrior",
                    "Lab Subordinate", "Horned Warrior", "Magma Ninja", "Lava Pirate",
                    "Ship Deckhand", "Ship Engineer", "Ship Steward", "Ship Officer",
                    "Arctic Warrior", "Snow Lurker", "Sea Soldier", "Water Fighter"
                }

                while _G.AutoRaidCastle do
                    pcall(function()
                        local char = plr.Character
                        if not char then return end
                        local Root = char:FindFirstChild("HumanoidRootPart")
                        if not Root then return end

                        if (CheckCFrame.Position - Root.Position).Magnitude <= 500 then
                            for _, e in pairs(workspace.Enemies:GetChildren()) do
                                if e:FindFirstChild("HumanoidRootPart") and e:FindFirstChild("Humanoid") and e.Humanoid.Health > 0 then
                                    if (e.HumanoidRootPart.Position - Root.Position).Magnitude <= 2000 then
                                        repeat
                                            task.wait()
                                            G.Kill(e, _G.AutoRaidCastle)
                                        until not _G.AutoRaidCastle or not e.Parent or e.Humanoid.Health <= 0
                                    end
                                end
                            end
                        else
                            for _, enemyName in pairs(EnemyList) do
                                if replicated:FindFirstChild(enemyName) then
                                    for _, n in pairs(replicated:GetChildren()) do
                                        if table.find(EnemyList, n.Name) then
                                            _tp(TargetCFrame)
                                        end
                                    end
                                end
                            end
                        end
                    end)
                    task.wait(0.1)
                end
            end)
        end
    end
})

-- AUTO SOUL REAPER
Tabs.Others:AddToggle("AutoSoulReaper", { -- CERTO
    Title = "Auto Soul Reaper",
    Default = false,
    Callback = function(v)
        _G.AutoHytHallow = v
    end,
})

spawn(function()
    while wait(Sec) do
        if _G.AutoHytHallow then
            pcall(function()

                local mob = GetConnectionEnemies("Soul Reaper")

                if mob then
                    repeat task.wait()
                        G.Kill(mob,_G.AutoHytHallow)
                    until mob.Humanoid.Health <= 0 or not _G.AutoHytHallow

                else
                    if not GetBP("Hallow Essence") then
                        repeat task.wait(.1)
                            replicated.Remotes.CommF_:InvokeServer("Bones","Buy",1,1)
                        until not _G.AutoHytHallow or GetBP("Hallow Essence")
                    else
                        local pos = CFrame.new(-8932.32,146.83,6062.55)
                        repeat wait(.1)
                            _tp(pos)
                        until not _G.AutoHytHallow or plr.Character.HumanoidRootPart.CFrame == pos
                        EquipWeapon("Hallow Essence")
                    end
                end

            end)
        end
    end
end)

-- 1. Deixe a variável de controle no topo do script (fora da toggle e do loop)
local Boud = false
local Sec = 1 -- Defina o tempo em segundos aqui

-- 2. O seu loop que executa a ação (ele fica rodando e checando a variável 'Boud')
task.spawn(function()
    while true do
        task.wait(Sec)
        if Boud then
            pcall(function()
                local I = { "HasBuso", "Buso" }
                if game.Players.LocalPlayer.Character and not game.Players.LocalPlayer.Character:FindFirstChild(I[1]) then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(I[2])
                end
            end)
        end
    end
end)

-- 3. A sua Toggle da UI (que controla o 'Boud')
Tabs.Config:AddToggle("AutoBusoHaki", {
    Title = "Auto turn Buso",
    Default = true,
    Callback = function(v)
        Boud = v
    end,
})

Tabs.Others:AddToggle("AutoSoulReaper", { -- CERTO
    Title = "Auto Elite",
    Default = false,
    Callback = function(v)
        _G.AutoElite = v
    end,
})

spawn(function()
	while wait(Sec) do
		pcall(function()
			if _G.FarmEliteHunt then
				if plr.PlayerGui.Main.Quest.Visible == true then
					if string.find(plr.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Diablo") or string.find(plr.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Urban") or string.find(plr.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text, "Deandre") then
						for I, e in pairs(replicated:GetChildren()) do
							if string.find(e.Name, "Diablo") or string.find(e.Name, "Urban") or string.find(e.Name, "Deandre") then
								_tp(e.HumanoidRootPart.CFrame);
							end;
						end;
						for I, e in pairs(Enemies:GetChildren()) do
							if (string.find(e.Name, "Diablo") or string.find(e.Name, "Urban") or string.find(e.Name, "Deandre")) and G.Alive(e) then
								repeat
									wait();
									G.Kill(e, _G.FarmEliteHunt);
								until not _G.FarmEliteHunt or plr.PlayerGui.Main.Quest.Visible == false or not e.Parent or e.Humanoid.Health <= 0;
							end;
						end;
					end;
				else
					replicated.Remotes.CommF_:InvokeServer("EliteHunter");
				end;
			end;
		end);
	end;
end);

Tabs.Race:AddToggle("Auto Shark V2/V3", {
    Title = "Auto Shark V2/V3",
    Description = "",
    Default = false,
    Callback = function(I)
        G.Auto_Fish = I;
    end,
});
spawn(function()
    while wait(Sec) do
        pcall(function()
            if G.Auto_Fish then
                if replicated.Remotes.CommF_:InvokeServer("Alchemist", "1") ~= -2 then
                    if replicated.Remotes.CommF_:InvokeServer("Alchemist", "1") == 0 then
                        replicated.Remotes.CommF_:InvokeServer("Alchemist", "2");
                    elseif replicated.Remotes.CommF_:InvokeServer("Alchemist", "1") == 1 then
                        if not plr.Backpack:FindFirstChild("Flower 1") and not plr.Character:FindFirstChild("Flower 1") then
                            tp(workspace.Flower1.CFrame);
                        elseif not plr.Backpack:FindFirstChild("Flower 2") and not plr.Character:FindFirstChild("Flower 2") then
                            tp(workspace.Flower2.CFrame);
                        elseif not plr.Backpack:FindFirstChild("Flower 3") and not plr.Character:FindFirstChild("Flower 3") then
                            local I = GetConnectionEnemies("Swan Pirate");
                            if I then
                                repeat wait();
                                    G.Kill(I, G.Auto_Fish);
                                until plr.Backpack:FindFirstChild("Flower 3") or not I.Parent or I.Humanoid.Health <= 0 or G.Auto_Fish == false;
                            else
                                tp(CFrame.new(980,121,1287));
                            end;
                        end;
                    elseif replicated.Remotes.CommF_:InvokeServer("Alchemist", "1") == 2 then
                        replicated.Remotes.CommF_:InvokeServer("Alchemist", "3");
                    end;
                elseif replicated.Remotes.CommF_:InvokeServer("Wenlocktoad", "1") == 0 then
                    replicated.Remotes.CommF_:InvokeServer("Wenlocktoad", "2");
                elseif replicated.Remotes.CommF_:InvokeServer("Wenlocktoad", "1") == 1 then
                    warn("Sea Beast Soon");
                end;
            end;
        end);
    end;
end)

Tabs.Items:AddToggle("Auto Shark V2/V3", {
 Title = "Auto Dragon Trident",
	Description = "",
	Default = false,
	Callback = function(I)
		_G.AutoTridentW2 = I;
	end,
});
spawn(function()
	while wait(.1) do
		pcall(function()
			if _G.AutoTridentW2 then
				local I = GetConnectionEnemies("Tide Keeper");
				if I then
					repeat
						task.wait();
						G.Kill(I, _G.AutoTridentW2);
					until _G.AutoTridentW2 == false or not I.Parent or I.Humanoid.Health <= 0;
				else
					_tp(CFrame.new(-3795.6423339844, 105.88877105713, -11421.307617188));
				end;
			end;
		end);
	end;
end);
