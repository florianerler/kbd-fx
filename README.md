# kbd-fx

Effekte für die Tastaturbeleuchtung unter Linux (Hyprland): Die einfarbige,
3-stufige Hintergrundbeleuchtung zeigt Akku-, CPU- und Fensterereignisse an.

## Funktionen

- **Akku:** Warnblinks bei niedrigem Stand, Dauer-Herzschlag bei kritischem Stand
- **Laden:** "Atmen" nach dem Einstecken des Netzteils
- **CPU-Last:** Puls, dessen Tempo mit der Last steigt
- **Hyprland-Events:** Blinks bei Workspace-Wechsel, dringenden Fenstern,
  Monitor an/aus und Fullscreen
- **Abends:** dezente Beleuchtung beim Tippen, sonst aus

## Voraussetzungen

- Linux mit Hyprland
- Python 3.11 oder neuer
- `brightnessctl`
- Eine Tastatur mit steuerbarer Hintergrundbeleuchtung (`/sys/class/leds/*kbd_backlight`)
- Leserechte auf das Tastatur-Eventdevice (Gruppe `input`)

## Installation

```bash
git clone https://github.com/florianerler/kbd-fx.git
cd kbd-fx
install -Dm755 kbd-fx ~/.local/bin/kbd-fx
install -Dm644 kbd-fx.service ~/.config/systemd/user/kbd-fx.service
install -Dm644 config.example.toml ~/.config/kbd-fx/config.toml
systemctl --user daemon-reload
systemctl --user enable --now kbd-fx
```

## Konfiguration

Alle Einstellungen stehen in `~/.config/kbd-fx/config.toml`. Nicht gesetzte
Werte nehmen Standardwerte. Nach Änderungen:

```bash
systemctl --user restart kbd-fx
```

**Wichtig:** Die Sektion `[device]` ist auf mein Laptop zugeschnitten. Passe
`led` und `keyboard` an deine Hardware an:

```bash
ls /sys/class/leds | grep kbd_backlight    # Name der LED
ls /dev/input/by-path/ | grep kbd          # Tastatur-Device
```

## Datenschutz

kbd-fx liest das Tastatur-Eventdevice nur, um zu erkennen, *ob* getippt wird
(für den Abend-Modus). Es werden keine Tasten gespeichert oder ausgewertet.

## Testen

```bash
kbd-fx --demo
```

## Lizenz

MIT, siehe [LICENSE](LICENSE).
