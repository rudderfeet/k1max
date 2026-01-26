## K1 Max Configuration Backup

Authoritative source of truth for printer configuration.

Backup procedure:
1. Clone repo
2. Run backup script:
   backup_k1max.sh
3. Commit and push

Printer Notes:
- Printer runs Creality Buildroot firmware
- I've modified it for Moonraker + Mainsail only (no Fluid)
- No on-printer git tools available
- Creality has non-upstream Klipper components (prtouch_v2, multi-MCU layout)
- The OS is Buildroot-based and intentionally restrictive
- Small firmware updates can subtly invalidate assumptions to say the least, so don't do it!
- See: https://github.com/Mariusjuvet1/creality-k1-setup BUT NOTE THIS TOOK A LOT OF BACK AND FORTH EDITING WITH CHATGPT TO CORRECT
- Primary Klipper config location: /usr/data/printer_data/config/
- Moonraker data path: data_path: /usr/data/printer_data
- Read-Only or Vendor Managed: /usr/share/klipper, /usr/share/klippy-env, /etc/init.d/S55klipper_service
- So, to back these up, use this once: curl http://127.0.0.1:7125/server/info > /usr/data/printer_data/config/baseline_server_info.json
-   then do this periodically from my local Mac:
- scp -O -r root@<printer-ip>:/usr/data/printer_data ./k1max-backup/
- The rear chamber fan is "fan1" so 5% activation for fumes would be "SET_PIN PIN=fan1 VALUE=12"

Slicer Notes:
- Create one printer profile which points to an IP-reserved physical machine
- Create one print profile for each quality (layer height) and nozzle, and go as fast as the printer will allow
- Create one filament profile for each significantly different material, and limit speed using max volumetric rate
  - ABS, ASA: 12 mm3/s, 250C nozzle, 90C bed
  - PCTG: 14 mm3/s, 260C nozzle, 90C bed
  - PETG: 12 mm3/s, 250C nozzle, 90C bed
  - PA: 7 mm3/s, 275C nozzle, 100C bed
  - PC: 7 mm3/s, 275C nozzle, 100C bed
  - PLA: 20 mm3/s, 200C nozzle, 50C bed
  - TPU: 1.2-3 mm3/s, 200C nozzle, ambient-to-50C bed

Bed Leveling Notes:
- Follow the steps here: https://www.reddit.com/r/crealityk1/comments/17htfug/traming_your_k1_bed_2_electric_boogaloo/
- Remove the two scripts when done - they can cause strange issues with Creality's funky Klipper lifecycles

Filament Calibration (Mid-January 2026):
1. Pressure Advance (done through touchscreen)
2. Volumetric ceiling per material (see above!)
3. Temperature window sanity check (lack of stringing confirms this)
4. Extrusion Multiplier per material (https://teachingtechyt.github.io/calibration.html#flow, and use 50mm/s speeds)
   - ASA / ABS = 1.025x
   - PCTG = 1.00x
   - PETG = 0.98x
   - PC = 0.925x
   - PLA = 1.0x
   - TPU = 1.15x
 5. Shrinkage Compensation XY (dimensional accuracy) from Califlower tests, only after #4!
   - ASA / ABS = 0.41%
   - PCTG = 0.39%
   - PETG = 0.32%
   - PC = 0.51
   - PLA = 1.0x
   - TPU = 0.0%
6. Optional micro-optimizations (holes, elephant foot, etc.)
   - Top surface improvements
	  - Monotonic top infill
	  - Top solid flow ratio ~0.96–0.98
	  - Top solid speed ≤ 22 mm/s
