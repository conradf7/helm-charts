SHELL := /bin/bash
mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
current_dir := $(notdir $(patsubst %/,%,$(dir $(mkfile_path))))

pwd_name := $(notdir $(PWD))

include ../output.mk

.PHONY: edge
edge:
	if [ "$(pwd_name)" == "edge" ]; then \
		echo "run in makefile directory"; \
		helm install edge . -f ../global.yaml; \
	else \
		echo "run in helm-charts top directory"; \
		helm install edge edge -f ../global.yaml; \
	fi

.PHONY: remove-edge
remove-edge:
	helm uninstall edge
	
template-edge: $(BUILD_NUMBER_FILE)
	mkdir -p $(OUTPUT_PATH)
	helm template edge . --set=global.environment=kubernetes  -f ../global.yaml > $(OUTPUT_PATH)/helm-edge$(BN).yaml
