# nginx-plus is an example base image located in debian/examples/nginx-plus
FROM nginx-plus

# Start container with environment variables for CTRL_HOST and API_KEY
# docker run --name apigw --hostname apigw -e CTRL_HOST=10.20.30.40 -e API_KEY=deadbeef -d -P nginx-ctrl

# Install everything we will need to install the Controller Agent so that the container can start quickly
RUN apt-get update &&\
        apt install -y \
        curl python gnupg2 procps dh-python distro-info-data libmpdec2 \
        libpython3-stdlib libpython3.5-minimal libpython3.5-stdlib \
        lsb-release python3 python3-minimal python3.5 python3.5-minimal \
        sudo

EXPOSE 80 443 8080
STOPSIGNAL SIGTERM

WORKDIR /controller

# This script will download install and start the agent using `service controller-agent start` command.
# Agent service should be stopped gracefully using `service controller-agent stop` command, which
# is not done in this example as a solution strictly depends on the form of cmd/entrypoint of the base image.
# Example solution for multiple service management in docker image could be found in official docker
# documentation:
# https://docs.docker.com/config/containers/multi-service_container
RUN printf "curl -skSL https://\$CTRL_HOST:8443/1.4/install/controller/ | bash -s - -y\n exec nginx -g 'daemon off;'" > start

CMD ["sh", "/controller/start"]

