# GIS_OSM_Nominatim
Set up Nominatim server
## Installation  
https://nominatim.org/release-docs/latest/appendix/Install-on-Ubuntu-18/  

## Troubleshooting

Feb 22, 2020
### Apache is not starting up   

see https://askubuntu.com/questions/1053328/failed-to-start-the-apache-http-server  

sudo systemctl status apache2.service

● apache2.service - The Apache HTTP Server  
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)  
  Drop-In: /lib/systemd/system/apache2.service.d  
           └─apache2-systemd.conf  
   Active: failed (Result: exit-code) since Sat 2020-02-22 22:19:13 EST; 6min ago  
  Process: 3406 ExecStart=/usr/sbin/apachectl start (code=exited, status=1/FAILURE)  

Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: (98)Address already in use: AH00072: make_sock: could not bind to address [::]:80  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:80  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: no listening sockets available, shutting down  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: AH00015: Unable to open logs  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: Action 'start' failed.  
Feb 22 22:19:13 wk-XPS-8700 apachectl[3406]: The Apache error log may have more information.  
Feb 22 22:19:13 wk-XPS-8700 systemd[1]: apache2.service: Control process exited, code=exited status=1  
Feb 22 22:19:13 wk-XPS-8700 systemd[1]: apache2.service: Failed with result 'exit-code'.  
Feb 22 22:19:13 wk-XPS-8700 systemd[1]: Failed to start The Apache HTTP Server.  

--------------
sudo ss -ntlp 'sport = 80'  
State                                  Recv-Q                                  Send-Q                                                                    Local Address:Port                                                                   Peer Address:Port                                    
LISTEN                                 0                                       128                                                                             0.0.0.0:80                                                                          0.0.0.0:*                                 

/etc/init.d/lighttpd stop  
sudo systemctl restart apache2

===================== 
### Update nominatim database 3.1.0  

https://nominatim.org/release-docs/latest/admin/Import-and-Update/  

Update osm
pip3 install --user osmium

~/Nominatim-3.1.0/build$ vi ./settings/local.php  
add
@define('CONST_Pyosmium_Binary', '/srv/nominatim/.local/bin/pyosmium-get-changes')
./utils/update.php --init-updates  
./utils/update.php --import-osmosis-all  

=======================
Feb 23, 2020  
### postgresSQL account reset password and set privileges  
https://www.postgresql.org/docs/9.0/sql-alterrole.html  
