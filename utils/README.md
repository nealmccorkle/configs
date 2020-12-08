# Setup
All config files and data for the containers will be stored in /config. Everything will run as UID 2001 to ensure no issues between the various containers

1. mkdir /config
2. adduser --system --no-create-home --uid 2001 --group media
3. chown -R media.media /config
4. docker network create media_default
5. docker network create utils_default
6. docker-compose up -d
