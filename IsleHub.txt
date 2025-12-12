if not game:IsLoaded() then warn("Waiting till the game is loaded...") game.IsLoaded:Wait() end
-- Player
local LocalPlayer = game:GetService("Players").LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")
if PlayerGui:FindFirstChild("IsleHub") then
	return
elseif PlayerGui:FindFirstChild("HubGuiSmall") then
	warn("Click hub's small icon to open the hub. Rejoin to fix any problems.")
	return
end
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HRP = Character:WaitForChild("HumanoidRootPart")

-- Folders
local AIHunter = workspace.AIHunter
local Map = workspace.Map

-- Services
local TweenService = game:GetService("TweenService")

-- Script

-- Misc
local GameVersion = "V:10.1, GameEvent:'Winter', Updated:"
local HubVersion = "1.0.3" -- 1.10.0 -> 2.0.0; 1.0.10 -> 1.1.0
local UpdateLog = "New buttons, test"
local BigNews = "Fixed hub's buttons position logic"
local Warning = "none"

-- Functions archive
-- -- UI editors
local function AddUICorner(obj, CornerRadius)
    local uiCorner = Instance.new("UICorner")
    uiCorner.Parent = obj
    uiCorner.CornerRadius = CornerRadius
end
local function AddUIListLayout(Parent, Name, Padding, FillDirection, HorizontalAlignment, SortOrder, VerticalAlignment)
	local newList = Instance.new("UIListLayout")
	newList.Parent = Parent
	newList.Name = Name
	newList.Padding = Padding
	newList.FillDirection = FillDirection
	newList.HorizontalAlignment = HorizontalAlignment
	newList.SortOrder = SortOrder
	newList.VerticalAlignment = VerticalAlignment
	return newList
end
local function AddUIGridLayout(Parent, Name, CellPadding, CellSize, FillDirection, FillDirectionMaxCells, HorizontalAlignment, SortOrder, StartCorner, VerticalAlignment)
	local newGrid = Instance.new("UIGridLayout")
	newGrid.Parent = Parent
	newGrid.Name = Name
	newGrid.CellPadding = CellPadding
	newGrid.CellSize = CellSize
	newGrid.FillDirection = FillDirection
	newGrid.FillDirectionMaxCells = FillDirectionMaxCells
	newGrid.HorizontalAlignment = HorizontalAlignment
	newGrid.SortOrder = SortOrder
	newGrid.StartCorner = StartCorner
	newGrid.VerticalAlignment = VerticalAlignment
	return newGrid
end
local function AddUIPadding(Parent, Name, PaddingBottom, PaddingLeft, PaddingRight, PaddingTop)
	local newUIPadding = Instance.new("UIPadding")
	newUIPadding.Parent = Parent
	if Name ~= nil then
		newUIPadding.Name = Name
	end
	newUIPadding.PaddingBottom = PaddingBottom or UDim.new(0, 0)
    newUIPadding.PaddingLeft   = PaddingLeft   or UDim.new(0, 0)
    newUIPadding.PaddingRight  = PaddingRight  or UDim.new(0, 0)
    newUIPadding.PaddingTop    = PaddingTop    or UDim.new(0, 0)
end
local function AddUIDragDetector(Parent, Name)
	local newUIDragDetector = Instance.new("UIDragDetector")
	newUIDragDetector.Parent = Parent
	if Name ~= nil then
		newUIDragDetector.Name = Name
	end
end
local function AddBasicAttributes(instance, Parent, Name, Size, AnchorPoint, Position, BackgroundColor3, BackgroundTransparency, addUICorner, CornerRadius, TextScaled, TextColor3, Text, LayoutOrder)
	local newObj = instance
	newObj.Parent = Parent
	newObj.Name = Name
	newObj.Size = Size
	newObj.AnchorPoint = AnchorPoint
	newObj.BackgroundColor3 = BackgroundColor3
	newObj.BackgroundTransparency = BackgroundTransparency
	if addUICorner then
		AddUICorner(newObj, CornerRadius)
	end
	if not newObj.Parent:FindFirstChildOfClass("UIListLayout") or not newObj.Parent:FindFirstChildOfClass("UIListLayout") then
		newObj.Position = Position
	end
	if instance:IsA("TextButton") or instance:IsA("TextLabel") then
		newObj.TextScaled = TextScaled
		newObj.TextColor3 = TextColor3
		newObj.Text = Text
		newObj.LayoutOrder = LayoutOrder
		return newObj
	end
	if instance:GetAttribute("LayoutOrder") then
		newObj.LayoutOrder = LayoutOrder
	end
	return newObj
