Raspberry Pi Automated Setup with Ansible
This repository contains an Ansible playbook (setup.yml) that automates the setup of a Raspberry Pi for capturing images with a PiCamera, including installing necessary packages, configuring services, and setting up user environments.

Table of Contents
Prerequisites
Initial Raspberry Pi Setup
Installing Ansible
Running the Ansible Playbook
Post-Installation Steps
Troubleshooting
License
Prerequisites
A Raspberry Pi 4 with a fresh installation of Raspberry Pi OS (32-bit or 64-bit).
A network connection (Ethernet or Wi-Fi).
SSH access enabled.
A user account with sudo privileges (default user pi).
Initial Raspberry Pi Setup
1. Flash Raspberry Pi OS to SD Card
Download the latest Raspberry Pi OS from the official website.
Use Raspberry Pi Imager or Balena Etcher to flash the OS to your SD card.
2. Enable SSH Access
After flashing, enable SSH by placing an empty file named ssh (without any extension) in the boot partition of the SD card.
3. Configure Wi-Fi (Optional)
To connect via Wi-Fi, create a file named wpa_supplicant.conf in the boot partition with the following content:

conf
Kopier kode
country=YOUR_COUNTRY_CODE
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSWORD"
}
Replace YOUR_COUNTRY_CODE, YOUR_SSID, and YOUR_PASSWORD with your Wi-Fi details.

4. Boot the Raspberry Pi
Insert the SD card into the Raspberry Pi and power it on.
Wait for the Raspberry Pi to boot up. It should connect to your network.
5. Find Raspberry Pi IP Address
Use a network scanner or check your router's admin page to find the Raspberry Pi's IP address.
Alternatively, if your network supports it, you can use raspberrypi.local.
Installing Ansible
1. Update System Packages
Login to your Raspberry Pi via SSH:

bash
Kopier kode
ssh pi@<Raspberry_Pi_IP_Address>
Update the package list and upgrade installed packages:

bash
Kopier kode
sudo apt update
sudo apt full-upgrade -y
2. Install Ansible
Ansible is not available in the default Raspberry Pi OS repositories, but you can install it via pip3.

bash
Kopier kode
sudo apt install -y python3-pip
pip3 install --user ansible
3. Update Environment Variables
Add the local bin directory to your PATH:

bash
Kopier kode
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
Verify the installation:

bash
Kopier kode
ansible --version
Running the Ansible Playbook
1. Clone the Repository
bash
Kopier kode
git clone https://github.com/<your_username>/<your_repository>.git
Replace <your_username> and <your_repository> with your GitHub username and repository name.

2. Navigate to the Repository Directory
bash
Kopier kode
cd <your_repository>
3. Run the Ansible Playbook
bash
Kopier kode
ansible-playbook setup.yml
4. Follow the Prompts
The playbook may prompt you for certain information, such as whether to reboot after the installation.
If configured, it may also prompt for a hostname.
5. Wait for the Playbook to Finish
The playbook will install packages, configure services, and set up your environment.
This may take some time, depending on your network speed and Raspberry Pi performance.
Post-Installation Steps
1. Reboot the Raspberry Pi (If Not Already Done)
If you chose not to reboot during the playbook execution, it's recommended to reboot now:

bash
Kopier kode
sudo reboot
2. Verify Hostname
After rebooting, verify that the hostname has been updated:

bash
Kopier kode
hostname
3. Test Services
Ensure that the following services are running:

Apache2 Web Server

bash
Kopier kode
systemctl status apache2
vsftpd FTP Server

bash
Kopier kode
systemctl status vsftpd
4. Check Cloned Repositories
Verify that the Git repositories have been cloned to your home directory:

bash
Kopier kode
ls ~/pi-timelapse-3
ls ~/raspberrypi-picamera-timelapse
5. Test the Camera
Ensure that the PiCamera is working:

bash
Kopier kode
libcamera-hello
Troubleshooting
Ansible Command Not Found

If you receive a command not found error when running ansible-playbook, ensure that ~/.local/bin is in your PATH:

bash
Kopier kode
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
Permission Issues

If you encounter permission issues, ensure you are running the playbook with the correct user and that the user has sudo privileges.

Service Not Starting

Check the status of services and review logs for any errors:

bash
Kopier kode
journalctl -u apache2
journalctl -u vsftpd
Locale Issues

If you experience locale-related warnings or errors, ensure that the locale was generated correctly:

bash
Kopier kode
locale
License
This project is licensed under the MIT License - see the LICENSE file for details.