---
title: "Linux installation details -  Visual Studio Live Share | Microsoft Docs"
description: "Detailed information on installing Visual Studio Live Share on Linux."
ms.custom:
ms.date: 04/30/2018
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "liveshare"
ms.topic: "reference"
author: "chuxel"
ms.author: "clantz"
manager: "AmandaSilver"
ms.workload: 
  - "liveshare"
---

<!--
Copyright © Microsoft Corporation
All rights reserved.
Creative Commons Attribution 4.0 License (International): https://creativecommons.org/licenses/by/4.0/legalcode
-->

# Linux installation details

Linux is a highly variable environment and with the sheer number of desktop environments and distributions can be complicated to get working. If you stick to supported versions of **Ubuntu Desktop** (16.04+) or **Fedora Workstation** (27+) and only use **official distributions of VS Code** , you should find the process straight forward. However, in the event that you are using a non-standard configuration or downstream distribution, you may or may not run into some hiccups. This document provides some information on requirements and some troubleshooting details that might help you get up and running even if you configuration is only community supported.

## Install Linux prerequisites

Some distributions of Linux are missing libraries Live Share needs to function. By default, Live Share attempts to detect and install Linux prerequisites for you. You'll see a toast notification when Live Share encounters a problem that can originate from missing libraries asking you for permission to install them.

![Toast notification showing message that Linux pre-requisites are missing](../media/vscode-linux-prereq-missing.png)

When you click "Install", a terminal window will appear where you'll need to enter your admin (sudo) password to continue. Assuming it completes successfully, restart Visual Studio Code you should be all set! You may also want to check out **[tips by distribution](#tips-by-distribution)** for other hints and workarounds if any exist.

