---
title: "Getting Started with Development in Windows"
date: 2020-8-18
categories:
- Guide
- Setup & Installation
tags:
- Windows
cover: https://res.cloudinary.com/practicaldev/image/fetch/s--oSSacGod--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://betanews.com/wp-content/uploads/2019/06/windows-terminal.jpg
thumbnail: https://res.cloudinary.com/practicaldev/image/fetch/s--oSSacGod--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://betanews.com/wp-content/uploads/2019/06/windows-terminal.jpg
---
Microsoft has been doing great stuff for years now, and WSL2 *(Windows Subsystem for Linux - 2)* is everything that I personally neeeded to feel completely at home as a developer in Windows. WSL2 allows you to run a full fledged Linux kernel with greatly improved IO and support, as compared to WSL1. WSL2 connects flawlessly with a Windows installation of Visual Studio Code and supports Docker natively. In short, your Windows setup would serve you well in your development endeavors.

In this article, I'll discuss how to get WSL2 up and running on your system along with an integration with - **Windows Terminal** and **Visual Studio Code**.

<!-- more -->

## Pre-requisites
The sole major pre-requisite for installing WSL2 is a Windows installation version **2004**, with a build higher than or equal to **19041**. You can check your Windows installation version by pressing - **Windows Logo + R**. If your Windows version does not match the requirements there is no need to fret, you can freely upgrade your Windows to the required version by following the listed steps:

1. Install updates available for your Windows by clicking [here](ms-settings:windowsupdate). Post installation, check your Windows version again, if it meets the requirement, you can skip step 2 and move on to WSL2 Installation, if it doesn't step 2 will get you up to speed quickly.
2. If updates for your Windows does not include an upgrade to version 2004, you can do so manually, by installing [Windows Update Assisstant](https://www.microsoft.com/en-us/software-download/windows10). Run the Update Assisstant and follow instructions to update Windows to the May, 2020 release -- version 2004.

## WSL2 Installation
By this point, I am assuming that you have Windows 10 version 2004 up and running, if you haven't done so, this is a crucial pre-requisite and you should follow the steps mentioned in the previous section before coming here. If you've done so, follow the steps mentioned below to complete your WSL2 installation:

1. Open Windows PowerShell as an administrator and run the following commands:
   ```s
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
2. Restart your system. 
3. Download and run the [WSL2 Linux Kernel update package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi). Ensure that the package successfully installs before moving forward.
4. Download and install a linux distribution of your choice from the Microsoft Store.
5. Setup your new installation by creating a username and a password.
6. Check which version of WSL your distribution is using by running the following command in Windows PowerShell:
```s
wsl --list --verbose
```
7. To set your linux distribution ot choose WSL2 instead of WSL1, run the following command in Windows PowerShell -- (set distribution name to your distribution name, and versionNumber to 2):
```s
wsl --set-version <distribution name> <versionNumber>
```
8. Run the following command to set the default WSL version to 2 for new linux installations : 
```s
wsl --set-default-version 2
```
9. If you face any errors in your installation, you can check [Windows instllation guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
 
## Visual Studio Code
Visual Studio Code is one of the best, if not the outright best, code editors out there. Lightning fast, with a plethora of plugins and connections and lot of themes and customizations -- there aren't a lot of reasons to not use it. In this section, we'll install Visual Studio Code and connect it with your WSL2 installation. Follow the steps mentioned below:
1. Download and install [Visual Studio Code](https://code.visualstudio.com/) on your Windows PC. PS : **Do NOT** install it on your WSL2 based Linux distribution.
2. Lauch Visual Studio Code when it finishes installing and download the [Remote-WSL extension](vscode:extension/ms-vscode-remote.remote-wsl).
3. And that's it! Your VSC installation should be up and running on Windows with a WSL connection. If you face issues in installation, you can refer to [Microsoft's installation documentation](https://code.visualstudio.com/docs/remote/wsl-tutorial).

## Windows Terminal
Windows Terminal is a powerful shell for your linux distributions which enables you to access them from one single application in a highly customizable setup. To install and setup Windows Terminal, follow the steps mentioned below:
1. Download and install Windows Terminal from the Microsoft Store.
2. Launch Windows Terminal, click on the downward-arrow in the tab bar and click on settings.
3. The previous step would open a *settings.json* file that stores configurations for your Windows Terminal. Select the *"guid"* of your most used linux distribution installation and paste it against the *"defaultProfile"* key. This would load your chosen distribution everytime you open a new Terminal tab.
4. Terminal by default open in you *Windows* home folder, to change this to your *Linux* home, add a *cd* to your linux bash profile. Do the following from your linux shell:
```bash
vim ~/.bashrc
# add a new line and enter 'cd' in it
# save the file by pressing :wq
# restart your linux terminal
```

## Conclusion

And you're done! You've successfully setup a Windows Development Environment complete with VSC, WSL2 and Terminal. If you face any issues in any of the installations, feel free to reach out to me on [vivek.kaushal@outlook.com](mailto:vivek.kaushal@outlook.com).