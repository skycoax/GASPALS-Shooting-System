# GASPALS — Shooting System

A flexible, **data-driven third-person shooting system** built on top of Epic's
**Game Animation Sample (GASP)** with **ALS-style weapon overlays**, made entirely
in **Blueprints** for **Unreal Engine 5.8**.

Equip a weapon, aim, and shoot — hitscan fire with ammo, reloading, selectable
fire modes, recoil, a dynamic crosshair, an ammo HUD, and a simple damage / health
/ ragdoll target. Adding a new weapon is **one data asset**, no graph edits.

---

## ✨ Features

**Combat (this project's contribution)**
- **Data-driven weapons** — every weapon is a `PDA_WeaponDefinition` data asset
  (damage, RPM, range, magazine/reserve, fire modes, spread, recoil, muzzle socket…).
- **One reusable component** — `AC_WeaponSystem` on the character handles firing,
  fire-rate timing, ammo, reload, and fire-mode switching for *all* weapons.
- **Camera-accurate hitscan** — shots converge on the crosshair (camera→muzzle
  reconciliation), with a debug tracer from the muzzle to the impact point.
- **Fire modes** — semi / full-auto (per-weapon, cyclable in game).
- **Aim-to-fire** — you must aim (RMB) to shoot; the crosshair only shows while aiming.
- **Dynamic crosshair** — expands while moving and on each shot, tightens at rest.
- **Ammo HUD** + **reload** (with dry-fire when empty).
- **Damage / health / ragdoll** — `AC_Health` + `BP_TargetDummy` that takes damage
  and ragdolls on death (via Unreal's built-in point-damage path).
- **Overlay-aware** — picking a weapon pose (rifle / pistol) auto-selects its weapon.
- Ships with **M4A1** (rifle) and **M9** (pistol).

**Base framework (from the bundled sample content)**
- Full GASP locomotion (motion matching), ALS-style weapon-pose overlay switching,
  gameplay camera rigs (aim / strafe / first-person), traversal, ragdoll, and a
  character-select / overlay UI.

---

## 🧩 Requirements

- **Unreal Engine 5.8**
- ~2 GB of disk for the project content
- No C++ compilation required — this is a **Blueprint-only** project.

---

## 🚀 Getting started

```bash
git clone https://github.com/<your-username>/<your-repo>.git
```

> This repository stores its `.uasset` content directly in Git (no Git LFS), so a
> normal `git clone` or **Download ZIP** gives you everything — no extra setup.

1. Open **`GASPALS.uproject`** with Unreal Engine 5.8.
2. If prompted to build/compile, click **Yes** (Blueprint-only, so it's quick).
3. Press **Play**. The default level is `Content/GASPALS/Levels/DefaultLevel`.

To try shooting: equip a weapon pose (rifle/pistol) via the on-screen overlay
switcher, **hold Right Mouse to aim** (the crosshair appears), then **Left Mouse to fire**.
Drag a **`BP_TargetDummy`** into the level to shoot at.

---

## 🎮 Controls

| Action | Keyboard / Mouse | Gamepad |
|---|---|---|
| Move | `W` `A` `S` `D` | Left Stick |
| Look | Mouse | Right Stick |
| Sprint | `Left Shift` | Left Shoulder |
| Walk | `Left Ctrl` | Right Shoulder |
| Crouch | `C` | Face Right (B / ○) |
| Jump / Traverse | `Space` | Face Bottom (A / ✕) |
| **Aim (ADS)** | **Right Mouse** | Left Trigger |
| **Fire** | **Left Mouse** | Right Trigger |
| **Reload** | **`R`** | Face Left (X / □) |
| **Switch fire mode** | **`V`** | D-Pad Up |
| Interact | `E` | Face Top (Y / △) |
| Switch character | `N` | — |
| Ragdoll | `X` | D-Pad Right |

> ⚠️ You must be **aiming (hold RMB)** to fire and to see the crosshair.

---

## 🔧 How the weapon system works

```
CBP_SandboxCharacter
 ├─ AC_WeaponSystem   ← all firing logic + a WeaponDefinitions registry
 │    • reads the active PDA_WeaponDefinition (selected by overlay pose)
 │    • StartFire / StopFire / Reload / SwitchFireMode
 │    • hitscan from camera → crosshair, ApplyPointDamage, recoil
 └─ AC_Health         ← HP, death

BP_WeaponHUD (AHUD)   ← dynamic crosshair (while aiming) + ammo counter
```

### Add a new weapon (no Blueprint editing)

1. Duplicate `Content/GASPALS/Combat/Data/DA_Weapon_M4A1` → `DA_Weapon_<Name>` and
   set its fields (mesh muzzle socket, damage, RPM, magazine, fire modes, recoil…),
   including the **Overlay Pose** it maps to.
2. Add it to `AC_WeaponSystem → WeaponDefinitions` (on the character).

That's it — the single component handles the rest from data.

Key combat assets live under **`Plugins/GASPALS/Content/`**:
`Combat/` (enums, structs, weapon data), `Blueprints/Weapons/` (component, target),
`Combat/UI/BP_WeaponHUD`, and the input actions in `Input/`.

---

## 🙏 Credits & attribution

This project bundles third-party content. **All third-party assets remain the
property of their respective owners and are subject to their own licenses.**

- **Game Animation Sample (GASP)**, **MetaHuman**, and **UE Mannequin** content —
  © **Epic Games, Inc.**, provided under the Unreal Engine EULA / sample-content terms.
- **GASPALS** overlay plugin — by **Polygon Hive**
  ([YouTube](https://youtube.com/polygonhive)).
- **Mixamo** animations — © Adobe.
- The **shooting system** (weapon component, data assets, health/damage, HUD, input)
  is original work by this repository's author.

If you are a rights holder and want content removed, please open an issue.

---

## 📦 What's in the repo (and what isn't)

Committed: project `Config/`, the `GASPALS` plugin (`Content/`, `Config/`,
`Resources/`, `.uplugin`), and `GASPALS.uproject`.

Ignored (regenerated by the engine — see `.gitignore`): `Binaries/`, `Build/`,
`Intermediate/`, `Saved/`, `DerivedDataCache/`, and IDE/editor files.

---

## 🧭 Roadmap / not yet implemented

Fire & reload animation montages, recoil animation additive, muzzle/tracer/impact
Niagara VFX, weapon SFX, camera shake, the projectile path (a `BP_Projectile_Base`
stub exists), bullet-spread cone, and headshot multipliers. The firing logic and
data model already have the hooks for these.

---

## 📄 License

The **original shooting-system code/Blueprints** in this repository are released
under the [MIT License](LICENSE). **Bundled third-party content (Epic, Polygon Hive,
Adobe) is NOT covered by that license** and remains under its respective terms — see
**Credits & attribution** above.
