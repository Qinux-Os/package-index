# Package index

Defines which packages make up which part of Qinux. This is where modularity is
actually expressed: a component is a named set of packages, and installing or
removing a component is a set operation.

## Layout

| Path | Contents |
| --- | --- |
| [`schema/`](schema/) | JSON Schema for every file here |
| [`profiles/`](profiles/) | Components: `desktop`, `kde`, `installer`, and so on |
| [`index/`](index/) | The generated, machine-readable index |

## The model

Three layers, from coarse to fine:

| Layer | File | Example |
| --- | --- | --- |
| Profile | `profiles/desktop.json` | A complete installation, such as `kde` |
| Group | defined by the profile | A package group, such as `kde-plasma` |
| Package | defined by `pkgbuilds` | `kwin`, `plasma-workspace` |

A profile references groups and packages. It never lists individual packages
except where a component genuinely is one package.

## Profiles

| Profile | Contents |
| --- | --- |
| `minimal` | Boot, network and a shell. No graphical session |
| `base` | Minimal plus the standard system utilities |
| `desktop` | Base plus a desktop environment, as a variant |
| `desktop-kde` | Base plus KDE Plasma, Qinux configured |
| `desktop-gnome` | Base plus GNOME. Not built yet |
| `installer` | Everything needed to install from the ISO |

Desktop profiles inherit from `base` rather than repeating it:

```json
{
  "name": "desktop-kde",
  "inherits": "base",
  "groups": ["kde-plasma"],
  "packages": ["qinux-plasma-config", "qinux-themes"],
  "conflicts": ["gnome-shell"]
}
```

Inheritance is resolved when the index is generated, not by the package
manager. That keeps the resolution rules in one place and makes the generated
index auditable.

## Reading the index

```sh
# what is in the KDE desktop profile
qinux-cli profile show desktop-kde

# which profile owns a package
qinux-cli profile owner kwin
```

## Validation

The schema is authoritative:

```sh
qinux-cli index validate
```

Two rules matter and are checked in CI:

1. Every package referenced by a profile exists in `pkgbuilds`.
2. No package appears in two profiles that `conflict` with each other.

Rule 2 catches the common mistake: shipping a GNOME package inside the KDE
profile, which installs cleanly and then fights over the display manager.

## Related repositories

| Repository | Holds |
| --- | --- |
| [`pkgbuilds`](../pkgbuilds/) | The PKGBUILDs these profiles reference |
| [`packages`](../packages/) | How packages are grouped for building |
| [`metadata`](../metadata/) | Which packages a release contains |
| [`iso`](../iso/) | Which profiles become ISOs |

## License

CC0-1.0. A list of package names is a fact, not authorship.
