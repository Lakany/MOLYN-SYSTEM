-- Script Loader - Protection Layer
-- This script loads and executes remote code without exposing it directly

local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")

-- Configuration
local SCRIPT_URL = "https://raw.githubusercontent.com/Lakany/MOLYN-SPAMMER1/refs/heads/main/README.md"
local player = Players.LocalPlayer

-- Function to load and execute remote script
local function loadRemoteScript()
    local success, response = pcall(function()
        return HttpService:GetAsync(SCRIPT_URL)
    end)
    
    if success and response then
        -- Execute the loaded script
        local executeSuccess, executeError = pcall(function()
            loadstring(response)()
        end)
        
        if not executeSuccess then
            warn("Script execution failed: " .. tostring(executeError))
        end
    else
        warn("Failed to load remote script")
    end
end

-- Execute the loader
spawn(function()
    loadRemoteScript()
end)
