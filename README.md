Docker for Laravel
=======

# Table of Contents
1. [About](#about)
2. [Installation](#installation)
3. [Setup](#setup)
4. [Using Containers](#using)
5. [Using Selenium](#selenium)
6. [Further Info](#info)

<a href="about"></a>
## About

Docker for Laravel gives you everything you need to get started developing your Laravel application without the need for
Homestead or any other Virtual Machines on your local machine.

To use this package, all you will need to have is [Docker](https://docs.docker.com/) installed with **Docker Compose** 
(which requires a separate install for Linux systems).

There are 4 main containers:

| Service       | Description                                    |
| ------------- | ---------------------------------------------- |
| PHP           | Run the PHP code used in your project          |
| MySQL         | Create a database to store data                |
| Nginx         | Serve the site through localhost               |
| Selenium      | Run Javascript UI tests using headless browser |  

The PHP container includes:
- PHP 7.1
- Composer
- Node & NPM (including Yarn, Bower, Grunt & Gulp)
- Git
- Chrome & Chrome Driver (for headless browser support)

<a name="installation"></a>
## Installation

Download or clone the repository to your required location:

```
git clone https://github.com/SwipeAndTap/laravel-docker.git
```
<a name="setup"></a>
## Setup

### Adding Laravel Files

Place your Laravel project within the `code` folder which will be automatically linked to the `var/www` 
directory on the php service container.

Once this is done, all changes made to the files in this directory will be immediately reflected on the server.

### Database Settings

1. Copy the `.env.mysql.example` file and create a version with the name `.env.mysql`. Edit the following 3 fields to 
match your Laravel `.env` database configuration:
  - `MYSQL_DATABASE` : name of the database to be created
  - `MYSQL_USER` : name of the user with access to the database
  - `MYSQL_PASSWORD` : password of the user with access to the database

2. Update your Laravel `.env` file in the `code` directory to make sure that the host is changed to
`DB_HOST=mysql`

> IF USING GITHUB YOU SHOULD NEVER COMMIT YOUR ENVIRONMENT FILES. THE .env.mysql FILE WILL BE IGNORED BY DEFAULT

<a href="using"></a>
## Using Containers

### Start Containers

To start the containers, make sure that you are in the same location as the `docker-compose.yml` file and then run:

```
docker-compose up
```

> Stop the containers by pressing ctrl + c

If you would like to run the containers in the background, you can instead run:

```
docker-compose up -d
```

To stop background containers, from the same directory run:
```
docker-compose down
```

> The MySQL container caches data so that it isn't erased each time you recreate it.  
This can be viewed in `/storage/mysql` and manually deleted if needed.  

### Viewing site

The site can be viewed from `localhost:8888`.

> If the port is already in use by another service then this can be changed in the `docker-compose.yml` file under
under `services` > `web` > `ports`

### Access PHP Container

You can move into the php container by running:

```
docker-compose exec php bash
```
From here you can then perform the usual server commands such as artisan, composer, git and others.

<a href="selenium"></a>
## Using Selenium

The PHP container comes with Chrome and Chrome Driver installed so that the Selenium container can be used to run 
Javascript tests within a headless browser.

One of the main tools that is used in this way is Behat which works very well with Laravel for BDD tests.

You can set the required variables in your `behat.yml` file by making sure that the correct hosts are used in the 
following places:

- base_url: "http://web"
- wd_host: "http://browser:4444/wd/hub"

> Take a look at the [Behat documentation](http://behat.org/en/latest/guides.html) for more details on how to set up 
your environment correctly

<a href="info"></a>
## Further Info

To see all mentioned commands in more detail I would recommend looking at the [Docker Docs](https://docs.docker.com/) 
for any troubleshooting as well as some other tips and tricks to get you working smoothly with Docker.