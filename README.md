# pi-zero

[Ansible][ansible] based setup for my Raspberry Pi Zero.

## Setup

First, download the latest Raspberry Pi OS image from [the Raspberry Pi
website][pi-images]. The Lite version is good enough. Then, check the checksum
of the image,

```shell
diff <(sha256sum PATH-TO-IMAGE | cut -d ' ' -f 1) <(echo SHA-256-OF-IMAGE-ON-WEBSITE) 
```

If the diff does not show any differences then you're good to proceed.

Next, follow the [instructions][pi-doc] on the Raspberry Pi website to write the
image to your SD card.

[ansible]: https://www.ansible.com/ 
[pi-images]: https://www.raspberrypi.org/downloads/raspberry-pi-os/
[pi-doc]: https://www.raspberrypi.org/documentation/installation/installing-images/README.md