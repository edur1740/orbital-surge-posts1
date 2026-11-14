# BLASTER v2: ORBITAL SURGE competition robot (assembly)

This robot is designed to be built. Every part is a separate solid in a real assembly, and the robot is engineered to:
- shoot CELLS into the CORE from anywhere in the LAUNCH ZONE,
- carry a ROD to L1,
- climb HIGH (lowest bumper 31.2 in above the carpet while hanging).

All gears are real 20 DP involute gears at the correct center distances. Screws, nuts and standoffs are modeled to McMaster-Carr sizes and listed with McMaster part numbers.

## Files

| File | What it is |
|---|---|
| `BLASTER_full_robot.step` | Full assembly: 627 products (motors, gearboxes, electronics, sensors, routed wires and zip ties included), colored, in millimetres. 184 MB |
| `BLASTER_no_electronics.step` | Same robot with the Electrical subassembly, sensors and wiring removed. 144 MB |
| `BLASTER_BOM.csv` | Parts list: quantity, part number, description, subsystem, and a McMaster link for the hardware |
| `stl/` | STL meshes: full robot, no electronics, and each subsystem |
| `BLASTER_wiring_diagram.png` / `.svg` | Matches the routed 3D harness one-for-one: PDH channels, CAN chain order, Ethernet, RSL, motor leads and every sensor (DIO and SPARK MAX data ports) |
| `BLASTER_v2_real_parts_closeups.jpg` | Close-ups of the real motors, gearboxes and electronics in place |
| `BLASTER_electronics_before_after.jpg` | The electronics bay with stand-in blocks vs. real vendor parts |
| `BLASTER_v2_with_electronics.jpg`, `..._without_electronics.jpg`, `..._mechanisms.jpg` | Renders |

## Assembly tree

```
BLASTER
├─ Drivetrain
│  ├─ Frame
│  │  └─ Frame hardware (12 brackets, 48 bolts and nuts)
│  ├─ Drive side L
│  │  ├─ Wheels and belts L
│  │  └─ Gearbox L
│  └─ Drive side R (mirror)
├─ Bumpers (front, back, left, right)
├─ Turret and shooter
│  ├─ Superstructure
│  ├─ Turret drive
│  └─ Turret (rotating)
│     └─ Shooter
├─ Intake
├─ Hopper and indexer
├─ Rod cradle
├─ Climber
└─ Electrical
```

## How each mechanism works

| Mechanism | Design | Gearing | Result |
|---|---|---|---|
| Drive | 6-wheel tank, 4 in wheels, 2 Kraken X60 per side, HTD 5 mm belts (115T, 24T pulleys) | Kraken 14T → 50T, then 20T → 44T = **7.86 : 1** | 13.3 ft/s free, about 11.3 ft/s loaded. Pushing force is far past traction, so set the current limit to 60 A |
| Intake | Over-the-bumper pivot arm, 2 in roller | Pivot: NEO + 25:1 MAXPlanetary + 12T → 60T = **125 : 1**. Roller: NEO 550 + 3:1, GT2 belt | Deploys 90° in about 0.35 s; roller reaches 10.1 in past the frame (limit 12) |
| Hopper / indexer | Ramp feeds a vertical side-belt column; holds **exactly 2 CELLS** | 2× NEO 550 + 5:1, 1.25 in rollers | Belts lift at about 12 ft/s and squeeze each CELL 0.2 in per side |
| Turret | 10 in thin-section bearing, 200T ring gear, CELL passes up through the 8.5 in bore | NEO 550 + 9:1 + 12T → 200T = **150 : 1** | About 440°/s, soft limits ±135°, aimed by Limelight 3 |
| Shooter | Two 4 × 4 in flywheels in front of and behind the CELL; fixed hood sets a 58° exit | Kraken 14T → 30T = **2.14 : 1** per wheel | 2,800 rpm max (48.9 ft/s wheel surface). The CELL needs only 19–26 ft/s, so code caps the wheels at about 2,100 rpm |
| Rod cradle | Rear cradle holds 1 ROD at 18 in and rolls it straight onto L1 | NEO 550 + 2-stage MAXPlanetary | |
| Climber | 2-stage telescoping tubes, hook with a throat sized for the TETHER bar, 1 in winch spools | NEO + 25:1 + 16T → 48T = **75 : 1**, plus a ratchet pawl | Lifts the 141 lb robot about 4 in/s, raising it about 29 in so the lowest bumper clears HIGH. The ratchet holds with power off (R1102), and a pull pin releases it (R1104) |

### Shooter numbers

The CORE rim is 72 in high, and the hood exit is about 27 in. The table shows the exit speed needed and the peak height at a 58° launch.

| Distance to CORE center | 68 in | 100 in | 130 in | 160 in | 188 in |
|---|---|---|---|---|---|
| Exit speed needed | 18.6 ft/s | 20.4 ft/s | 22.2 ft/s | 24.1 ft/s | 25.7 ft/s |
| Peak height | 73 in | 83 in | 93 in | 105 in | 115 in |

- Every shot stays under the 40 ft/s launch limit (R1001) and below the 12 ft ceiling (R203).
- The CELL is descending when it reaches the rim, so it drops in.
- Tune the speed for each distance on the practice field and confirm with the I105 radar check.

## Rule check (measured from the model)

