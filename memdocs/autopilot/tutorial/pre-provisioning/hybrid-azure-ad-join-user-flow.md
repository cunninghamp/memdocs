---
title: Windows Autopilot for pre-provisioned deployment hybrid Azure AD join - Step 10 of 11 - User flow
description: How to - Windows Autopilot for pre-provisioned deployment hybrid Azure AD join - Step 10 of 11 - User flow.
ms.prod: windows-client
ms.localizationpriority: medium
author: frankroj
ms.author: frankroj
ms.reviewer: jubaptis
manager: aaroncz
ms.date: 04/24/2023
ms.topic: tutorial
ms.collection: 
  - tier1
  - highpri
ms.technology: itpro-deploy
appliesto:
  - ✅ <a href="https://learn.microsoft.com/windows/release-health/supported-versions-windows-client" target="_blank">Windows 11</a>
  - ✅ <a href="https://learn.microsoft.com/windows/release-health/supported-versions-windows-client" target="_blank">Windows 10</a>
---

# Pre-provision hybrid Azure AD join: User flow

Windows Autopilot for pre-provisioned deployment hybrid Azure AD join steps:
- Step 1: [Set up Windows automatic Intune enrollment](hybrid-azure-ad-join-automatic-enrollment.md)
- Step 2: [Install the Intune Connector (OU)](hybrid-azure-ad-join-intune-connector.md)
- Step 3: [Increase the computer account limit in the Organizational Unit](hybrid-azure-ad-join-computer-account-limit.md)
- Step 4: [Register devices as Autopilot devices](hybrid-azure-ad-join-register-device.md)
- Step 5: [Create a device group](hybrid-azure-ad-join-device-group.md)
- Step 6: [Configure and assign Autopilot Enrollment Status Page (ESP)](hybrid-azure-ad-join-esp.md)
- Step 7: [Create and assign hybrid Azure AD join Autopilot profile](hybrid-azure-ad-join-autopilot-profile.md)
- Step 8: [Configure and assign domain join profile](hybrid-azure-ad-join-domain-join-profile.md)
- Step 9: [Assign Autopilot device to a user (optional)](hybrid-azure-ad-join-assign-device-to-user.md)
- Step 10: [Technician flow](hybrid-azure-ad-join-technician-flow.md)
> [!div class="checklist"]
> - **Step 10: User flow**

