# Nixos- Nixos withe DWM and ST

#Dwm Part:
# My config allows you to configure dwm on your own desire.

- How to configure your own DWM 
  
  1.You need to enable x11 

  $ services.xserver.enable = true;
   
   $ # Enable the X11 windowing system.

  $ services.xserver.enable = true;
 
2. Then clone Dwm for suckless website or for exaple my repository 

 3. Make a folder in /etc/nixos/ directory or where you located you : configuration.nix and flake.nix if you have it.
If locate in diffrent location in my not work it must be in the same directory like configuration.nix
ex. /etc/nixos/dotfilles/

 4. Move you dwm configuration for directory you clone the dwm to location that you create in /etc/nixos.
ex.sudo mv Downloads/dwm  /etc/nixos/dotfilles 
and ls /etc/nixos/dotfilles/
if you have all the necery files like config.def.h etc. you can continue

 5. For safty I recomand to copy configuration.nix for exmple $ cp /etc/nixos/configuration.nix ~/configuration.nix_mybackups, so that if you make mistake you can always recplice withe your old config

 6. Add a this this function to conaiguration.nix and specifide witch directory you put you dwm 
in this line ex :

src = ./dotfilles/dwm; # Replace with your actual path


 #DWM and override
services.xserver.windowManager.dwm = {
  
  enable = true;
  
  package = pkgs.dwm.overrideAttrs {
    
    src = ./dotfilles/dwm; # Replace with your actual path
  
  };

};

Last save you: nixos-rebuild switch or other command to rebuild nixos
