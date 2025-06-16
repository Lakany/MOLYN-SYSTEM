--[[
  Roblox Script Loader
  بواسطة Lakany
  يقوم بتحميل وتنفيذ سكربت من رابط مباشر
--]]

local ScriptURL = "https://raw.githubusercontent.com/Lakany/MOLYN-SPAMMER1/refs/heads/main/README.md"

-- وظيفة لتحميل وتنفيذ السكربت
local function LoadScript()
    print("جاري تحميل السكربت...")
    
    local Success, Error = pcall(function()
        local ScriptContent = game:HttpGet(ScriptURL, true)
        local LoadedScript = loadstring(ScriptContent)
        LoadedScript()
    end)
    
    if not Success then
        warn("فشل تحميل السكربت: " .. Error)
    else
        print("تم تشغيل السكربت بنجاح!")
    end
end

-- تشغيل الوظيفة
LoadScript()
