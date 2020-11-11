# AWS

## 2020-10-19
### ERROR
```
Access denied for user 'root'@'%' to database 'crafter'
Query is : GRANT ALL PRIVILEGES ON crafter.* TO 'crafter'@'localhost'
WITH GRANT OPTION
```

### CAUSE
You can't grant `SUPER` privilege on RDS, so you CANNOT use the `ALL PRIVILEGES` shortcut.
You must name the privileges you want to grant explicitly, even if it means listing every privilege except SUPER.
You can find out what privileges you have with
```sql
 SHOW GRANTS FOR root
```

### SOLUTION
Use root user and don't try to create another user. Or list the specific permissions you want to grant to the new user.
Given that crafter executes the command `GRANT ALL PRIVILEGES` the only solution was using root user who already has permissions
on the crafter db.

## 2020-11-11
### ERROR
```
When running sudo os-prober the Windows volume didn't show up.
I tried editing /etc/default/grub file and I was able to show the GRUB menu, but it only showed Manjaro option
After editing /etc/default/grub you must run sudo update-grub
```

### CAUSE
```
Manjaro was installed with the EFI boot option (even the boot partion was tag as /boot/efi) and Windows was installed with BIOS mode (legacy), you can check the windows mode by running Win + R followed by mdInfo32. Check the option BIOS Mode under System summary. Windows and Manjaro had different boot mode.
The cause was when selecting the boot device (USB) I selected UEFI USB Driver instead of just USB Driver
Pay attention when selecting the installation method (manual particion, delete disk, etc) at the top it must show BIOS instead of EFI
```

### SOLUTION
```
Select the option without UEFI when choosing the USB driver before installing Manjaro
this will install Manjaro with BIOS mode.
```