end
local function AddButton(Name, Text, Parent, LayoutOrder)-- Position,
    local newButton = Instance.new("TextButton")
    newButton.Parent = Parent
    newButton.Name = Name
    newButton.Size = UDim2.new(0.2, 0, 0.1, 0)
	--if Position ~= nil then newButton.Position = Position end
    newButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    newButton.BackgroundTransparency = 0.2
    newButton.TextScaled = true
	newButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    newButton.Text = Text
    newButton.LayoutOrder = LayoutOrder
    AddUICorner(newButton, UDim.new(0, 8))
    return newButton
end
local function AddTextBox(Name, PlaceholderText, Parent, LayoutOrder)
    local textBox = Instance.new("TextBox")
    textBox.Parent = Parent
    textBox.Name = Name
	textBox.PlaceholderText = PlaceholderText
    textBox.Size = UDim2.new(0.2, 0, 0.1, 0)
    textBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    textBox.BackgroundTransparency = 0.2
	textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
    textBox.TextScaled = true
	textBox.Text = ""
    textBox.LayoutOrder = LayoutOrder
    AddUICorner(textBox, UDim.new(0, 4))
    return textBox
end
local function AddScreenGui(Parent, Name, ResetOnSpawn, IgnoreGuiInset)
	local newGui = Instance.new("ScreenGui")
	newGui.Parent = Parent
	newGui.Name = Name
	newGui.ResetOnSpawn = ResetOnSpawn
	newGui.IgnoreGuiInset = IgnoreGuiInset
	return newGui
end
local function AddDropdown(Parent, Name, Size, AnchorPoint, Position, DropdownBackgroundColor3, DropDownBackgroundTransparency, DropdownTextColor3, DropdownText, DropdownLayoutOrder, DropdownAddUICorner, ListBackgroundColor3, ListBackgroundTransparency, ListAddUICorner, varsAmout, varBackgroundColor3, varBackgroundTransparency, varTextColor3, varTexts, varAddUICorner)
	local newDropdown = Instance.new("TextButton")
	newDropdown.Parent = Parent
	newDropdown.Name = Name
	newDropdown.Size = Size
	newDropdown.AnchorPoint = AnchorPoint
	if not Position == nil then newDropdown.Position = Position end
	newDropdown.BackgroundColor3 = DropdownBackgroundColor3
	newDropdown.BackgroundTransparency = DropDownBackgroundTransparency
	newDropdown.TextColor3 = DropdownTextColor3
	newDropdown.TextScaled = true
	newDropdown.Text = DropdownText
	newDropdown.LayoutOrder = DropdownLayoutOrder
	if DropdownAddUICorner then AddUICorner(newDropdown, UDim.new(0, 8)) end
	local List = Instance.new("ScrollingFrame")
	List.Parent = newDropdown
	List.Name = "List"
	List.Size = UDim2.new(1, 0, 0, 0)
	List.Visible = false
	List.Position = UDim2.new(0, 0, 1, 0)
	List.BackgroundColor3 = ListBackgroundColor3
	List.BackgroundTransparency = ListBackgroundTransparency
	List.AutomaticCanvasSize = Enum.AutomaticSize.Y
	List.ScrollingDirection = Enum.ScrollingDirection.Y
	List.ScrollBarImageTransparency = 0.7
	List.ScrollBarThickness = 7
	if ListAddUICorner then
		AddUICorner(List, UDim.new(0, 8))
	end
	local UIGridLayout = AddUIGridLayout(List, "UIGridLayout", UDim2.new(0, 5, 0, 5), UDim2.new(0.9, 0, 0.1, 0), Enum.FillDirection.Horizontal, 1, Enum.HorizontalAlignment.Center, Enum.SortOrder.LayoutOrder, Enum.StartCorner.TopLeft, Enum.VerticalAlignment.Top)
	for i = 1, varsAmout do
		local var = Instance.new("TextButton")
		var.Parent = List
		var.Name = i
		var.Size = UDim2.new(0.9, 0, 0.1, 0)
		var.Position = UDim2.new(0.05, 0, 0.05, 0)
		var.BackgroundColor3 = varBackgroundColor3
		var.BackgroundTransparency = varBackgroundTransparency
		var.TextColor3 = varTextColor3
		var.TextScaled = true
		var.Text = varTexts[i]
		var.LayoutOrder = i
		if varAddUICorner then AddUICorner(var, UDim.new(0, 8)) end
	end
	return newDropdown
