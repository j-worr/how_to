### Legacy docker containers may not build properly on M1 mac. 

Add environmental variable to your machine. Also maybe makes slower:

`export DOCKER_DEFAULT_PLATFORM=linux/amd64`

This is discussed here:

https://stackoverflow.com/questions/65612411/forcing-docker-to-use-linux-amd64-platform-by-default-on-macos

If Dockerfile can be changed, maybe

`FROM --platform=linux/arm64 <image name>`
