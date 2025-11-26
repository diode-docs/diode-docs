---
_schema: default
title: File Syncing
nav_title: File Syncing
nav_section: Features
weight: 203
draft: false
---
Your Diode Collab app can be configured to sync files from the Zone to your local device.  File syncing is end to end encrypted, and the files are never stored on a server or device that you do not control.

The File Syncing capability can be used as a replacement or augmentation to cloud storage systems like Microsoft OneDrive, Google Drive, and Dropbox.  As opposed to those systems, the vendor (in our case, Diode) has no access to your information.

## Member Sync Settings

It is important to note that the Zone has a <a href="https://app.docs.diode.io/docs/features/file-sync-policy/" target="_blank" rel="noopener">File Sync Policy</a>  that is configured by the Zone Owner or Administrator that dictates which File Sync Settings are available to the Members.  For example, if the File Sync Policy for the Zone doesn't allow Members to choose their sync setting, then the Drives in your Zone will operate in the sync setting specified by the Zone's policy - you can't configure your own setting.

For Zones whose File Sync Policy allows Members to choose their Drives' sync setting, there are three options:

1. **2-Way**: Any change you make, and any change another person makes will be automatically synchronized to your device and to the Drive
2. **Upload Only**: Changes you make will be automatically sync'd to the Drive, but changes others make must be manually downloaded by you.  This setting is sometimes referred to as the "Lite Zone" setting.
3. **Off (manual)**: You cannot add files or modifications to the Drive, but you can manually download files from the Drive.

Many people use the "Upload Only" sync mode in order to conserve space on their device.  They are able to author files and changes, but the only files they have from the Drive are the ones they have intentionally downloaded.

Note that File Sync Settings can be modified per <a href="https://app.docs.diode.io/docs/private-drives/" target="_blank" rel="noopener">Drive in the Zone</a> (if the Zone File Sync Policy allows it).

In order to modify your system's File Sync Setting, go to the Drive you'd like to adjust (click the folder icon in the quick access bar twice and the click the Drive) and the click the gear icon in the File/Folder breadcrumb (to the right of the Drive name):

![](/uploads/image-204.png)

Scroll down to the Local Settings section and click the File Syncing setting:

![](/uploads/image-205.png)

Inside the setting, you will be able to select your desired File Sync setting for the Drive.

**NOTE:** You can also access the sync setting more directly by clicking the sync icon (rotating-arrow icon) in the Zone header.

![](/uploads/image-206.png)

That will open the File Transfer Activity window - at the top, it lists all Drives' sync settings there.  Clicking the gear icon by the Drive will take you directly to the sync setting.

![](/uploads/image-207.png)

Different roles have different permissions for setting the local sync settings:

* **In Moderation:** Sync always Off, cannot be changed.
* **Member:** Sync setting determined by File Sync Policy and the local Sync Setting as described above
* **Admin:** Can always set the local Sync Setting as desired (regardless of Zone policy)
* **Owner:** Can always set the local Sync Setting as desired (regardless of Zone policy)

Please note that the File Sync Policy and local Sync Settings do not apply to chat attachments.  Chat attachments will be synchronized regardless of the role, policy, or sync setting.