end
local function AddDraggableWindow(WindowInstance, CreateGui, GuiName, WindowParent, Size, Position, WindowBackgroundColor3, WindowBackgroundTransparency, WindowAddUICorner, varsAmout, varBackgroundColor3, varBackgroundTransparency, varTextColor3, varTexts, varAddUICorner)
	local newGui
	local newWindow = WindowInstance
	if CreateGui then
		newGui = AddScreenGui(PlayerGui, GuiName, false, true)
		newWindow.Parent = newGui
	else
		newWindow.Parent = WindowParent
	end
	newWindow.Name = "Window"
	newWindow.Size = Size
	newWindow.Position = Position
	newWindow.BackgroundColor3 = WindowBackgroundColor3
	newWindow.BackgroundTransparency = WindowBackgroundTransparency
	newWindow.AutomaticCanvasSize = Enum.AutomaticSize.Y
	newWindow.ScrollingDirection = Enum.ScrollingDirection.Y
	newWindow.ScrollBarImageTransparency = 0.7
	newWindow.ScrollBarThickness = 7
	if WindowAddUICorner then AddUICorner(newWindow, UDim.new(0, 8)) end
	local UIDragDetector = AddUIDragDetector(newWindow, nil)
	local UIGridLayout = AddUIGridLayout(newWindow, "UIGridLayout", UDim2.new(0, 5, 0, 5), UDim2.new(0.9, 0, 0.1, 0), Enum.FillDirection.Horizontal, 1, Enum.HorizontalAlignment.Center, Enum.SortOrder.LayoutOrder, Enum.StartCorner.TopLeft, Enum.VerticalAlignment.Top)
	local UIPadding = Instance.new("UIPadding")
	UIPadding.Parent = newWindow
	UIPadding.PaddingTop = UDim.new(0.01, 0)
	for i = 1, varsAmout do
		local var = Instance.new("TextButton")
		var.Parent = newWindow
		var.Name = i
		var.Size = UDim2.new(0.9, 0, 0.1, 0)
		var.Position = UDim2.new(0.05, 0, 0.05, 0)
		var.BackgroundColor3 = varBackgroundColor3
		var.BackgroundTransparency = varBackgroundTransparency
		var.TextColor3 = varTextColor3
		var.TextScaled = true
		var.Text = varTexts[i]
		var.LayoutOrder = i
		if varAddUICorner then AddUICorner(var, UDim.new(0, 8)) end
	end
	if CreateGui then return newGui else return newWindow end
end

