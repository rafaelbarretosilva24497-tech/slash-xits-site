local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Window = Rayfield:CreateWindow({
	Name = "👻 Ghost Hub",
	LoadingTitle = "Ghost Hub",
	LoadingSubtitle = "ESP & Aimbot",
	ConfigurationSaving = {Enabled = false}
})

local function RGB(t)
	return Color3.fromHSV((tick()*t)%1,1,1)
end

spawn(function()
	while task.wait() do
		pcall(function()
			Window.Title.TextColor3 = RGB(0.15)
		end)
	end
end)

local TabAimbot = Window:CreateTab("1 • Aimbot 👻", 4483362458)
local TabESP = Window:CreateTab("2 • ESP", 4483362458)
local TabConfig = Window:CreateTab("3 • Configuração", 4483362458)

local AimbotOn = false

TabAimbot:CreateToggle({
	Name = "Aimbot",
	CurrentValue = false,
	Callback = function(v)
		if v and not AimbotOn then
			AimbotOn = true
			loadstring(game:HttpGet("https://raw.githubusercontent.com/Joshingtonn123/JoshScript/refs/heads/main/SyrexhubSniperOrDie"))()
		end
	end
})

local PlayerESPOn = false
local CarESPOn = false
local PlayerESP = {}
local CarESP = {}

local Cars = {
	["Escalade"]="Cadillac Escalade",
	["Ferrari"]="Ferrari",
	["Purosangue"]="Ferrari Purosangue",
	["BMW"]="BMW",
	["X6"]="BMW X6M",
	["X7"]="BMW X7",
	["Lancer"]="Lancer Blindado",
	["Rolls"]="Rolls Royce",
	["G63"]="Mercedes G63",
	["Dodge"]="Dodge Charger",
	["488"]="Ferrari 488",
	["Africa"]="Africa Twin",
	["Hornet"]="Hornet",
	["R1300"]="BMW R1300"
}

local function Dist(pos)
	if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
		return 0
	end
	return math.floor((LocalPlayer.Character.HumanoidRootPart.Position - pos).Magnitude)
end

local function PlayerESPCreate(p)
	if p == LocalPlayer then return end
	if p.Team == LocalPlayer.Team then return end
	if not p.Character or not p.Character:FindFirstChild("HumanoidRootPart") then return end
	if PlayerESP[p] then return end

	local g = Instance.new("BillboardGui", p.Character.HumanoidRootPart)
	g.Size = UDim2.new(0,200,0,40)
	g.StudsOffset = Vector3.new(0,3,0)
	g.AlwaysOnTop = true

	local t = Instance.new("TextLabel", g)
	t.Size = UDim2.new(1,0,1,0)
	t.BackgroundTransparency = 1
	t.TextScaled = true
	t.Font = Enum.Font.GothamBold
	t.TextColor3 = Color3.fromRGB(255,70,70)

	PlayerESP[p] = {g,t}
end

local function CarESPCreate(m)
	if not m:IsA("Model") then return end
	if not m.PrimaryPart then return end
	if CarESP[m] then return end

	local name = nil
	for k,v in pairs(Cars) do
		if string.find(string.lower(m.Name), string.lower(k)) then
			name = v
		end
	end
	if not name then return end

	local g = Instance.new("BillboardGui", m.PrimaryPart)
	g.Size = UDim2.new(0,220,0,40)
	g.StudsOffset = Vector3.new(0,4,0)
	g.AlwaysOnTop = true

	local t = Instance.new("TextLabel", g)
	t.Size = UDim2.new(1,0,1,0)
	t.BackgroundTransparency = 1
	t.TextScaled = true
	t.Font = Enum.Font.GothamBold
	t.TextColor3 = Color3.fromRGB(255,215,0)

	CarESP[m] = {g,t,name}
end

TabESP:CreateToggle({
	Name = "ESP Player (Inimigos)",
	CurrentValue = false,
	Callback = function(v)
		PlayerESPOn = v
		if not v then
			for _,d in pairs(PlayerESP) do d[1]:Destroy() end
			PlayerESP = {}
		end
	end
})

TabESP:CreateToggle({
	Name = "ESP Car",
	CurrentValue = false,
	Callback = function(v)
		CarESPOn = v
		if not v then
			for _,d in pairs(CarESP) do d[1]:Destroy() end
			CarESP = {}
		end
	end
})

RunService.RenderStepped:Connect(function()
	if PlayerESPOn then
		for _,p in pairs(Players:GetPlayers()) do
			if not PlayerESP[p] then
				PlayerESPCreate(p)
			end
			if PlayerESP[p] and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
				PlayerESP[p][2].Text = p.Name.." | "..Dist(p.Character.HumanoidRootPart.Position).."m"
			end
		end
	end

	if CarESPOn then
		for _,m in pairs(workspace:GetChildren()) do
			if not CarESP[m] then
				CarESPCreate(m)
			end
			if CarESP[m] then
				CarESP[m][2].Text = CarESP[m][3].." | "..Dist(m.PrimaryPart.Position).."m"
			end
		end
	end
end)

TabConfig:CreateButton({
	Name = "Fechar Menu",
	Callback = function()
		Rayfield:Destroy()
	end
})# slash-xits-site
