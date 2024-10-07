
# Laragon config




## xdebug


```js

[xdebug]
zend_extension = xdebug
xdebug.mode=debug
xdebug.start_with_request=Yes
xdebug.client_port=9001
xdebug.remote_port=9001
xdebug.log_level=7
xdebug.idekey=VSCODE
xdebug.remote_enable=1
xdebug.remote_autostart=1
xdebug.client_host="127.0.0.1"

```
```js
.vscode
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9001
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9001
        }
    ]
}
```
```js
settings.json

"terminal.integrated.profiles.windows": {
    "Laragon": {
      "path": "C:\\laragon\\bin\\git\\bin\\bash.exe",
    }
  },
  "php.validate.executablePath": "C:\\laragon\\bin\\php\\php-8.1.10-Win32-vs16-x64\\php.exe",
  "terminal.integrated.env.windows": {
    "path": "C:\\laragon\\bin;C:\\laragon\\bin\\apache\\httpd-2.4.54-win64-VS16\\bin;C:\\laragon\\bin\\composer;C:\\laragon\\bin\\git\\bin;C:\\laragon\\bin\\git\\cmd;C:\\laragon\\bin\\git\\mingw64\\bin;C:\\laragon\\bin\\git\\usr\\bin;C:\\laragon\bin\\laragon\\utils;C:\\laragon\\bin\\mysql\\mysql-8.0.30-winx64\\bin;C:\\laragon\\bin\\nginx\\nginx-1.22.0;C:\\laragon\\bin\\ngrok;C:\\laragon\\bin\\nodejs\\node-v18;C:\\laragon\\bin\\notepad++;C:\\laragon\\bin\\php\\php-8.1.10-Win32-vs16-x64;C:\\laragon\\bin\\python\\python-3.10;C:\\laragon\\bin\\python\\python-3.10\\Scripts;C:\\laragon\\bin\\redis\\redis-x64-5.0.14.1;C:\\laragon\\bin\\telnet;C:\\laragon\\usr\\bin;C:\\Users\\Nam\\AppData\\Local\\Yarn\\config\\global\\node_modules\\.bin;C:\\Users\\Nam\\AppData\\Roaming\\Composer\\vendor\\bin;C:\\Users\\Nam\\AppData\\Roaming\\npm;;${env:PATH}"
  },


```
```js
frontend.test.conf

server {
    listen 80;
    listen 443 ssl;
    server_name mxhclient.test *.mxhclient.test;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    location /_next/webpack-hmr {
        proxy_pass http://localhost:3000/_next/webpack-hmr;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Enable SSL
    ssl_certificate "C:/laragon/etc/ssl/laragon.crt";
    ssl_certificate_key "C:/laragon/etc/ssl/laragon.key";
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
    ssl_prefer_server_ciphers on;
	
    charset utf-8;
	
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    location ~ /\.ht {
        deny all;
    }
}

```
```js
server.test.conf

server {
    listen 80;
    listen 443 ssl;
    server_name mxhserver.test *.mxhserver.test;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    location /socket.io/ {
        proxy_pass http://localhost:8080/socket.io/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    # Enable SSL
    ssl_certificate "C:/laragon/etc/ssl/laragon.crt";
    ssl_certificate_key "C:/laragon/etc/ssl/laragon.key";
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
    ssl_prefer_server_ciphers on;
	
    charset utf-8;
	
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
    location ~ /\.ht {
        deny all;
    }
}

```

```js
<VirtualHost *:8080>
    DocumentRoot "C:/laragon/www/mxhserver"
    ServerName mxhserver.test
    ServerAlias *.mxhserver.test

    
    ProxyRequests Off
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
    ProxyPass /wss ws://mxhserver.test:8080
	ProxyPassReverse /wss ws://mxhserver.test:8080

    <Directory "C:/laragon/www/mxhserver">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

<VirtualHost *:8443>
    DocumentRoot "C:/laragon/www/mxhserver"
    ServerName mxhserver.test
    ServerAlias *.mxhserver.test

    
    ProxyRequests Off
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/
    ProxyPass /wss ws://mxhserver.test:8080
	ProxyPassReverse /wss ws://mxhserver.test:8080

    <Directory "C:/laragon/www/mxhserver">
        AllowOverride All
        Require all granted
    </Directory>

    SSLEngine on
    SSLCertificateFile      C:/laragon/etc/ssl/laragon.crt
    SSLCertificateKeyFile   C:/laragon/etc/ssl/laragon.key
</VirtualHost>

```
