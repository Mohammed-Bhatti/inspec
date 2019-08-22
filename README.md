# inspec

## Clone the repo
    git clone https://github.com/CrunchyData/crunchy-containers-private.git
  
## Set environment variables
        export CCP_BASEOS=centos7
        export CCP_PGVERSION=11
        export CCP_PG_FULLVERSION=11.4
        export CCP_VERSION=2.4.1
        export CCP_IMAGE_PREFIX=crunchydata 
        export CCP_IMAGE_TAG=$CCP_BASEOS-$CCP_PG_FULLVERSION-$CCP_VERSION

## Start the container
        cd crunchy-containers-private-master/examples/docker/primary
        sudo systemctl start docker
        ./run.sh
        
## Login to psql from localhost
        psql -h localhost -U testuser -W userdb
        
## Login to the container as root and install relevant packages
        docker exec -u 0 -it <container_name_or_id> /bin/bash
        yum install net-tools git ssh -y
        yum install https://packages.chef.io/files/stable/inspec/4.12.0/el/7/inspec-4.12.0-1.el7.x86_64.rpm
        
## Create the attributes.yml file
        pg_owner: 'postgres'
        pg_group: 'postgres'
        pg_owner_password: 'password'

        pg_dba: 'postgres'
        pg_dba_password: 'password'

        pg_user: 'testuser'
        pg_user_password: 'password'

        pg_host: '127.0.0.1'
        pg_port: '5432'

        pg_db: 'userdb'
        pg_table: ''

        # priv user account that can login to the postgres host
        login_user: ''
        login_host: ''

        pg_version: '11.4'

        pg_data_dir: "/pgdata/primary"
        pg_conf_file: "/pgdata/primary/postgresql.conf"
        pg_user_defined_conf: "/pgdata/primary/stig-postgresql.conf"
        pg_hba_conf_file: "/pgdata/primary/pg_hba.conf"
        pg_ident_conf_file: "/pgdata/primary/pg_ident.conf"

        pg_shared_dirs: [
          "/usr/pgsql-11.4",
          "/usr/pgsql-11.4/bin",
          "/usr/pgsql-11.4/lib",
          "/usr/pgsql-11.4/share"
          ]

        pg_conf_mode: '0600'
        pg_ssl: 'on'
        pg_log_dest: 'syslog'
        pg_syslog_facility: ['local0']
        pg_syslog_owner: 'postgres'

        pgaudit_log_items: ['ddl','role','read','write']
        pgaudit_log_line_items: ['%m','%u','%c']

        pg_superusers: [
          'postgres',
          ]

        pg_users: [
          '',
          ]

        pg_replicas: [
          '',
          ]

        pg_max_connections: '100'

        pg_timezone: 'UTC'
        
 ## Run a sample inspec report
        inspec exec pgstigcheck-inspec --attrs attributes.yml | pgstigcheck-inspec/tools/ansi2html.sh --bg=dark > inspec-report.html
        
 # STIG version
 ## Install cerstrap
        git clone https://github.com/square/certstrap
        cd certstrap
        ./build
        
 ## Set the PATH
        export PATH=$PATH:/home/admin/go/bin
        
## Start the container
        cd crunchy-containers-private-master/examples/docker/stig
        ./run.sh
