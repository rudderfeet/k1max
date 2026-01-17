## K1 Max Configuration Backup

Authoritative source of truth for printer configuration.

Backup procedure:
1. Clone repo
2. Run backup script:
   scp -O -r root@<printer-ip>:/usr/data/printer_data ./printer_data
3. Commit and push

Printer Notes:
- Printer runs Creality Buildroot firmware
- Moonraker + Mainsail only (no Fluid)
- No on-printer git tools available
- Creality has non-upstream Klipper components (prtouch_v2, multi-MCU layout)
- The OS is Buildroot-based and intentionally restrictive
- Small firmware updates can subtly invalidate assumptions to say the least, so don't do it!
- See: https://github.com/Mariusjuvet1/creality-k1-setup
- BUT NOTE THIS TOOK A LOT OF BACK AND FORTH EDITING WITH CHATGPT TO CORRECT
- Primary Klipper config location: /usr/data/printer_data/config/
- Moonraker data path: data_path: /usr/data/printer_data
- Read-Only or Vendor Managed: /usr/share/klipper, /usr/share/klippy-env, /etc/init.d/S55klipper_service
- So, to back these up, use this once: curl http://127.0.0.1:7125/server/info > /usr/data/printer_data/config/baseline_server_info.json
-   then do this periodically from my local Mac:
- scp -O -r root@<printer-ip>:/usr/data/printer_data ./k1max-backup/

Slicer Notes:
- Create one printer profile which points to an IP-reserved physical machine
- Create one print profile for each quality (layer height) and nozzle, and go as fast as the printer will allow
- Create one filament profile for each significantly different material, and limit speed using max volumetric rate
  - PLA: 20 mm3/s, 200C nozzle, 45C bed
  - ABS, ASA: 12 mm3/s, 250C nozzle, 100C bed
  - PETG, PCTG: 14 mm3/s, 250C nozzle, 100C bed
  - PA, PC: 7, 275C nozzle, 100C bed
  - TPU: 4, 200C nozzle