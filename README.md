
# Paxmod

Paxmod is a Firefox theme wth multiple tab rows and site-dependent tab colors. If you enjoy a straightforward, compact UI with customizable colors, fonts and more, give it a try.

![Demo](https://i.imgur.com/bjYnDKY.gif)

## Installation

- Use an up-to-date version (57+) of either [Firefox Developer](https://www.mozilla.org/en-US/firefox/developer/) or Nightly.

- Enable legacy extensions and disable extension signature checks. (Go to `about:config` and set `extensions.legacy.enabled` to `true`, and `xpinstall.signatures.required` to `false`. [Why?](#why-cant-i-install-paxmod-as-a-verified-extension-through-mozilla))

- Install the [Stylesheet API](https://github.com/numirias/stylesheet-api-experiment). (Download the `.xpi` file from  [here](https://github.com/numirias/stylesheet-api-experiment/releases/latest) and load it in Firefox.)

- Install Paxmod. (Download the `.xpi` file from [here](https://github.com/numirias/paxmod/releases/latest) and load it in Firefox.)

## Customization

You can change the font, colors and other settings at `about:addons` > *Extensions* > *Paxmod* > *Preferences*.

If you want Paxmod to look exactly the same as in the demo, you need the [Terminus font](http://terminus-font.sourceforge.net/). I chose a bitmap font since font rendering in small sizes on dark backgrounds tends to look blurry. But you can set any font you like.

Also, you may want to remove clutter from the toolbar to get a cleaner UI. Open the burger menu (☰), click *Customize* and drag away unneeded items (in particular, remove the invisible placeholders to get rid of the empty space). I prefer to keep only the navigation arrows and the download button.

Additionally, disable UI animations for less jiggly tab movement. (Go to `about:config` and set `toolkit.cosmeticAnimations.enabled` to `false`.)

### Custom CSS

You can load a custom CSS file by specifying a path in the settings. If you want to load a local file, make sure you prefix the path with `file://`, e.g. `file:///tmp/foo.css`.

Each color in the settings has a *Var* field where you can specify the name of a CSS variable to get the color from. Then, if your custom CSS file sets these variables, they are automatically applied to the UI. This is useful if you want dynamic UI colors generated by a tool like [pywal](https://github.com/dylanaraps/pywal). (E.g., you can reference the file `/home/<username>/.cache/wal/colors.css` to directly load the colors generated by wal.)



## FAQ

### Why can't I install Paxmod as a verified extension through Mozilla?

Mozilla has dropped support for legacy extensions, so all new extensions need to comply with the [WebExtension](https://developer.mozilla.org/en-US/Add-ons/WebExtensions) standard. However, Paxmod needs to be able to modify the browser UI, a feature for which there is no existing WebExtension API, and probably never will be. Therefore, Paxmod relies on an "experimental API" that you have to install as a separate add-on. Unfortunately, due to this extra dependency, Mozilla won't sign the extension anymore and therefore it can't be distributed over the official channels.

### How are tab colors calculated?

Each tab color is generated dynamically from the favicon. A dominant color is extracted from the image and eventually adjusted to an appropriate lightness in the [Lab color space](https://en.wikipedia.org/wiki/Lab_color_space). This works reasonably well and ensures that all tab labels are legible.

## Troubleshooting

### I can't install the add-on.

If Firefox complains that the add-on isn't verified, you probably didn't turn extension signature checks off. Also note that turning the checks off has no effect in the standard releases, which is why you need to use Firefox Developer or Nightly. If Firefox complains that the add-on isn't *compatible*, you probably forgot to turn on legacy extensions or didn't install the [Stylesheet API](https://github.com/numirias/stylesheet-api-experiment).

### The design looks off.

Make sure you have all other themes disabled (look under `about:addons` > *Themes*), because some themes might apply aggressive CSS rules that interfere with Paxmod. Also, *Paxmod is mainly tested on Linux*, so there are most likely some design quirks on MacOS or Windows that I'm not aware of. If in doubt, please [file an issue](https://github.com/numirias/paxmod/issues/new). Any help with making Paxmod compatible with other OSes is more than welcome.

### The font looks bad.

Paxmod defaults to the [Terminus font](http://terminus-font.sourceforge.net/) which you can change in the settings.

On Arch Linux, you can install Terminus via:

    # pacman -S terminus-font

### Firefox still doesn't find Terminus.

Make sure you type in the exact name of the font. Some fonts have non-obvious names. (On my machine, Terminus is registered as `xos4 Terminus`, not `Terminus`.) You can list your installed fonts with `fc-list`, e.g.:

    $ fc-list | grep -i terminus
    ...
    /usr/share/fonts/misc/ter-x16b.pcf.gz: xos4 Terminus:style=Bold
    /usr/share/fonts/misc/ter-x14n.pcf.gz: xos4 Terminus:style=Regular
    ...

## Contributing

Use [web-ext](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Getting_started_with_web-ext) to develop locally. After cloning the repository, you can run Paxmod similar to this:

    web-ext run --firefox /usr/bin/firefox-nightly --firefox-profile dev-paxmod --source-dir src

(Make sure the chosen profile has the stylesheet API installed, you're in the project's root directory, and you're pointing to an existing Firefox binary.)

And to build the addon, run:

    web-ext build --overwrite-dest --source-dir src 

This will produce a `.zip` file in `web-ext-artifacts/`. Rename the file to `.xpi` to have it recognized as a Firefox extension.
