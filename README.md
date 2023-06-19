# assignments
assignment-submission


Question 1)	Check if docker and docker-compose is installed on the system. If not present, install the missing packages.
Installing docker and:
•	Launching a ec2-instance & login into server
•	yum install docker -y
•	using above command installed docker 
•	usermod -aG docker ec2-user – docker added into ec2-user group
installing docker compose:
•	DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
•	mkdir -p $DOCKER_CONFIG/cli-plugins
•	curl -SL https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
apply executable permissions
•	chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
•	docker compose version – to check docker compose available or not



Question 6&7) Add another subcommand to enable/disable/delete the site (stopping/starting the containers)
•	created a project directory mkdir my-lemp-stack
•	cd my-lemp-stack
•	in this directory, empty file created using touch docker_delete.sh and given the execution permissions usinf chmod +x docker_delete.sh
•	./docker_delete.sh using this command executed the script
      
      
      #!/bin/bash
start_site() {
   echo "Starting containers for the site..."
   # Replace the following command with the appropriate command to start your containers
   docker compose up -d
}
stop_site() {
   echo "Stopping containers for the site..."
   # Replace the following command with the appropriate command to stop your containers
   docker compose down
}
delete_site() {
   echo "Deleting the site..."
   stop_site
   # Replace the following command with the appropriate command to delete local files/directories
   rm -rf /path/to/site
}
enable_site() {
   echo "Enabling the site..."
   start_site
}
disable_site() {
   echo "Disabling the site..."
   stop_site
}
# Check the provided subcommand and call the corresponding function
case "$1" in
   enable)
      enable_site
      ;;
   disable)
      disable_site
      ;;
   delete)
      delete_site
      ;;
   *)
echo "Usage: $0 {enable|disable|delete}"
      exit 1
esac
