# Mixfeed widget for Omarchy

Bar widget for [Mixfeed](https://github.com/vpacher/mixfeed). Mixfeed receives an [NDI®](https://ndi.video/) source on Linux and gives it to your applications as a microphone ("Mixfeed Mic") and a webcam ("Mixfeed").

The widget shows the state of the microphone and the camera, and lights up while an application uses them (on air). Its panel selects the NDI source and the reference level, and controls Mute, Keep connected and the headphone Monitor.

NDI® is a registered trademark of Vizrt NDI AB. Mixfeed is not a product of NDI and is not affiliated with NDI.

| On air | Ready |
|---|---|
| ![Mixfeed panel while an application uses the microphone and the camera](preview.png) | ![Mixfeed panel with no application on the microphone or the camera](ready.png) |

## Requirements

The widget is the control for the Mixfeed service `mixfeedd`. It does not receive NDI itself. Without `mixfeedd` it shows "mixfeedd not running" and a link to the install.

| Part | Where |
|---|---|
| Omarchy 4 (shell plugins) | [omarchy.org](https://omarchy.org/) |
| Mixfeed (`mixfeedd`, `mfctl`) | [github.com/vpacher/mixfeed](https://github.com/vpacher/mixfeed) |
| NDI runtime 6 | AUR `ndi-sdk` ([ndi.video](https://ndi.video/)) |

## Install

This widget release was tested with Mixfeed commit `16b41934c48dc3dc4ef10be57e44dce444045816`. The procedure installs exactly that commit, not a branch or a tag.

1. Do the preparation steps 1–3 of the Mixfeed README at that commit: [Mixfeed, section 3 "Install"](https://github.com/vpacher/mixfeed/blob/16b41934c48dc3dc4ef10be57e44dce444045816/README.md#3-install) (requirements, camera module).

2. Get Mixfeed at the tested commit, then build and install it:

   ```sh
   git clone https://github.com/vpacher/mixfeed.git ~/.local/share/mixfeed/source &&
     git -C ~/.local/share/mixfeed/source checkout --detach 16b41934c48dc3dc4ef10be57e44dce444045816 &&
     ~/.local/share/mixfeed/source/linux/install.sh
   ```

   The install script starts only if the checkout of the commit is successful. It installs `mixfeedd` and `mfctl` in `~/.local/bin` and starts the systemd user service `mixfeedd`. It does not install or change this widget.

3. If ufw is active, do step 5 of the Mixfeed README (firewall rules for NDI video).

4. Add the widget, from the Omarchy plugin marketplace or with:

   ```sh
   omarchy plugin add https://github.com/vpacher/omarchy-mixfeed.git --enable
   ```

5. Restart the browser if it was open during the install. Browsers read the camera list when they start.

## Update

1. Update the widget:

   ```sh
   omarchy plugin update vpacher.mixfeed
   ```

2. Read the Mixfeed commit in the updated README, `~/.config/omarchy/plugins/vpacher.mixfeed/README.md`, section "Install". Put that commit in place of `<commit>`:

   ```sh
   git -C ~/.local/share/mixfeed/source fetch origin &&
     git -C ~/.local/share/mixfeed/source checkout --detach <commit> &&
     ~/.local/share/mixfeed/source/linux/install.sh
   ```

Settings stay.

## Remove

```sh
omarchy plugin remove vpacher.mixfeed
~/.local/share/mixfeed/source/linux/install.sh remove
rm -rf ~/.local/share/mixfeed/source
```

The second command stops and removes the service and the programs. Your settings stay in `~/.config/mixfeed`. To remove the widget only, use the first command.

## Use

Click the Mixfeed icon in the bar to open the panel.

Keys in the panel: `j`/`k` or the arrow keys move, `Enter` or `Space` selects, `r` refreshes, `Esc` closes.

The widget calls `~/.local/bin/mfctl` (installed with Mixfeed) to read the state and to change settings. It opens a browser for the link to ndi.video.

## Security

Report vulnerabilities privately; see [SECURITY.md of Mixfeed](https://github.com/vpacher/mixfeed/blob/16b41934c48dc3dc4ef10be57e44dce444045816/SECURITY.md).

## Licence

MIT or Apache-2.0, at your choice. See `LICENSE`, `LICENSE-MIT` and `LICENSE-APACHE`.
