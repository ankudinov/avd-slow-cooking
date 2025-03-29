CURRENT_DIR := $(shell pwd)

.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: example-l3ls
example-l3ls: ## copy AVD single DC l3ls example to the current directory
	cp -r /home/avd/.ansible/collections/ansible_collections/arista/avd/examples/single-dc-l3ls/* $(CURRENT_DIR)

.PHONY: install-yq
install-yq: ## Install yq tool
	@sudo wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/bin/yq
	@sudo chmod +x /usr/bin/yq

.PHONY: git-init
git-init: ## init new git repo
	wget https://raw.githubusercontent.com/ankudinov/avd-slow-cooking/refs/heads/master/.gitignore
	git init
	git config --global user.email "avd@lab.net"
	git config --global user.name "AVD lab user"
	git add .
	git commit -m .
