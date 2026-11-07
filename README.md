<p align="center"><img src="Icon.png" width="112" alt="AyaEX logo"></p>

# AyaEX

**AyaEX prebuilt clients and server.**

AyaEX (Aya Extended) is a revival made of two improved forks working as one:
the [Mercury Core](https://github.com/tp-link-extender/MercuryCore) website and
Aya, the Kiseki project's 2016 client. Players sign in on the website and play
in their browser, or on a Nintendo Switch or Wii U. Gameservers and avatar
renders run on a native Linux `Aya.Server`.

The client's source isn't public. This repository has its builds, in the
[releases](https://github.com/AyaEX-PUBLIC/AyaEX/releases).

## Public repositories

| Repository | What |
| --- | --- |
| [AyaEX-PUBLIC/AyaEX-MercuryCore](https://github.com/AyaEX-PUBLIC/AyaEX-MercuryCore) | The website, the Economy service, and the setup and deployment scripts |
| [AyaEX-PUBLIC/AyaEX-MercuryRCC](https://github.com/AyaEX-PUBLIC/AyaEX-MercuryRCC) | The Orbiter and RCC proxy, which run gameservers and avatar renders |
| [AyaEX-PUBLIC/AyaEX](https://github.com/AyaEX-PUBLIC/AyaEX) | This repository: prebuilt clients and server |

These are downstream mirrors. AyaEX is developed in a private, self-hosted Git
repository, and the public repositories are updated from it, so they can be
behind it.

## Downloads

Each [release](https://github.com/AyaEX-PUBLIC/AyaEX/releases) has these files:

| File | What |
| --- | --- |
| `AyaEX-Web-<version>.tar.gz` | The WebAssembly app: the Player, Studio and browser-hosted games. An AyaEX site serves it at `/runtime`. |
| `AyaEX-Server-linux-x64-<version>.tar.gz` | `Aya.Server` for x86-64 Linux and the files it runs with. It runs gameservers and avatar renders for an AyaEX site. |
| `AyaEX-WiiU-<version>.wuhb` | The Wii U app |
| `AyaEX-Switch-<version>.nro` | The Nintendo Switch homebrew app |
| `SHA256SUMS` | Checksums of the files above. Check them with `sha256sum -c SHA256SUMS`. |

## Running your own site

Clone the website and the gameserver services into one folder, then unpack
both archives in that same folder:

```bash
git clone https://github.com/AyaEX-PUBLIC/AyaEX-MercuryCore MercuryCore
git clone https://github.com/AyaEX-PUBLIC/AyaEX-MercuryRCC MercuryRCC
tar -xzf AyaEX-Web-<version>.tar.gz
tar -xzf AyaEX-Server-linux-x64-<version>.tar.gz
```

The archives unpack into `aya-dev/`, laid out like the client's build tree,
which is where MercuryCore and MercuryRCC look for it. Then follow
MercuryCore's README:

- `aya/setup-data.sh` makes the web app the site's stable client.
- `RCC_EXE` in MercuryRCC's `Proxy/.env` is the absolute path of
  `aya-dev/build/x64-linux-rel/bin/Aya.Server`, and `--contentPath` in
  `RCC_ARGS` is the absolute path of `aya-dev/client/resources/content`.

To update, unpack a newer release into a fresh `aya-dev/`. Run MercuryCore's
`aya/promote-stable.sh` to make the new web app the stable client. Then restart
the RCC services with MercuryRCC's `stop.sh` and `run.sh` while nobody is
playing.

### Server requirements

`Aya.Server` needs x86-64 Linux with glibc 2.38 or newer and GCC 13's
libstdc++, as on Debian 13 or Ubuntu 24.04 and later. On Debian or Ubuntu,
install its libraries with:

```bash
apt install libatomic1 libglu1-mesa libglx0 libice6 libopengl0 libpulse0 \
	libsm6 libstdc++6 libx11-6 libxext6 libgl1-mesa-dri
```

Avatar renders need an X display (`DISPLAY`) for their OpenGL context.
Gameservers don't.

## Wii U

Needs [Aroma](https://aroma.foryour.cafe). Copy `AyaEX-WiiU-<version>.wuhb` to
`SD:/wiiu/apps/` and start AyaEX from the Wii U Menu.

## Nintendo Switch

Needs a console running homebrew (Atmosphère and the Homebrew Menu). Copy
`AyaEX-Switch-<version>.nro` to `sdmc:/switch/aya/Aya.Switch.nro`. Launch it
through a title override, by holding R while starting a game, so it gets the
full application memory.

## Using the console apps

Both apps open the AyaEX launcher. From there you can play local places from
`SD:/aya/maps/`, or sign into an AyaEX site to browse and join its games.

They use `https://mercury.nodium.lol/` by default. To use another AyaEX site,
set `mercury_url=https://your-site.example/` in `SD:/aya/config.txt`. To sign
in, the app shows a short code. Approve it on the site's `/device` page while
signed in there.

## Disclaimer

AyaEX isn't endorsed by Roblox Corporation or the Mercury 3 developers.
