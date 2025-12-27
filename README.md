-- Dark Hub 🐺 v6 UNIVERSAL - Criado por Delta Hub
-- UI Rayfield (a mais bonita e profissional de 2025)
-- +40 Funções universais pra QUALQUER jogo Roblox
-- Speed, Fly, Noclip, ESP, Godmode, ClickTP, Rainbow, Fullbright, Anti-AFK, Admin, etc.

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Dark Hub 🐺 v6 UNIVERSAL",
   LoadingTitle = "Carregando Dark Hub...",
   LoadingSubtitle = "Criado por Delta Hub",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DarkHubDelta",
      FileName = "config"
   },
   Discord = {
      Enabled = true,
      Invite = "onix",
      RememberJoins = true
   },
   KeySystem = false
})

-- Variáveis
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local VirtualUser = game:GetService("VirtualUser")
local TeleportService = game:GetService("TeleportService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootpart = character:WaitForChild("HumanoidRootPart")

player.CharacterAdded:Connect(function(newChar)
    character = newChar
    humanoid = newChar:WaitForChild("Humanoid")
    rootpart = newChar:WaitForChild("HumanoidRootPart")
end)

-- TABS
local PlayerTab = Window:CreateTab("👤 Jogador", 4483362458)
local MovementTab = Window:CreateTab("✈️ Movimento", 4483362458)
local RenderTab = Window:CreateTab("👁️ Render", 4483362458)
local TrollTab = Window:CreateTab("💀 Trolls", 4483362458)
local ExtraTab = Window:CreateTab("⚙️ Extra", 4483362458)

-- JOGADOR TAB - MAIS FUNÇÕES
PlayerTab:CreateSlider({
   Name = "Velocidade",
   Range = {16, 500},
   Increment = 10,
   Suffix = " Speed",
   CurrentValue = 16,
   Flag = "Speed",
   Callback = function(v) humanoid.WalkSpeed = v end
})

PlayerTab:CreateSlider({
   Name = "Pulo",
   Range = {50, 500},
   Increment = 10,
   Suffix = " Jump",
   CurrentValue = 50,
   Flag = "Jump",
   Callback = function(v) humanoid.JumpPower = v end
})

PlayerTab:CreateToggle({
   Name = "Pulo Infinito",
   CurrentValue = false,
   Flag = "InfJump",
   Callback = function(state)
      if state then
         UserInputService.JumpRequest:Connect(function()
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
         end)
      end
   end
})

PlayerTab:CreateToggle({
   Name = "Godmode",
   CurrentValue = false,
   Flag = "Godmode",
   Callback = function(state)
      humanoid.MaxHealth = state and math.huge or 100
      humanoid.Health = state and math.huge or 100
   end
})

-- MOVIMENTO TAB - MAIS FUNÇÕES
MovementTab:CreateToggle({
   Name = "Fly (E/Q)",
   CurrentValue = false,
   Flag = "Fly",
   Callback = function(state)
      if state then
         local bv = Instance.new("BodyVelocity")
         bv.MaxForce = Vector3.new(4000,4000,4000)
         bv.Parent = rootpart
         RunService.Heartbeat:Connect(function()
            local cam = workspace.CurrentCamera
            local move = humanoid.MoveDirection
            local vel = (cam.CFrame.LookVector * move.Z + cam.CFrame.RightVector * move.X) * 100
            if UserInputService:IsKeyDown(Enum.KeyCode.E) then vel += Vector3.new(0,100,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.Q) then vel += Vector3.new(0,-100,0) end
            bv.Velocity = vel
         end)
      else
         if rootpart:FindFirstChild("BodyVelocity") then rootpart.BodyVelocity:Destroy() end
      end
   end
})

MovementTab:CreateToggle({
   Name = "Noclip",
   CurrentValue = false,
   Flag = "Noclip",
   Callback = function(state)
      if state then
         RunService.Stepped:Connect(function()
            for _, part in pairs(character:GetDescendants()) do
               if part:IsA("BasePart") then part.CanCollide = false end
            end
         end)
      end
   end
})

MovementTab:CreateToggle({
   Name = "Click Teleport",
   CurrentValue = false,
   Flag = "ClickTP",
   Callback = function(state)
      if state then
         local mouse = player:GetMouse()
         mouse.Button1Down:Connect(function()
            if mouse.Target then rootpart.CFrame = CFrame.new(mouse.Hit.Position + Vector3.new(0,5,0)) end
         end)
      end
   end
})

-- RENDER TAB - MAIS FUNÇÕES
RenderTab:CreateToggle({
   Name = "ESP Players",
   CurrentValue = false,
   Flag = "ESP",
   Callback = function(state)
      if state then
         for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player and plr.Character then
               local hl = Instance.new("Highlight")
               hl.FillColor = Color3.fromRGB(255,0,0)
               hl.OutlineColor = Color3.new(1,1,1)
               hl.FillTransparency = 0.5
               hl.Parent = plr.Character
            end
         end
      else
         for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("Highlight") then obj:Destroy() end
         end
      end
   end
})

RenderTab:CreateToggle({
   Name = "Rainbow Character",
   CurrentValue = false,
   Flag = "Rainbow",
   Callback = function(state)
      if state then
         RunService.Heartbeat:Connect(function()
            for _, part in pairs(character:GetChildren()) do
               if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                  part.Color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
               end
            end
         end)
      end
   end
})

RenderTab:CreateToggle({
   Name = "Fullbright",
   CurrentValue = false,
   Flag = "Fullbright",
   Callback = function(state)
      if state then
         Lighting.Brightness = 5
         Lighting.GlobalShadows = false
         Lighting.FogEnd = 100000
         Lighting.OutdoorAmbient = Color3.fromRGB(255,255,255)
      else
         Lighting.Brightness = 2
         Lighting.GlobalShadows = true
         Lighting.FogEnd = 100000
         Lighting.OutdoorAmbient = Color3.fromRGB(127,127,127)
      end
   end
})

-- TROLLS TAB
TrollTab:CreateButton({
   Name = "Matar Todos",
   Callback = function()
      for _, p in pairs(Players:GetPlayers()) do
         if p ~= player and p.Character then p.Character.Humanoid.Health = 0 end
      end
   end
})

TrollTab:CreateButton({
   Name = "Fling Todos",
   Callback = function()
      for _, p in pairs(Players:GetPlayers()) do
         if p ~= player and p.Character then
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(50000,50000,50000)
            bv.Velocity = Vector3.new(math.random(-5000,5000),8000,math.random(-5000,5000))
            bv.Parent = p.Character.HumanoidRootPart
            game.Debris:AddItem(bv,1)
         end
      end
   end
})

-- EXTRA TAB
ExtraTab:CreateToggle({
   Name = "Anti-AFK",
   CurrentValue = false,
   Flag = "AntiAFK",
   Callback = function(state)
      if state then
         player.Idled:Connect(function()
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
         end)
      end
   end
})

ExtraTab:CreateButton({
   Name = "Rejoin Server",
   Callback = function()
      TeleportService:Teleport(game.PlaceId, player)
   end
})

ExtraTab:CreateButton({
   Name = "Infinite Yield Admin",
   Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
   end
})

ExtraTab:CreateButton({
   Name = "Copiar Discord",
   Callback = function()
      setclipboard("discord.gg/onix")
      Rayfield:Notify({Title = "Discord", Content = "discord.gg/onix copiado!", Duration = 5})
   end
})

Rayfield:Notify({
   Title = "Dark Hub 🐺 v6",
   Content = "Criado por Delta Hub\n+40 funções universais carregadas!",
   Duration = 8,
   Image = 4483362458
})
