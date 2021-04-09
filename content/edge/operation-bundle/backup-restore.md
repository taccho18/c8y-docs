---
weight: 50
title: Backup and restore
layout: redirect
aliases:
  - /edge/operation/#backup-restore
---

### Backup

To create a backup, follow the steps below. 

1. To power off the VM, right-click on the respective VM (e.g. EDGE-server) in the VirtualBox Manager and click **Close** > **Power off**.<br><br>
<img src="/images/edge/edge-poweroff.jpg" name="Poweroff Edge" style="width:75%;"/> 
<br>
1. Click **File** at the top right and then **Export Appliance**.<br><br>
<img src="/images/edge/edge-backup2.jpg" name="Backup Edge" style="width:75%;"/>  
<br>
2. Enter the location and name as desired and click **Export**.<br><br>
<img src="/images/edge/edge-backup1.jpg" name="Backup Edge" style="width:75%;"/> 

>**Info**: You may append the current date to the image name (e.g. EDGE-server-12-06-2018.ova) so as to have a track of the backups. 

### Restore

To create a restore, follow the steps below.

1. In the VirtualBox Manager, click **File** at the top right and then **Import Appliance**.<br><br>
<img src="/images/edge/edge-restore1.jpg" name="Restore Edge" style="width:75%;"/> 
<br> 
2. In the upcoming window, browse for the OVA image and select it. <br><br>
<br>
3. Power on the VM by clicking on the **Start** button. 