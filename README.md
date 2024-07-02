# Enroot_Env_Setup
Enroot Environment Creation on RHEL 8
Enroot Environment Creation on RHEL 8
This document outlines the steps required to set up an Enroot environment on RHEL 8, including the installation of necessary dependencies and the Pyxis Slurm plugin.

Step 1: Install Required Packages
On the login node, execute the following command to install the necessary packages:

bash
Copy code
yum install epel-release jq parallel zstd
Step 2: Install Enroot
For RHEL-based distributions:

Determine the system architecture:

bash
Copy code
arch=$(uname -m)
Install the EPEL repository (required on some distributions):

bash
Copy code
sudo dnf install -y epel-release
Install Enroot:

bash
Copy code
sudo dnf install -y https://github.com/NVIDIA/enroot/releases/download/v3.4.1/enroot-hardened-3.4.1-1.el8.${arch}.rpm
sudo dnf install -y https://github.com/NVIDIA/enroot/releases/download/v3.4.1/enroot-hardened+caps-3.4.1-1.el8.${arch}.rpm # optional
For more details, refer to the Enroot Installation Documentation.

Step 3: Install Pyxis
Download Pyxis:

bash
Copy code
wget https://github.com/NVIDIA/pyxis/archive/refs/tags/v0.14.0.zip
Unzip the downloaded file:

bash
Copy code
unzip v0.14.0.zip
Change to the Pyxis directory:

bash
Copy code
cd pyxis-0.14.0
Build the RPM package:

bash
Copy code
make rpm
Install the Pyxis RPM package:

bash
Copy code
rpm -i nvslurm-plugin-pyxis-*.rpm
Create the directory for the Slurm plugin:

bash
Copy code
mkdir -p /usr/local/lib/slurm/
Copy the Pyxis SPANK plugin to the Slurm plugin directory:

bash
Copy code
cp spank_pyxis.so /usr/local/lib/slurm/
For more details, refer to the Pyxis GitHub Repository.

Step 4: Update Slurm Configuration
Add Pyxis configuration to Slurm plugstack:

bash
Copy code
echo 'include /etc/slurm/plugstack.conf.d/pyxis.conf' | sudo tee -a /etc/slurm/plugstack.conf
Verify that the entry is correct in the /etc/slurm/plugstack.conf file.

Step 5: Create Plugstack Configuration Directory
Create the directory for plugstack configuration:
bash
Copy code
mkdir /etc/slurm/plugstack.conf.d/
Step 6: Add Pyxis Plugin to Plugstack Configuration
Add Pyxis SPANK plugin to the plugstack configuration:

bash
Copy code
echo 'required /usr/local/lib/slurm/spank_pyxis.so' | sudo tee -a /etc/slurm/plugstack.conf.d/pyxis.conf
Verify that the entry is correct in the /etc/slurm/plugstack.conf.d/pyxis.conf file.
