## K1 Max Configuration Backup

Authoritative source of truth for printer configuration.

Backup procedure:
1. Clone repo
2. Run backup script:
   scp -O -r root@<printer-ip>:/usr/data/printer_data ./printer_data
3. Commit and push

Notes:
- Printer runs Creality Buildroot firmware
- Moonraker + Mainsail only
- No on-printer git tools available
- See: https://github.com/Mariusjuvet1/creality-k1-setup
- BUT NOTE THIS TOOK A LOT OF BACK AND FORTH EDITING WITH CHATGPT TO CORRECT
- Primary Klipper config location: /usr/data/printer_data/config/
- Moonraker data path: data_path: /usr/data/printer_data
- Read-Only or Vendor Managed: /usr/share/klipper, /usr/share/klippy-env, /etc/init.d/S55klipper_service
- So, to back these up, use this once: curl http://127.0.0.1:7125/server/info > /usr/data/printer_data/config/baseline_server_info.json
-   then do this periodically from my local Mac:
- scp -O -r root@<printer-ip>:/usr/data/printer_data ./k1max-backup/