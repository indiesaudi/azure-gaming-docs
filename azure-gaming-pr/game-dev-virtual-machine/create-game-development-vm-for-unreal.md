---
title: Create a Game Development Virtual Machine with Unreal Engine
description: Get up and running with a Windows 10 or Server 2019 Game Development Virtual Machine that has Unreal Engine and other common game development tools pre-installed.
author: meaghanlewis
ms.topic: quickstart
ms.date: 09/07/2022
ms.author: mosagie
ms.prod: azure-gaming
---

# Quickstart: Create a Game Development Virtual Machine with Unreal Engine

Get up and running with a Windows 10 or Windows Server 2019 Game Development Virtual Machine which has Unreal Engine and other common game development tools pre-installed. Unreal Engine is an incredibly powerful and advanced real-time 3D creation tool for photorealistic visuals and immersive experiences.

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.com/free). Please note Azure free accounts do not support GPU enabled virtual machine SKUs. To understand what Azure subscriptions support GPU SKUs, please refer to this <a href="./offer-types.md" target="_blank">offer types chart</a>.
- An [Epic Games account](https://www.epicgames.com/id/login) to accept Epic Games store End User License Agreement after signing in to this VM.
- If you need deploy this VM with Windows 10 as the operating system, Multi-tenant Hosting Rights for Windows 10 is required. Verify you have an [eligible Windows 10 subscription license](/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment#subscription-licenses-that-qualify-for-multitenant-hosting-rights) or a [Visual Studio subscription](/azure/virtual-machines/windows/client-images) for dev/test scenarios.
If you choose to access the VM with either Teradici or Parsec, you need to have your own license keys to use during deployment.  

## Create your Game Development Virtual Machine

To create a Game Dev VM instance:

1. Go to the [Azure portal](https://portal.azure.com/). Or, go to the [Azure Game Development VM](https://aka.ms/gamedevvm) marketplace page, click "Get It Now" under the Azure icon, then click "Continue" in the pop up window to give consent.
In both ways, you might be prompted to sign in to your Azure account if you're not already signed in. If you do the latter, you can skip step 2 and jump to step 3.
2. Find the virtual machine listing by typing in **game development** and selecting **Azure Game Development Virtual Machine**.
3. Select the  **Create**  button.
4. You should be redirected to the **create Azure Game Development Virtual Machine** blade.
5. Fill in the  **Basics**  tab:

    - **Subscription** : If you have more than one subscription, select the one on which the machine will be created and billed. You must have resource creation privileges for this subscription.
    - **Resource group** : Create a new group or use an existing one.
    - **Region** : Select the datacenter that's most appropriate. For fastest network access, it's the datacenter that has most of your existing workloads or is the closest to your physical location. Learn more about [Azure Regions](https://azure.microsoft.com/global-infrastructure/regions/).
    - **Use as a build server** : This is optional. If you do not expect to use 3D applications or work with 3D content on this VM, instead you want to use it for game build purpose, as illustrated in <a href="./overview.md#using-as-build-servers" target="_blank">this build servers example</a>, you can check this box. It gives you more VM size options since GPU is not required.
    - **VM Size** : This VM currently supports the sizes of [NV](/azure/virtual-machines/nv-series), [NVv3](/azure/virtual-machines/nvv3-series), [T4](/azure/virtual-machines/nct4-v3-series), and [A10](/azure/virtual-machines/nva10v5-series) if you are not using it as a build server. Choose a size that is appropriate for your workloads. Read more about [Windows VM sizes in Azure](/azure/virtual-machines/sizes).
    - **Virtual machine name** : Enter the name of the virtual machine. This is how it will appear in your Azure portal.
    - **Admin username** : Enter the administrator username. This is the username you will use to log into your virtual machine, and need not be the same as your Azure username.
    - **Password** : Enter the password you will use to log into your virtual machine. Repeat the same password And **Confirm password** in the next field.
    - **Operating System** : Choose either Windows 10 or Windows Server 2019.

> [!NOTE]
> If you choose Windows 10, you need confirm that you have an eligible license with [multi-tenant hosting rights](/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment), unless you want to use this VM for dev/test scenarios if you have an appropriate [Visual Studio (formerly MSDN) subscription](/azure/virtual-machines/windows/client-images).

6. Click  **Next: Game Development Tools**.
7. You will see **Select Game Engine**. Select your desired engine and version from the dropdown. A list of pre-installed common free game Development tools will show up. If you don't need install any game engine from the list, you can choose **No game engine installed** option.  

    - This VM supports [Unreal Pixel Streaming](https://docs.unrealengine.com/4.27/SharingAndReleasing/PixelStreaming/). You can check the box if you want to enable this feature, which opens the required ports.
    - This VM can be configured to pull down a repository from Perforce after deployment if you already have a Perforce Helix Core version control server in place. If desired, check the box **Connect to and sync a Perforce depot** to configure the Perforce depot to pull from. If you do not have a Perforce server setup and want to know more, please refer to documentation <a href="./integrate-perforce-depot.md" target="_blank">Integrate with a Perforce depot</a>.
    - You can select which version of Microsoft Game Development Kit (GDK) to be included. The version selected for the GDK will be installed in the background once you first login. For Xbox console development, there will need to be additional [steps to enable this development](/gaming/gdk/_content/gc/tools-console/gc-tools-console-toc), as specified in the NDA developer program.
    - If you need use Incredibuild, you can either get a trial license by checking the box, or upload the license file you already have. This VM will automatically install and activate Incredibuild for you. Otherwise, Incredibuild will not be installed. 

> [!NOTE]
> A dedicated Azure data disk will be created and connected to the VM if you choose the game engine. This disk has a volume of 255GB with drive letter E and saves all the game engine files.

8. Click Next: **Remote Access Configuration**.
9. Choose RDP, Teradici or Parsec from the **Remote Access Technology** dropdown. If Teradici or Parsec is chosen, you need fill additional licensing information that is used to register the desired agent. 
   From here, you can also choose whether to have Azure Virtual Desktop (AVD) integration. By default, the box is unchecked. For more details, please refer to documentation <a href="./integrate-azure-virtual-desktop.md" target="_blank">Integrate with Azure Virtual Desktop (AVD)</a>
10. Click **Next: VM Network**. Under **Configure Public IP and DNS** , and **Configure virtual networks,** you can create new resources, or pick the existing resources, depending on your business needs. **Note:** If you select “None” for the Public IP Address, a private IP address will be assigned to this VM. That private IP address is the next available IP address in the subnet you configure for this VM.
11. Click **Next: Data Storage**.

    - From here you can create a disk volume in the VM for data storage that is striped across multiple data disks, which improves the disk throughput. If you don’t want to use the striped disk, please set the number of data disks to be 1. If you don’t want to attach a data disk at all, please set the number to be 0. This VM will still have an OS disk and a temporary data disk as the default configuration. **Note:** It is highly recommended to choose at least 2 disks that will be striped together for the best IOPS performance, and to install your source control and game projects on that drive for the best experience and available space on the VM. The C drive defaults to 256GB and could quickly run out of space for medium to large projects.
    - You can also mount an existing Azure Storage File Share if you have one, which allows for network shares across your team in Azure.

12. Click **Next: Management**.
  
    - You can choose to integrate Azure Active Directory (AAD). It is a similar experience as you enable AAD for a regular Azure virtual machine. With AAD, you can use your corporate credentials to login to this Game Dev VM. Please check both boxes if you want to enable AAD. Otherwise, leave those two boxes blank. [Learn more about integrating with Azure Active Directory](./integrate-vm-with-aad.md).
    - Please also choose how you want to patch Windows OS with **Guest OS updates** options.

13. Click **Next: Advanced**. This is an optional step if you plan to use the Game Dev VM image as your studio's beginning image and then further customize it to make your new base image. For example, adding/removing tools, changing VM configuration like wallpaper. If you will do so, please check the box "VM will be used with sysprep to create a custom image". Otherwise, leave it blank.  

14. Click **Next: Tags**. Tags are used to categorize resources, usually for billing management purposes. If you don't need tags now, you can skip this page and click **Next: Review + create**

15. **Review+create**.

    - The Azure Portal will validate whether all the required information has been filled. If any information is missing or incorrect, you will see the validation failed message at the top. You can click **View error details** to know what is causing the error. You will then need to go back to complete the missing field(s) or make the correction.
    - Once validation is passed, please verify that all the information you entered is correct
    - Select  **Create**.

> [!NOTE]
>  
>- There is no additional charge for the software that comes pre-loaded on the virtual machine. You do pay the compute cost for the VM size that you chose in the  **Size**  step. If you choose Teradici or Parsec as your remote access tool, you bring your own license to the VM on Azure.
>- Provisioning takes 10 to 20 minutes. You can view the status of your VM on the Azure portal.

## Access the Game Development VM

After the VM is created and provisioned, there are three methods to access this VM, depending on the **Remote access technology** you chose before.

**Method 1:** RDP. This remote method is always available.

1. Follow the steps listed to [connect and sign on to Azure-based virtual machine](/azure/virtual-machines/windows/connect-logon). Use the credentials that you configured when created this virtual machine before. If you enable AAD for this VM, you can also use your corporate credentials for RDP access [if you meet the requirements](/azure/active-directory/devices/howto-vm-sign-in-azure-ad-windows#requirements).  
2. Once you sign on to the VM, you'll be prompted immediately to accept Epic Games store End User License Agreement (EULA). If your Epic Games account has already accepted the latest EULA agreement, there is no need to accept it again, and you will be redirected to the desktop after your Epic Games account is authenticated. This is a one-time step when first deploying a new game development VM, and you don’t need repeat this step when you access this VM again.

:::image type="content" source="./media/create-game-development-vm-for-unreal/sign-eula-agreement.png" alt-text="Screenshot of prompt to sign the EULA agreement for Epic Games":::

> [!NOTE]
> Your VM may stay at the Windows welcome screen for up to 1 minute until you see the above EULA Agreement--this is normal.  

3. You're ready to start using the tools that are installed and configured on the VM. Many of the tools can be accessed through Start menu tiles and desktop icons.

> [!NOTE]
> You may see a command prompt window pop up which shows the Microsoft GDK or other components are being installed in the background. This may take up to 10 minutes. You can safely ignore it but leave the window open, as it will automatically close once all the tasks are finished.

:::image type="content" source="./media/create-game-development-vm-for-unreal/user-configuration-tasks-messages-terminal.png" alt-text="Screenshot of terminal window showing user configuration tasks are still completing":::

Alternatively, you can follow the steps to remote into the Game Development Virtual Machine with either [Teradici](./remote-to-vm-with-teradici.md) or [Parsec](./remote-to-vm-with-parsec.md) depending on your chosen method of remote access technology.

## Clean up resources

When no longer needed, you can delete the resource group, virtual machine, and all related resources.

1. On the Overview page for the VM, select the Resource group link.
2. At the top of the page for the resource group, select Delete resource group.
3. A page will open warning you that you are about to delete resources. Type the name of the resource group and select Delete to finish deleting the resources and the resource group.

## Next steps

- Explore the tools on the Game Dev VM by opening the **Start** menu.
- Start to learn and use [Unreal Engine](https://www.unrealengine.com/learn).
- Learn about game development on Azure by reading [Azure for Gaming](/gaming/azure/) and trying out tutorials.
- Read more about the [Game Development Virtual Machine](./overview.md).
