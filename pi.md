# Photobooth setup
### Install needed packages
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install -y git apache2 php php-gd ffmpeg
```

### Install node
```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Install yarn
```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install -y yarn
```

### Install gphoto2
```
wget https://raw.githubusercontent.com/gonzalo/gphoto2-updater/master/gphoto2-updater.sh && sudo bash gphoto2-updater.sh
```

### Give our webserver user access to /var/www
```
sudo chown -R www-data:www-data /var/www/
```

### Give our webserver user access to the usb device
```
echo SUBSYSTEM=="usb", ATTRS{idVendor}=="04b0", ATTRS{idProduct}=="0424", OWNER="www-data" | sudo tee /etc/udev/rules.d/50-usb-camera.rules
```

### Remove execution permission on gphoto2 Volume Monitor
```
sudo chmod -x /usr/lib/gvfs/gvfs-gphoto2-volume-monitor
```

### Reboot once
```
reboot
```

### Get the source and build it
```
cd /var/www/
sudo -u www-data -s
rm -r html/
git clone https://github.com/andi34/photobooth html
cd /var/www/html
git fetch origin
git checkout origin/master
git submodule update --init
cp config/config.inc.php config/my.config.inc.php
yarn install
yarn build
exit
```
### Give sudo rights to the webserver - SECURITY ISSUE - NEEDS TO BE FIXED
`sudo nano /etc/sudoers`

Add the following:
`www-data ALL=(ALL) NOPASSWD: ALL`