# DotFiles
---------------------------------------------------------------------------------------
Layouts:

https://awesomewm.org/doc/api/documentation/07-my-first-awesome.md.html#
---------------------------------------------------------------------------------------
- Main Floating
- Games Fullscreen
- Programming Tile
	
	awful.layout.layouts = {
	    awful.layout.suit.floating,
	    -- awful.layout.suit.tile,
	    -- awful.layout.suit.tile.left,
	    -- awful.layout.suit.tile.bottom,
	    -- awful.layout.suit.tile.top,
	    -- awful.layout.suit.fair,
	    -- awful.layout.suit.fair.horizontal,
	    awful.layout.suit.spiral,
	    -- awful.layout.suit.spiral.dwindle,
	    -- awful.layout.suit.max,
	    -- awful.layout.suit.max.fullscreen,
	    -- awful.layout.suit.magnifier,
	    -- awful.layout.suit.corner.nw,
	    -- awful.layout.suit.corner.ne,
	    -- awful.layout.suit.corner.sw,
	    -- awful.layout.suit.corner.se,
	}
	local names = { "Main", "Games", "Coding" }
	local l = awful.layout.suit  -- Just to save some typing: use an alias.
	local layouts = { l.floating, l.max, l.spiral }
	-- Fullscreen mal testen
	--local layouts = { l.floating, l.max.fullscreen, l.spiral }
	awful.tag(names, s, layouts)

/*---------------------------------------------------------------------------------------*/
https://awesomewm.org/doc/api/libraries/awful.rules.html
/*---------------------------------------------------------------------------------------*/	
Programme auf Workspace
	- Lutris, Steam, Teamspeak -> Games (Autofollow)
	- Code -> Coding (Autofollow)

	{ rule = { instance = "lutris" },
	  properties = { tag = "Games", switchtotag = true } }
	{ rule = { instance = "steam" },
	  properties = { tag = "Games", switchtotag = true } }
	{ rule = { instance = "teamspeak" },
	  properties = { tag = "Games", switchtotag = true } }
	{ rule = { instance = "code" },
	  properties = { tag = "Coding", switchtotag = true } }
	
/*---------------------------------------------------------------------------------------*/
https://awesomewm.org/doc/api/libraries/mouse.html
/*---------------------------------------------------------------------------------------*/		
Shortcuts
	- WIN + TAB -> Next Window
	- ALT + WIN + TAB -> Previous Window
	- WIN + MWHEEL CLICK -> Close Window
	- WIN + MWHEEL UP -> UnMinimize
	- WIN + MWHEEL DOWN -> Minimize
	

	awful.key({ modkey,           }, "Tab",
	    function ()
	        -- awful.client.focus.history.previous()
	        awful.client.focus.byidx(1)
	        if client.focus then
	            client.focus:raise()
	        end
	    end),
	
	awful.key({ modkey, "Control"   }, "Tab",
	    function ()
	        -- awful.client.focus.history.previous()
	        awful.client.focus.byidx(-1)
	        if client.focus then
	            client.focus:raise()
	        end
	    end),
		
	root.buttons(gears.table.join(
	    --awful.button({ }, 3, function () mymainmenu:toggle() end),
	    --awful.button({ }, 3, awful.tag.viewnext),
	    awful.button({ modkey }, 3, function (c) c:kill() end),
	    awful.button({ modkey, "Control" }, 5, function (c) c.minimized = true end),
		awful.button({ modkey, "Control" }, 4, function () local c = awful.client.restore() if c then c:emit_signal("request::activate", "key.unminimize", {raise = true}) end end),
		-- Fullscrenn or maximized
		awful.button({ modkey }, 4, function (c) c.fullscreen = not c.fullscreen c:raise() end),
		-- awful.button({ modkey }, 4, function (c) c.maximized = not c.maximized c:raise() end),
		-- Standart
		awful.button({ }, 4, awful.tag.viewnext),
	    awful.button({ }, 5, awful.tag.viewprev)
	))

/*---------------------------------------------------------------------------------------*/
DotFiles rc.lua
/*---------------------------------------------------------------------------------------*/		
LAYOUT SWAP UND CLIENT SWAP VERTAUSCHEN

    awful.key({ modkey,           }, "space", function () awful.layout.inc( 1)                end,
              {description = "select next", group = "layout"}),
    awful.key({ modkey, "Shift"   }, "space", function () awful.layout.inc(-1)                end,
              {description = "select previous", group = "layout"}),
		
ÄNDERN IN		
			  
    awful.key({ modkey,  "Control"         }, "space", function () awful.layout.inc( 1)                end,
              {description = "select next", group = "layout"}),
    awful.key({ modkey, "Control", "Shift"   }, "space", function () awful.layout.inc(-1)                end,
              {description = "select previous", group = "layout"}),
			  
			  
    awful.key({ modkey, "Control" }, "space",  awful.client.floating.toggle                     ,
              {description = "toggle floating", group = "client"}),

ÄNDERN IN

    awful.key({ modkey,  }, "space",  awful.client.floating.toggle                     ,
              {description = "toggle floating", group = "client"}),