-- Gui
local HubGui = AddScreenGui(PlayerGui, "IsleHub", false, true)
local Header = AddBasicAttributes(Instance.new("Frame"), HubGui, "Header", UDim2.new(0.8, 0, 0.2, 0), Vector2.new(0.5, 1), UDim2.new(0.5, 0, 0.2, 0), Color3.fromRGB(50, 50, 50), 0.2)--, addUICorner, CornerRadius, TextScaled, TextColor3, Text, LayoutOrder
local Base = AddBasicAttributes(Instance.new("Frame"), HubGui, "Base", UDim2.new(0.8, 0, 0.8, 0), Vector2.new(0.5, 0.5), UDim2.new(0.5, 0, 0.6, 0), Color3.fromRGB(50, 50, 50), 0.1)--, addUICorner, CornerRadius, TextScaled, TextColor3, Text, LayoutOrder
--local UIListLayoutHeader = AddUIListLayout(Header, "HeaderUIListLayout", UDim.new(0.05, 0), Enum.FillDirection.Horizontal, Enum.HorizontalAlignment.Left, Enum.SortOrder.LayoutOrder, Enum.VerticalAlignment.Center)
--local UIListLayoutBase = AddUIListLayout(Base, "BaseUIListLayout", UDim.new(0.01, 0), Enum.FillDirection.Vertical, Enum.HorizontalAlignment.Left, Enum.SortOrder.LayoutOrder, Enum.VerticalAlignment.Center)
local BaseUIGridLayout = AddUIGridLayout(Base, "BaseUIGridLayout", UDim2.new(0, 5, 0, 5), UDim2.new(0.2, 0, 0.1, 0), Enum.FillDirection.Horizontal, 0, Enum.HorizontalAlignment.Center, Enum.SortOrder.LayoutOrder, Enum.StartCorner.TopLeft, Enum.VerticalAlignment.Top)
local HubNameLabel = AddBasicAttributes(Instance.new("TextLabel"), Header, "HubName", UDim2.new(0.2, 0, 0.8, 0), Vector2.new(0, 0.5), UDim2.new(0.02, 0, 0.5, 0), Color3.fromRGB(40, 40, 40), 0.2, true, UDim.new(0.5, 0), true, Color3.fromRGB(255, 255, 255), "IsleHub", 1)
local PrintInfoButton = AddBasicAttributes(Instance.new("TextButton"), Header, "PrintInfo", UDim2.new(0.1, 0, 0.5, 0), Vector2.new(0, 0.5), UDim2.new(0.25, 0, 0.5, 0), Color3.fromRGB(40, 40, 40), 0.2, true, UDim.new(0.5, 0), true, Color3.fromRGB(255, 255, 255), "PrintInfo", 2)
local CloseButton = AddBasicAttributes(Instance.new("TextButton"), Header, "Close", UDim2.new(0.2, 0, 0.4, 0), Vector2.new(1, 0), UDim2.new(0.95, 0, 0.05, 0), Color3.fromRGB(255, 0, 0), 0.2, true, UDim.new(0, 8), true, Color3.fromRGB(255, 255, 255), "X", 4)
local CrashButton = AddBasicAttributes(Instance.new("TextButton"), Header, "Crash", UDim2.new(0.2, 0, 0.4, 0), Vector2.new(1, 0), UDim2.new(0.95, 0, 0.55, 0), Color3.fromRGB(255, 0, 0), 0.2, true, UDim.new(0, 8), true, Color3.fromRGB(255, 255, 255), "Crash", 5)

-- Add buttons
local RemoveTrees = AddButton("RemoveTrees", "RemoveTrees", Base, 1)
local RemoveVegetation = AddButton("RemoveVegetation", "RemoveVegetation", Base, 2)
local RemoveVegetationThick = AddButton("RemoveVegetationThick", "RemoveVegetationThick", Base, 3)
local RemoveDartTraps = AddButton("RemoveDartTraps", "RemoveDartTraps", Base, 4)
local PrintMercsLocation = AddButton("PrintMercsLocation", "PrintMercsLocation", Base, 5)
local PrintMercsDistance = AddButton("PrintMercsDistance", "PrintMercsDistance", Base, 6)
local PrintMyStats = AddButton("PrintMyStats", "PrintMyStats", Base, 7)
local ActivateButtons = AddButton("ActivateButtons", "ActivateButtons", Base, 8)
local GotoPeanutsBox = AddButton("GotoPeanutsBox", "GotoPeanutsBox", Base, 9)
local GotoWaterPylon = AddButton("GotoWaterPylon", "GotoWaterPylon", Base, 10)
local GotoCrates = AddButton("GotoCrates", "GotoCrates", Base, 11)
--local GotoPlace = AddDropdown(Base, "GotoPlace", UDim2.new(0.2, 0, 0.1, 0), Vector2.new(0, 0), nil, Color3.fromRGB(50, 50, 50), 0.2, Color3.new(255, 255, 255), "GotoPlace", 12, true, Color3.fromRGB(50, 50, 50), 0, true, 3, Color3.fromRGB(40, 40, 40), 0, Color3.fromRGB(255, 255, 255), {"Ship", "Generators", "Lighthouse"}, true)
--local GotoPlace = AddDraggableWindow(Instance.new("ScrollingFrame"), true, "GotoPlace", nil, UDim2.new(0.2, 0, 0.3, 0), UDim2.new(0, 0, 0, 0), Color3.fromRGB(50, 50, 50), 0.2, true, 3, Color3.fromRGB(50, 50, 50), 0.1, Color3.fromRGB(255, 255, 255), {"Ship", "Generators", "Lighthouse"}, true)
local GotoPlace = AddBasicAttributes(Instance.new("TextButton"), Base, "GotoPlace", UDim2.new(0.2, 0, 0.1, 0), Vector2.new(0, 0), UDim2.new(0, 0, 0, 0), Color3.fromRGB(50, 50, 50), 0.2, true, UDim.new(0, 8), true, Color3.fromRGB(255, 255, 255), "GotoPlace", 12)

