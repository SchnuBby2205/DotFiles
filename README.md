# Installation Guide
https://wiki.archlinux.org/title/installation_guide
(Bis Punkt 3.8 "Boot loader" durchgehen)

# Step by Step (Base Installation)
# Keyboard Layout
 	
Anzeigen aller Keyboard Layouts:
	
 	ls /usr/share/kbd/keymaps/**/*.map.gz
Keyboard Layout laden:

	loadkeys de-latin1

Bei einer englischen Tastatur (wie meiner) diesen Schritt erst vor "Chroot - Login ins neue System" ausführen.

# Bootmodus testen
	
 	cat /sys/firmware/efi/fw_platform_size

Returncode: 64 = UEFI 64Bit

Returncode: 32 = UEFI 32Bit

Datei Existiert nicht BIOS oder CMS Boot
 
# Internet prüfen

	ip link
Testen:  	
   	
	ping www.google.de

# Systemzeit anpassen
Testen:
	
 	timedatectl
Zeitzone setzen:
	
	timedatectl set-timezone "Europe/Berlin"
NTP aktivieren:
 	
  	timedatectl set-ntp true

# Paritionieren über fdisk
	fdisk /dev/the_disk_to_be_partitioned
Standard Layout:
	<table>   
	<th>Mount Name</th><th>Partition</th><th>Partitionstyp</th><th>Mindestgröße</th>
  	<tr><td>/mnt</td><td>/dev/root_partition</td><td>Linux x84-64 root (/)</td><td>[Min 100GB]</td></tr>
	<tr><td>/mnt/boot</td><td>/dev/efi_system_partition</td><td>EFI system partition</td><td>[Min 1GB]</td></tr>
  	<tr><td>[SWAP]</td><td>/dev/swap_partition</td><td>Linux swap</td><td>[Min 2GB]</td></tr>
 	</table>

# Partitionen formatieren
Root

	mkfs.ext4 /dev/root_partition
Efi	
 	
  	mkfs.fat -F 32 /dev/efi_system_partition
Swap	
  	
   	mkswap /dev/swap_partition
   
# Partitionen mounten
Root
	
 	mount /dev/root_partition /mnt
Efi
	
 	mount --mkdir /dev/efi_system_partition /mnt/boot
Swap
 	
  	swapon /dev/swap_partition

# Mirrors konfigurieren und in Pacman hinterlegen
Reflector sortieren	

	reflector -c Germany -a 6 --save /etc/pacman.d/mirrorlist
Pacman aktualisieren	

	pacman -Syyy

# Basis Installation starten
Intel
	
 	pacstrap -K /mnt base linux linux-firmware sudo nano intel-ucode
AMD

	pacstrap -K /mnt base linux linux-firmware sudo nano amd-ucode
 
# Fstab generieren
	genfstab -U /mnt >> /mnt/etc/fstab
Testen:

	nano /mnt/etc/fstab

# Falls noch nicht geschehen Keyboard Layout jetzt setzen.

# Chroot - Login ins neue System
	arch-chroot /mnt

# Zeitzone setzen
	ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
Hardware Clock schreiben
 
 	hwclock --systohc

# Lokalisierung einstellen
	nano /etc/locale.gen
de_DE.UTF-8 auskommentieren
 	
  	locale-gen
In locale.conf übernehmen

	echo "LANG=de_DE.UTF-8" >> /etc/locale.conf
VConsole einstellen  

	echo "KEYMAP=de-latin1" >> /etc/vconsole.conf

# Netzwerk konfigurieren
	echo "[HOSTNAME]" >> /etc/hostname
Hosts bearbeiten

  	nano /etc/hosts
Eintragen

	127.0.0.1	localhost
	::1		localhost
	127.0.1.1	[Hostname].localdomain	[Hostname]

# Initramfs erstellen
	mkinitcpio -P

# Root Passwort vergeben
	passwd
 
# Step by Step (Post Base Installation)
# GRUB + Software installieren
Programme installieren
		
  	pacmans -S grub efibootmgr os-prober ntfs-3g networkmanager dialog mtools dosfstools base-devel linux-headers git pulseaudio
Grub Installieren
		
  	grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
Grub Config schreiben

	grub-mkconfig -o /boot/grub/grub.cfg

# Dienste für Autostart hinterlegen
	systemctl enable NetworkManager

# Neuen "normalen" User anlegen
	useradd -mG wheel [NAME]
Passwort für neuen Benutzer
	
 	passwd [NAME]
SUDO für neuen Benutzer

	EDITOR=nano visudo
Zeile "%wheel ALL=(ALL) ALL" finden und einkommentieren.

# Reboot
Ausloggen

	exit (CTRL+D)
Mount entfernen

	umount -R /mnt

Reboot
	
 	reboot

# POSTINSTALL
# Graphics Driver
Intel Graphics:

	sudo pacman -S xf86-video-intel
AMD Graphics:

	sudo pacman -S xf86-video-amdgpu
NVIDIA Graphics:

	sudo pacman -S nvidia nvidia-utils

# Display Server
	sudo pacman -S xorg xorg-init

# AUR Helper (yay)
clonen:
    
    git clone https://aur.archlinux.org/yay.git
Verzeichnis wechseln    
    
    cd yay/
Kompilieren

    makepkg -si PKGBUILD

# TODO
Software Downloads 
![image](https://github.com/SchnuBby2205/DotFiles/assets/80288097/59e17de2-3b38-47ae-a3ea-cf0fa903d9bd)
	pulseaudio-control
	jonaburg-picom
 	vscode
YAY Font Downloads
Configs runterladen
oh my fish theme
lightdm + webkit2 + webkit greeter (glorious) + systemctl enable lightdm
Firefox theme nordic-dark
nano entfernen 
multilib
vulkan driver
vulkan support



# Layout
https://awesomewm.org/doc/api/documentation/07-my-first-awesome.md.html#
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

# Programme auf Workspace
https://awesomewm.org/doc/api/libraries/awful.rules.html
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
	
# Shortcuts
https://awesomewm.org/doc/api/libraries/mouse.html
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

# LAYOUT SWAP UND CLIENT SWAP VERTAUSCHEN
DotFiles rc.lua

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
