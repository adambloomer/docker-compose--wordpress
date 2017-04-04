Docker-compose Wordpress
=======================

The files contained within this repo will allow you to spin up Wordpress via docker-compose with NGINX, SQL & PHP servers, a phpMyAdmin GUI (if you want it) and topping it all off nicely with a virtual host alias. The first time you run `$ docker-compose up` this will install wordpress in your `www` directory if you haven't already got a pre-existing intallation. It will also automatically populate your `wp-config.php` with the login information for your database based on the `docker-compose.yml` settings you specify. It will take a while for the containers all to boot and become in sync. Along the way you'll get a fair few errors and warnings but be patient and it will sort itself out. 

If you already have a Wordpress site that you want to convert to the docker server method then you either need to move your wordpress install to a `www` folder or edit the docker-compose.yml file to change the folder mappings.

I've purposefully not put in a build process (or populated the dev folder with anything) as this is literally meant to be purely for docker and not a Wordpress build boilerplate as we have one of those already.

file structure
------------------
The following file structure can be used, however you can tailor this to your preference using the mappings in the `docker-compose.yml` file in the `web:` - >  `volumes:` config block.

```
 - project_folder
   - www (wordpress install)
   - dev (all our precompiled code)
   - docker-configs
      - client_max_body_size.conf
      - uploads.ini
  docker-compose.yml
```
Virtual Hosts
------------------
In the `docker-compose.yml` file you'll find the virtual host alias in the `web` config block under:

```
environment:
	VIRTUAL_HOSTS: virtual-host-name.dev
```

Once you've changed this to your preference, edit your system `.hosts` file and add the virtual host alias next to the localhost IP like so:

`127.0.0.1  virtual-host-name.dev`

Restart your containers and `http://virtual-host-name.dev` should point to your Wordpress install in your browser.

File upload limits
------------------

By default the NGINX server has a maximum file transfer limit of 1Mb which isn't very helpful when using a content management system. There is a two step process to get this to work but the files in this repo will automate that for you.

The `uploads.ini` file is a PHP configuration file and is mapped via the `web` config block.

The `client_max_body_size.conf` is an NGINX configuration file and is mapped into the NGINX container via the `nginx`  docker-compose configuration block.

I have set this to default to 64Mb but that's probably quite generous. 


Connecting to the database
--------------------------

I've included phpMyAdmin for easy setup and access to your database but you can easily remove the whole phpMyAdmin block and connect via Sequel Pro (other database GUI's are available).

To connect via Sequel pro it's simply a case of connecting to host `127.0.0.1` with username of `root` and your database credentials as set in the `mysql:` config block in `docker-compose.yml`