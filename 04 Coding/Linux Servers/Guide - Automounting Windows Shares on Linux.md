
This document outlines the procedure for mounting a Windows (SMB/CIFS) share onto a Linux machine (e.g., Ubuntu Server) so that it persists across reboots and is accessible to local users and Docker containers.

## Prerequisites
Ensure you have the necessary CIFS utilities installed:
```bash
sudo apt update
sudo apt install cifs-utils
```

---

## 1. Create the Mount Point
Create a directory where the remote share will be "attached." It is recommended to place this in your home directory or a standard mount location.

```bash
# Example: Creating the folder in your home directory
mkdir -p /home/ubuntu/Media

# Set permissions (Standard practice)
sudo chown -R nobody:nogroup /home/ubuntu/Media
sudo chmod -R 0755 /home/ubuntu/Media
```

---

## 2. Configure Credentials
To avoid putting your plaintext password directly into the `/etc/fstab` file, create a hidden credentials file.

1. Create the file:
   ```bash
   sudo nano /etc/win_cred
   ```
2. Add the following lines (replace with your actual details):
   ```text
   username=windows_user_name
   password=windows_password
   ```
3. Secure the file so only the root user can read it:
   ```bash
   sudo chown root: /etc/win_cred
   sudo chmod 600 /etc/win_cred
   ```

---

## 3. Configure Automount via fstab
Edit the filesystem table to define how and when the share should be mounted.

1. Open the configuration file:
   ```bash
   sudo nano /etc/fstab
   ```
2. Add the following line at the end of the file:
   ```text
   //10.0.0.201/Media /home/ubuntu/Media cifs credentials=/etc/win_cred,iocharset=utf8,sec=ntlmsv2,vers=3.0,uid=1000,gid=1000,_netdev,x-systemd.automount 0 0
   ```

### Key Parameters Explained:
*   **`credentials=/etc/win_cred`**: Points to your secure password file.
*   **`iocharset=utf8`**: Ensures special characters in filenames are handled correctly.
*   **`sec=ntlmsv2`**: Uses a modern, secure authentication protocol.
*   **`vers=3.0`**: Forces the use of SMB 3.0 (more stable and faster than older versions).
*   **`uid=1000,gid=1000`**: Ensures your local Linux user has permission to read/write the files (replace `1000` with your specific UID if different).
*   **`_netdev`**: Tells the system this mount requires network connectivity.
*   **`x-systemd.automount`**: **(Recommended)** This ensures that if the network isn't ready immediately at boot, the system will "mount on demand" as soon as you try to access the folder.

---

## 4. Test the Configuration
After editing `fstab`, you should test the mount without rebooting.

**To mount the share manually:**
```bash
sudo mount /home/ubuntu/Media
```

**To verify the connection:**
```bash
df -h
# Or check specific permissions:
ls -l /home/ubuntu/Media
```

**To unmount for testing purposes:**
```bash
sudo umount /home/ubuntu/Media
```

---

## Troubleshooting
*   **Permission Denied:** Ensure the `uid` and `gid` in `/etc/fstab` match your local user (check by typing `id` in the terminal).
*   **Mount Fails:** Check if the IP address is reachable from the Linux box: `ping 10.0.0.201`.
*   **Slow Mounts:** Ensure you are using `vers=3.0` or higher to avoid fallback to slower, older protocols.