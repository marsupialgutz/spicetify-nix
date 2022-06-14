# Spicetify-Nix

Modifies Spotify using [spicetify-cli](https://github.com/khanhas/spicetify-cli).

[spicetify-themes](https://github.com/morpheusthewhite/spicetify-themes) are included and available, including Dribblish.

Usage as a home-manager module:
```nix
let
  av = pkgs.fetchFromGitHub {
    owner = "amanharwara";
    repo = "spicetify-autoVolume";
    rev = "d7f7962724b567a8409ef2898602f2c57abddf5a";
    sha256 = "1pnya2j336f847h3vgiprdys4pl0i61ivbii1wyb7yx3wscq7ass";
  };

  # fetchFromGitHub should work too
  spicetify = fetchTarball https://github.com/pietdevries94/spicetify-nix/archive/master.tar.gz;
in
# The module is meant to be imported by the user
home-manager.users.piet = {
  imports = [ (import "${spicetify}/module.nix") ];

  programs.spicetify = {
    enable = true;
    theme = "Dribbblish";
    colorScheme = "horizon";
    enabledCustomApps = ["reddit"];
    enabledExtensions = ["newRelease.js" "autoVolume.js"];
    thirdPartyExtensions = {
      "autoVolume.js" = "${av}/autoVolume.js";
    };
  };
}
```

Usage as a package:
```nix
let
  av = pkgs.fetchFromGitHub {
    owner = "amanharwara";
    repo = "spicetify-autoVolume";
    rev = "d7f7962724b567a8409ef2898602f2c57abddf5a";
    sha256 = "1pnya2j336f847h3vgiprdys4pl0i61ivbii1wyb7yx3wscq7ass";
  };

  spicetify = fetchTarball https://github.com/pietdevries94/spicetify-nix/archive/master.tar.gz;
in
pkgs.callPackage (import "${spicetify}/package.nix") {
  inherit pkgs;
  theme = "Dribbblish";
  colorScheme = "horizon";
  enabledCustomApps = ["reddit"];
  enabledExtensions = ["newRelease.js" "autoVolume.js"];
  thirdPartyExtensions = {
    "autoVolume.js" = "${av}/autoVolume.js";
  };
}
```

To add third-party themes, extensions or custom apps use `thirdPartyThemes`, `thirdPartyExtensions` or `thirdPartyCustomApps`. These expect a set, where the key is the name of the new theme/extension and the value the path. Don't forget to enable it seperatly.

For all available options, check module.nix or package.nix and the spicetify repo. Everything is optional and will revert to the defaults from spicetify.

## macOS
This package has no macOS support, because Spotify in nixpkgs has no macOS support.
