# Setup
All config files and data for the containers will be stored in /config. Everything will run as UID 2001 to ensure no issues between the various containers

1. mkdir /config
1. adduser --system --no-create-home --uid 2001 --group media
1. chown -R media.media /config
1. docker network create utils_default
1. docker-compose up -d
