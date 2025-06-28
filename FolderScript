local TweenService = game:GetService("TweenService")
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "ScriptViewerV6.0"
gui.ResetOnSpawn = false

local darkMode = true

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 850, 0, 500)
main.Position = UDim2.new(0, 60, 0, 60)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true

local header = Instance.new("Frame", main)
header.Size = UDim2.new(1, 0, 0, 40)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -40, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "ScriptViewer V6.0"
title.Font = Enum.Font.SourceSansSemibold
title.TextSize = 22
title.TextXAlignment = Enum.TextXAlignment.Left

local minimize = Instance.new("TextButton", header)
minimize.Size = UDim2.new(0, 30, 0, 30)
minimize.Position = UDim2.new(1, -35, 0.5, -15)
minimize.Text = "-"
minimize.Font = Enum.Font.SourceSansBold
minimize.TextSize = 22

local contentHolder = Instance.new("Frame", main)
contentHolder.Size = UDim2.new(1, 0, 1, -40)
contentHolder.Position = UDim2.new(0, 0, 0, 40)
contentHolder.BackgroundTransparency = 1

local buttonFrame = Instance.new("Frame", contentHolder)
buttonFrame.Position = UDim2.new(0, 10, 0, 10)
buttonFrame.Size = UDim2.new(0, 260, 0, 40)
buttonFrame.BackgroundTransparency = 1

local uiList = Instance.new("UIListLayout", buttonFrame)
uiList.FillDirection = Enum.FillDirection.Horizontal
uiList.Padding = UDim.new(0, 8)
uiList.SortOrder = Enum.SortOrder.LayoutOrder

local function createButton(text)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 80, 1, 0)
	btn.Text = text
	btn.Font = Enum.Font.SourceSans
	btn.TextSize = 16
	btn.Parent = buttonFrame
	return btn
end

local runBtn = createButton("Ejecutar")
local clearBtn = createButton("Limpiar")
local copyBtn = createButton("Copiar")
local temaBtn = createButton("Tema")

local container = Instance.new("Frame", contentHolder)
container.Position = UDim2.new(0, 10, 0, 60)
container.Size = UDim2.new(1, -20, 1, -70)
container.BorderSizePixel = 0

local lineNumbers = Instance.new("TextLabel")
lineNumbers.Size = UDim2.new(0, 40, 0, 2000)
lineNumbers.Position = UDim2.new(0, 0, 0, 0)
lineNumbers.Text = "1"
lineNumbers.TextXAlignment = Enum.TextXAlignment.Right
lineNumbers.TextYAlignment = Enum.TextYAlignment.Top
lineNumbers.Font = Enum.Font.Code
lineNumbers.TextSize = 14
lineNumbers.BackgroundTransparency = 1
lineNumbers.TextColor3 = Color3.fromRGB(150, 150, 150)
lineNumbers.Parent = nil

local scroll = Instance.new("ScrollingFrame", container)
scroll.Position = UDim2.new(0, 0, 0, 0)
scroll.Size = UDim2.new(1, 0, 1, 0)
scroll.CanvasSize = UDim2.new(0, 0, 0, 2000)
scroll.ScrollBarThickness = 6
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0
scroll.ClipsDescendants = true

lineNumbers.Parent = scroll

local codeBox = Instance.new("TextBox")
codeBox.Size = UDim2.new(1, -45, 0, 2000)
codeBox.Position = UDim2.new(0, 45, 0, 0)
codeBox.TextXAlignment = Enum.TextXAlignment.Left
codeBox.TextYAlignment = Enum.TextYAlignment.Top
codeBox.BackgroundTransparency = 1
codeBox.ClearTextOnFocus = false
codeBox.MultiLine = true
codeBox.Text = ""
codeBox.Font = Enum.Font.Code
codeBox.TextSize = 14
codeBox.TextColor3 = Color3.new(1, 1, 1)
codeBox.ZIndex = 2
codeBox.TextTransparency = 1
codeBox.Parent = scroll

