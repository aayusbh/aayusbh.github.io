---
title: "OpenFOAM Setup in Windows using WSL"
date: 2026-07-09T18:58:27+05:45
draft: true
description: ""
tags: [openFOAM, wsl, linux]
categories: [CFD, software]
---

Hi there !

Today we will be laying out the steps for setting up openFOAM environment in Windows sytem using the power of WSL.

To install OpenFOAM 9 (.org) and OpenFOAM v2412 (.com) on Windows using the Windows Subsystem for Linux (WSL), follow these step-by-step instructions.

# Phase 1: Prepare Windows and Install WSL
Before installing OpenFOAM, you must enable the necessary Windows features and install a Linux distribution.

1. __Enable Windows Features__: Open "Turn Windows features on or off" in your Windows search bar.
2. __Select Required Components__: Ensure that "Virtual Machine Platform" and "Windows Subsystem for Linux" are both checked.
3. **Restart Your System**: Click OK and restart
your computer when prompted.
4. **Install Ubuntu via PowerShell**:
    - Open Windows PowerShell as an Administrator.
    - Run the command: wsl --install to begin the installation of Ubuntu (typically version 22.04).
    - Restart again if prompted by the terminal.
5. **Set Up Ubuntu**: Launch the Ubuntu app. You will be asked to create a username and password. Choose a simple password you will remember, as it will not be visible when you type it into the terminal.

# Phase 2: Install OpenFOAM 9 (Foundation Version)
1. Open your Ubuntu terminal and execute the following commands to install the Foundation version of OpenFOAM.
2. **Add GPG Key**: Add the public key for the repository to enable package verification: `sudo sh -c "wget -O - https://dl.openfoam.org/gpg.key > /etc/apt/trusted.gpg.d/openfoam.asc"`.
3. **Add the Repository**: Add the OpenFOAM software repository: `sudo add-apt-repository http://dl.openfoam.org/ubuntu`.
4. **Update Package List**: Update your system's package list to include the new repository: sudo apt-get update.
5. **Install OpenFOAM 9**: Run the installation command: sudo apt-get -y install openfoam9.

>**Note**: If you encounter dependency errors regarding `csh`, enable the universe repository first: `sudo apt-add-repository universe ` followed by `sudo apt-get update`.

# Phase 3: Install OpenFOAM v2412 (ESI Version)
The ESI version has a slightly different setup process. You can perform this in the same Ubuntu terminal.
1. **Set Up the Repository**: Use the curl command to add the official ESI repository: `curl -s https://dl.openfoam.com/add-debian-repo.sh | sudo bash`.
2. **Update the Repository**: Refresh your package list: `sudo apt-get update`.
3. **Install OpenFOAM v2412**: Install the default version, which includes tutorials: `sudo apt-get install openfoam2412-default`.
>**Note**: Using the `-default` suffix ensures you get the full version with tutorials and documentation.

# Phase 4: User Configuration (Setting Up Aliases)
Because you have two versions installed, you should use "aliases" to switch between them easily. Otherwise, they may conflict in your `.bashrc` file.
1. **Install a Text Editor**: If you don't have one, install gedit: sudo apt-get install gedit.
2. **Open `.bashrc`**: Open the configuration file in your home directory: `gedit ~/.bashrc`.
3. **Add Version Aliases**: Scroll to the very bottom of the file and add these lines:
    - *For OpenFOAM 9*: `alias of9='source /opt/openfoam9/etc/bashrc'`.
    - *For OpenFOAM v2412*: `alias of2412='source /usr/lib/openfoam/openfoam2412/etc/bashrc'`.
4. **Save and Refresh**: Save the file and close the editor. In the terminal, run: `source ~/.bashrc`.

# Phase 5: Verification and Running a Test Case
To ensure everything is working, run a basic simulation for each version.
1. **Activate the Version**: Type `of9` or `of2412` in your terminal to source the specific version you want to use.
2. **Create a Run Directory**: Create a project folder to avoid modifying system files: `mkdir -p $FOAM_RUN run`.
3. **Copy a Tutorial**: Copy the pitzDaily incompressible flow example: `cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily` ..
4. **Run the Simulation**:
    - Enter the directory: `cd pitzDaily`.
    - Generate the mesh: `blockMesh`.
    - Run the solver: `simpleFoam`.
5. **Visualize Results**: Type `paraFoam` to open the results in ParaView. If `paraFoam` fails, you can install ParaView manually using `sudo apt-get install paraview`.
