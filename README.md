# Camera Workshop

A workshop for building camera systems with Elixir, Phoenix and Nerves, based on a special fork of the [ExNVR](https://github.com/evercam/ex_nvr) project.

## Prerequisites

You will need the following installed:

- **Elixir** ~> 1.18
- **Erlang/OTP** ~> 28.1
- **Node.js** ~> 23.6
- **Windows users:** [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) (follow the Linux instructions inside your WSL2 environment)

### Installing with asdf (recommended)

We recommend using [asdf](https://asdf-vm.com/) to manage tool versions.

1. **Install asdf** by following the [official install guide](https://asdf-vm.com/guide/getting-started.html).

2. **Add the required plugins:**

```bash
asdf plugin add erlang
asdf plugin add elixir
asdf plugin add nodejs
```

3. **Install the correct versions** from the project's `.tool-versions` file:

```bash
cd ex_nvr/ui
asdf install
```

This reads the `.tool-versions` file in the directory and installs the exact versions needed.

### Nerves tooling

You will also need the Nerves tooling. Follow the [Nerves installation guide](https://hexdocs.pm/nerves/installation.html) for your platform.

Install the required Mix archives:

```bash
mix archive.install hex nerves_bootstrap
```

## Getting the code

Clone the repository and check out the `workshop` branch:

```bash
git clone git@github.com:lawik/ex_nvr.git
cd ex_nvr
git checkout workshop
```

## Running the UI project

The `ui` directory contains the Phoenix web application.

```bash
cd ui
```

### Setup

Install dependencies and build frontend assets:

```bash
mix setup
```

This fetches Elixir dependencies and runs `npm install` + asset builds in the `assets` directory.

### Create the database

```bash
mix ecto.migrate
```

### Start the server

```bash
mix phx.server
```

The app will be available at [http://localhost:4000](http://localhost:4000).

Default login credentials:
- **Username:** admin@localhost
- **Password:** P@ssw0rd

## Building firmware for Raspberry Pi

The `nerves_community` directory contains a Nerves firmware project that runs ExNVR on a Raspberry Pi 4 or Raspberry Pi 5.

From the repo root:

```bash
cd nerves_community
export MIX_TARGET=rpi5  # or rpi4
export MIX_ENV=prod
mix deps.get
mix firmware
```

Insert an SD card and burn the firmware:

```bash
mix burn
```

### Upload firmware over the network

If the device is already running Nerves firmware and is reachable on the network:

```bash
mix firmware.gen.script
./upload.sh <device-ip>
```

### Default configuration

- SSH access with user `exnvr` and password `nerves`
- Network configured via DHCP
- Web UI accessible on port 4000
