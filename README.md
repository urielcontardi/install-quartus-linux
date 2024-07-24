# Installing Quartus Prime on Linux

Quartus Prime is Altera's development tool for FPGAs. Through it, development can be done using blocks or using an HDL, such as VHDL or Verilog. It supports both Windows and Linux environments, but installing it on Linux is not as straightforward as on Windows.

This tutorial was executed and tested on a Pop!_OS GNU/Linux operating system, but it's very similar for other Linux distributions.

# Preparing the system

First of all, it's important to keep the system up to date. Run the following commands:

```bash
sudo apt update
sudo apt upgrade
```

Quartus natively supports 64-bit systems. Depending on the Quartus version, the simulation tool may be:

- ModelSim (32-bit tool)
- Questa (64-bit version of ModelSim)

If you wish to use ModelSim, some 32-bit libraries need to be installed. The installation of such libraries can be done by running the following commands in a terminal. I recommend installing them even if you won't use ModelSim:

```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libxft2:i386 libxext6:i386 libncurses5:i386 bzip2:i386
sudo apt install g++-multilib 
```

# Installation

After downloading, navigate to the folder where Quartus was downloaded and extract the .tar file (this may take a while). Enter the extracted folder, right-click on the setup.sh file, go to properties, then permissions, and ensure that reading and writing are enabled in all fields, as well as allowing you to run the file as a program.

Then, in the same directory, right-click on any blank space in the file manager, click open in the terminal, and type:

```bash
./setup.sh
```

After a few moments, the Quartus installation window will appear.

NOTE: Do not close the terminal as it's running the installer.

Similar to any Windows installation, click next until the program starts to install. Once installed, close the installer and wait for Quartus to run for the first time. The first time, a message related to the software license will appear, but just click on "run the Quartus Prime software" and wait for it to load.

Note: Keep the option selected to generate the desktop icon

At this point, Quartus Prime is already installed on the system and can now be used as an IDE.

NOTE: The terminal used to run the installer can now be closed.

# Add a license for Questa

# Running the programs through the Terminal

Let's add the programs to the environment variables. Open the terminal and type:

```bash
nano ~/.bashrc
```

Go to the end of the opened file and include the following lines at the end:

```bash
# Run Quartus
export PATH="$PATH:/home/contardii/intelFPGA/22.1std/quartus/bin"

# Run Questa
export PATH="$PATH:/home/contardii/intelFPGA/22.1std/questa_fse/bin"
alias questa='/home/contardii/intelFPGA/22.1std/questa_fse/bin/vsim'
```

Save the file again (in nano, press Ctrl + O, then Enter, and then Ctrl + X to exit).
Update your user profile to apply the changes made:

```bash
source ~/.bashrc
```

At this point, you should be able to open Quartus and Questa from the terminal by typing: `quartus` or `questa`.

# Adding icons to the system

## Quartus Icon

Copy the generated icon on the Desktop (`Quartus (Quartus Prime 22.1std) Lite Edition.desktop`) to the following directory:

```bash
/home/YOUR_USER/.local/share/applications
```

If this `.desktop` file does not exist, you must create one in the described folder:

```bash
cd /home/YOUR_USER/.local/share/applications
nano 'Quartus Prime Lite.desktop'
```

Copy the commands and press Ctrl + O, then Enter, and then Ctrl + X to exit.

```bash
[Desktop Entry]
Type=Application
Version=0.9.4
Name=Quartus (Quartus Prime 22.1std) Lite Edition
Comment=Quartus (Quartus Prime 22.1std)
Icon=/home/contardii/intelFPGA/22.1std/quartus/adm/quartusii.png
Exec=/home/contardii/intelFPGA/22.1std/quartus/bin/quartus --64bit
Terminal=false
Path=/home/contardii/intelFPGA/22.1std
```

## Questa Icon

The icon for Questa must be created entirely manually:

Access the folder through the terminal

```bash
cd /home/YOUR_USER/.local/share/applications
```

```bash
nano 'Questa.desktop'
```

Copy the commands and press Ctrl + O, then Enter, and then Ctrl + X to exit.

```bash
[Desktop Entry]
Type=Application
Version=1.0
Name=Questa (Questa Prime 22.1std)
Comment=Questa (Questa Prime 22.1std)
Icon=/home/YOUR_USER/intelFPGA/22.1std/questa_fse/icon.png
Exec=/home/YOUR_USER/intelFPGA/22.1std/questa_fse/bin/vsim
Terminal=false
Categories=Development;
```

Note: The `.png` Icon=/home/YOUR_USER/intelFPGA/22.1std/questa_fse/icon.png does not exist, download the icon and place it in the folder.

## Creating defaults.list

Once the `.desktop` files have been created, if you execute them by clicking on them, you should be able to open the applications. Now we will include them in the system's program collections so that they appear in the applications.

Still in the same folder, if there are no `defaults.list`, you will need to create it. To do this, right-click on any blank space in the file manager and open it in the terminal. Enter this command:

Access the folder through the terminal

```bash
cd /home/YOUR_USER/.local/share/applications
nano defaults.list 
```

In the text editor, you should include the `.desktop` files that were created.

```bash
'Questa.desktop'
'Quartus Prime Lite.desktop'
```

Note: If you copied `Quartus Prime Lite.desktop`, it should have a name like `Quartus (Quartus Prime 22.1std) Lite Edition.desktop`.

After adding the `.desktop` file, it's time to update the list of Ubuntu applications. Run this command:

```bash
sudo update-mime-database /usr/share/mime
```

At this stage, your applications should appear in your system's applications.

# USB Blaster Drivers

In my case, I faced issues with the operation of the JTAG USB Blaster. The following steps were followed:

1. Open a terminal and type:

```
gedit /etc/udev/rules.d/altera-usb-blaster.rules 
```

2. Inside the file, paste the following content:

```
ATTR{idVendor}=="09fb", ATTR{idProduct}=="6001", MODE="666"
ATTR{idVendor}=="09fb", ATTR{idProduct}=="6002", MODE="666"
ATTR{idVendor}=="09fb", ATTR{idProduct}=="6003", MODE="666"
ATTR{idVendor}=="09fb", ATTR{idProduct}=="6010", MODE="666"
ATTR{idVendor}=="09fb", ATTR{idProduct}=="6810", MODE="666"
```

3. Restart your computer (This step is important).
4. To verify if the JTAG was detected, access the application within the Quartus installation directory:

```
/opt/altera/intelFPGA/20.1/quartus/bin/jtagconfig
```

# References

https://github.com/Jefferson-Lopes/quartus-installation

https://github.com/arthurmteodoro/install-quartus-linux?tab=readme-ov-file#preparando-o-sistema
