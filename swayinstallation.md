# Sddm

```bash
pacman -S --needed sddm qt5‑graphicaleffects qt5‑quickcontrols2 qt5‑svg
```

```bash
sudo systemctl enable sddm
```

```bash
cd /usr/share/sddm/themes
```

```bash
git clone https://github.com/MacKenzie779/sddm-sugar-candy
```
```bash
cd /etc
mkdir sddm.conf.d
cd sddm.conf.d
git clone https://github.com/MacKenzie779/sddmconf
cd sddmconf
mv sddm.conf ..
```
