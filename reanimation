-- H3x0Rs Ownership Script v2.1.1 (http://bit.ly/gainownership)
local PlayerInstance;
local sethiddenprop = (sethiddenproperty or set_hidden_property or sethiddenprop or set_hidden_prop)
local setsimulationrad = setsimulationradius or set_simulation_radius or function(Radius) sethiddenprop(PlayerInstance, "SimulationRadius", Radius) end
if not getgenv or not sethiddenprop or not setsimulationrad then return false end -- Not supported
if NETWORKOWNER then NETWORKOWNER:Disconnect() NETWORKPLAYERCHECK:Disconnect() NETWORKPLAYERCHECK2:Disconnect() end
getgenv().NETWORK_RADIUS = NETWORK_RADIUS or math.huge

if not isfile("network-ownership.log") then
    writefile("network-ownership.log", "Script executed on game "..game.PlaceId.."!\n")
else
    appendfile("network-ownership.log", "Script executed on game "..game.PlaceId.."!\n")
end

if not game:IsLoaded() then
    appendfile("network-ownership.log", "Waiting for game to load...\n")
    game.Loaded:Wait() -- Wait for game
end

-- Grab services
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
PlayerInstance = Players.LocalPlayer

-- Optimize
local PlayerList = {}
for _, Plr in pairs(Players:GetPlayers()) do
    if Plr ~= PlayerInstance then
        PlayerList[Plr] = true
    end
end

getgenv().NETWORKPLAYERCHECK = Players.PlayerAdded:Connect(function(Plr)
    PlayerList[Plr] = true
end)

getgenv().NETWORKPLAYERCHECK2 = Players.PlayerRemoving:Connect(function(Plr)
    local Success, Err = pcall(function() PlayerList[Plr] = nil end)
    if not Success then
        appendfile("network-ownership.log", "Error while de-registering player that left: "..tostring(Err).."\n")
    end
end)

-- Configure network services
settings().Physics.AllowSleep = false -- Keep the current physics system from sleeping. (Non-moving parts lose ownership.)
settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled -- Keep the physics from throttling.

-- Start network runtime
getgenv().NETWORKOWNER = RunService.Stepped:Connect(function()
    -- Revoke ownership from others
    for Plr, _ in pairs(PlayerList) do
        sethiddenprop(Plr, "MaximumSimulationRadius", 0.01)
        sethiddenprop(Plr, "SimulationRadius", 0.01)
    end

    -- Claim ownership for me
    sethiddenprop(PlayerInstance, "MaximumSimulationRadius", NETWORK_RADIUS)
    setsimulationrad(NETWORK_RADIUS)
end)

return true
