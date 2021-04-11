# AWS

## 2020-10-19
### ERROR
`AWS` `RDS`
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
`GRUB` `Manjaro`

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

# 2020-11-18
## CASE
`apache`
We needed an apache vhost config that proxies the urls matching /static-assets/files* but at the same time we wanted NOT to proxy /static-assets/<anything different than files>
## SOLUTION
`ProxyPass /static-assets/(?!files/).* !`
 
 Another important configs are:
 `Header set X-Frame-Options: "SAMEORIGIN"`
 
 If you are seeing urls with localhost
 `ProxyPreserveHost On`
 
 # 2020-11-20
 `AWS` `S3`
 ## CASE
 - There is a S3 bucket in the aws account `A`
 - There is an Administrator user `user_A` in the same account
 - An external program uses a second user `user_B` which belongs to another Account `B` to push data into the bucket.
 
 # CAUSE
 - `user_A` cannot access the files on the S3 bucket because the files were copied using `user_B`. so the files/objects belong to the account `B`.

# 2020-12-09
## ERROR
`terraform`
Error: Get "http://localhost/api/v1/namespaces/kube-system/configmaps/aws-auth": dial tcp [::1]:80: connect: connection refused

## SOLUTION
```sh
tf state rm kubernetes_config_map.aws_auth
```