For an overview of the Windows Autopilot for pre-provisioned deployment hybrid Azure AD join workflow, see [Windows Autopilot for pre-provisioned deployment hybrid Azure AD join overview](hybrid-azure-ad-join-workflow.md#workflow)

## User flow

> [!IMPORTANT]
>
> The user flow hybrid portion of the Azure AD join process requires connectivity to both the Internet and a domain controller.

Once the technician flow step of the pre-provisioning process completes successfully and the device is resealed, the device can be delivered to the end-user. The end-user then completes the normal Windows Autopilot user-driven process. This final step is know as the user flow and involves the following steps:

1. If a wired network connection is available, connect the device to the wired network connection.

1. Power on the device.

1. Once the device boots up, one of two things occurs depending on the state of network connectivity:

   - If the device is connected to a wired network and has network connectivity, the Enrollment Status Page (ESP) appears.

   - If the device isn't connected to a wired network or if it doesn't have network connectivity, it prompts to connect to a network. Connectivity to the Internet and a domain controller is required during the user flow phase of a hybrid Azure AD join:

     1. The **Let's connect you to a network** screen appears. At this screen, either plug the device into a wired network (if available), or select and connect to a wireless Wi-Fi network.

     1. Once network connectivity is established, the **Next** button should become available. Select **Next**. After some time, the Enrollment Status Page (ESP) appears.

    > [!IMPORTANT]
    >
    > Make sure that the connected network has both connectivity to the Internet and to a domain controller. If the connected network doesn't have connectivity to a domain controller, a solution such as VPN is required to establish connectivity to a domain controller. The VPN needs to connect to a network that has connectivity to a domain controller during the ESP.

    > [!NOTE]
    >
    > Additional screens such as License Terms, Privacy, Language, and Keyboard may appear before the Enrollment Status Page (ESP) depending on how the Autopilot profile was configured at the [Create and assign Autopilot profile](hybrid-azure-ad-join-autopilot-profile.md) step.

1. The Enrollment Status Page (ESP) displays progress during the provisioning process across three phases:

   - **Device preparation** (Device ESP)
   - **Device setup** (Device ESP)
   - **Account setup** (User ESP)

    The first two phases of **Device preparation** and **Device setup** are part of the Device ESP while the final phase of **Account setup** is part of the User ESP.

    For the user flow of Windows Autopilot for pre-provisioned deployment, the **Device setup** phase of the Device ESP and the **Account setup** phase of the User ESP runs. The **Device preparation** phase of the Device ESP doesn't run during the user flow since it already ran and completed during the [Technician flow](hybrid-azure-ad-join-technician-flow.md). The **Device setup** phase of the Device ESP runs again during the user flow in case any new or additional policies or applications assigned to the device became available during the time frame that the technician flow ran and when the user flow runs after the device was delivered to the end-user.

    > [!NOTE]
    >
    > To view and hide detailed progress information in the ESP during the provisioning process:
    >
    > - Windows 10: To show details, next to the appropriate phase select **Show details**. To hide the details, next to the appropriate phase select **Hide details**.
    > - Windows 11: To show details, next to the appropriate phase select **∨**. To hide the details, next to the appropriate phase select **∧**.

1. Once the **Device setup** phase of the Device ESP is complete, the Windows sign-on screen appears:

   1. Select <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>DEL</kbd> on the keyboard to initiate Windows sign on.
   2. Enter the on-premises domain credentials for the end-user.
   3. Select <kbd>ENTER</kbd> on the keyboard to sign the end-user into the device.

    > [!IMPORTANT]
    >
    > If on-premises domain end-user credentials are different from Azure AD end-user credentials, make sure that the **on-premises domain end-user credentials** are used to sign into the device at this step. Don't use the Azure AD end-user credentials to attempt to sign into the device at this step.

1. The Enrollment Status Page (ESP) appears again and the **Account setup** of the user ESP begins.

   1. After a short period of time, the Azure AD sign-in page may appear. Sign in with the end-user's Azure AD credentials.

      > [!IMPORTANT]
      >
      > If on-premises domain end-user credentials are different from Azure AD end-user credentials, make sure that **Azure AD end-user credentials** are used to sign in at this step. Don't use on-premises credentials to sign in at this step.

      > [!NOTE]
      >
       Under certain circumstances, the Azure AD sign-in page may not appear and the end-user may be automatically signed into Azure AD. For example, if using Active Directory Federation Services (ADFS) and single sign-on (SSO). If the end-user is automatically signed into Azure AD, then the Autopilot deployment will proceed on to the next step automatically.

   1. The **Stay signed in to all your apps** screen appears. Make sure that the option **Allow my organization to manage my device** is selected, and then select **OK**.

   1. The **You're all set!** screen appears. Select **Done**.

      > [!NOTE]
      >
      > If the device is left alone with no interaction during the **Account setup** and the user ESP process, the device may enter the Windows lock screen. If the device does enter the Windows lock screen during **Account setup** and the user ESP process, unlock the device by selecting <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>DEL</kbd> on the keyboard, entering the on-premises domain credentials for the end-user, and then selecting <kbd>ENTER</kbd> on the keyboard. Unlocking the device should go back to the Enrollment Status Page (ESP) and display the current progress of **Account setup**.

1. Once **Account setup** and the user ESP process completes, the provisioning process completes and the ESP finishes. Select the **Sign out** button to dismiss the ESP and go to the Windows sign on screen. At this point, the end-user can sign into the device using their on-premises domain end-user credentials and start using the device.

## More information

[!INCLUDE [More information user flow](../includes/more-info-user-flow.md)]
