sauerworld
==========

## SDoS Dev Stack ##

The SDoS stack consists of 3 components:

- sdos-site - the front-end website
- storage - the storage layer
- auth-server - auth server implementation

## Installing ##

### Dependencies ###

The SDoS stack needs java, clojure, leiningen and immutant

If you do `java -version` it should be HotSpot 1.7.something, and
ideally be the server build. If this is not the case, follow the
instructions for your OS to install Oracle Java 1.7, server version.

For the other dependencies, we are fortunate in that leiningen will
take care of all of them. (leiningen is the standard dependency and
management tool for clojure projects).

**Install leiningen**

Follow the instructions here:
[https://github.com/technomancy/leiningen](https://github.com/technomancy/leiningen).

**Installing everything else**

The first time you run `lein` it will download clojure, so all that's
left is immutant.

Follow the instructions here: [http://immutant.org/install/](http://immutant.org/install/).

### Installation ###

First, clone all 3 repos.

```sh
git clone https://github.com/sauerworld/storage.git
git clone https://github.com/sauerworld/sdos-site.git
git clone https://github.com/sauerworld/auth-server.git
```

Download the demo database file from the sauerbraten repo and put it
into the correct resources directory.

```sh
wget https://github.com/sauerworld/sauerworld/raw/master/resources/demo.h2.db
mkdir storage/resources
mkdir storage/resources/db
mv demo.h2.db storage/resources/db/main.h2.db
```

The database is an H2 flat file, and needs to be called `main.h2.db`
and located in the `resources/db` folder in the storage project.

Now, set up the immutant deployments.

```sh
cd storage
lein immutant deploy
cd ../sdos-site
lein immutant deploy
cd ../auth-server
lein immutant deploy
```

You are now good to go.

## Running ##

At present, not much configuration is supported. To run, do:

```sh
lein immutant run
```

The frontend site should now come up at `localhost:8080`.

The auth server should be running at `127.0.0.1:28787`.

The demo database comes with some sample web users.

**Admin User**
Login/password: admin

**Normal User**
Login/password: test

## Site CSS ##

The site's CSS is built using SCSS. The sources are in
`sdos-site/assets`. To work with this, follow the instructions for
your OS to install SASS. After editing the SCSS files, compile the app
CSS file with:

```sh
scss sdos-site/assets/app.scss sdos-site/resources/public/css/app.css
```

Or you can have it automatically rebuild the app CSS file whenever the
SCSS changes as you're working with:

```sh
scss --watch sdos-site/assets/app.scss:sdos-site/resources/public/css/app.css
```

## Testing Auth Server ##

The demo database users have the following auth key pairs:

admin:
  - Private key: 'f373de2d49584e7a16166e76b1bb925f24f0130c63ac9332'
  - Public key: '+2c1fb1dd4f2a7b9d81320497c64983e92cda412ed50f33aa'

test:
  - Private key: '0978245f7f243e61fca787f53bf9c82eabf87e2eeffbbe77'
  - Public key: '-afe5929327bd76371626cce7585006067603daf76f09c27e'
