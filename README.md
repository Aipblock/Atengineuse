# Atengineuse
Backup

## Install
```ssh
wget http://tengine.taobao.org/download/tengine-2.3.3.tar.gz
```
```ssh
tar -zxvf tengine-2.3.3.tar.gz
```
```ssh
cd tengine-2.3.3
```
### then install ev
```ssh
apt-get install libpcre3 libpcre3-dev
```
```ssh
apt-get install openssl libssl-dev
```
```ssh
apt-get install zlib1g-dev
```

```ssh
apt-get install gcc
```
```ssh
apt-get install build-essential
```

## Make
### config
```ssh
./configure --with-http_realip_module --without-http_upstream_keepalive_module --with-stream --with-stream_ssl_module --with-stream_sni --add-module=modules/ngx_http_upstream_* --add-module=modules/ngx_debug_* --add-module=modules/ngx_http_slice_module --add-module=modules/ngx_http_user_agent_module --add-module=modules/ngx_http_reqstat_module --add-module=modules/ngx_http_proxy_connect_module --add-module=modules/ngx_http_footer_filter_module
```
```ssh
make && make install
```

## Service
### link
```ssh
ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx
```
### nano
```ssh
nano /lib/systemd/system/nginx.service
```
```
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking 
PIDFile=/usr/local/nginx/logs/nginx.pid
# Nginx will fail to start if /run/nginx.pid already exists but has the wrong SELinux context. This might happen when running `nginx -t` from the 
# cmdline. https://bugzilla.redhat.com/show_bug.cgi?id=1268621
ExecStartPre=/usr/bin/rm -f /usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/bin/nginx -t
ExecStart=/usr/bin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/bin/kill -s HUP
$MAINPID KillSignal=SIGQUIT
TimeoutStopSec=5
KillMode=process
PrivateTmp=true 

[Install]
WantedBy=multi-user.target
```
## Use
### first
```ssh
systemctl daemon-reload
systemctl enable nginx.service
systemctl start nginx.service
```
### other
#### restart
```ssh
systemctl restart nginx
```
#### stop
```ssh
systemctl stop nginx
```
#### hot refresh
```ssh
nginx -s reload
```

## Configuration file path
> /usr/local/nginx/conf/nginx.conf




