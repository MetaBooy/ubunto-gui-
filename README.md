# Run as root (or with sudo)
```
sudo -i
```


# 1. Update system
```
apt update -y && apt upgrade -y
```

# 2. Install XFCE and xRDP
```
apt install xfce4 xfce4-goodies xrdp -y
```

# 3. Enable xRDP service
```
systemctl enable --now xrdp
```

# 4. Create a new user (optional)
```
USERNAME=clouduser
PASSWORD=Cloud@123
```

# Check if user exists
```
if id "$USERNAME" &>/dev/null; then
    echo "User $USERNAME already exists"
else
    echo "Creating user $USERNAME"
    adduser --disabled-password --gecos "" "$USERNAME"
    echo "$USERNAME:$PASSWORD" | chpasswd
    usermod -aG sudo "$USERNAME"
fi
```
# 5. Configure session for XFCE
```
echo "startxfce4" > /home/$USERNAME/.xsession
chown $USERNAME:$USERNAME /home/$USERNAME/.xsession

# 6. Add xrdp user to ssl-cert group
adduser xrdp ssl-cert
```

# 7. Restart xRDP
```
systemctl restart xrdp
````

# Done
echo -e "\n✅ Done! You can now RDP into your VM using IP and login:\n"
echo "Username: $USERNAME"
echo "Password: $PASSWORD"
echo -e "\n📌 TIP: Allow TCP port 3389 in Oracle Cloud firewall (Security List)\n"
