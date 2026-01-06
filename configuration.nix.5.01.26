# Edit this configuration file to define what should be installed on
# your system.  Help is available in the configuration.nix(5) man page
# and in the NixOS manual (accessible by running 'nixos-help').

{ config, pkgs, pkgs-unstable, my-c-program, ... }:


{

imports =
    [ # Include the results of the hardware scan.
      ./hardware-configuration.nix

#<home-manager/nixos>
     ];

  # Bootloader.
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

# Upadtes for nix channels create 13.09.25r.
   

  system.autoUpgrade = {
    enable = true;
    dates = "12:00";
   # Proper flake update flags
    flags = [
      "--update-input" "nixpkgs"
      "--update-input" "nixpkgs-unstable"
      "--update-input" "home-manager"
      "--commit-lock-file"
    ];
  };

  networking.hostName = "nixos"; # Define your hostname.
  # networking.wireless.enable = true;  # Enables wireless support via wpa_supplicant.

  # Configure network proxy if necessary
  # networking.proxy.default = "http://user:password@proxy:port/";
  # networking.proxy.noProxy = "127.0.0.1,localhost,internal.domain";
  
  # new 16.09.25
  #nix.settings.experimental-features = [ "nix-command" "flakes" ];

  # Enable networking
  networking.networkmanager.enable = true;

  # Set your time zone.
  time.timeZone = "Europe/Warsaw";

  # Select internationalisation properties.
  i18n.defaultLocale = "en_US.UTF-8";

  i18n.extraLocaleSettings = {
    LC_ADDRESS = "pl_PL.UTF-8";
    LC_IDENTIFICATION = "pl_PL.UTF-8";
    LC_MEASUREMENT = "pl_PL.UTF-8";
    LC_MONETARY = "pl_PL.UTF-8";
    LC_NAME = "pl_PL.UTF-8";
    LC_NUMERIC = "pl_PL.UTF-8";
    LC_PAPER = "pl_PL.UTF-8";
    LC_TELEPHONE = "pl_PL.UTF-8";
    LC_TIME = "pl_PL.UTF-8";
  };

hardware.bluetooth.enable = true;
hardware.bluetooth.package = pkgs.bluez;








  # Enable the X11 windowing system.
  # You can disable this if you're only using the Wayland session.
  services.xserver.enable = true;

  #DWM and override 
services.xserver.windowManager.dwm = {
  enable = true;
  package = pkgs.dwm.overrideAttrs {
    src = ./dotfilles/dwm; # Replace with your actual path
  };
};

# ST (Simple Terminal) override with custom configuration # MY VERSION
nixpkgs.config.packageOverrides = pkgs: {
  st = pkgs.st.overrideAttrs (oldAttrs: 
    let
      patchesDir = ./dotfilles/st/patches;
      # Get all .diff and .patch files from patches directory
      patchFiles = builtins.filter 
        (name: pkgs.lib.hasSuffix ".diff" name || pkgs.lib.hasSuffix ".patch" name)
        (builtins.attrNames (builtins.readDir patchesDir));
      # Convert filenames to full paths using proper string interpolation
      patches = map (name: patchesDir + ("/" + name)) patchFiles;
    in {
      src = ./dotfilles/st;
      buildInputs = oldAttrs.buildInputs or [] ++ [ pkgs.harfbuzz ];
      patches = patches;
    });
};

# Xmonad
services.xserver.windowManager.xmonad.enable = true;
services.xserver.windowManager.xmonad.enableContribAndExtras = true;

#i3 wm 
services.xserver.windowManager.i3.enable = true;

#leftwm
#services.xserver.windowManager.leftwm.enable = true;   

#Spectrwm 
services.xserver.windowManager.spectrwm.enable = true;  


#Sway

  programs.sway.enable = true; 
  programs.hyprland.enable = true; 
  # niri
  programs.niri.enable = true;

  # Enable the KDE Plasma Desktop Environment.
 #services.displayManager.sddm.enable = true;
   # services.displayManager.sddm.enable = true;







   services.displayManager.ly.enable = true; 
    #services.displayManager.gdm.enable = true; 
  # services.xserver.displayManager.lightdm.enable = true;
   services.desktopManager.plasma6.enable = true;

# Cosmic desktop 
#services.displayManager.cosmic-greeter.enable = true;
services.desktopManager.cosmic.enable = true;
# vistualisation 
virtualisation.virtualbox.host.enable = true;
virtualisation.virtualbox.host.enableExtensionPack = true;
users.extraGroups.vboxusers.members = [ "rafal" ];
 

  # Configure keymap in X11
  services.xserver.xkb = {
    #exportConfiguration = true; #new
    layout = "us,pl";
    #xkbOptions = "grp:alt_shift_toggle";   # new
variant = "";

 # layout = "us";
   # variant = "";





  };

  # Enable CUPS to print documents.
  services.printing.enable = true;
  services.printing.drivers = with pkgs; [
    gutenprint
    gutenprintBin
    hplip
    hplipWithPlugin
    epson-escpr
    epson-escpr2
    brlaser
    brgenml1lpr
    brgenml1cupswrapper
    cnijfilter2
    carps-cups
    samsung-unified-linux-driver
    splix
  ];
  
  # Enable Avahi for network printer discovery
  services.avahi = {
    enable = true;
    nssmdns4 = true;
    openFirewall = true;
  };

  # Enable sound with pipewire.
 services.pulseaudio.enable = false;
  security.rtkit.enable = true;
  services.pipewire = {
    enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    pulse.enable = true;
    # If you want to use JACK applications, uncomment this
    #jack.enable = true;

    # use the example session manager (no others are packaged yet so this is enabled by default,
    # no need to redefine it in your config for now)
    #media-session.enable = true;
  };

  #Enable touchpad support (enabled default in most desktopManager).
  services.libinput.enable = true;

  # Screen saving configuration - 30 minutes timeout
  # X11 screensaver and power management settings
  services.xserver.displayManager.sessionCommands = ''
    # Set screen blanking timeout (screen goes black after 30 minutes of inactivity)
    ${pkgs.xorg.xset}/bin/xset s 300 300
    # Set DPMS (Display Power Management Signaling) timeouts
    # Monitor standby after 35 minutes, suspend after 40 minutes, off after 45 minutes
    ${pkgs.xorg.xset}/bin/xset dpms 2100 2400 2700
  '';
  
  # Enable screen locking with xss-lock (works with X11 sessions)
  programs.xss-lock = {
    enable = false;
    # Use i3lock as the screen locker (simple black screen)
    lockerCommand = "${pkgs.i3lock}/bin/i3lock -c 000000";
  };

  # Laptop lid handling - Power Management Configuration
  services.logind = {
    # What to do when laptop lid is closed
    lidSwitch = "suspend";           # Options: suspend, hibernate, poweroff, ignore, lock
    lidSwitchDocked = "ignore";      # What to do when lid is closed and docked to external monitor
    lidSwitchExternalPower = "suspend";  # What to do when on external power
    
    # Power button behavior
    powerKey = "suspend";            # Options: suspend, hibernate, poweroff, ignore
    
    # Additional power management settings
    powerKeyLongPress = "poweroff";  # Long press power button
    rebootKey = "reboot";
    suspendKey = "suspend";
    hibernateKey = "hibernate";
  };

  # Enable suspend-then-hibernate (suspend for a while, then hibernate to save battery)
  systemd.sleep.extraConfig = ''
    HibernateDelaySec=2h
    SuspendState=mem
  '';

  # For Plasma/KDE users: KDE has its own power management
  # You can configure screen saving in System Settings > Power Management
  # or through the following service (uncomment if needed):
  # services.kdeconnect.enable = true;

  # Define a user account. Don't forget to set a password with 'passwd'.
  users.users.rafal = {
    isNormalUser = true;
    description = "rafal";
    extraGroups = [ "networkmanager" "wheel" ];
    packages = with pkgs; [
      kdePackages.kate  
      #vim 
       

    #  thunderbird
    ];
  };

  # Home Manager configuration
  # Home Manager configuration is now handled via flake.nix

  # Install firefox.
  programs.firefox.enable = true;

  # Allow unfree packages
  nixpkgs.config.allowUnfree = true;

  # List packages installed in system profile. To search, run:
  # $ nix search wget
  environment.systemPackages = with pkgs; [
    my-c-program.packages.x86_64-linux.default
  #  vim # Do not forget to add an editor to edit configuration.nix! The Nano editor is also installed by default.     
       taffybar
       git # new 17.09
       wget
       dmenu
        st
      pkgs.gimp
      # brave   
       kitty
     # chromium
      alacritty  
    # hyperland pkgs:
      wofi  waybar hyprpaper xmobar # Xmonad status bar
haskellPackages.xmonad-contrib haskell-language-server pkgs.swaybg 
   # vim pkgs:
    pkgs.xclip      
    pkgs.neovim
    pkgs.ripgrep #Nvim
    pkgs.fd #Nvim
    pkgs.gnumake #Nvim
    pkgs.stylua 
    pkgs.lua-language-server
    # IDE  
      pkgs.eclipses.eclipse-cpp pkgs.audacious #MP3 pkgs.picom # x11
      system-config-printer
      pkgs.krusader #krusader 
      pkgs.brightnessctl # screen brightness
      sddm-chili-theme 
      pkgs.xsecurelock # screenlock for X11 
     #IP and country
      pkgs.ipfetch
      pkgs.fish
      pkgs.xterm
          #AI TERMINALS#####
          #pkgs.warp-terminal
          pkgs.waveterm
           pkgs.xorg.libX11 # X11 tools
	  
          #
          pkgs.mpv # terminal mp3 player
          pkgs.neofetch
          pkgs.starship
          pkgs.htop
          pkgs.bat
          pkgs.libreoffice-qt6-fresh
         # pkgs.gccgo15 # GCC-wrapper C COMPUILER
           pkgs.gcc
	  pkgs.timeshift # serstoring too          
          pkgs.nemo #file manager           
          #pkgs.adobe-reader  # adobe-reader 09.11.25
          pkgs.onlyoffice-desktopeditors # office suit
          pkgs.picom # transparet x11 
          pkgs.dropbox
          pkgs.dropbox-cli
          pkgs.vlc
          pkgs.audacity  
          pkgs.unzip

# Screen saving and locking packages
          pkgs.i3lock          # Simple screen locker
          pkgs.xss-lock        # Automatic screen locking
          pkgs.xautolock       # Alternative screen locker
          pkgs.xscreensaver    # Traditional X11 screensaver
          pkgs.podman
          pkgs.rofi
    # UNSTABLE PACKAGES:
    pkgs-unstable.vscode     # Latest VS Code
    pkgs-unstable.warp-terminal          
    pkgs-unstable.zoom-us   
pkgs-unstable.chromium
    pkgs-unstable.brave
    pkgs-unstable.localsend    
    pkgs-unstable.joplin-desktop # JopLin notatnik z synchronizacją    
    pkgs-unstable.distrobox    

];




  # Some programs need SUID wrappers, can be configured further or are
  # started in user sessions.
  # programs.mtr.enable = true;
  # programs.gnupg.agent = {
  #   enable = true;
  #   enableSSHSupport = true;
  # };

  # List services that you want to enable:

  # Enable the OpenSSH daemon.
  services.openssh.enable = false;
  

  # Open ports in the firewall.
  # networking.firewall.allowedTCPPorts = [  ];
 # networking.firewall.allowedUDPPorts = [  ];
  # Or disable the firewall altogether.
  networking.firewall.enable = true;

  # LocalSend ports
  #networking.firewall.allowedTCPPorts = [ ];
  #networking.firewall.allowedUDPPorts = [  ];

# networking.firewall.allowedICMPTypes = [ ];  # Block all ICMP types, including pingi;

  # Restrict printer ports to Brother MFC-J3930DW only 
  #networking.firewall.extraCommands = ''
    # Block outbound connections to printer ports except to Brother printer
    
  







              ### Moje






             ## koniec  



  # This value determines the NixOS release from which the default
  # settings for stateful data, like file locations and database versions
  # on your system were taken. It's perfectly fine and recommended to leave
  # this value at the release version of the first install of this system.
  # Before changing this value read the documentation for this option
  # (e.g. man configuration.nix or on https://nixos.org/nixos/options.html).
  system.stateVersion = "25.05"; # Did you read the comment?

nix.settings.experimental-features = ["nix-command" "flakes" ];

}

