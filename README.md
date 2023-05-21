# klipper-config

After installing KIAUH, download the config folder:
```
cd ~/printer_data/
mv config config_orig
git clone git@github.com:thiagosalles/klipper-config.git config
cp -r config_orig/. config/
rm -rf config_orig/
cd config/ && git checkout printer.cfg
```

## Reference

- https://github.com/Klipper3d/klipper/blob/master/config/sample-macros.cfg
