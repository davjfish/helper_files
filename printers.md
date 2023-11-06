# Label printer notes

## Wireless label printer setup
There are a few steps to getting a brother label printer wirelessly connected to the cups server. 
1. Getting into the printer's config.  After turning the printer on, a new netwrok should appear (DIRECT-99039-QL810).  Connect to this network and go to the gateway which is the printer config page (default seems to be 192.168.118.1)
2. Login to the config and connect the printer to the cups network, Network -> Wireless -> Wireless (personal) -> browse + add
3. Set a static IP if desired / find the assigned ip with `sudo arp-scan -l | grep Brother`
4. See if the printer shows up in cups (192.168.0.100:631)
5. Set the printer's config with lpadmin: `lpadmin -p Brother_QL_1110NWB_0080775cc4f1 -E -v ipp://192.168.0.36/ipp/print -m everywhere -D "Brother Printer" -L "ctd control room"`
6. Add the new entries in Andes, delete the old entry in CUPS.