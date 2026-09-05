---
name: gpb-to-mxb
description: Converts PiBoSo GPBikes mods to MX Bikes — bikes, tracks, tyres, helmets, suits, and other assets. Use when converting GPBikes mods to MX Bikes, porting bikes/tracks/tyres, or handling PiBoSo .edf/.cfg/.geom formats.
---

# GPB to MXB Converter

Converts any PiBoSo GPBikes mod to MX Bikes format. Handles bikes, tracks, tyres, gear (helmets/suits/boots), and stands.

## How it works

### Step 1 — Detect mod type

Inspect source folder structure and file patterns:

| Pattern | Mod type |
|---|---|
| `bikes/<id>/BIKE.edf + <id>.cfg + <id>.geom + *.hrc` | Bike |
| `tracks/<name>/*.edf + track.ini + centerline` or `TRKASPH` objects | Track |
| `tyres/<pack>/f_tyre.edf + r_tyre.edf + <pack>.cfg + *.tyre` | Tyre pack |
| `helmets/<name>/helmet.edf` or `riders/<name>/` or `suits/` | Gear (helmet/suit/boots) |
| `misc/stands/<name>/bikestand.edf` | Stand |
| `<name>.pkz` (renamed .zip) | Archive — unpack first |

```bash
# unpack if needed (.pkz = .zip)
unzip mod.pkz -d ./source
ls -R ./source
```

### Step 2 — Create MX Bikes target layout

MX Bikes expects mods under `My Documents/PiBoSo/MX Bikes/mods/` (Wine: `<prefix>/drive_c/users/<user>/My Documents/PiBoSo/MX Bikes/mods/`):

```
mods/
├── bikes/<BikeID>/
├── tracks/<category>/<TrackID>/
├── tyres/<PackID>/
├── helmets/<HelmetID>/
├── boots/<BootID>/
├── rider/riders/<RiderID>/  # rider suits — NOTE singular "rider"
└── misc/stands/<StandID>/
```

> **Path correction:** Rider suits install to `mods/rider/riders/<RiderID>/` (singular `rider`), **not** `mods/riders/` or `mods/rider/<RiderID>/`. Verified against stock MX Bikes install (`rider.pkz` unpacks to `rider/riders/default_mx/`).

```bash
mkdir -p "MX Bikes/mods/bikes" "MX Bikes/mods/tyres" "MX Bikes/mods/tracks" "MX Bikes/mods/misc/stands" "MX Bikes/mods/helmets" "MX Bikes/mods/boots" "MX Bikes/mods/rider/riders"
```

### Step 3 — Convert by type

#### A. Bikes

| GPB file | Action | MXB target | Notes |
|---|---|---|---|
| `BIKE.edf` (`bike.edf` lowercase variant) | COPY+RENAME | `model.edf` | Identical .edf format (FBX2EDF) |
| `BIKE_s.edf` | COPY+RENAME | `model_shadow.edf` | Shadow/occlusion |
| `*.hrc` (6: chassis/steer/fsusp/rsusp/fwheel/rwheel) | COPY+EDIT | same | `scene = bike.edf` → `scene = model.edf` |
| `<id>.cfg` | REBUILD | `<id>.cfg` | See § Rebuild spec |
| `<id>.ini` | COPY | same | Stand ref kept `stand = MotoGP_23` |
| `<id>.geom` | COPY | same | BikeEd v2 compatible |
| `ENGINE_A/B/C.engn` | COPY | same | EngineEd |
| `engine_EN.scl / engine_EX.scl` | COPY | same | MaxSCL |
| `*.wav` | COPY | same | Plain, not encrypted |
| `*.tga` fonts + `rpm.spr` + `c_chassis.wss` | COPY | same | HUD + sprites |
| `gfx.cfg` | REBUILD | `gfx.cfg` | `BIKE_s.edf→model_shadow.edf` (6), `BIKE.edf→model.edf` (3), delete `rainlight` |
| `hud.cfg` | REBUILD | `hud.cfg` | MXB template |
| `cameras.cfg` | REBUILD | `cameras.cfg` | Port positions |
| `sfx.cfg / dashboard.cfg / inertia.cfg` | COPY | same | Verify default refs |
| `paints/*.pnt` | COPY | same | PaintEd |

**Bike .cfg rebuild — section by section:**

