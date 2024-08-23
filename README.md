# pi-zero

[Ansible][ansible] based setup for my Raspberry Pi Zero.

## Getting started

### Install the OS

Follow the [instructions][pi-doc] on the Raspberry Pi website to install the
operating system onto an SD card using the Imager software.

After selecting your device, OS and storage, click on the _EDIT SETTINGS_ button
to apply OS customisations. In the _GENERAL_ tab, set the hostname for your
device, the username and password of the default user, and the wifi settings.

Next, in the _SERVICES_ tab, enable SSH and select the option to
_Use password authentication_.

Finally, click _SAVE_ to save the customisation and then _YES_ to apply it,
before writing the OS image to the SD card.

### Setup

1. After the Imager has finished writing the OS image to the SD card, unmount
   it from your machine and put it into your Raspberry Pi Zero.
1. Start the Raspberry Pi Zero (usually by connecting it to a power supply) and
   wait several minutes until the activity LED has stopped flashing.
1. Copy the `hosts.example.yml` file to `hosts.yml`.
1. Open `host.yml` in a text editor and replace `<HOSTNAME>`, `<USERNAME>` and
   `<PASSWORD>` with the hostname, username and password, respectively, that you
   set when installing the OS above.
1. Replace `<PATH_TO_PUBLIC_SSH_KEY>` with the full path to your public SSH key
   on your host machine.
1. Save `hosts.yml` and exit your editor.
1. Run the setup playbook:
   ```shell
   ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i hosts.yml setup.yml
   ```

**Note:** The `setup` playbook assumes that your platform has `sshpass`
installed. This allows Ansible to create an SSH connection to your Raspberry Pi
Zero with a password.

#### RTL8188eu Wifi Dongle

I have a wifi dongle based on the RTL8188eu chipset on one of my Raspberry Pis.
Wifi should still work with the setup described above, but the [instructions
here][wifi-dongle-doc] should be followed to ensure that the appropriate kernel
modules are installed and loaded.

## Playbooks

This project includes the following playbooks, which you can use to install
additional services onto your Raspberry Pi Zero.

### Prometheus

The `prometheus.yml` playbook can be used to install [Prometheus][prometheus]
on your Raspberry Pi Zero. It can be run with,

    ansible-playbook -i hosts.yml -K prometheus.yml

After the playbook ends, the Prometheus browser will be available to view on
`http://<HOSTNAME>:9090/graph`.

[ansible]: https://www.ansible.com/
[pi-doc]: https://www.raspberrypi.com/documentation/computers/getting-started.html#install-using-imager
[prometheus]: https://prometheus.io/
[wifi-dongle-doc]: https://zsitko.com/wifi-rtl8188eu-raspberry-pi-zero/