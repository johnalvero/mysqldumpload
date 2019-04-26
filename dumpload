#!/bin/bash

# Test if running inside screen
if test -z "$STY";
then
	echo "Please run inside a screen session. Type 'screen -S <name>'"
	exit
fi

# Source Replica
OLDDB_HOST=
OLDDB_USER=
OLDDB_PASS=""

# Destination DB
NEWDB_HOST=
NEWDB_USER=
NEWDB_PASS=""

DBS="database1 database2"

# Dump and Pipe
for DB in $DBS
do
    time mysqldump -u$OLDDB_USER -p$OLDDB_PASS -h$OLDDB_HOST --verbose --single-transaction --quick --compress --databases $DB  | pv -pterabc -N inbound | dd obs=16384K | dd obs=16384K | dd obs=16384K | dd obs=16384K | pv -pterabc -N outbound | mysql -u$NEWDB_USER -p$NEWDB_PASS -h$NEWDB_HOST --compress
done

echo "Done"
touch /tmp/migrate.done
