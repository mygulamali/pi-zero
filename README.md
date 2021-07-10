# pi-zero

[Ansible][ansible] based setup for my Raspberry Pi Zero.

## Getting started

### Bootstrap

1. Download the latest Raspberry Pi OS image from [the Raspberry Pi
   website][pi-images]. The Lite version is good enough.
1. Validate the checksum of the image,
   ```shell
   diff <(sha256sum PATH-TO-IMAGE | cut -d ' ' -f 1) <(echo SHA-256-OF-IMAGE-ON-WEBSITE)
   ```
   If the diff does not show any differences then you're good to proceed.
1. Follow the [instructions][pi-doc] on the Raspberry Pi website to write the
   image to your SD card.
1. Connect the SD card to your machine and ensure the `boot` partition is mounted.
1. Copy the example variables file for the `bootstrap` playbook,
   ```shell
   cp roles/bootstrap/vars/main.example.yml roles/bootstrap/vars/main.yml
   ```
   and fill in the variables therein.
1. Run the bootstrap playbook:
   ```shell
   ansible-playbook -i default-hosts.yml bootstrap.yml
   ```

**Note:** This `bootstrap` playbook expects the `wpa_passphrase` command to be
present on the host machine. This comes as standard on Linux OS.

### Setup

1. After the bootstrap playbook (above) has finished running, unmount the SD
   card from your machine and put it into your Raspberry Pi Zero.
1. Start the Raspberry Pi Zero (usually by connecting it to a power supply) and
   wait several minutes until the activity LED has stopped flashing.
1. Copy the example variables file for the `setup` playbook,
   ```shell
   cp roles/setup/vars/main.example.yml roles/setup/vars/main.yml
   ```
   and fill in the variables therein.
1. Run the setup playbook:
   ```shell
   ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i default-hosts.yml setup.yml
   ```

**Note:** The `setup` playbook assumes that your platform has `sshpass`
installed, to enable Ansible to create an SSH connection to your Raspberry Pi
Zero with a password.

#### RTL8188eu Wifi Dongle

I have a wifi dongle based on the RTL8188eu chipset on one of my Raspberry Pis.
Wifi should still work with the setup described above, but the [instructions
here](https://zsiti.eu/wifi-rtl8188eu-raspberry-pi-zero/) should be followed to
ensure that the appropriate kernel modules are installed and loaded.

## Playbooks

The following playbooks assume that you have an inventory file named
`hosts.yml`, similar to `hosts.example.yml`. It should describe the
configuration for your Raspberry Pi Zero.

The `<USERNAME>` parameter in the command listed below is the username of your
user on your Raspberry Pi Zero.

### Prometheus

The `prometheus.yml` playbook can be used to install [Prometheus][prometheus]
on your Raspberry Pi Zero. It can be run with,

    ansible-playbook -i hosts.yml -u <USERNAME> -K prometheus.yml

After the playbook ends, the Prometheus browser will be available to view on
`http://<HOSTNAME>:9090/graph`, where `<HOSTNAME>` is the hostname or IP address
of your Raspberry Pi Zero.

[ansible]: https://www.ansible.com/
[pi-images]: https://www.raspberrypi.org/downloads/raspberry-pi-os/
[pi-doc]: https://www.raspberrypi.org/documentation/installation/installing-images/README.md
[prometheus]: https://prometheus.io/