local highlight = Instance.new("TextLabel", scroll)
highlight.Size = codeBox.Size
highlight.Position = codeBox.Position
highlight.BackgroundTransparency = 1
highlight.TextXAlignment = Enum.TextXAlignment.Left
highlight.TextYAlignment = Enum.TextYAlignment.Top
highlight.Font = codeBox.Font
highlight.TextSize = codeBox.TextSize
highlight.RichText = true
highlight.TextColor3 = Color3.fromRGB(180, 180, 180)
highlight.ZIndex = 1
highlight.Text = ""

-- Palabras y categorías
local palabrasReservadas = {
    ["and"]=true,["break"]=true,["do"]=true,["else"]=true,["elseif"]=true,
    ["end"]=true,["false"]=true,["for"]=true,["function"]=true,["if"]=true,
    ["in"]=true,["local"]=true,["nil"]=true,["not"]=true,["or"]=true,
    ["repeat"]=true,["return"]=true,["then"]=true,["true"]=true,["until"]=true,["while"]=true
}
local funcionesBuiltIn = {
    ["print"]=true,["warn"]=true,["error"]=true,["wait"]=true,
    ["spawn"]=true,["pcall"]=true,["xpcall"]=true,["loadstring"]=true,
    ["type"]=true,["typeof"]=true,["pairs"]=true,["ipairs"]=true
}
local objetosRoblox = {
    ["game"]=true,["workspace"]=true,["script"]=true,["Instance"]=true,
    ["Vector3"]=true,["Color3"]=true,["CFrame"]=true
}
local metodosComunes = {
    ["FindFirstChild"]=true,["WaitForChild"]=true,["Clone"]=true,
    ["Destroy"]=true,["new"]=true,["IsA"]=true
}

local function escaparHTML(str)
	return str:gsub("&", "&amp;"):gsub("<", "&lt;"):gsub(">", "&gt;")
end