If you see a message indicating the script does not support your distribution, see **[tips for unsupported distributions](#tips-for-unsupported-distros)** for community tips and issues.

If you **prefer not to have VS Code run the command for you**, you can also opt to re-run the very latest version of this script at any time manually by running the following command from a Terminal window:

    wget -O ~/vsls-reqs https://aka.ms/vsls-linux-prereq-script && bash ~/vsls-reqs

### Details on required libraries

Visual Studio Live Share uses the .NET Core runtime which requires a number of libraries be installed. The following libraries may be missing from certain **Debian/Ubuntu** distributions or derivatives:

- libunwind8
- liblttng-ust0
- libcurl3
- libssl1.0.0 (Ubuntu 16.X, 17.X, 18.X)
- libssl1.0.2 (Debian 9)
- libuuid1
- libkrb5-3
- zlib1g
- libicu55 (for Ubuntu 16.X)
- libicu57 (for Ubuntu 17.X)
- libicu60 (for Ubuntu 18.X)
- gettext
- apt-transport-https

In addition, the following are libraries **Live Share itself depends on** that may be missing in some instances (e.g. distributions not using Gnome):

- gnome-keyring
- libsecret-1-0
- desktop-file-utils

Libraries may be installed on Debian/Ubuntu based distributions by running `sudo apt install <library-name>` in a terminal. For example, this will install everything for Ubuntu 16.04/17.10/18.04 or Mint 18.3:

    sudo apt install libunwind8 liblttng-ust0 libcurl3 libuuid1 libkrb5-3 zlib1g gnome-keyring libsecret-1-0 desktop-file-utils gettext apt-transport-https libssl1.?.? libicu??

The last two parts of the command automatically determines which version of libssl and libicu to install.

**Fedora/CentOS/RHL** requires similar packages but with slightly different names:

- libunwind
- lttng-ust
- libcurl
- openssl-libs
- libuuid
- krb5-libs
- libicu
- zlib

As with Debian/Ubuntu, **Live Share itself** depends on the following:

- gnome-keyring
- libsecret
- desktop-file-utils

Libraries may be installed on Fedora/CentOS/RHL based distributions by running `sudo yum install <library-name>` in a terminal. For example, this will install everything:

    sudo yum install libunwind lttng-ust libcurl openssl-libs libuuid krb5-libs libicu zlib gnome-keyring libsecret desktop-file-utils

Other distributions will require the same libraries, but the package names may be subtly different. You can [read more about .NET Core 2.0 prerequisites for other distributions here](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x#linux-distribution-dependencies).

## Linux browser integration

Visual Studio Live Share typically **does not require additional installation steps** to enable browser integration on Linux.

To accomplish this, Live Share automatically places a desktop file in `~/.local/share/applications` and the required launcher itself in `~/.local/share/vsliveshare` when the extension first initializes. No action is required on your part if this succeeds.

In some cases, distributions either do not support this location or require tweaks to get it to work with their vanilla installs (e.g. CentOS 7). In these cases, Live Share falls back to using `/usr/local/share` instead. As a result, **you may be notified that your admin (sudo) password is required** to complete the installation process. A terminal window will appear telling you where the browser launcher will be installed. Simply enter your password when prompted and press enter once the installation completes to close the terminal window.

If you'd prefer to run the command yourself instead, you can click "Copy instead" which will copy the terminal command to the clipboard instead.

Finally, if you opt to skip this step entirely, you can still [join collaboration sessions manually](../use/vscode.md#join-manually), but you will not be able to join by opening an invite link in the browser. Note that you can always access the command again later, by hitting **Ctrl+Shift+P** and selecting the "Live Share: Launcher Setup" command.

## Tips by distribution

While the prerequisite install script above should cover Debian / Ubuntu and RHL / Fedora / CentOS, you may be wondering what is typically missing from vanilla installations of these distributions. The following list shows the key libraries that were missing in a fresh install of the distribution. The list also provides some tips that can help you get up and running if you hit a problem.

| Distribution | Vanilla install missing libraries | Additional steps |
|--------|-------------------|----|
| Ubuntu Desktop 18.04 (64-bit) | `libcurl3 liblttng-ust0 apt-transport-https` | &lt;none&gt; |
| Ubuntu Desktop 16.04 (64-bit) | &lt;none&gt; | &lt;none&gt; |
| Kubuntu 18.04 (64-bit) | `libcurl3 liblttng-ust0 gnome-keyring desktop-file-utils gettext apt-transport-https` | <ul><li>If you run into trouble with Live Share's browser integration, be sure `desktop-file-utils` is installed and then restart VS Code.</li></ul> |
| Kubuntu 16.04 (64-bit) | `libunwind8 liblttng-ust0 gnome-keyring desktop-file-utils` | <ul><li>If you run into trouble with Live Share's browser integration, be sure `desktop-file-utils` is installed and then restart VS Code.</li></ul> |
| Xubuntu 18.04 (64-bit) |`libcurl3 liblttng-ust0 apt-transport-https` | <ul><li>Ensure "Launch GNOME services on startup" is checked in the "Advanced" tab of "Session and Startup".</li><li>If you run into sign-in trouble, install `seahorse`, start "Passwords and Keys", verify you have "Login" keyring and that you can unlock it.</li></ul> |
| Xubuntu 16.04 (64-bit) | `libunwind8 liblttng-ust0` | <ul><li>Ensure "Launch GNOME services on startup" is checked in the "Advanced" tab of "Session and Startup".</li><li>If you run into sign-in trouble, install `seahorse`, start "Passwords and Keys", verify you have "Login" keyring and that you can unlock it.</li></ul> |
| Mint 18.3 Cinnamon (64-bit) | `libcurl3` | &lt;none&gt; |
| Debian 9 GNOME Desktop (64-bit) | `libunwind8 liblttng-ust0 apt-transport-https gettext` | <ul><li>You will need to install `sudo` if you have not already to run the automated prerequisite installer.</li></ul>  |
| Fedora Workstation 27 (64-bit) | &lt;none&gt; | &lt;none&gt; |
| Fedora Workstation 28 (64-bit) | &lt;none&gt; | &lt;none&gt; |
| CentOS 7 GNOME Desktop (64-bit) | &lt;none&gt; | &lt;none&gt; |
| openSuSE 12 (64-bit) | &lt;none&gt; | &lt;none&gt; |

See **[tips for unsupported distributions](#tips-for-unsupported-distros)** for information about whether certain non-Debian / Ubuntu or RHL based distributions are working or not.

Note that the Linux ecosystem moves quickly and package names will be different in certain distributions, so your results may vary. Additional details can be found above on the libraries Live Share needs.

## Tips for unsupported distros

Distributions outside of the Debian / Ubuntu or RHL trees are not officially supported by Visual Studio Code or .NET Core. Therefore, by extension, they are not officially supported by Visual Studio Live Share either. However, the Live Share community has helped understand whether Live Share is working on certain unsupported, if so, how to get it up and running.

> **PRs welcome:** If you're interested in updating this information with your favorite distribution, submit a PR for [this file](https://github.com/MicrosoftDocs/live-share/tree/master/docs/reference/linux.md) in our docs GitHub repo. Even better, if you'd like to get the dependency installer supporting your favorite distribution, you can submit a PR [for this file](https://github.com/MicrosoftDocs/live-share/blob/master/scripts/linux-prereqs.sh).

| Distribution | Working? | Vanilla install missing libraries | Additional Steps |
|--------------|----------|-------------------|------------------|
| ArchLinux | Yes | Varies. Use the prerequisite install script. Most common are `gnome-keyring` and `libsecret`.  | <ul><li>Use the [visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin) AUR package for VS Code.</li><li>`sudo` will need to be installed  to use the automated prerequisite install script.</li><li>`gnome-keyring` may require additional [setup steps](https://wiki.archlinux.org/index.php/GNOME/Keyring) in some desktop environments.</lu></ul> |
| Manjaro 17.1 | Yes | Use the prerequisite install script. | <ul><li>Use the [visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin) AUR package for VS Code.</li></ul> |
| Solus 3 | **No** | None known. |**Issues:** <ul><li>VS Code package is missing product.json values ([see below](#vs-code-oss-issues)).</li><li>Even with workaround, fails due to a .NET Core bug (see [here](https://github.com/dotnet/corefx/issues/24952) and [here](https://github.com/dotnet/corefx/issues/19718)).</li></ul> |
| Gentoo | **No** | Highly variable. | **Issue:** <ul><li>Fails due to a .NET Core bug (see [here](https://github.com/dotnet/corefx/issues/24952) and [here](https://github.com/dotnet/corefx/issues/19718)).</li></ul> |

### VS Code OSS Issues

> **ArchLinux/Manjaro Users:** Use the [visual-studio-bin](https://aur.archlinux.org/packages/visual-studio-code-bin) AUR package to avoid this problem.

Non-official distributions of Visual Studio Code can be missing a critical value in  `product.json` file that prevents Visual Studio Live Share from activating. In this case, if you go to Help > "Toggle Developer Tools", you will see stack traces indicating the Live Share extension did not activate due to it using a "proposed API."

To verify this is your issue, check the contents of `product.json`. The file could be located in one of the following locations:

- `/usr/share/code/resources/app/product.json`
- `/usr/share/vscode/resources/app/product.json`

If the `extensionAllowedProposedApi` property is missing or you do not see "ms-vsliveshare.vsliveshare" referenced, you are using an unofficial version with this problem. **Contact the VS Code distribution owner to get the issue patched.**

As a **workaround**, you can add the following into the product.json:

        "extensionAllowedProposedApi": [
          "ms-vsliveshare.vsliveshare",
          "ms-vscode.node-debug",
          "ms-vscode.node-debug2"
     ]

See [above](#tips-for-unsupported-distros) for additional details on whether the distribution you are using is known to work.

## See also

- [How-to: Collaborate using Visual Studio Code](../use/vscode.md)
- [Connectivity requirements for Live Share](connectivity.md)
- [Security features of Live Share](security.md)

Having problems? See [troubleshooting](../troubleshooting.md) or [provide feedback](../support.md).