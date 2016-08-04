# INSTALL INSTRUCTIONS
1. Run: `sudo mkdir /usr/share/grub/themes` (IF DONE SKIP TO TWO.)
2. Run: `sudo cp --recursive ./surface /usr/share/grub/themes`
3. Add the following line to `/etc/default/grub`:

    `GRUB_THEME=/usr/share/grub/themes/surface/theme.txt`
4. Make the repair title and icon for kernel repair.

    Open `/etc/grub.d/10_linux` and search for (towards bottom):<br>
    `echo "submenu '$(gettext_printf "Advanced options for %s" "${OS}" | grub_quote)'`<br>
    Insert the following immediatly after:<br>
    `--class recovery --class repair`<br>
    (note: edit "Advanced options for %s" to "Repair")

5. Make the Secure Boot title and icon.

    Open `/etc/grub.d/30_uefi-firmware` and search for (towards bottom) :<br>
    `menuentry '$LABEL'`<br>
    Insert the following immediatly after:<br>
    `--class secure --class recovery`<br>
    (note: edit `LABEL=System Setup` to `LABEL=Secure Boot`)<br>

6. Make the Windows title and icon for Windows launch.

    If you have run `boot-repair`, open `/etc/grub.d/25_custom` and you will see the following:
    ```
    menuentry "Windows UEFI bkpbootmgfw.efi" { 
      search --fs-uuid --no-floppy --set=root BE36-A896 
      chainloader (${root})/EFI/Microsoft/Boot/bkpbootmgfw.efi 
    } 
    menuentry "Windows Boot UEFI loader" { 
      search --fs-uuid --no-floppy --set=root BE36-A896 
      chainloader (${root})/EFI/Boot/bkpbootx64.efi 
    }
    ```
    Delete one of the menuentry settings then insert `--class windows` after the `"`:<br>
    (note: edit title as desired.)
    
    If not, open `/etc/grub.d/30_os-prober` and search for :<br>
    `'$(echo "${LONGNAME} $onstr" | grub_quote)' --class windows`<br>
    Replace `${LONGNAME}` to Windows (note: edit title as desired.)
    
7. Run: `sudo update-grub`
