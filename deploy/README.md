# Deployment scripts to deploy OpenTSDB

Some automation to help setup the system

# Steps

- yum install gnuplot
- get opentsdb package
- rpm install

- setup opentsdb.conf
  - this is also the place for other extra configs
- setup jaas.conf
- update /bin/tsdb
  - replace: JVMARGS=${JVMARGS-'-enableassertions -enablesystemassertions'}
  - with: JVMARGS=${JVMARGS-'-Djava.security.krb5.conf=/etc/krb5.conf -Dhbase.security.authentication=kerberos -Dhbase.kerberos.regionserver.principal=hbase/_HOST@{{ krb_domain }} -Dhbase.rpc.protection=authentication -Dhbase.sasl.clientconfig=Client -Djava.security.auth.login.config=/etc/opentsdb/jaas.conf -enableassertions -enablesystemassertions'}
- run create table
  - make sure the user that is kinited has table creation rights
    - env COMPRESSION=NONE HBASE_HOME=/opt/cloudera/parcels/{{ cdp_version }}/ /usr/share/opentsdb/tools/create_table.sh
- start daemon

# Yet to figure out

### For full kinda prodish version= 

FreeIPA demo only
- opentsdb service user
- kerberos for opentsdb user - keytab instead?


- a service config for auto start stop
See: https://groups.google.com/g/opentsdb/c/-phl3X2LUik
```
[Unit]
Description=OpenTSDB

[Service]
Type=simple
User=opentsdb
Group=opentsdb
ExecStart=tsdb tsd

[Install]



```
