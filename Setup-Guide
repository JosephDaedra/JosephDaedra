# 🚀 The Definitive **qBittorrent + NordVPN Docker Simple Setup Guide**

This guide provides a step-by-step walkthrough to set up a **qBittorrent** Docker container routed through **NordVPN** using **OpenVPN**, with external drives configured for downloading.

## 📚 **1. Prerequisites**

Ensure you have the following:

- **Ubuntu Server**
- **Docker and Docker Compose** installed
- **NordVPN Service Credentials** (not your account credentials; find them in your NordVPN dashboard under *Manual Setup*)
- External drives properly mounted:
  - `/media/Primary_Drive`
  - `/media/Secondary_Drive`

### **Common Issues and Fixes:**

#### **Drives not detected:**

- Use `lsblk` to check if the drives are detected by the system:
  ```bash
  lsblk
  ```
- If the drive isn’t listed, verify physical connections and BIOS settings.

#### **Permission Denied:**

- Set ownership and permissions:
  ```bash
  sudo chown -R 99:100 /media/Primary_Drive
  sudo chmod -R 777 /media/Primary_Drive
  ```
- Verify changes:
  ```bash
  ls -ld /media/Primary_Drive
  ```

#### **fstab Issues:**

- Open `/etc/fstab`:
  ```bash
  sudo nano /etc/fstab
  ```
- Ensure entries use `defaults,rw`:
  ```
  /dev/sda1  /media/Primary_Drive  ntfs  defaults,rw  0  2
  ```
- Apply changes:
  ```bash
  sudo mount -a
  ```

#### **Device Busy Errors:**

- Check processes using the drive:
  ```bash
  sudo fuser -mv /dev/sdX
  ```
- Kill processes if necessary:
  ```bash
  sudo fuser -k /dev/sdX
  ```

#### **Read-Only Drives:**

- Remount with `rw` permissions:
  ```bash
  sudo mount -o remount,rw /media/Primary_Drive
  ```
- Verify:
  ```bash
  mount | grep /media/Primary_Drive
  ```

---

## 🛠️ **2. Prepare External Drives**

Make sure the drives are properly mounted and accessible:

```bash
sudo mkdir -p /media/Primary_Drive
sudo mkdir -p /media/Secondary_Drive
sudo chown -R 99:100 /media/Primary_Drive
sudo chown -R 99:100 /media/Secondary_Drive
sudo chmod -R 777 /media/Primary_Drive
sudo chmod -R 777 /media/Secondary_Drive
```

Verify the permissions:

```bash
ls -ld /media/Primary_Drive
ls -ld /media/Secondary_Drive
```

Ensure both have `drwxrwxrwx` permissions.

### **Common Issues and Fixes:**

#### **Drives not showing up:**

- Verify entries in `/etc/fstab`:
  ```bash
  sudo cat /etc/fstab
  ```
- Ensure no syntax errors.
- Reload `fstab`:
  ```bash
  sudo mount -a
  ```

#### **Mounting Errors:**

- Check mount logs:
  ```bash
  dmesg | grep mount
  ```

#### **Permissions Not Persisting After Reboot:**

- Add correct ownership in `/etc/fstab`:
  ```
  /dev/sda1 /media/Primary_Drive ntfs defaults,uid=99,gid=100 0 2
  ```
- Test by rebooting.

#### **Drives Mounted as Root:**
- Ensure drives are mounted with `rw` and not as root.
  ```bash
  sudo mount -o remount,rw /media/Primary_Drive
  ```
- Verify with:
  ```bash
  mount | grep /media/Primary_Drive
  ```

---

## 🔑 **3. Set Up OpenVPN Config for NordVPN**

Create a directory for NordVPN configuration:

```bash
sudo mkdir -p /media/Primary_Drive/nordvpn
```

Copy your NordVPN `.ovpn` configuration file:

```bash
sudo cp /path/to/your/nordvpn-config.ovpn /media/Primary_Drive/nordvpn/
```

Verify the file exists:

```bash
ls -l /media/Primary_Drive/nordvpn
```

### **Common Issues and Fixes:**

#### **Invalid Configuration File:**

- Ensure the `.ovpn` file is from NordVPN.
- Verify content integrity.

#### **Missing Credentials:**

- Edit the `.ovpn` file and ensure `auth-user-pass credentials.conf` exists.

#### **File Permissions:**

- Secure permissions:
  ```bash
  sudo chmod 600 /media/Primary_Drive/nordvpn/nordvpn-config.ovpn
  ```

#### **Missing Tun Interface:**

- Check if `tun0` exists:
  ```bash
  ifconfig
  ```
- Enable it manually if missing:
  ```bash
  sudo modprobe tun
  ```

---

## 🐳 **4. Deploy qBittorrent + NordVPN Docker Container**

Run the Docker container:

```bash
sudo docker run -d \
  --name=TorrentVPN \
  --cap-add=NET_ADMIN \
  --cap-add=NET_RAW \
  --device=/dev/net/tun \
  --dns=8.8.8.8 \
  --dns=8.8.4.4 \
  -v /media/Primary_Drive:/downloads \
  -v /media/Primary_Drive/nordvpn:/config/openvpn \
  -v /media/Secondary_Drive:/backup \
  -v /home/user/qbittorrent-config:/config \
  -e VPN_ENABLED=yes \
  -e VPN_PROV=nordvpn \
  -e VPN_USER=<Service_Username> \
  -e VPN_PASS=<Service_Password> \
  -e WEBUI_PORT=8080 \
  -e LAN_NETWORK=192.168.0.0/24 \
  -e PUID=99 \
  -e PGID=100 \
  -e UMASK=002 \
  -p 8080:8080 \
  -p 6881:6881 \
  -p 6881:6881/udp \
  --restart=unless-stopped \
  binhex/arch-qbittorrentvpn
```

### **Common Issues and Fixes:**

#### **VPN Not Connecting:**
- Verify NordVPN credentials.
- Ensure `.ovpn` is mapped correctly.

---

**This is the definitive simple setup guide for qBittorrent + NordVPN via Docker, based on extensive troubleshooting and real-world testing.** 🚀

