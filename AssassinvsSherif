--====================================================
-- SCRIPT CRIADO POR: @feinscriptsauras
-- ttk: @feinscriptsauras
--====================================================

--// SERVICES
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local LocalPlayer = Players.LocalPlayer

--====================================================
-- WATERMARK CENTRAL
--====================================================
local wmGui = Instance.new("ScreenGui", game.CoreGui)
wmGui.Name = "TTK_Watermark"

local wm = Instance.new("TextLabel", wmGui)
wm.AnchorPoint = Vector2.new(0.5,0.5)
wm.Position = UDim2.new(0.5,0,0.5,0)
wm.Size = UDim2.new(0,220,0,18)
wm.BackgroundTransparency = 1
wm.Text = "ttk: @feinscriptsauras"
wm.Font = Enum.Font.Gotham
wm.TextSize = 10
wm.TextColor3 = Color3.fromRGB(180,180,180)
wm.TextStrokeTransparency = 0.7

--====================================================
-- CONFIG
--====================================================
local AIMBOT_ON = false
local ESP_ON = false
local TARGET_PART = "HumanoidRootPart"

local AIM_SMOOTHNESS = 0.12
-- 0.06 = muito suave | 0.10~0.12 = balanceado | 0.18+ = mais agressivo

local MOVE_THRESHOLD = 0.15

--====================================================
-- UI
--====================================================
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "MiniHub"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.fromOffset(200,170)
Main.Position = UDim2.fromScale(0.05,0.4)
Main.BackgroundColor3 = Color3.fromRGB(20,20,20)
Main.Active = true
Main.Draggable = true
Instance.new("UICorner", Main)

local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1,0,0,25)
Title.BackgroundTransparency = 1
Title.Text = "FeinHub | 💀"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 13
Title.TextColor3 = Color3.new(1,1,1)

local function makeBtn(text, y)
	local b = Instance.new("TextButton", Main)
	b.Size = UDim2.fromOffset(160,30)
	b.Position = UDim2.fromOffset(20,y)
	b.Text = text
	b.Font = Enum.Font.GothamBold
	b.TextSize = 12
	b.TextColor3 = Color3.new(1,1,1)
	b.BackgroundColor3 = Color3.fromRGB(40,40,40)
	Instance.new("UICorner", b)
	return b
end

local AimBtn = makeBtn("AIMBOT: OFF", 35)
local EspBtn = makeBtn("ESP: OFF", 75)
local TpBtn  = makeBtn("TP TO NEAREST", 115)

--====================================================
-- FUNÇÕES
--====================================================
local function getHRP(char)
	return char and char:FindFirstChild("HumanoidRootPart")
end

local function getClosestPlayer()
	local myChar = LocalPlayer.Character
	local myHRP = getHRP(myChar)
	if not myHRP then return end

	local closest, dist = nil, math.huge
	for _,plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character then
			local hrp = getHRP(plr.Character)
			if hrp then
				local d = (hrp.Position - myHRP.Position).Magnitude
				if d < dist then
					dist = d
					closest = plr
				end
			end
		end
	end
	return closest
end

--====================================================
-- AIMBOT SUAVIZADO (LOCK + SMOOTH)
--====================================================
RunService.RenderStepped:Connect(function()
	if not AIMBOT_ON then return end

	local target = getClosestPlayer()
	if target and target.Character then
		local part = target.Character:FindFirstChild(TARGET_PART)
		if part then
			local camPos = Camera.CFrame.Position
			local desired = CFrame.new(camPos, part.Position)
			Camera.CFrame = Camera.CFrame:Lerp(desired, AIM_SMOOTHNESS)
		end
	end
end)

--====================================================
-- ESP (ATIVA APÓS SE MOVER UMA VEZ E NÃO SOME)
--====================================================
local ESP_CACHE = {}
local LAST_POS = {}
local MOVED_ONCE = {}

local function createLine()
	local l = Drawing.new("Line")
	l.Thickness = 2
	l.Color = Color3.fromRGB(0,255,0)
	l.Visible = false
	return l
end

RunService.RenderStepped:Connect(function()
	for _,plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character then
			local hrp = getHRP(plr.Character)
			if hrp then
				LAST_POS[plr] = LAST_POS[plr] or hrp.Position
				if (hrp.Position - LAST_POS[plr]).Magnitude > MOVE_THRESHOLD then
					MOVED_ONCE[plr] = true
				end
				LAST_POS[plr] = hrp.Position

				if ESP_ON and MOVED_ONCE[plr] then
					if not ESP_CACHE[plr] then
						ESP_CACHE[plr] = createLine()
					end

					local head = plr.Character:FindFirstChild("Head")
					if head then
						local v1, s1 = Camera:WorldToViewportPoint(hrp.Position)
						local v2, s2 = Camera:WorldToViewportPoint(head.Position)
						if s1 and s2 then
							local line = ESP_CACHE[plr]
							line.From = Vector2.new(v1.X, v1.Y)
							line.To   = Vector2.new(v2.X, v2.Y)
							line.Visible = true
						end
					end
				end
			end
		end
	end
end)

--====================================================
-- BOTÕES
--====================================================
AimBtn.MouseButton1Click:Connect(function()
	AIMBOT_ON = not AIMBOT_ON
	AimBtn.Text = AIMBOT_ON and "AIMBOT: ON" or "AIMBOT: OFF"
end)

EspBtn.MouseButton1Click:Connect(function()
	ESP_ON = not ESP_ON
	EspBtn.Text = ESP_ON and "ESP: ON" or "ESP: OFF"
end)

TpBtn.MouseButton1Click:Connect(function()
	local target = getClosestPlayer()
	if target and target.Character then
		LocalPlayer.Character.HumanoidRootPart.CFrame =
			target.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-3)
	end
end)
