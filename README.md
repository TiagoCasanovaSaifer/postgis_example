# PostGIS Example App

A (simple) Rails example app using PostGIS

## Migrations

There are three intersting points in the migrations:

  * How you just `t.<choose-one-geom> :name`

  * SRID 4326, more info [here](http://en.wikipedia.org/wiki/World_Geodetic_System)

  * `:spatial => true` on an add\_index

## Models

### Mark (Point)

A simple X,Y Point.


### Path (Line String)

In other words, an array of points..


### Region (Polygon)

Put it simple, an array of points but a\[0\] should eql a\[-1\] (careful with rings)


## PostgreSQL

Creating an UTF8 database (optional, you might have one)

    # export PGROOT="/var/lib/postgres"
    # mkdir -p $PGROOT/data && chown postgres.postgres $PGROOT/data
    # su - postgres -c "/usr/bin/initdb -E utf8 --locale=en_US.UTF-8 $PGROOT/data"


## PostGIS

Installing PostGIS is simple:

    # su - postgres
    $ psql template1

    template1=# create database template_postgis with template = template1;
    template1=# UPDATE pg_database SET datistemplate = TRUE where datname = 'template_postgis';
    template1=# \c template_postgis
    template_postgis=# CREATE LANGUAGE plpgsql;


Now just find the right path for your OS, some that I know:

* Linux Arch

        template_postgis=# \i /usr/share/postgresql/contrib/postgis.sql;
        template_postgis=# \i /usr/share/postgresql/contrib/spatial_ref_sys.sql;

* Centos x86\_64

        template_postgis=# \i /usr/share/pgsql/contrib/lwpostgis-64.sql;
        template_postgis=# \i /usr/share/pgsql/contrib/spatial_ref_sys.sql;

* Debian

        template_postgis=# \i /usr/share/postgresql-8.3-postgis/lwpostgis.sql;
        template_postgis=# \i /usr/share/postgresql-8.3-postgis/spatial_ref_sys.sql;

* Ubuntu (10.4) with postgresql 8.4

        template_postgis=# \i /usr/share/postgresql/8.4/contrib/postgis.sql;
        template_postgis=# \i /usr/share/postgresql/8.4/contrib/spatial_ref_sys.sql;

* Ubuntu (10.10) with postgresql 8.4

        template_postgis=# \i /usr/share/postgresql/8.4/contrib/postgis-1.5/postgis.sql;
        template_postgis=# \i /usr/share/postgresql/8.4/contrib/postgis-1.5/spatial_ref_sys.sql;

* Mac OS X

        template_postgis=# \i /opt/local/postgis/lwpostgis.sql;
        template_postgis=# \i /opt/local/postgis/spatial_ref_sys.sql;

* Mac OS X with postgresql 9+ and postgist installed via homebrew

    postgis 1.5.2:

        template_postgis=# \i /usr/local/Cellar/postgis/1.5.2/share/postgis/postgis.sql;
        template_postgis=# \i /usr/local/Cellar/postgis/1.5.2/share/postgis/spatial_ref_sys.sql;

    postgis 1.5.3:

        template_postgis=# \i /usr/local/Cellar/postgis/1.5.3/share/postgis/postgis.sql;
        template_postgis=# \i /usr/local/Cellar/postgis/1.5.3/share/postgis/spatial_ref_sys.sql;


Now to finish:

    GRANT ALL ON geometry_columns TO PUBLIC;
    GRANT ALL ON spatial_ref_sys TO PUBLIC;
    VACUUM FREEZE;


## FAQ

### I can\`t find postgis.sql / spatial\_ref\_sys.sql location. What should i do?

Well, it's depend on your system.

* Debian / Ubuntu

        $ dpkg -L postgresql-VERSION-postgis | egrep "(spatial_ref_sys|postgis).sql"

* Arch Linux (not tested, but should work)

        $ pacman -Ql postgis | egrep "(spatial_ref_sys|postgis).sql"

* CentOS / Fedora / Redhat / where installed yum ;) (not tested, but should work)

    Install _yum-utils_ which contains _repoquery_, then:

        $ repoquery --list PACKAGE | egrep "(spatial_ref_sys|postgis).sql"

* Mac OS X (homebrew)

        $ brew list postgis | egrep "(spatial_ref_sys|postgis).sql"


### I upgrade postgresql via homebrew on Mac OS X and something going wrong.

Reinstall postgis:

    $ brew uninstall postgis
    $ brew install postgis