| Section | Action |
|---|---|
| Header `type/ID/geom_id/geom` | KEEP; delete duplicate `geom_id` if present |
| `engine Clutch` | Delete `Slipper=1`, `RampsAngle{}` , `PressurePlate{}` |
| `fueltank / gearbox / driveline` | KEEP |
| `tyres {id}` | KEEP `id = <PackID>` (must exist in `mods/tyres/`) |
| `wheel0 / wheel1` tyre blocks | Delete `numsets`, `Friction 1.5→1.1` |
| `chassis` | KEEP Mass/Inertia; keep per-bike stiffness or retune |
| `aero` | DELETE entire block |
| `steer / front_suspension / rear_suspension` | KEEP (sportbike travel 110mm = supermoto; for dirt retune via BikeEd) |
| `ecu / autoclutch` | KEEP |

Normalize CRLF→LF when rebuilding.

#### B. Tracks

| Aspect | GPB | MXB | Conversion |
|---|---|---|---|
| Terrain | Mesh `TRK*` in `.edf` | Heightmap `.hmf/.tht/.raw` + `.trh` OR mesh-only | For supermoto/road: re-export mesh via FBX2EDF → `track.map` + `track.trp` (no ruts). For dirt: rebuild as heightmap in TerrainEd/TrackEd (requires manual sculpt) |
| Collision | `.trp` from FBX2EDF | `.trh` (heightmap) or `.trp` (mesh) | Generate via FBX2EDF Collision mode |
| Centerline/pits | TrackEd | TrackEd → `.rdf` (MXB: needs `gate.edf`) | Recreate in TrackEd |
| Env | `.ini/.amb/skydome.edf` | Same | COPY |
| Paints | `.pnt` | `.pnt` | COPY |
| Tools | `ct.zip/tt.zip` (FBX2EDF) | Same | Shared |

Choose path via:

| Track type | MXB path | Dynamic ruts? |
|---|---|---|
| Road / supermoto | Mesh-only (FBX2EDF Map+Collision) | No — static |
| Dirt MX | Heightmap rebuild (TerrainEd) | Yes |

#### C. Tyres

Copy whole pack `tyres/<PackID>/` to `mods/tyres/<PackID>/`: `f_tyre.edf / r_tyre.edf / _s` + `gfx.cfg` + `<pack>.cfg` + `*.tyre`. Fix `params =` to resolve in same folder. Add `.lyr` dirt layers optionally (v2 — bikes work without).

#### D. Gear

##### D1. Helmets / Boots — COPY (identical format)

Helmets and boots **are** identical between GPB and MXB — verbatim copy works:

| GPB source | MXB target | Action |
|---|---|---|
| `helmets/<HelmetID>/helmet.edf + gfx.cfg + *.tga/*.dds` | `mods/helmets/<HelmetID>/` | COPY |
| `boots/<BootID>/boots.edf + gfx.cfg + *.tga` | `mods/boots/<BootID>/` | COPY |

Preserve folder name as ID. No `.cfg` rebuild needed. Textures/shaders copy as-is.

> **Exclude riders/suits from this rule** — see D2 below. Riders are NOT identical.

##### D2. Riders / Suits — FULL RE-RIG REQUIRED (NOT identical)

> [!WARNING]
> **Verbatim copy of a GPB rider to MX Bikes will result in T-pose, detached helmet/boots, and no animation.** Skeletons are disjoint — no compatible animation. Do NOT use COPY for riders. Full re-rig + cfg rebuild required.

**Why COPY fails — EDF skeleton incompatibility (verified against `rider.pkz`):**

Extract reference: `unzip "/mnt/data/SteamLibrary/steamapps/common/MX Bikes/rider.pkz" -d /tmp/mxb_rider && strings rider.edf | head`

