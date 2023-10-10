local emojies = {"?","?","?","?","?","?","?","?","?","?","?"}
    local repo = "https://raw.githubusercontent.com/HTDBarsi/LinoriaLib/main/";
    local Library = (loadstring(game:HttpGet(repo .. "Library.lua")))();
    local ThemeManager = (loadstring(game:HttpGet(repo .. "addons/ThemeManager.lua")))();
    local SaveManager = (loadstring(game:HttpGet(repo .. "addons/SaveManager.lua")))();
    local color = BrickColor.new("Really red");
    local plr = game.Players.LocalPlayer;
    local empty = Vector3.new();
    local hit = game.ReplicatedStorage.Remotes.ParryAttempt;
    local cam = workspace.CurrentCamera;
    local Settings = {
        Autoparry = {
            Toggle = false,
            Cooldown = 0.1,
            Range = 0.05,
            ConstantRange = 5
        }
    };
    local Window = Library:CreateWindow({
        Title = "Asteria BladeBall",
        Center = true,
        AutoShow = true,
        TabPadding = 8,
        MenuFadeTime = 0
    });
    local Tabs = {
        Main = Window:AddTab("All features"),
        ["UI Settings"] = Window:AddTab("UI Settings")
    };
    local LeftGroupBox = Tabs.Main:AddLeftGroupbox("Main");
    function getspeed()
        for i,v in pairs(workspace.Balls:GetChildren()) do
            if v.zoomies.VectorVelocity ~= empty then
                return v.zoomies.VectorVelocity.Magnitude;
            end
        end
    end
    function estimateTime(ball)
        return (getspeed() and getdistance(ball) / getspeed()) or 69
    end;

    function getdistance(ball)
        return (ball.Position - plr.Character.HumanoidRootPart.Position).Magnitude
    end

    LeftGroupBox:AddToggle("Autoparry Toggle", {
        Text = "Autoparry "..emojies[math.random(1,#emojies)],
        Default = false,
        Tooltip = "Automatically hits the ball for you!",
        Callback = function(Value)
            Settings.Autoparry.Toggle = Value;
        end
    });

    LeftGroupBox:AddSlider("Parry cooldown", {
        Text = "Parry cooldown",
        Default = 0.1,
        Min = 0,
        Max = 1,
        Rounding = 2,
        Compact = false,
        Callback = function(Value)
            Settings.Autoparry.Cooldown = Value;
        end
    })

    LeftGroupBox:AddSlider("Parry constant range", {
        Text = "Parry constant range - studs",
        Default = 5,
        Min = 0,
        Max = 30,
        Rounding = 0,
        Compact = false,
        Callback = function(Value)
            Settings.Autoparry.ConstantRange = Value;
        end
    })

    LeftGroupBox:AddSlider("Parry range", {
        Text = "Parry timed range - seconds",
        Default = 0.05,
        Min = 0,
        Max = 1,
        Rounding = 2,
        Compact = false,
        Callback = function(Value)
            Settings.Autoparry.Range = Value;
        end
    })

    LeftGroupBox:AddButton({
        Text = "Blatant aura",
        Func = function()
            local function getspeed()
                for i,v in pairs(workspace.Balls:GetChildren()) do
                    if v.zoomies.VectorVelocity ~= empty then
                        return v.zoomies.VectorVelocity;
                    end
                end
            end
            workspace.Balls.ChildAdded:Connect(function(v)
                v.Changed:Connect(function(prop)
                    c = plr.Character
                    if getspeed().X > getspeed().Z then
                        c.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(0,5,10);
                    else
                        c.HumanoidRootPart.CFrame = v.CFrame + Vector3.new(10,5,0);
                    end
                    cam.CameraSubject = v
                    if prop == "BrickColor" and v[prop] == color then
                        while v[prop] == color and v and c.Humanoid.Health ~= 0 do
                            cnt = 0
                            hit:FireServer(0.5,cam.CFrame,{},workspace.Balls:GetChildren())
                            while v.BrickColor == color and cnt ~= 20 do cnt += 1 task.wait() end
                        end
                        task.wait()
                    end;
                end);
            end);
        end,
        DoubleClick = false,
        Tooltip = "Rejoin to stop!"
    })

    -- Shoutout to bluwu for helping fr
    local hitcd = false
    
    function counter()
        task.wait(Settings.Autoparry.Cooldown)
        hitcd = false
    end
    workspace.Balls.ChildAdded:Connect(function(v)
        v:GetPropertyChangedSignal("BrickColor"):Connect(function()
            if v.BrickColor ~= color then
                hitcd = false
            end
        end)
        v:GetPropertyChangedSignal("Position"):Connect(function()
            if Settings.Autoparry.Toggle and v["BrickColor"] == color then
                if (estimateTime(v) < Settings.Autoparry.Range or getdistance(v) < Settings.Autoparry.ConstantRange) and not hitcd then
                    hitcd = true
                    task.spawn(counter)
                    print("Hit",Settings.Autoparry.Range,Settings.Autoparry.ConstantRange,Settings.Autoparry.Cooldown)
                    hit:FireServer(0.5,CFrame.new(),{},{})
                end
            end
            task.wait()
        end)
    end)
    
    local MenuGroup = Tabs["UI Settings"]:AddLeftGroupbox("Menu");
    (MenuGroup:AddLabel("Menu bind")):AddKeyPicker("MenuKeybind", {
        Default = "End",
        NoUI = true,
        Text = "Menu keybind"
    });
    MenuGroup:AddButton("Unload", function()
        Library:Unload();
    end);
    MenuGroup:AddButton("Copy discord invite", function()
        setclipboard("https://discord.gg/t2cXFpkGBh");
    end);
    Library.ToggleKeybind = Options.MenuKeybind;
    ThemeManager:SetLibrary(Library);
    SaveManager:SetLibrary(Library);
    SaveManager:IgnoreThemeSettings();
    SaveManager:BuildConfigSection(Tabs["UI Settings"]);
    ThemeManager:SetFolder("Asteria/BB");
    SaveManager:SetFolder("Asteria/BB");
    ThemeManager:ApplyToTab(Tabs["UI Settings"]);
    SaveManager:LoadAutoloadConfig();
