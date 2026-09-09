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

1. Open an explorer window
2. Enable viewing hidden files (you won't be able to see the .config folder unless you do)
3. Go to C:\\Users\\&lt;user&gt;\\.config\\ddrive\\ folder
   1. Where &lt;user&gt; is your Windows username
4. Locate the update folders (all named update-&lt;version&gt; - e.g. update-1.7.2)
5. Delete the update folders and all their contents
6. Restart your machine
7. Start Diode Collab

---

&nbsp;