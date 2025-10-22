A step-by-step guide on how I deployed my React web application using Nginx on Windows through the Command Prompt.

🚀 Deploying a React App on Nginx (Using Command Prompt on Windows)

This guide explains how I hosted my React web application on an Nginx server using only the Windows Command Prompt.
All steps are done manually and can be replicated on any Windows system running Nginx.

🧩 Prerequisites

Before you start, make sure you have:

✅ Node.js and npm installed
(You can verify using node -v and npm -v)

✅ A working React project (created using create-react-app or similar)

✅ Nginx downloaded and extracted on your Windows machine

✅ Command Prompt with administrator privileges

💻 Step 1: Install Nginx on Windows

Download the Nginx Windows package from the official site:
👉 https://nginx.org/en/download.html

Choose the “mainline version” for Windows and extract the .zip file to a convenient location, for example:

C:\nginx


To verify installation, open Command Prompt and navigate to your Nginx folder:

cd C:\nginx


Start Nginx:

start nginx


Open your browser and visit:

http://localhost


If you see the Nginx Welcome Page, installation was successful 🎉

⚙️ Step 2: Build the React App

In your React project folder, run:

npm run build


This will generate an optimized production build inside a folder named build/.

📂 Step 3: Move React Build Files to Nginx HTML Directory

Now copy your build folder contents into Nginx’s html directory.

xcopy build "C:\nginx\html" /E /H /C /I


💡 /E copies all subdirectories
/H includes hidden files
/C continues copying even if errors occur
/I assumes destination is a directory

After this step, your app files should be inside:

C:\nginx\html\index.html
C:\nginx\html\static\

🛠️ Step 4: Edit the Nginx Configuration File

Navigate to the Nginx configuration folder:

cd C:\nginx\conf


Open the Nginx configuration file using Notepad:

notepad nginx.conf


Inside nginx.conf, find the existing server block and replace it with the following configuration:

server {
    listen       80;
    server_name  localhost;  # or your IP/domain

    root   html;
    index  index.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;
}


Save and close the file.

🔄 Step 5: Restart Nginx to Apply Changes

To reload the updated configuration, run:

cd C:\nginx
nginx -s reload


If Nginx isn’t running yet, start it manually:

start nginx


To stop the Nginx server:

nginx -s stop

🌐 Step 6: Test Your Deployment

Open your web browser and go to:

http://localhost


or use your local IP address (e.g., http://192.168.x.x) if you’re testing on another device on the same network.

You should now see your React app running live 🎉

🧰 Optional (For Remote Deployment on Linux)

If you later host your app on a remote Linux server:

Copy your build folder to /var/www/react-app

scp -r build/ username@server-ip:/var/www/react-app


Edit Nginx configuration:

sudo nano /etc/nginx/sites-available/react-app


Add this configuration:

server {
    listen 80;
    server_name your-domain.com;

    root /var/www/react-app;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;
}


Enable site and restart:

sudo ln -s /etc/nginx/sites-available/react-app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

🧾 Directory Structure Summary
C:\nginx\
│
├── conf\
│   └── nginx.conf
├── html\
│   ├── index.html
│   ├── static\
│   │   ├── js\
│   │   ├── css\
│   │   └── media\
│   └── favicon.ico
├── logs\
└── nginx.exe

⚡ Common Troubleshooting
Issue	Cause	Fix
Page not loading	Nginx not running	Run start nginx
React routes not working	Missing try_files rule	Ensure try_files $uri /index.html; is in your config
“Port already in use” error	Another service using port 80	Stop IIS or change Nginx listen port
Old version showing	Cached files	Clear browser cache or hard refresh (Ctrl + F5)
🔁 Updating Your App

When you make changes to your React app:

Rebuild the app:

npm run build


Copy the new build files to Nginx:

xcopy build "C:\nginx\html" /E /H /C /I /Y


Reload Nginx:

nginx -s reload


Your updates will go live instantly 🚀

✅ Summary
Step	Description
1️⃣	Install Nginx
2️⃣	Build React project
3️⃣	Copy files to C:\nginx\html
4️⃣	Configure nginx.conf using Notepad
5️⃣	Restart Nginx
6️⃣	Test your deployment# React-Deployed-Through-Nginx-Server-

