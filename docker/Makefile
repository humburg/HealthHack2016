# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

# Image name can be overidden with env vars.
DOCKER_IMAGE ?= humburg/healthhack-2016

# Get the latest commit.
GIT_COMMIT = $(strip $(shell git rev-parse --short HEAD))

# Get the version number from the code
CODE_VERSION = $(strip $(shell git describe --exact-match 2> /dev/null || echo "`git symbolic-ref HEAD 2> /dev/null | cut -b 12-`-`git log --pretty=format:\"%h\" -1`"))

# Find out if the working directory is clean
GIT_NOT_CLEAN_CHECK = $(shell git status --porcelain)
ifneq (x$(GIT_NOT_CLEAN_CHECK), x)
DOCKER_TAG_SUFFIX = "-dirty"
endif

# Use the version number as the release tag.
DOCKER_TAG = $(CODE_VERSION)

# Additional checks before pushing to Docker Hub
ifeq ($(MAKECMDGOALS),release)
# Don't push to Docker Hub if this isn't a clean repo
ifneq (x$(GIT_NOT_CLEAN_CHECK), x)
$(error echo You are trying to release a build based on a dirty repo)
endif
endif

docker_build:
	# Build Docker image
	docker build \
  --label org.label-schema.build-date=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
  --label org.label-schema.version=$(CODE_VERSION) \
  --label org.label-schema.vcs-url=`git config --get remote.origin.url` \
  --label org.label-schema.vcs-ref=$(GIT_COMMIT) \
	-t $(DOCKER_IMAGE):$(DOCKER_TAG) .

docker_push:
	# Tag image as latest
	docker tag $(DOCKER_IMAGE):$(DOCKER_TAG) $(DOCKER_IMAGE):latest

	# Push to DockerHub
	docker push $(DOCKER_IMAGE):$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):latest

output:
	@echo Docker Image: $(DOCKER_IMAGE):$(DOCKER_TAG)