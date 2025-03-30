CURRENT_DIR := $(shell pwd)
TIMESTAMP := $(shell date +"%s")

.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: update
update: ## update Makefile
	wget https://raw.githubusercontent.com/ankudinov/avd-slow-cooking/refs/heads/master/tools/Makefile\?timestamp\=$(TIMESTAMP) -O Makefile

.PHONY: install-yq
install-yq: ## Install yq tool
	@sudo wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/bin/yq
	@sudo chmod +x /usr/bin/yq

.PHONY: example-l3ls-install
example-l3ls-install: ## copy AVD single DC l3ls example to the current directory
	cp -r /home/avd/.ansible/collections/ansible_collections/arista/avd/examples/single-dc-l3ls/* $(CURRENT_DIR)

.PHONY: example-l3ls-rm-groups
example-l3ls-rm-groups: ## delete some groups in exaple inventory
	yq -i '.all.children.FABRIC.children.DC1.children.DC1_SPINES.vars.type = "spine"' inventory.yml
	yq -i '.all.children.FABRIC.children.DC1.children.DC1_L3_LEAVES.vars.type = "l3leaf"' inventory.yml
	yq -i '.all.children.FABRIC.children.DC1.children.DC1_L2_LEAVES.vars.type = "l2leaf"' inventory.yml
	rm group_vars/DC1_SPINES.yml
	rm group_vars/DC1_L3_LEAVES.yml
	rm group_vars/DC1_L2_LEAVES.yml
