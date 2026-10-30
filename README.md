local Lighting = game:GetService("Lighting")

--// SKYBOX
local skyData = {
    SkyboxBk = "rbxassetid://106771111577360",
    SkyboxDn = "rbxassetid://120543510604286",
    SkyboxFt = "rbxassetid://113323211871826",
    SkyboxLf = "rbxassetid://127625156082693",
    SkyboxRt = "rbxassetid://92126106430527",
    SkyboxUp = "rbxassetid://133072754041041"
}

--// TEXTURE REPLACEMENT
local OLD_ID = "7658055825"
local NEW_ID = "6372755229"

for _, object in ipairs(workspace:GetDescendants()) do
    if object:IsA("Texture") or object:IsA("Decal") then
        if string.find(object.Texture, OLD_ID, 1, true) then
            object.Texture = "rbxassetid://" .. NEW_ID
        end

    elseif object:IsA("SurfaceAppearance") then
        if string.find(object.ColorMap, OLD_ID, 1, true) then
            object.ColorMap = "rbxassetid://" .. NEW_ID
        end
    end
end

print("Texture replacement complete!")

--// APPLY SKYBOX
local function applySkybox()
    local sky = Lighting:FindFirstChildOfClass("Sky")

    if not sky then
        sky = Instance.new("Sky")
        sky.Parent = Lighting
    end

    for property, value in pairs(skyData) do
        sky[property] = value
    end
end

applySkybox()

--// KEEP SKYBOX ACTIVE
Lighting.ChildRemoved:Connect(function(child)
    if child:IsA("Sky") then
        task.wait()
        applySkybox()
    end
end)
