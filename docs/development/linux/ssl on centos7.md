# SSL on Centos 7

---

### <a name="frontend"></a> Frontend

---

1. Install apache http web server

```
yum install -y httpd mod_ssl
```

2. Optional: To change the port of apache
    * `cd /etc/httpd/conf/httpd.conf`
    * Append the following
    ``` 
    Listen 80
    <IfModule ssl_module>
    Listen 443
    </IfModule>
    ```

3. Start server

```
systemctl start httpd
systemctl enable httpd
```

4. Find the document root on your server

```
grep -ri DocumentRoot /etc/httpd
```

> You will find an address like
>
> `/etc/httpd/conf.d/ssl.conf:#DocumentRoot "/var/www/html"`

5. Paste your index.html file here

```
echo demo > /var/www/html/index.html
```

6. If this demos is not available at server:80

> Add this to rebootinstructions.sh as well

```
sudo /sbin/iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 80
```

8. Also change this in the `rebootinstructions.sh`

9. Change default document root
    * Edit httpd.conf file`vi /etc/httpd/conf/httpd.conf`
    * And change DocumentRoot “desired path”
    * Also change
      `<Directory "/home/webmaster/website/www">`

> i.e. change 2 occurrences of /var/www/html with your desired path

9. Give permission to webmaster

```
chmod 777 /home/webmaster
```

11. Restart httpd

```
Systemctl restart httpd
```

12. Stop existing pm2 task for hosting since it is not required now

```
pm2 kill
```

13. Add exception to the firewall (if you are using firewalld)

```
firewall-cmd –permanent –zone=public –add-service=http
firewall-cmd –permanent –zone=public –add-service=https
firewall-cmd –reload
```

> or Use ufw allow to open your ports 80 & 443

13. Install epel repository

```
yum install -y epel-release
```

14. Install cert bot application

```
yum install certbot -y
```

16. You can search for your server using (if you are not using apache)

```
yum search certbot
```

16. Install certbot for apache

```
yum install -y python2-certbot-apache
```

17. Generate ssl use –apache if you know your server else follow certbot

```
certbot --apache -d your-domain 
```

> if that doesn’t work

* first run

```
certbot certonly -d your-domain --webroot -w /home/webmaster/website/www
```

* Choose option (3: Place files in webroot directory (webroot))

* and then

```
certbot --apache -d your-domain
```

### Backend

---

For a https front end, all the backend calls should be https as well hence you need to serve back end with https as well

1. Go to `/etc/letsencrypt/live/yoursitefolder**`

2. Generate keystore using

```
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out keystore.p12 -name tomcat -CAfile chain.pem -caname root
```

3. Download this and Paste the generated keystore in `“src/main/resources/keystore”`

> Or to the jar, since we have used this location in softrade code

4. In application.properties paste the following

```
# The format used for the keystore. It could be set to JKS in case it is a JKS file
server.ssl.keyStoreType=PKCS12
# The path to the keystore containing the certificate
server.ssl.key-store=classpath:keystore/keystore.p12
# The password used to generate the certificate
server.ssl.key-store-password=<your-password-here>
# The alias mapped to the certificate
server.ssl.keyAlias=tomcat
```

> change `server.ssl.key-store-password=<your-password-here>` to your desired password 

> Also, you can use direct location such as server.ssl.key-store=/root/jar/keystore.p12

5. Disabling ssl on run time

```java
@Configuration
public class CustomContainer implements EmbeddedServletContainerCustomizer {

   @Autowired
   private SystemPrefService systemPrefService;
   
   @Override
   public void customize(ConfigurableEmbeddedServletContainer container) {
     if(!systemPrefService.isSSL()) {
         Ssl ssl = new Ssl();
         ssl.setEnabled(false);
         ssl.setKeyStore("");
         container.setSsl(ssl);
     }
   }
}
```

6. Step 4-5 have been done in code, simply change the `syspref isSSL = true`

* then restart the server

### Renew License

---

1. Run `certbot --apache -d your-domain`

2. Run following command in `/etc/letsencrypt/live/your-domain-folder`

```bash
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out keystore.p12 -name tomcat -CAfile chain.pem -caname root
```

3. Restart spring server

* Delete SSL

* `sudo certbot delete --cert-name <certname>`

* `yum remove -y httpd mod_ssl`

* re-install from [top of page](#frontend)
