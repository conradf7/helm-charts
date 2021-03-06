SHELL := /bin/bash
NAME-SENSE:=sense
CHART-SENSE := .

include ../output.mk

wsa-sense:
	$(eval WAITERSACHECK-SENSE=$(shell kubectl get sa waiter-sa | tail -n +2 | awk '{if ($$1=="waiter-sa") print "--set=global.waiter.service_account.create=false"}'))
	echo $(WAITERSACHECK-SENSE)

.PHONY: catalog
catalog: wsa-sense
	helm install $(NAME-SENSE) catalog $(WAITERSACHECK-SENSE) -f ../global.yaml

.PHONY: dashboard
dashboard: wsa-sense
	helm install $(NAME-SENSE) dashboard $(WAITERSACHECK-SENSE) -f ../global.yaml

.PHONY: slo
slo: wsa-sense
	helm install $(NAME-SENSE) slo $(WAITERSACHECK-SENSE) -f ../global.yaml

clean-sense:
	rm -f ./charts/*

package-sense: clean-sense
	echo "target hit package-sense"
	helm dep up .

template-sense: package-sense $(BUILD_NUMBER_FILE)
	mkdir -p $(OUTPUT_PATH)
	helm template $(NAME-SENSE) . $(WAITERSACHECK-SENSE)  -f ../global.yaml > $(OUTPUT_PATH)/helm-$(NAME-SENSE)$(BN).yaml

.PHONY: sense
sense: package-sense wsa-sense
	helm install $(NAME-SENSE) . $(WAITERSACHECK-SENSE) -f ../global.yaml --timeout 10m --wait

.PHONY: remove-sense
remove-sense:
	helm uninstall $(NAME-SENSE)
