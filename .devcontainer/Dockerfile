# Codespaces/devcontainer image for dezren39/fleet (umbrella repo).
# Same thin-image design proven in the developing-today/code pilot
# (PR #855): Ubuntu base, direnv, single-user Nix with flakes on.
# The submodules' own flakes provide the real toolchains.
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

USER root
RUN apt-get update \
 && apt-get install -y --no-install-recommends direnv \
 && rm -rf /var/lib/apt/lists/*

USER vscode
# Single-user (no-daemon) Nix install for the vscode user.
RUN curl -L https://nixos.org/nix/install | sh -s -- --no-daemon \
 && mkdir -p "$HOME/.config/nix" \
 && printf 'experimental-features = nix-command flakes\nbuild-users-group =\n' > "$HOME/.config/nix/nix.conf"

ENV PATH="/home/vscode/.nix-profile/bin:${PATH}"
