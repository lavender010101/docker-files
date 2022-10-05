## Download the image

```docker
docker push lavender010101/oracle-xe:tagname
```

## Running in a container
To run your Oracle Database image use the docker run command as follows:

```docker
docker run --name <container name> \
-p <host port>:1521 -p <host port>:5500 -p <host port>:2484\
-e ORACLE_SID=<your SID> \
-e ORACLE_PDB=<your PDB name> \
-e ORACLE_PWD=<your database passwords> \
-e INIT_SGA_SIZE=<your database SGA memory in MB> \
-e INIT_PGA_SIZE=<your database PGA memory in MB> \
-e INIT_CPU_COUNT=<cpu_count init-parameter> \
-e INIT_PROCESSES=<processes init-parameter> \
-e ORACLE_EDITION=<your database edition> \
-e ORACLE_CHARACTERSET=<your character set> \
-e ENABLE_ARCHIVELOG=true \
-e ENABLE_TCPS=true \
-v [<host mount point>:]/opt/oracle/oradata \
lavender010101/oracle-xe:tagname
```


## Parameters
### Variables
| Symbol   | Discription                                              |
|----------|----------------------------------------------------------|
| `--name` | The name of the container (default: auto generated).     |
| `-p`     | The port mapping of the host port to the container port.<br/>The following ports are exposed: 1521 (Oracle Listener), 5500 (OEM Express), 2484 (TCPS Listener Port if TCPS is enabled).|

### Environments

| Name | Discription |
| ---- | ----------- |
| ORACLE_SID| The Oracle Database SID that should be used (default: ORCLCDB).|
| ORACLE_PDB| The Oracle Database PDB name that should be used (default: ORCLPDB1).                                       |
| ORACLE_PWD| The Oracle Database SYS, SYSTEM and PDB_ADMIN password (default: auto generated).                           |
| INIT_SGA_SIZE| The total memory in MB that should be used for all SGA components (optional). Supported by Oracle Database 19.3 onwards.|
| INIT_PGA_SIZE| The target aggregate PGA memory in MB that should be used for all server processes attached to the instance (optional). Supported by Oracle Database 19.3 onwards.|
| INIT_CPU_COUNT| Specifies the number of CPUs available for Oracle Database to use. On CPUs with multiple CPU threads, it specifies the total number of available CPU threads (optional).|
| INIT_PROCESSES| Specifies the maximum number of operating system user processes that can simultaneously connect to Oracle Database. <br/>Its value should allow for all background processes such as locks, job queue processes, and parallel execution processes (optional). |
| AUTO_MEM_CALCULATION| To enable auto calculation of the DBCA total memory limit during the database creation, based on the available memory of the container, which can be constrained using the `docker run --memory` option. <br/>If set to 'false', the total memory will be set as 2GB (default: true). Note that this parameter is not taken into account if the `-e INIT_SGA_SIZE` or `-e INIT_PGA_SIZE` are set.Supported by Oracle Database 19.3 onwards. |
| ORACLE_EDITION| The Oracle Database Edition (enterprise/standard).Supported by Oracle Database 19.3 onwards.|
| ORACLE_CHARACTERSET| The character set to use when creating the database (default: AL32UTF8).|
| ENABLE_ARCHIVELOG| To enable archive log mode when creating the database (default: false).Supported by Oracle Database 19.3 onwards.|
| ENABLE_TCPS | To enable TCPS connections for Oracle Database.Supported by Oracle Database 19.3 onwards. |

### Volumns

| Path                          | Description                                                  |
| ----------------------------- | ------------------------------------------------------------ |
| `/opt/oracle/oradata`         | The data volume to use for the database.  <br/>Has to be writable by the Unix "oracle" (uid: 54321) user inside the container!<br/> If omitted the database will not be persisted over container recreation. |
| `/opt/oracle/scripts/startup` | Optional: A volume with custom scripts to be run after database startup.  <br/>For further details see the "Running scripts after setup and on startup" section below. |
| `/opt/oracle/scripts/setup`   | Optional: A volume with custom scripts to be run after database setup.   <br/> For further details see the "Running scripts after setup and on startup" section below. |

