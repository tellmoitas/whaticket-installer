Interactive CLI tool for installing and updating whaticket

### download & setup

Firstly, you need to download it:


```bash
sudo apt -y update && apt -y upgrade
sudo apt install -y git
git clone https://github.com/riservato-xyz/whaticket-installer.git
```

Now, all you gotta do is making it executable:

```bash
sudo chmod +x ./whaticket-installer/whaticket
```

### usage

After downloading and make it executable, you need to **navigate into** the installer directory and **run the script with sudo**:

```bash
cd ./whaticket-installer
```

```bash
sudo ./whaticket
```

### After install

Make pm2 auto start afeter reboot:

```bash
pm2 startup ubuntu -u deploy
```

Copy the last line outputed from previus command and run it, its something like:

```bash
sudo env PATH=\$PATH:/usr/bin pm2 startup ubuntu -u YOUR_USERNAME --hp /home/YOUR_USERNAM
```

### Upgrading

WhaTicket is a working in progress and we are adding new features frequently. To update your old installation and get all the new features, you can use a bash script like this:

Note: Always check the .env.example and adjust your .env file before upgrading, since some new variable may be added.

```bash
nano updateWhaticket
```

```bash
#!/bin/bash
echo "Updating Whaticket, please wait."

cd ~
cd whaticket
git pull
cd backend
npm install
rm -rf dist
npm run build
npx sequelize db:migrate
npx sequelize db:seed
cd ../frontend
npm install
rm -rf build
npm run build
pm2 restart all

echo "Update finished. Enjoy!"
```

Make it executable and run it:

```bash
chmod +x updateWhaticket
./updateWhaticket
```
