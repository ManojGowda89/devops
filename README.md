# Update package list
sudo apt update

# Install Node.js and npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -


sudo apt install -y nodejs

# Verify installation
node -v
npm -v

# Install nginx
sudo apt install -y nginx

# Start nginx and enable it to run at boot
sudo systemctl start nginx
sudo systemctl enable nginx

# Check nginx status
sudo systemctl status nginx


# Install PM2 globally
sudo npm install -g pm2

pm2 save 




sudo ./svc.sh install 



sudo ./svc.sh start 


sudo ./svc.sh status
# setup ngnix revers proxy

sudo nano /etc/nginx/sites-available/api-skoegle-config

server {
    listen 80;          # Listen for HTTP connections on port 80
    listen [::]:80;     # Also listen on IPv6
    
    server_name api.skoegle.com;   # Only respond to requests for this domain
    
    location / {        # For all URL paths
        # Forward requests to your Node.js app running on port 12000
        proxy_pass http://localhost:12000;
        
        # Support WebSocket connections if your app uses them
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        
        # Pass the original host header to your application
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        
        # Additional headers that help with logging and security
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}


sudo ln -s /etc/nginx/sites-available/api-skoegle-config /etc/nginx/sites-enabled/

sudo rm /etc/nginx/sites-enabled/default



sudo nginx -t

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok


nginx: configuration file /etc/nginx/nginx.conf test is successful

sudo systemctl reload nginx 


github actions


sudo ./svc.sh install


sudo ./svc.sh start



sudo ./svc.sh status 




server {
    listen 80;
    listen [::]:80;
    server_name vmarg.skoegle.com;

    root /var/www/vmarg.skoegle.com/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /assets/ {
        root /var/www/vmarg.skoegle.com/dist;
    }

    location /favicon.ico {
        root /var/www/vmarg.skoegle.com/dist;
    }

    location /robots.txt {
        root /var/www/vmarg.skoegle.com/dist;
    }

    error_page 404 /index.html;
}

server {
    listen 80;
    listen [::]:80;
    server_name api.skoegle.com;

    location / {
        proxy_pass http://localhost:12000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}



ls -lah ~/vmarg/dist





 npm run build



sudo mv ~/vmarg/dist /var/www/vmarg.skoegle.com/



ls -lah /var/www/vmarg.skoegle.com/



sudo chown -R www-data:www-data /var/www/vmarg.skoegle.com
sudo chmod -R 755 /var/www/vmarg.skoegle.com




sudo systemctl restart nginx 




sudo rm -rf /var/www/vmarg.skoegle.com/dist



sudo mv ~/vmarg/dist /var/www/vmarg.skoegle.com/


ls -lah /var/www/vmarg.skoegle.com/



sudo chown -R www-data:www-data /var/www/vmarg.skoegle.com



sudo chmod -R 755 /var/www/vmarg.skoegle.com



sudo systemctl restart nginx



sudo systemctl reload nginx




sudo nginx -t







app.use(express.static(path.join(__dirname, '..', 'vmarg', 'dist')));








app.get('*', (req, res) => {
  res.sendFile(path.resolve(__dirname, '..', 'vmarg', 'dist', 'index.html'));
});






upstream backend {
    server 127.0.0.1:13001;  # First container exposed on host port 13001
    server 127.0.0.1:13002;  # Second container exposed on host port 13002
  server 127.0.0.1:13003;  # Second container exposed on host port 13002
}

server {
    listen 80;
    listen [::]:80;

    server_name _;  # Catch-all for all hostnames/IPs

    location / {
        proxy_pass http://backend;  # Forward requests to the "backend" upstream

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;

        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}


//ec2
docker pull manoj20002/deploymentimage:latest 
 manoj20002/deploymentimage:latest

 sudo docker run -d -p 13000:13000 manoj20002/deploymentimage 




//docker hub
 docker tag deploymentimage manoj20002/deploymentimage:latest

 docker build -t deploymentimage .

docker push manoj20002/deploymentimage:latest  
 
docker run -d --name redis-server -p 6379:6379 redis
docker exec -it redis-server redis-cli 



docker volume create mongodb_data
 docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -v mongodb_data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
  mongo
mongodb://admin:admin123@localhost:27017
upstream backend {
    server 127.0.0.1:12001;
    server 127.0.0.1:12002;
    server 127.0.0.1:12003;
    server 127.0.0.1:12004;
    server 127.0.0.1:12005;
}

    server {
         listen 80;
      listen [::]:80;

       server_name dev-vmarg.skoegle.com;

           location / {
        proxy_pass http://backend;

            proxy_http_version 1.1;
         proxy_set_header Upgrade $http_upgrade;
         proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
         proxy_cache_bypass $http_upgrade;

         proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
     }
}

nohup serve -s dist -l 3000 > serve.log 2>&1 &
kill %1
kill -9 %1

