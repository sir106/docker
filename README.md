
# docker

Docker image for volkszaehler middleware and frontend


## Installing

To run this image simply pull the built version for your architecture from docker hub (https://hub.docker.com/r/volkszaehler/volkszaehler):

	docker pull volkszaehler/volkszaehler
	
For maximum flexibility, the image does NOT contain a database itself but expects a mysql database at host `mysql` with standard volkszaehler credentials:

      - MYSQL_ROOT_PASSWORD=volkszaehler
      - MYSQL_DATABASE=volkszaehler
      - MYSQL_USER=vz
      - MYSQL_PASSWORD=demo


## Running on x86

If not already running, start a mysql database using the volkszaehler credentials and get it's container id:

	DATABASE=$(docker run -d -e MYSQL_ROOT_PASSWORD=volkszaehler -e MYSQL_DATABASE=volkszaehler -e MYSQL_USER=vz -e MYSQL_PASSWORD=demo mysql)

**NOTE** Currently volkszaehler does not support specifying database connection parameters using environment variables. If different credentials or a non-dockerized mysql database or hostname are required, its parameters must be set in `src/volkszaehler.conf.php` and the image rebuilt.

Then the actual volkszaehler image can be started:

	docker run --link $DATABASE:mysql -p 8080:8080 -t volkszaehler/volkszaehler

This exposes both frontend and middleware at port `8080`. The database schema will automatically be created or updated.


## Running on Raspberry Pi

See section before but select the docker images for the ARM architecture:

	DATABASE=$(docker run -d -e MYSQL_ROOT_PASSWORD=volkszaehler -e MYSQL_DATABASE=volkszaehler -e MYSQL_USER=vz -e MYSQL_PASSWORD=demo --name mysql hypriot/rpi-mysql)

	docker run --link $DATABASE:mysql -p 8080:8080 -t volkszaehler/volkszaehler

This setup is already available by using the pre-composed `docker-compose.yml` configuration consisting of MySQL database and volkszaehler runtime:

	docker-composer up

**NOTE** To persist the database, update `docker-compose.yml` and make sure that the MySQL data folder is mapped to a folder on the docker host machine.


## Building

Simply run the build script:

	./build.sh

