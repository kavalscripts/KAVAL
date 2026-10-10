local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local PLACE_ID = 96342491571673
local function getServers(cursor)
    local url = "https://games.roblox.com/v1/games/" .. PLACE_ID .. "/servers/Public?sortOrder=Asc&limit=100"
    if cursor and cursor ~= "" then
        url = url .. "&cursor=" .. cursor
    end
    local ok, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet(url))
    end)
    if not ok then return nil end
    return result
end
local function findServer()
    local best = nil
    local cursor = ""
    repeat
        local data = getServers(cursor)
        if not data or not data.data then break end
        for _, server in ipairs(data.data) do
            local n = server.playing
            if n ~= nil and n <= 2 then
                if n == 1 then
                    return server
                end
                if best == nil or n < best.playing then
                    best = server
                end
            end
        end
        cursor = data.nextPageCursor or ""
    until cursor == "" or cursor == nil
    return best
end
local server = findServer()
if server then
    print("joining server with " .. server.playing .. " players")
    TeleportService:TeleportToPlaceInstance(PLACE_ID, server.id, Players.LocalPlayer)
else
    print("no servers found under 2 players")
end
