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
1. Connect the SD card to your machine and ensure both of the partitions on it
   (`boot` and the Linux OS partition) are mounted.
1. Copy the example variables file for the `bootstrap` playbook,
   ```shell
   cp roles/bootstrap/vars/main.example.yml roles/bootstrap/vars/main.yml
   ```
   and fill in the variables therein.
1. Run the bootstrap playbook:
   ```shell
   ansible-playbook -i hosts bootstap.yml
   ```

**Note:** This `bootstrap` playbook needs to be run from a Linux OS, so that it
can read/write Linux (ext4) partitions, and use the `wpa_passphrase` command.

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
   ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i hosts setup.yml
   ```

**Note:** The `setup` playbook assumes that your platform has `sshpass`
installed, to enable Ansible to create an SSH connection to your Raspberry Pi
Zero with a password.

[ansible]: https://www.ansible.com/
[pi-images]: https://www.raspberrypi.org/downloads/raspberry-pi-os/
[pi-doc]: https://www.raspberrypi.org/documentation/installation/installing-images/README.md
