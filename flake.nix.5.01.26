{
  description = "My NixOS system with Home Manager and stable/unstable packages";

  inputs = {
    # Stable channel (25.05)
    nixpkgs.url = "nixpkgs/nixos-25.05";
    
    # Unstable channel
    nixpkgs-unstable.url = "nixpkgs/nixos-unstable";
    
    # Home Manager (using stable branch)
    home-manager.url = "github:nix-community/home-manager/release-25.05";
    home-manager.inputs.nixpkgs.follows = "nixpkgs";
    
    # Your C program
    my-c-program.url = "path:/home/rafal/c-example";
    my-c-program.inputs.nixpkgs.follows = "nixpkgs";
  };

  outputs = { self, nixpkgs, nixpkgs-unstable, home-manager, my-c-program, ... }: 
   let 
    lib = nixpkgs.lib;
    system = "x86_64-linux";
    
    # Configuration for allowing unfree packages
    config = {
      allowUnfree = true;
    };
    
    # Stable packages with config
    pkgs = import nixpkgs {
      inherit system;
      inherit config;
    };
    
    # Unstable packages with config (allows unfree)
    pkgs-unstable = import nixpkgs-unstable {
      inherit system;
      inherit config;
    };
    
   in { 
    nixosConfigurations = { 
      nixos = lib.nixosSystem {
        inherit system;
        
        # Make both stable and unstable packages available
        specialArgs = {
          inherit pkgs-unstable;
          inherit my-c-program;
        };

        modules = [ 
          ./configuration.nix 
          
          # Make home-manager available as a NixOS module
          home-manager.nixosModules.home-manager
          {
            home-manager.useGlobalPkgs = true;
            home-manager.useUserPackages = true;
            
            # Pass unstable packages to home-manager too
            home-manager.extraSpecialArgs = {
              inherit pkgs-unstable;
          inherit my-c-program;
            };
            
            home-manager.users.rafal = import ./home.nix;
          }
        ];
      };
    };
  };
}