| File | GPBikes | MX Bikes (`default_mx/`) | Incompatibility |
|---|---|---|---|
| `rider.edf` | 25 bones, 3 sections, 26.4 MB | **70 bones**, **5 sections**, 28.3 MB | Disjoint skeletons — no animation retarget possible without re-rig |
| `c_rider.edf` | 1 section, 5.8 MB | **2 sections**, 27.2 MB | Section count mismatch |
| `rider_s.edf` (shadow) | 4.6 MB | **186 KB** | Size inversion indicates different mesh/LOD strategy |
| Bone names (GPB) | `calf_l`, `hand_l`, `spine_01`, `lowerarm_l`, `upperarm_l`, `thigh_l` ... (25 total) | `riderRIG_Root`, `riderRIG_Pelvis`, `riderRIG_Spine1`, `riderRIG_Spine2`, `riderRIG_Spine3`, `riderRIG_Spine4`, `riderRIG_LeftShoulder`, `riderRIG_LeftElbow`, `riderRIG_LeftWrist`, `riderRIG_RightWrist`, `riderRIG_Head`, `riderRIG_LeftKneeTwist`, `riderRIG_armour`, `riderRIG_LeftAnkle` ... (70 total) | No 1:1 mapping; bone hierarchy completely different |
| Example bone table GPB (25) | `spine_01`, `spine_02`, `neck_01`, `head`, `clavicle_l/r`, `upperarm_l/r`, `lowerarm_l/r`, `hand_l/r`, `thigh_l/r`, `calf_l/r`, `foot_l/r`, `toe_l/r` | `riderRIG_Root`, `riderRIG_Pelvis`, `riderRIG_Spine1-4`, `riderRIG_Neck`, `riderRIG_Head`, `riderRIG_LeftCollar`/`RightCollar`, `riderRIG_LeftShoulder`/`RightShoulder`, `riderRIG_LeftElbow`/`RightElbow`, `riderRIG_LeftWrist`/`RightWrist`, `riderRIG_LeftFingers` etc., `riderRIG_LeftUpLeg`, `riderRIG_LeftKnee`, `riderRIG_LeftKneeTwist`, `riderRIG_LeftAnkle`, `riderRIG_armour` | MXB uses `riderRIG_*` prefix convention; GPB uses anatomical short names |

Verification commands:
```bash
# bone name check
strings rider.edf | grep -E "calf_l|hand_l|spine_01|riderRIG" | sort | uniq -c
# section count (FBX2EDF header or strings)
strings rider.edf | grep -c "section"
ls -lh rider.edf c_rider.edf rider_s.edf
```

**Required conversions for riders:**

**1. `gfx.cfg` — REBUILD (before/after template diff):**

GPB `gfx.cfg` (example):
```
suit
{
  texture = suit
}
helmetlinkobj = helmet
leftarm
{
  linkobj = hand_l
}
rightarm
{
  linkobj = hand_r
}
leftknee
{
  linkobj = calf_l
}
rightknee
{
  linkobj = calf_r
}
leftboot
{
  linkobj = foot_l
}
rightboot
{
  linkobj = foot_r
}
maxlean = 64
```

MXB `gfx.cfg` (required):
```
suit
{
  texture = rider
}
instance
{
  model = i_rider.edf
}
helmetlinkobj = riderRIG_Head
leftarmlinkobj = riderRIG_LeftWrist
rightarmlinkobj = riderRIG_RightWrist
leftelbowlinkobj = riderRIG_LeftElbow
rightelbowlinkobj = riderRIG_RightElbow
leftshoulderlinkobj = riderRIG_LeftShoulder
rightshoulderlinkobj = riderRIG_RightShoulder
leftbootlinkobj = riderRIG_LeftKneeTwist
rightbootlinkobj = riderRIG_RightKneeTwist
neckbracelinkobj = riderRIG_Spine4
armourlinkobj = riderRIG_armour
animations = mx
# or animations = sm for supermoto variant
```

Diff summary:

| GPB key | MXB key | Action |
|---|---|---|
| `suit { texture = suit }` | `suit { texture = rider }` | RENAME slot `suit` → `rider` |
| `helmetlinkobj = helmet` | `helmetlinkobj = riderRIG_Head` | RETARGET bone |
| `leftarm { linkobj = hand_l }` + `lowerarm_l`/`upperarm_l` blocks | `leftarmlinkobj = riderRIG_LeftWrist` + `leftelbowlinkobj = riderRIG_LeftElbow` + `leftshoulderlinkobj = riderRIG_LeftShoulder` | SPLIT into Wrist/Elbow/Shoulder |
| `leftknee { linkobj = calf_l }` / `rightknee` | **DELETE** — no knee blocks in MXB rider gfx | REMOVE 4x leftknee/rightknee/leftboot/rightboot blocks |
| `leftboot { linkobj = foot_l }` | `leftbootlinkobj = riderRIG_LeftKneeTwist` | RETARGET to KneeTwist |
| — | `instance { model = i_rider.edf }` | **ADD** required instance |
| — | `neckbracelinkobj = riderRIG_Spine4` | **ADD** |
| — | `armourlinkobj = riderRIG_armour` | **ADD** |
| `maxlean = 64` | *(removed from rider gfx — handled by bike physics)* | **DELETE** |
| — | `animations = mx` or `sm` | **ADD** (`mx` for off-road, `sm` for supermoto) |

