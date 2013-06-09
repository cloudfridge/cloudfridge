
Archiver
==

Setup and management of the sMAP Archiver and Time-Series Database readingdb.

Installation was done from source repositories following the instructions:
http://www.cs.berkeley.edu/~stevedh/smap2/archiver_manual.html#archiver-install-manual

The following components need to be installed:
- ReadingDB (time-series database)
- PostgreSQL (metadata storage)
- powerdb2 (administration front-end)
- Archiver (the actual service)

Postgres DB access
===

As administrator:

$ psql -p 5432 -h localhost -U postgres
Password for user postgres: <Ducati996>
psql (9.1.9)
SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
Type "help" for help.

postgres=# 

As user archiver owner of DB archiver used in sMAP:

$ psql -p 5432 -h localhost -U archiver -W archiver
Password for user archiver: <Ducati996>
psql (9.1.9)
SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
Type "help" for help.

archiver=> 


To reload the configuration:
`sudo /etc/init.d/postgresql reload`


ARD Configuration Front-End
===

http://service.cometa.io/admin

Username: ubuntu
Password: Ducati996

http://service.cometa.io/admin/smap/subscription/add

Subscription added:
Description: Cloudfridge test
Key: WGbzpRDwaWX60dGrIsk8RywWwHlQdG29R24B
Can view: ubuntu

POST http://service.cometa.io:8079/add/WGbzpRDwaWX60dGrIsk8RywWwHlQdG29R24B

[{"/A/true_energy": {"Readings": [[1323283884000, 828.1]], "uuid": "d5912eb5-67af-5f54-a5fd-5642416c6bfd", "Properties": {"Timezone": "America/Los_Angeles", "UnitofMeasure": "kWh", "ReadingType": "double"}}}]

select uuid where Path like '/sensor0'
select data before now limit 5 where uuid = "d24325e6-1d7d-11e2-ad69-a7c2fa8dba61"

select Metadata/SourceName where Path like '/sensor0'
select  data before now limit 5 where uuid = "d24325e6-1d7d-11e2-ad69-a7c2fa8dba61" and Properties/UnitofMeasure = "Watt"

Launching ARD
===

To manually launch :

from ~/smap-data/python

`sudo /usr/bin/python /usr/bin/twistd smap-archiver /etc/smap/archiver.ini`

do not demonize:
`~/smap-data/python$ sudo /usr/bin/python /usr/bin/twistd -n smap-archiver /etc/smap/archiver.ini`

As a `monit` task:

`$ cat /etc/monit/conf.d/archiver`

`check process archiver pidfile /var/run/archiver.pid
    start program = "/usr/bin/twistd --logfile=/var/log/archiver.log --pidfile=/var/run/archiver.pid smap-archiver /etc/smap/archiver.ini"
	stop program = "/bin/sh -c '/bin/kill $(cat /var/run/archiver.pid)'"`

`monit` may require a reboot after changing the configuration file.


Powerdb2
===

A Django front-end application.

To manually launch:

from ~/powerdb2

`$ sudo python manage.py runserver 0.0.0.0:80`

For some reason the HTTP server needs to be on port 80, not the default 8000.

Front-End
===

`http://service.cometa.io/admin/`

Administrator login:

Username: ubuntu
Password: Ducati996

Users that are not staff or superuser cannot login the adminstrative site.

`http://service.cometa.io/plot/?login=`

Very useful to search data and streams using metadata:

`http://service.cometa.io/status/`

The powerdb2 frontend has the ability to create different tree views of 
data based on metadata tags. This allows to "slice" your available data streams in different ways; 
for instance, to expose an instrumentation-centric view which shows which streams are originating from 
which instrument, or a spatial view, showing which streams come from which room.

A menu tree was created for Cloudfridge using the admin interface and replacing
the default "Instrumentation" tree.

For adding and editing menu trees check the documentation at:

`https://code.google.com/p/smap-data/wiki/SlicrApi`



