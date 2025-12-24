# Ling Shell

**Ling Shell** là một shell UI viết bằng **QuickShell**, thiết kế để chạy trên Wayland compositor (như **niri**, **Hyprland**) và tích hợp tốt với **NixOS**.

---

## Installation

### Nix (Flakes)

#### 1. Thêm Ling Shell vào `flake.nix`

```nix
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";

    ling = {
      url = "github:imtraf02/ling-shell";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };
}
```

#### 2. Cài package vào hệ thống (systemPackages)

```nix
inputs.ling.packages.${system}.default
```

Ví dụ:

```nix
environment.systemPackages = [
  inputs.ling.packages.${system}.default
];
```

---

### Home Manager

Ling Shell cung cấp Home Manager module để bật và cấu hình shell.

```nix
{
  inputs,
  ...
}: {
  imports = [
    inputs.ling.homeModules.default
  ];

  programs.ling-shell = {
    enable = true;
  };
}
```

Sau đó rebuild:

```bash
sudo nixos-rebuild switch --flake .#<hostname>
```

---

## Manual Installation (Non-Nix)

> Phù hợp cho distro khác hoặc test nhanh.

### Yêu cầu

- Wayland compositor
- `quickshell` (qs)
- Qt 6
- `git`

### Clone repo

```bash
git clone https://github.com/imtraf02/ling-shell.git
cd ling-shell
```

### Chạy trực tiếp bằng QuickShell

```bash
qs -c .
```

Hoặc nếu có entry riêng:

```bash
qs -c ling-shell
```

> ⚠️ Cách này **không cài system-wide**, chỉ dùng để test hoặc phát triển.

---

## Usage

### Commands

- `ling-shell`
  → Khởi động Ling Shell (khi cài bằng Nix)

---

## Autostart với niri

Ling Shell có thể được start tự động khi login vào **niri**.

### Cấu hình

Mở file:

```text
~/.config/niri/config.kdl
```

Thêm:

```kdl
spawn-at-startup "ling-shell"
```

Nếu cần chỉ rõ đường dẫn:

```kdl
spawn-at-startup "/run/current-system/sw/bin/ling-shell"
```

---

## Related Projects

- QuickShell: [https://git.outfoxxed.me/outfoxxed/quickshell](https://git.outfoxxed.me/outfoxxed/quickshell)
- niri: [https://github.com/YaLTeR/niri](https://github.com/YaLTeR/niri)
- NixOS: [https://nixos.org](https://nixos.org)