**2. `rider.hrc` — REBUILD to 3 LODs:**

GPB: 1 LOD only.
MXB: 3 LODs required:

```
# rider.hrc (MXB)
hrc
{
  lod
  {
    model = rider.edf
  }
  lod
  {
    model = riderb.edf
    switch = 5
  }
  lod
  {
    model = riderc.edf
    switch = 10
  }
}
```

Generate `riderb.edf`/`riderc.edf` by decimating re-rigged mesh to ~50% / ~25% poly count and exporting via FBX2EDF. Switch distances 5m and 10m are stock MXB values (verified in `default_mx/rider.hrc`).

Alternatively copy LOD structure from `default_mx/rider.hrc` as template and replace model names.

**3. `.ini` — REBUILD type:**

| GPB | MXB | Action |
|---|---|---|
| `type = modern` | `type = mx` or `type = sm` | REPLACE — `modern`/`type 1` invalid in MXB |
| `type = 1` | `type = mx` | REPLACE numeric → string |
| *(missing)* | `helmet_type = ...` | ADD — links to `mods/helmets/` |
| *(missing)* | `boots_type = ...` | ADD — links to `mods/boots/` |
| *(missing)* | `protection_type = ...` | ADD — neck brace/armour |

Example MXB `rider.ini`:
```ini
[info]
name = My Rider
type = mx
helmet_type = default_mx
boots_type = default_mx
protection_type = default_mx
```

**4. Paints (`.pnt`) — material slot rename:**

| GPB slot | MXB slot | Action |
|---|---|---|
| `arms` | `rider` | RENAME |
| `boots` | `rider` | MERGE into `rider` |
| `suit` | `rider` | RENAME |

Use PaintEd or edit `.pnt` text: replace `material = arms` → `material = rider`. Verify with `grep "material" *.pnt`.

**5. Install path:**

```
# WRONG (old skill, GPB layout)
mods/riders/<RiderID>/
mods/rider/<RiderID>/

# CORRECT (MXB)
mods/rider/riders/<RiderID>/
# Example
mods/rider/riders/my_suit/rider.edf
mods/rider/riders/my_suit/rider.hrc
mods/rider/riders/my_suit/c_rider.edf
mods/rider/riders/my_suit/gfx.cfg
mods/rider/riders/my_suit/rider.ini
```

**6. Manual Blender re-rig workflow (required — no automated converter):**

No automated GPB→MXB rider converter exists. The only proven path is manual re-rig in Blender:

```bash
# 1. Unpack GPB rider
unzip gpb_rider.pkz -d ./gpb_rider
# GPB rider.edf is FBX2EDF format — use edf2fbx (from ct.zip/tools) to unpack if available
# or use community edf2fbx.py:
python3 edf2fbx.py gpb_rider/rider.edf -o gpb_rider.fbx

# 2. Extract MXB skeleton reference
unzip "/mnt/data/SteamLibrary/steamapps/common/MX Bikes/rider.pkz" -d /tmp/mxb_ref
python3 edf2fbx.py /tmp/mxb_ref/rider/riders/default_mx/rider.edf -o mxb_skeleton.fbx
# Alternatively import default_mx/rider.edf directly via FBX2EDF import if Blender plugin available

# 3. In Blender:
#    a. Import gpb_rider.fbx (mesh + 25-bone armature)
#    b. Import mxb_skeleton.fbx (70-bone riderRIG_* armature) — keep as target skeleton
#    c. Select GPB mesh -> delete old armature modifier
#    d. Parent mesh to MXB armature with "With Empty Groups"
#    e. Weight paint to riderRIG_* bones:
#       - riderRIG_Pelvis / Spine1-4 for torso
#       - riderRIG_LeftShoulder/Elbow/Wrist for arms
#       - riderRIG_LeftUpLeg/Knee/KneeTwist/Ankle for legs
#       - riderRIG_Head for helmet link, riderRIG_armour for armour
#    f. Verify no vertices weighted to old bone names (calf_l etc.)
#    g. Export as FBX (Apply Transform, Armature + Mesh, binary FBX)

# 4. Convert back to EDF via FBX2EDF (ct.zip)
# Use Track/Bike Conversion Tools -> FBX2EDF or command-line:
wine FBX2EDF.exe gpb_rerigged.fbx rider.edf
# Repeat for c_rider.edf (collision mesh, simplified) and rider_s.edf (shadow)
# Generate LODs: decimate modifier 0.5 -> riderb.fbx -> riderb.edf, 0.25 -> riderc.fbx -> riderc.edf

# 5. Verify bone names in output
strings rider.edf | grep riderRIG | sort | uniq | head -20
# Expect: riderRIG_Root, riderRIG_Pelvis, riderRIG_Spine1...
strings rider.edf | grep -E "calf_l|hand_l|spine_01" && echo "FAIL: old GPB bones still present" || echo "OK: no GPB bones"
ls -lh rider.edf c_rider.edf rider_s.edf
# Expect ~28MB / ~27MB / ~186KB with 5/2/1 sections respectively
```

