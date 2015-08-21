owncloud
========
( inspired from [ReadMe](http://registry.hub.docker.com/u/jchaney/owncloud) from [jchaney](http://hub.docker.com/u/jchaney) )

## Details

This ownCloud 7 configuration was built to run on top of Nginx & you can choose PostgreSQL, MySQL, or SQLite for the Database

## Database

For a super easy setup, I would recommend using SQLite.. but if you need more performance, I would recommend linking to the “trusted” postgresql image.

Before starting ownCloud, fire up: 

    docker run --name postgresql -d postgres

Then link it to that database when you run ownCloud:

    docker run --link postgresql:db ...

## Options

Nginx Configuration

Nginx runs on 80 OR 443 depending on whether or not you pass env variables for SSL. Make sure you expose one of these ports.

    docker run -p 80:80 ...

## Persistent storage for datastore

Attach host volume to /var/www/owncloud/data

    docker run -v /home/jchaney/myfiles:/var/www/owncloud/data ...

## SSL support

Generate your own certificates:

    openssl genrsa -des3 -out <your path>/ssl/myssl.key 1024
    openssl req -new -key <your path>/ssl/myssl.key -out <your path>/ssl/myssl.crt

You will need to attach a host volume containing your SSL key and cert, and pass the path to those in env variables SSL_KEY and SSL_CERT

    docker run -v /home/jchaney/ssl:/root/ssl -e "SSL_KEY=/root/ssl/myssl.key" -e "SSL_CERT=/root/ssl/myssl.crt" ...

## How to run
This is a NO SSL example:

    docker run -h <host.domain> -p 80:80 --name nginx80 --link postgresql:db -v <your path>/files:/var/www/owncloud/data -d giodegas/owncloud
    
This is a full example, utilizing all options. But feel free to remove what you don't want.

    docker run -h my.domain.com -p 443:443 --name nginx443 --link postgresql:db -v <your path>/files:/var/www/owncloud/data -v <your path>/ssl:/root/ssl -e "SSL_KEY=/root/ssl/myssl.key" -e "SSL_CERT=/root/ssl/myssl.crt" giodegas/owncloud

## Note

Make sure to pass the -h option to Docker run with your FQDN if you want webdav to work. See full example above, under “how to run”.

## Post-Run Instructions

NOTE: Make sure you use the final hostname/FQDN you plan on accessing your ownCloud server with when connecting and doing initial configuration below. ownCloud will take the URL used to access the Installation Wizard and insert that into the config.php file for the ‘trusted_domains’ setting. Users will only be able to log into ownCloud when they point their browsers to a domain name listed in the ‘trusted_domains’ setting. An IPv4 address can be specified instead of a domain name.

Head to 

    http://hostname/owncloud or https://hostname/owncloud
Choose your own username and password
* db username is postgres (if you used the postgres image mentioned previously)
* db password can be left blank (unless you changed this before hand)
* db name can be whatever you want
use the database alias as the hostname, from my example you would use: db

## Resources

http://owncloud.org/
