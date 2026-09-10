---
_schema: default
title: Diode Collab Won't Start
nav_title: Why Won't Diode Collab Start?
nav_section: Troubleshooting
weight: 10002
draft: false
---
If Diode Collab won't start on your system, it could be that the auto-update process failed. Despite the auto-update process using a checksum to validate the update was successful, there have been intermittent reports that leads us to believe the process can still sometimes fail, resulting in Diode Collab not being able to load the correct system packages at startup.

To fix, you need to remove the update folder and allow it to re-populate via auto-update by doing the following:

### **MacOS and Linux**

1. Open a terminal window
2. Carefully and precisely type:

`rm -r ~/.config/ddrive/update-*`

3. Start Diode Collab

### **Windows**

**Possible cause: Broken Over the Air Update**

Although Diode Collab does a checksum check on new over the air updates, we still get reports from time to time that "something" has gone wrong after an update and the app won't start.  To fix this, you'll need to  delete your update folder that contains the OTA update, and restart your machine.

1. Open an explorer window
2. Enable viewing hidden files (you won't be able to see the .config folder unless you do)
3. Go to C:\\Users\\&lt;user&gt;\\.config\\ddrive\\ folder
   1. Where &lt;user&gt; is your Windows username
4. Locate the update folders (all named update-&lt;version&gt; - e.g. update-1.7.2)
5. Delete the update folders and all their contents
6. Restart your machine
7. Start Diode Collab

**Possible cause: File lock on a log file**

Windows Defender and its ilk love to inspect active files and sometimes things can go wrong and the "helpful" Windows snooper can lock a file critical to Diode Collab.  This seems to usually be the log files.  To fix this, you'll need to erase your log files and restart your machine.

1. Open an explorer window
2. Enable viewing hidden files (you won't be able to see the .config folder unless you do)
3. Go to C:\\Users\\&lt;user&gt;\\.config\\ddrive\\ folder
   1. Where &lt;user&gt; is your Windows username
4. Locate the log files -&gt; debug.log and error.log
   1. NOTE: if you run multiple profiles, the log files at C:\\Users\\&lt;user&gt;\\.config\\ddrive\\ are for your first profile, but your other profiles' log files at C:\\Users\\&lt;user&gt;\\.config\\ddrive\\profiles\\profile&lt;N&gt;\\  may also need to be deleted for whatever your currently loading profile is
5. Delete the log files
6. Restart your machine
7. Start Diode Collab

**Possible cause: Broken Fresh Install**

Diode Collab v.1.22.11 and earlier binary installs did not fully kill the existing application before running the new installation process.  This could result in a non-compatible DLL or BEAM file not being updated by the new binary, leaving a mixed installation that would never successfully run.  You'll need to fully kill all Diode Collab related processes and then reinstall and reboot your machine to fix this.

1. Open a terminal window
2. Type "taskkill /f /im heart.exe /im epmd.exe /im ddrive.exe"
3. Type it again and confirm that you get three ERROR: The process "&lt;process&gt;" not found.
4. Download the latest binary from https://collab.diode.io/download
5. Install it
6. Retart your machine
7. Start Diode Collab

&nbsp;

---

&nbsp;