Tools: `ct.zip` (contains FBX2EDF, `edf2fbx` helper), Blender 3.x+, MX Bikes `rider.pkz` as skeleton reference. Shared PiBoSo tool chain — same as bikes/tracks.

#### E. Stands / Misc

`misc/stands/<name>/` → COPY: `bikestand.edf/_s/_as.edf + gfx.cfg + <name>.ini`. Bike `.ini` `stand = <name>` must match folder (case-insensitive).

### Step 4 — Handle shared PiBoSo tool chain

| Tool | Purpose | Shared |
|---|---|---|
| FBX2EDF (`ct.zip/tt.zip`) | FBX→.edf / .map/.trp | Yes |
| BikeEd | .geom + inertia.cfg | Yes |
| EngineEd | .engn | Yes |
| TyreEd | .tyre | Yes |
| PaintEd | .pnt | Yes |
| MaxSCL | .scl sounds | Yes |
| TrackEd/TerrainEd/MapView | Track data | Yes |

No dedicated GPB→MXB converter exists — manual steps above are the community-proven path.

### Step 5 — Verify

```bash
# bikes — per bike
grep -c "aero" <bike>.cfg; echo "expect 0"
grep -c "Slipper\|RampsAngle\|PressurePlate\|numsets" <bike>.cfg; echo "expect 0"
grep "id = <TyrePack>" <bike>.cfg; echo "expect 1"
grep "^ID =" <bike>.cfg; echo "must match folder name"
grep -c "scene = model.edf" *.hrc; echo "expect 6"
# gfx
grep -c "model_shadow.edf" gfx.cfg; echo "expect 6"
grep -c "rainlight" gfx.cfg; echo "expect 0"
# global
grep -r "scene = model.edf" mods/bikes --include="*.hrc" | wc -l; echo "expect 48 for 8 bikes"
grep -r "BIKE" mods/bikes --include="gfx.cfg" | wc -l; echo "expect 0"

# riders — per rider (NEW)
echo "=== rider EDF bone check ==="
strings mods/rider/riders/<RiderID>/rider.edf | grep -c "riderRIG"; echo "expect 70"
strings mods/rider/riders/<RiderID>/rider.edf | grep -E "calf_l|hand_l|spine_01" && echo "FAIL old bones" || echo "OK no GPB bones"
ls -lh mods/rider/riders/<RiderID>/rider.edf mods/rider/riders/<RiderID>/c_rider.edf mods/rider/riders/<RiderID>/rider_s.edf
# expect ~28MB / 27MB / 186KB
echo "=== rider gfx check ==="
grep -c "riderRIG_Head" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 1"
grep -c "riderRIG_LeftWrist" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 1"
grep -c "suit.*rider" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 1"
grep -c "instance" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 1 (i_rider.edf)"
grep -c "maxlean" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 0"
grep -c "leftknee\|rightknee" mods/rider/riders/<RiderID>/gfx.cfg; echo "expect 0"
echo "=== rider hrc check ==="
grep -c "riderb.edf" mods/rider/riders/<RiderID>/rider.hrc; echo "expect 1"
grep -c "riderc.edf" mods/rider/riders/<RiderID>/rider.hrc; echo "expect 1"
grep -c "switch" mods/rider/riders/<RiderID>/rider.hrc; echo "expect 2 (5 and 10)"
echo "=== rider ini check ==="
grep -E "type = (mx|sm)" mods/rider/riders/<RiderID>/rider.ini; echo "expect 1"
grep -c "helmet_type" mods/rider/riders/<RiderID>/rider.ini; echo "expect 1"
echo "=== rider paint check ==="
grep -r "material = rider" mods/rider/riders/<RiderID> --include="*.pnt" | wc -l; echo "expect >=1"
grep -r "material = arms" mods/rider/riders/<RiderID> --include="*.pnt" | wc -l; echo "expect 0"
echo "=== rider path check ==="
ls -d mods/rider/riders/<RiderID> && echo "OK path" || echo "FAIL path should be mods/rider/riders/<ID>"
```