local function aplicarColores(texto)
	local resultado = ""
	for linea in texto:gmatch("[^\r\n]+") do
		local lineaResaltada = ""
		local pos = 1
		while pos <= #linea do
			local longComment = linea:match("^%-%-%[%[.-%]%]", pos)
			if longComment then
				lineaResaltada ..= '<font color="#888888">' .. escaparHTML(longComment) .. '</font>'
				pos += #longComment
			elseif linea:sub(pos, pos+1) == "--" then
				local comentario = linea:sub(pos)
				lineaResaltada ..= '<font color="#888888">' .. escaparHTML(comentario) .. '</font>'
				break
			elseif linea:sub(pos, pos) == '"' then
				local fin = linea:find('"', pos + 1) or (#linea + 1)
				local cadena = linea:sub(pos, fin)
				lineaResaltada ..= '<font color="#00FF00">' .. escaparHTML(cadena) .. '</font>'
				pos += #cadena
			elseif linea:sub(pos, pos) == "'" then
				local fin = linea:find("'", pos + 1) or (#linea + 1)
				local cadena = linea:sub(pos, fin)
				lineaResaltada ..= '<font color="#00FF00">' .. escaparHTML(cadena) .. '</font>'
				pos += #cadena
			elseif linea:sub(pos):match("^%d") then
				local numero = linea:match("^%d+%.?%d*", pos)
				lineaResaltada ..= '<font color="#FFD700">' .. numero .. '</font>'
				pos += #numero
			else
				local palabra, simbolos = linea:match("([_%a][_%w]*)([^_%w]*)", pos)
				if not palabra then
					lineaResaltada ..= escaparHTML(linea:sub(pos))
					break
				end
				local palabraEsc = escaparHTML(palabra)
				local simbolosEsc = escaparHTML(simbolos or "")
				if palabrasReservadas[palabra] then
					lineaResaltada ..= '<font color="#00A2FF">' .. palabraEsc .. '</font>'
				elseif funcionesBuiltIn[palabra] then
					lineaResaltada ..= '<font color="#FFB347">' .. palabraEsc .. '</font>'
				elseif objetosRoblox[palabra] then
					lineaResaltada ..= '<font color="#FF66CC">' .. palabraEsc .. '</font>'
				elseif metodosComunes[palabra] then
					lineaResaltada ..= '<font color="#7DF9FF">' .. palabraEsc .. '</font>'
				else
					lineaResaltada ..= palabraEsc
				end
				lineaResaltada ..= simbolosEsc
				pos += #palabra + #simbolos
			end
		end
		resultado ..= lineaResaltada .. "\n"
	end
	return resultado
end

local function actualizar()
	local linesCount = select(2, codeBox.Text:gsub("\n", "\n")) + 1
	local lineText = ""
	for i = 1, linesCount do
		lineText ..= i .. "\n"
	end
	lineNumbers.Text = lineText

	local lineHeight = codeBox.TextSize + 2
	local totalHeight = linesCount * lineHeight

	codeBox.Size = UDim2.new(1, -45, 0, totalHeight)
	highlight.Size = codeBox.Size
	lineNumbers.Size = UDim2.new(0, 40, 0, totalHeight)
	scroll.CanvasSize = UDim2.new(0, 0, 0, totalHeight)

	highlight.Text = aplicarColores(codeBox.Text)
end

codeBox:GetPropertyChangedSignal("Text"):Connect(actualizar)

scroll:GetPropertyChangedSignal("CanvasPosition"):Connect(function()
	local scrollY = -scroll.CanvasPosition.Y
	highlight.Position = UDim2.new(0, 45, 0, scrollY)
	codeBox.Position = UDim2.new(0, 45, 0, scrollY)
	lineNumbers.Position = UDim2.new(0, 0, 0, scrollY)
end)

actualizar()

runBtn.MouseButton1Click:Connect(function()
	local success, err = pcall(function()
		loadstring(codeBox.Text)()
	end)
	if not success then
		warn("Error:", err)
	end
end)

clearBtn.MouseButton1Click:Connect(function()
	codeBox.Text = ""
end)

copyBtn.MouseButton1Click:Connect(function()
	if setclipboard then
		setclipboard(codeBox.Text)
	end
end)

local minimized = false
minimize.MouseButton1Click:Connect(function()
	minimized = not minimized
	local goalSize = minimized and UDim2.new(0, 850, 0, 40) or UDim2.new(0, 850, 0, 500)
	TweenService:Create(main, TweenInfo.new(0.3), {Size = goalSize}):Play()

	for _, v in pairs(contentHolder:GetChildren()) do
		if v:IsA("GuiObject") then
			v.Visible = not minimized
		end
	end
end)

local function aplicarTema()
	local fondo = darkMode and Color3.fromRGB(30,30,30) or Color3.fromRGB(240,240,240)
	local fondo2 = darkMode and Color3.fromRGB(45,45,45) or Color3.fromRGB(220,220,220)
	local texto = darkMode and Color3.fromRGB(255,255,255) or Color3.fromRGB(0,0,0)
	local texto2 = darkMode and Color3.fromRGB(150,150,150) or Color3.fromRGB(70,70,70)
	local boton = darkMode and Color3.fromRGB(60,60,60) or Color3.fromRGB(200,200,200)

	main.BackgroundColor3 = fondo
	header.BackgroundColor3 = fondo2
	title.TextColor3 = texto
	minimize.BackgroundColor3 = boton
	minimize.TextColor3 = texto

	codeBox.TextColor3 = texto
	lineNumbers.TextColor3 = texto2
	lineNumbers.BackgroundColor3 = darkMode and Color3.fromRGB(35,35,35) or Color3.fromRGB(230,230,230)
	container.BackgroundColor3 = fondo2

	for _, btn in pairs(buttonFrame:GetChildren()) do
		if btn:IsA("TextButton") then
			btn.BackgroundColor3 = boton
			btn.TextColor3 = texto
		end
	end
end

temaBtn.MouseButton1Click:Connect(function()
	darkMode = not darkMode
	aplicarTema()
end)

aplicarTema()