| Rule | Limit | BLASTER v2 |
|---|---|---|
| R101 frame perimeter | 120 in | 108 in (27 × 27 in) |
| R109 / G302 starting box with bumpers | 38 × 38 × 42 in | 33.5 × 33.5 × 37.6 in |
| G302 nothing past the frame at the start | bumpers only | only the 0.10 in button heads of the frame bolts, behind the bumper plywood |
| R103 / R107 extension | 12 in, one 90° arc | 10.1 in, front only |
| R106 height | 72 in in the ENDGAME | 71.5 in with the climber up (hook top) |
| R104 weight (no bumpers or battery) | 125 lb | **117.4 lb**, with every plate solid, all wiring and sensors, and a 6 lb steel ballast plate that levels the robot while hanging |
| R105 / G401 piece limit | 2 CELLS + 1 ROD | the hopper physically holds 2; the cradle holds 1 |
| R1001 launch speed | 40 ft/s | the geared limit allows only a little more than 40 ft/s; capped in code, verify with radar |

## Put it in Onshape

1. Create a new Onshape document and name it **BLASTER**.
2. Click **+ → Import** and choose `BLASTER_full_robot.step`.
3. When it asks, turn on **Import as a single part studio and assembly** (or leave the default, which creates an Assembly tab and a Part Studio). Leave units as they are: the file stores millimetres, and Onshape shows inches if your document uses inches.
4. Onshape builds an **Assembly** tab that keeps the tree above, with every part already in place. The 182 MB file can take 10–20 minutes to import. If Onshape times out, import `BLASTER_no_electronics.step` first.
5. Repeat with `BLASTER_no_electronics.step` for the robot without electronics, or suppress the **Electrical** subassembly instance in the first one.

### Make it move (add mates)

Imported parts come in fixed, so mates are what make the robot move. These take about 15 minutes:

1. **Fix the frame:** right-click **Frame** → **Fix**.
2. **Fasten what doesn't move:** group each rigid subassembly (Superstructure, Hopper and indexer, Rod cradle, Electrical, Bumpers) and fasten it to the Frame with **Group** or **Fasten**.
3. **Shafts spin (Revolute mates):** add a Revolute mate between each hex shaft and its bearing. These are the wheel axles, the gearbox intermediate and output shafts, the flywheel shafts, the intake pivot and roller, and the winch shaft.
4. **Gears and pulleys turn the right amount:** use **Mate connector → Relations → Gear** between the two Revolute mates, with these ratios:
   - Drive: pinion to 50T is 14:50, and 20T to 44T is 20:44.
   - Shooter: 14:30.
   - Turret: 12:200.
   - Intake pivot: 12:60.
   - Winch: 16:48.
   - Belted wheels: 1:1.
5. **Turret:** add a Revolute mate between the **Turret (rotating)** group and the turret bearing, with limits of −135° to +135°.
6. **Intake arms:** add a Revolute mate on the pivot shaft, limited from 85° (stowed) to −35° (deployed).
7. **Climber:** add Slider mates for the middle and inner tubes, with 22 in of travel each.

Now you can drag a wheel and watch the gearbox and motors turn, swing the turret, or deploy the intake.

## Hardware (McMaster-Carr)

| Part number | Item | Qty | Checked |
|---|---|---|---|
| 91255A263 | 10-32 × 3/8" button head, black-oxide alloy steel | 36 | McMaster page |
| 91255A265 | 10-32 × 1/2" button head | 2 | McMaster page |
| 91255A273 | 10-32 × 1" button head | 8 | cross-reference listing |
| 91255A275 | 10-32 × 1-1/2" button head | 64 | cross-reference listing |
| 91251A542 | 1/4"-20 × 1" socket head, black-oxide alloy steel | 4 | McMaster page |
| 91581A345 | 10-32 thin nylon-insert locknut, black | 72 | McMaster page |
| 91780A304 | 3/8" hex standoff, 10-32 female, 1-1/4" long, aluminum | 6 | cross-reference listing |

"Cross-reference listing" means a distributor lists it as the McMaster equivalent. Check those part numbers on mcmaster.com before you order. Buy about 20% extra of every screw and nut.

FRC-specific parts come from WCP, REV, CTRE, AndyMark and VEXpro, and are named in the BOM: motors, MAXPlanetary gearboxes, 20 DP gears, hex bearings, hex shaft, belts, wheels and electronics.

## What still needs a real build

The design closes on paper: the gears mesh, the speeds and forces work, and it fits every rule. These things only show up on a real robot:

- **Vendor CAD:** every motor (Kraken X60, NEO, NEO 550), every MAXPlanetary (built slice by slice to the right stage count), and all the electronics (roboRIO 2.0, PDH, VH-109, Radio Power Module, Pigeon 2.0, Limelight 3, SPARK MAX, 120 A breaker, battery and RSL) are the real vendor models, taken from your Onshape export as meshes. They import into Onshape as faceted solids. The motor-mounting plates have #10 clearance holes on the 2 in bolt circle so the motor face screws line up. Round parts in the custom-made pieces are faceted.
- **Holes:** bearing bores, motor pilots, bolt holes and the 1 in tube-hole pattern are cut. Check every plate against the mating parts before waterjet.
- **Prototype the shooter:** the CELL squeeze (0.6 in each side) and the 58° hood are calculated. Build a wood test rig and tune them before cutting aluminum.
- **Wiring:** every wire is routed in the model, 62 cables in all (about 65 ft): battery leads, the PDH to each controller, 3-phase motor leads, the CAN chain, Ethernet, the RSL, the Limelight and every sensor, plus a turret service loop, an intake pivot flex loop and zip ties. Follow it and the wiring diagram, and test the E-STOP and the 1-second stop before your first match.
- **Sensors (all wired):** two IR beam breaks across the indexer (entry and shooter feed) and one across the rod cradle, on roboRIO DIO 0–5; a REV magnetic limit switch to home the turret (magnet on the ring gear) and one for the climber's retracted position, each on its SPARK MAX data port; and a REV Through Bore Encoder on the intake pivot shaft for an absolute arm angle.