-- Button click
PrintInfoButton.MouseButton1Click:Connect(function()
    print(GameVersion)
    print(HubVersion)
    print(UpdateLog)
	print(Warning)
end)
CloseButton.MouseButton1Click:Connect(function()
	if HubGui:IsDescendantOf(PlayerGui) then
		HubGui.Enabled = false
		if PlayerGui:FindFirstChild("HubGuiSmall") then warn("Click hub's small icon to open the hub. Rejoin to fix any problems.") return end

		local HubGuiSmall = AddScreenGui(PlayerGui, "HubGuiSmall", false, true)
		--[[local HubGuiSmall = Instance.new("ScreenGui")
		HubGuiSmall.Parent = PlayerGui
		HubGuiSmall.Name = "HubGuiSmall"
		HubGuiSmall.ResetOnSpawn = false
		HubGuiSmall.IgnoreGuiInset = true]]
		local IconOpenButton = AddBasicAttributes(Instance.new("ImageButton"), HubGuiSmall, "HubGuiSmall", UDim2.new(0, 50, 0, 50), Vector2.new(0.5, 0), UDim2.new(0.5, 0, 0.05, 0), Color3.fromRGB(50, 50, 50), 0, true, UDim.new(1, 0), false, Color3.fromRGB(0, 0, 0), "", 1)
		IconOpenButton.MouseButton1Click:Connect(function()
			HubGuiSmall:Destroy()
			if not HubGui.Enabled then HubGui.Enabled = true end
		end)
	else
		error(HubGui.Name.." not found.")
	end
end)
CrashButton.MouseButton1Click:Connect(function()
	if HubGui then
		HubGui:Destroy()
	end
end)
-- -- other
RemoveTrees.MouseButton1Click:Connect(function()
	local CollideTrees = Map.Ignore:WaitForChild("CollideTrees")
	if #CollideTrees > 0 then
		for _, tree in pairs(CollideTrees:GetChildren()) do
			tree:Destroy()
		end
	else
		warn("No collide trees left to destroy")
	end
	local NoCollideTrees = Map.Ignore:WaitForChild("NoCollideTrees")
	if #NoCollideTrees > 0 then
		for _, tree in pairs(NoCollideTrees:GetChildren()) do
			if tree.Name == "#Extras" then continue end
			tree:Destroy()
		end
	else
		warn("No no-collide trees left to destroy")
	end
end)
RemoveVegetation.MouseButton1Click:Connect(function()
	local Vegetation = Map.Ignore:WaitForChild("Vegetation")
	if #Vegetation > 0 then
		for _, grass in pairs(Vegetation:GetChildren()) do
			if grass.Name == "#Extras" then continue end
			grass:Destroy()
		end
	else
		warn("No vegetation left to destroy")
	end
end)
RemoveVegetationThick.MouseButton1Click:Connect(function()
	local VegetatonThick = Map.Ignore:WaitForChild("VegetationThick")
	if #VegetatonThick > 0 then
		for _, bush in pairs(VegetationThick:GetChildren()) do
			if bush.Name == "#Extras" then continue end
			bush:Destroy()
		end
	else
		warn("No vegetation thick left to destroy")
	end
end)
RemoveDartTraps.MouseButton1Click:Connect(function()
	local Props = Map.Ignore:WaitForChild("Props")
	if #Props > 0 then
		for _, obj in pairs(Props:GetChildren()) do
			if obj.Name == "Dart Trap" then
				obj:Destroy()
			end
		end
	else
		warn("No dart traps left to destroy")
	end
end)
PrintMercsLocation.MouseButton1Click:Connect(function()
	local Location = AIHunter:WaitForChild("Location")
	if Location.Value then
		print("Mercenaries location: "..Location.Value.Name)
	else
		warn("No mercenaries' location found.")
	end
	
end)
PrintMercsDistance.MouseButton1Click:Connect(function()
	local mercs = {}
	for _, merc in pairs(AIHunter:GetChildren()) do
		if merc.Name == "Helicopter" then continue end
		if merc:IsA("Model") then
			if merc:FindFirstChild("HumanoidRootPart") then
				local Distance = (merc.HumanoidRootPart.Position - HRP.Position).Magnitude
				print(merc.Name.." is in "..math.round(Distance).." studs")
				table.insert(mercs, merc.Name)
			else
				warn(merc.Name.." HumanoidRootPart not found")
			end
		end
	end
	if #mercs == 0 then warn("There are no mercenaries at the moment") end
end)
PrintMyStats.MouseButton1Click:Connect(function()
	local Statistics = workspace.GameState.Statistics
	local LocalStats = Statistics:FindFirstChild(LocalPlayer.Name)
	if LocalStats then
		print("Distane traveled: "..LocalStats["Distance Traveled"].Value)
		print("Players killed: "..LocalStats["Players Killed"].Value)
		print("Score: "..LocalStats.Score.Value)
		print("Tank oxygen consumed: "..LocalStats["Tank Oxygen Consumed"].Value)
	else
		error(LocalPlayer.Name.." stats folder not found")
	end
end)
GotoPeanutsBox.MouseButton1Click:Connect(function()
	warn("W.I.P")
end)
ActivateButtons.MouseButton1Click:Connect(function()
	warn("W.I.P")
end)
GotoWaterPylon.MouseButton1Click:Connect(function()
	warn("W.I.P")
end)
GotoCrates.MouseButton1Click:Connect(function()
	local tweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.In, 0, false, 0)
	for _, obj in pairs(workspace:GetChildren()) do
		if obj.Name == "Crate" then
			local tween = TweenService:Create(HRP, tweenInfo, {CFrame = obj.PrimaryPart.CFrame})
			tween:Play()
			tween.Completed:Wait()
			warn("Open the chest in 10 seconds!")
			task.wait(10)
		else
			continue
		end
	end