## Rules

- Never overwrite without reading existing MXB mod of same ID — ask before.
- Preserve model scale 1:1 meters (verify against MXB template blend).
- Keep sportbike geometry for supermoto use; only retune suspension via BikeEd if user wants dirt handling.
- Tyre `Friction 1.5→1.1` for MXB; keep original chassis stiffness per variant.
- Textures must be power-of-2 (256/512/1024/2048) TGA/BMP/DDS DXT1/3/5.
- `.lyr` mud layers are optional v2 — bikes load without.
- Respect author permission — conversion for personal use unless permission granted.
- Normalize CRLF→LF in rebuilt text files; keep `.edf` binary intact.
- Mesh-only tracks have no dynamic ruts — document limitation.
- **Riders require re-rig — never COPY GPB rider.edf verbatim; verify `riderRIG_*` bones via `strings`.**

## Output format

```markdown
## GPB→MXB Conversion — <ModName>

**Type:** Bike | Track | Tyre | Gear | Stand | Mixed
**Source:** <path>  Target: <path>
**Handling:** Sportbike (supermoto) | Dirt-retuned

**Converted:**
- Bikes: <list> (model rename, hrc fix, cfg rebuild)
- Tyres: <pack> (13 files)
- Stands/Tracks/Gear: <list>
- Riders: <list> (re-rigged to riderRIG_*, gfx/hrc/ini/pnt rebuilt, installed to mods/rider/riders/)

**Verification:**
- Bikes: 8/8 no aero/Slipper/numsets, Friction 1.1, 48/48 hrc OK
- Gfx: 6 shadow / 3 model, no BIKE/rainlight
- Tyres: params resolve 7/7
- Riders: <N>/N riderRIG 70 bones, gfx retarget OK, 3 LODs, type mx/sm, material rider

**Notes:** Mud layers skipped | Track mesh-only no ruts | Permission personal-use
**Next:** Copy `mods/` into `My Documents/PiBoSo/MX Bikes/mods/` and test in-game
```

## After conversion

- Chain @quality (verify mode) for file verification if user requests.
- Offer v2 enhancements: .lyr mud layers, BikeEd geometry retune, cockpit/temp models.

## Changelog

- **2026-08-21 — v1.1 Rider fix:** Corrected Gear section. Previous version incorrectly stated "Gear: .edf + textures identical — COPY. No .cfg rebuild needed" for all gear. Verified against real `rider.pkz` (`/mnt/data/SteamLibrary/steamapps/common/MX Bikes/rider.pkz`): GPB `rider.edf` (25 bones, 3 sections, 26.4 MB) vs MXB `default_mx/rider.edf` (70 bones `riderRIG_*`, 5 sections, 28.3 MB) are disjoint — no compatible animation. Split Gear into D1 (helmets/boots — COPY) and D2 (riders — REBUILD). Documented: EDF skeleton incompatibility table, `gfx.cfg` before/after diff (Wrist/Elbow/Shoulder retarget, `suit→rider`, `instance{i_rider.edf}`, `leftbootlinkobj=riderRIG_LeftKneeTwist`, `neckbracelinkobj`, `armourlinkobj`, `animations=mx/sm`, delete `leftknee` blocks/`maxlean`), `rider.hrc` 3 LODs (switch 5/10), `.ini` `modern type 1 → mx/sm` + `helmet_type/boots_type/protection_type`, `.pnt` `arms→rider`, correct path `mods/rider/riders/` (singular), manual Blender re-rig workflow (edf2fbx → import MXB skeleton → weight paint → FBX2EDF ct.zip → `strings` verify). Added warning box (T-pose/detached helmet/boots) and rider-specific verification block. Helmets/boots remain COPY; riders explicitly excluded.
