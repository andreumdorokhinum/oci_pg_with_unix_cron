# using linux crond service as a workaround for absence of scheduler extensions in OCI postgres

### ssh into a virtual machine that can connect to your pg instance (Oracle allows you to create free VMs, that are suitable here)
    ssh -p 22 username@host

### install a package providing a cron implementation, for example:
    sudo dnf install cronie

### enable the crond service
    sudo systemctl enable crond.service

### start the crond service
    sudo systemctl start crond.service

### check the status of the crond service
    sudo systemctl status crond.service

### install the psql utility that will communicate with your postgres server
    sudo dnf install postgresql

### set a password to your pg instance in an env variable for crond to use
    sudo vim /etc/environment
        pwd="your_very_secure_password"
        host="aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa-primary.postgresql.bb-ccccccccc-1.oc1.oraclecloud.com"
        db="db"
        uname="uname"

### edit cron jobs for local user
    crontab -e

### the following job will be executed every minute (* * * * *) and run the "SELECT VERSION();" query
    * * * * * PGPASSWORD="${pwd}" psql --host "${host}" --port 5432 --dbname "${db}" --username "${uname}" <<< "SELECT VERSION();"

### list cron jobs for local user
    crontab -l

### check the cron logs
    sudo vim /var/log/cron
