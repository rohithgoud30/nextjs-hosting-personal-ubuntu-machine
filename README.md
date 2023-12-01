Here is the markdown format of the text you provided:

# Step 1: Set Up Your Ubuntu Desktop

Ensure that your Ubuntu desktop is up to date:

    sudo apt update
    sudo apt upgrade -y

# Step 2: Install NVM and Node.js  

For the latest link and steps, please visit:  
NVM GitHub Repository  
https://github.com/nvm-sh/nvm/blob/master/README.md#install--update-script

Alternatively, you can follow the commands below:

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash

Add the following to `~/.bashrc`:

    export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

Troubleshooting on Linux  
On Linux, if you encounter the error `nvm: command not found` or see no feedback after running `command -v nvm`, close your current terminal, open a new one, and try again. Alternatively, you can run the following commands for different shells:

    source ~/.bashrc

To see all available Node.js versions:

    nvm ls-remote

To install a specific Node.js version with NVM:

    nvm install 16.x.x
    Replace `16.xx` with the desired version

Check if Node.js is installed properly:

    node -v
    npm -v

# Step 3: Clone Your Next.js App  

Clone your Next.js app from GitHub to your Ubuntu desktop:  
Replace YOUR_USER_NAME/path with your username and desired path

    cd /home/YOUR_USER_NAME/path
    git clone https://github.com/warengonzaga/sample-nextjs-app.git ubuntu-next-app
    cd ubuntu-next-app

# Step 4: Install Dependencies

    npm install

# Step 5: Automate `npm run dev` with systemd  

Open the systemd service file:

    sudo nano /etc/systemd/system/nextjs-app.service   

Note:  
If you don't know your username, type this command in the terminal:

    whoami

If you don't know your group name, type this command in the terminal:

    id -gn

Update the content of the file to the following:

    [Unit]
    Description=Next.js App  
    After=network.target

    [Service]
    WorkingDirectory=/home/rohith/Code/ubuntu-next-app
    ExecStart=/bin/bash -lc 'source /home/rohith/.nvm/nvm.sh && cd /home/rohith/Code/ubuntu-next-app && nvm use v20.10.0 && npm run dev'
    User=rohith
    Group=rohith

    [Install] 
    WantedBy=multi-user.target

Save the file and exit.  

Reload the systemd daemon, restart the service, and check the status:

    sudo systemctl daemon-reload
    sudo systemctl restart nextjs-app.service 
    sudo systemctl status nextjs-app.service


# Step 6: Set Up Loophole Tunnel

1. Download the Loophole archive and move Loophole to the home directory:

        mv ~/Downloads/path/loophole ~/

2. Create a systemd service file:

        sudo nano /etc/systemd/system/tunnel.service

Note:
If you don't know your username, type this command in the terminal:

    whoami

If you don't know your group name, type this command in the terminal: 

    id -gn

Add the following content:

        [Unit]
        Description=Loophole Tunnel
        After=network.target

        [Service] 
        WorkingDirectory=/home/YOUR_USER_NAME/
        ExecStart=/bin/bash -lc './loophole http 3000'
        User=rohith
        Group=rohith

        [Install]
        WantedBy=default.target

3. Create an account and log in to Loophole:

        sudo ./loophole account login

4. Enable and start the service:

        sudo systemctl start tunnel.service  
        sudo systemctl status tunnel.service

5. To view blob data, type:

        sudo journalctl -u tunnel.service  

6. On checking the status, you will get a Loophole Tunnel link like `https://randomstring.loophole.site`.

# Step 7: Set Up Nginx as a Reverse Proxy  

If you haven't installed Nginx yet, you can install it:

    sudo apt-get install nginx

Create an Nginx server block configuration file:

    sudo nano /etc/nginx/sites-available/nextjs

Note:  
If you want to find YOUR_IP_ADDRESS, go to  
https://ipv4.icanhazip.com/ and copy it.  

Add the following configuration, replacing the `proxy_pass`:

        server {
            listen 80;
            server_name YOUR_IP_ADDRESS;
        
            location / {
                proxy_pass https://randomstring.loophole.site;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
            }
        }

Create a symbolic link to enable the Nginx configuration:

    sudo ln -s /etc/nginx/sites-available/nextjs /etc/nginx/sites-enabled  

Remove the default Nginx server block:

    sudo rm /etc/nginx/sites-enabled/default

Test the Nginx configuration:

    sudo nginx -t

Reload Nginx to apply the changes: 

    sudo service nginx reload

# Step 8: Access Your Next.js App Through Loophole  

Try accessing your Next.js app through the Loophole URL (`https://randomstring.loophole.site`). This should bypass the router login issue and allow external access.