end)
--[[GotoPlace.MouseButton1Click:Connect(function()
	--local size = #vars * vars[1].Size
	local List = GotoPlace:WaitForChild("List")
	if List.Size == UDim2.new(1, 0, 0, 0) and List.Visible == false then
		List.Size = UDim2.new(1, 0, 0, 200)
		List.Visible = true
	else
		List.Size = UDim2.new(1, 0, 0, 0)
		List.Visible = false
	end
	local vars = {}
	for _, obj in pairs(GotoPlace:GetChildren()) do
		if  obj == List or obj:IsA("UIGridLayout") then continue end
		table.insert(vars, obj)
	end
	local Place
	local tweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.In, 0, false, 0)
	local tween = TweenService:Create(HRP, tweenInfo, {CFrame = Place})
	for _, button in pairs(vars) do
		if button:IsA("UICorner") then continue end
		button.MouseButton1Click:Connect(function()
			if button.Text == "Ship" then
				Place = nil
				tween:Play()
			elseif button.Text == "Generators" then
				Place = nil
				tween:Play()
			elseif button.Text == "Lighthouse" then
				Place = nil
				tween:Play()
			else
				error(button.Name.." button in "..GotoPlace.Name.." doesn't have any text")
			end
		end)
	end
end)]]
GotoPlace.MouseButton1Click:Connect(function()
	local GotoPlaceGui = PlayerGui:FindFirstChild("GotoPlace")
	if GotoPlaceGui then GotoPlaceGui:Destroy() return end
	local List = AddDraggableWindow(Instance.new("ScrollingFrame"), true, "GotoPlace", nil, UDim2.new(0.2, 0, 0.3, 0), UDim2.new(0, 0, 0, 0), Color3.fromRGB(50, 50, 50), 0.2, true, 3, Color3.fromRGB(50, 50, 50), 0.1, Color3.fromRGB(255, 255, 255), {"Ship", "Generators", "Lighthouse"}, true)
	
	local vars = {}
	for _, obj in pairs(List:GetChildren()) do
		if obj:IsA("UIGridLayout") or obj:IsA("ScrollingFrame") then continue end
		table.insert(vars, obj)
	end
	local Place
	local tweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.In, 0, false, 0)
	local tween = TweenService:Create(HRP, tweenInfo, {CFrame = Place})
	for _, button in pairs(vars) do
		if button:IsA("UICorner") then continue end
		button.MouseButton1Click:Connect(function()
			if button.Text == "Ship" then
				Place = nil
				tween:Play()
			elseif button.Text == "Generators" then
				Place = nil
				tween:Play()
			elseif button.Text == "Lighthouse" then
				Place = nil
				tween:Play()
			else
				error(button.Name.." button in "..GotoPlace.Name.." doesn't have any text")
			end
		end)
	end
end)
