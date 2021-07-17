FROM ubuntu:latest

#Install Cron
RUN apt-get update
RUN apt-get -y install cron


# Run the command on container startup
CMD cron && tail -f /var/log/cron.log