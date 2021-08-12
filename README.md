# Guacamole-Install-Notes

## Install Guacamole
Download the installer script.
>wget https://raw.githubusercontent.com/MysticRyuujin/guac-install/master/guac-install.sh

Make the installer script executeable.
>chmod +x guac-install.sh

Run the installer.
>./guac-install.sh

## Install NGINX
Install nginx.
>apt install nginx

Enable the service nginx.
>systemctl enable nginx

Generate a new certificate with a valid day lenght of 1825 days.
>openssl req -x509 -nodes -days 1825 -newkey rsa:4096 -keyout /etc/ssl/private/guacamole-selfsigned.key -out /etc/ssl/certs/guacamole-selfsigned.crt

Download the nginx config example and save it under the nginx configuration directory.
>wget https://raw.githubusercontent.com/KennethDhoe/Guacamole-Nginx/main/nginx-guacamole-ssl >> /etc/nginx/sites-available/nginx-guacamole-ssl

Generate Deffie-Hellman certificate to ensure a secured key exchange
>openssl dhparam -dsaparam -out /etc/nginx/dhparam.pem 4096

Link our configuration to the nginx sites-enabled directory
>ln -s /etc/nginx/sites-available/nginx-guacamole-ssl /etc/nginx/sites-enabled/

Edit our server.xml file from tomcat9 to make sure our source ip is available in tomcat.
>nano /etc/tomcat9/server.xml

add line \<Valve className="org.apache.catalina.valves.RemoteIpValve"/> on top of the end tag  </host>
>\<Valve className="org.apache.catalina.valves.RemoteIpValve"/>

The result should look like this:

       <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
        <Valve className="org.apache.catalina.valves.RemoteIpValve"/>
       </Host>
      </Engine>
     </Service>
    </Server>

Restart the tomcat service
>systemctl restart tomcat9.service

## Install black-theme

Download the black theme jar file and save it under the guacamole extensions folder.
>wget https://raw.githubusercontent.com/KennethDhoe/Guacamole-Dark/e10efcd8b8af00afb9e34b099009ca296ab789e4/branding.jar >> /etc/guacamole/extensions/branding.jar

Restart the Guacmole service.
>systemctl restart guacd

Restart the Tomcat service.
>systemctl restart tomcat9
