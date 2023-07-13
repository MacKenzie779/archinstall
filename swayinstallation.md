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
# Sway
```bash
cd ~
git clone https://github.com/zimsneexh/zws-setup-sway
cd zws-setup-sway
./install.sh
```

```bash
yay -S alacritty zsh
mv zws-setup-sway/zshrc ./.zshrc
```

# Rofi
```bash
git clone https://github.com/lr-tech/rofi-themes-collection.git
cd rofi-themes-collection
mkdir -p ~/.local/share/rofi/themes/
cp themes/squared-nord.rasi ~/.local/share/rofi/themes/
```
Run rofi-theme-selector and select squared-nord

#Nemo

#Swaylock
yay -S swaylock-effects-git swayidle

#Brightness
yay -S brightnessctl

#PulseAudio
yay -S pulseaudio pavucontrol
