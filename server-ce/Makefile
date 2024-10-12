# Makefile

MONOREPO_ROOT := ../
export MONOREPO_REVISION := $(shell git rev-parse HEAD)
export BRANCH_NAME ?= test#$(shell git rev-parse --abbrev-ref HEAD)
export OVERLEAF_BASE_BRANCH ?= ghcr.io/benediktbertsch/sharelatex:$(BRANCH_NAME)
export OVERLEAF_BASE_LATEST ?= ghcr.io/benediktbertsch/sharelatex
export OVERLEAF_BASE_TAG ?= ghcr.io/benediktbertsch/sharelatex-base:$(BRANCH_NAME)-$(MONOREPO_REVISION)
export OVERLEAF_BRANCH ?= ghcr.io/benediktbertsch/sharelatex:$(BRANCH_NAME)
export OVERLEAF_LATEST ?= ghcr.io/benediktbertsch/sharelatex
export OVERLEAF_TAG ?= ghcr.io/benediktbertsch/sharelatex:$(BRANCH_NAME)-$(MONOREPO_REVISION)
export OVERLEAF_DOCKER_PLATFORMS ?= linux/arm64/v8,linux/amd64

all: build-base build-community

build-base:
	cp .dockerignore $(MONOREPO_ROOT)
	docker buildx build \
	  --build-arg BUILDKIT_INLINE_CACHE=1 \
	  --progress=plain \
	  --file Dockerfile-base \
	  --platform $(OVERLEAF_DOCKER_PLATFORMS) \
	  --pull \
	  --push \
	  --cache-from $(OVERLEAF_BASE_LATEST) \
	  --cache-from $(OVERLEAF_BASE_BRANCH) \
	  --tag $(OVERLEAF_BASE_TAG) \
	  --tag $(OVERLEAF_BASE_BRANCH) \
	  $(MONOREPO_ROOT)


build-community:
	cp .dockerignore $(MONOREPO_ROOT)
	docker buildx build \
	  --build-arg BUILDKIT_INLINE_CACHE=1 \
	  --progress=plain \
	  --build-arg OVERLEAF_BASE_TAG \
	  --build-arg MONOREPO_REVISION \
	  --cache-from $(OVERLEAF_LATEST) \
	  --cache-from $(OVERLEAF_BRANCH) \
	  --file Dockerfile \
	  --platform $(OVERLEAF_DOCKER_PLATFORMS) \
	  --push \
	  --tag $(OVERLEAF_TAG) \
	  --tag $(OVERLEAF_BRANCH) \
	  $(MONOREPO_ROOT)


.PHONY: all build-base build-community
