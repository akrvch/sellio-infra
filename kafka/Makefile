CHART_NAME=kafka
NAMESPACE=sellio-data
ENV?=prod
VALUES=../envs/$(ENV)/values-$(CHART_NAME).yaml

.PHONY: add-repos deps install upgrade diff uninstall

add-repos:
	helm repo add redpanda https://charts.redpanda.com || true
	helm repo update

deps:
	helm dependency update || true

install:
	helm upgrade --install $(CHART_NAME) . -n $(NAMESPACE) -f $(VALUES)

upgrade:
	helm upgrade --install $(CHART_NAME) . -n $(NAMESPACE) -f $(VALUES)

diff:
	@if helm plugin list | grep -q diff; then \
		helm diff upgrade $(CHART_NAME) . -n $(NAMESPACE) -f $(VALUES); \
	else \
		echo "helm-diff not installed; showing dry-run"; \
		helm upgrade --install $(CHART_NAME) . -n $(NAMESPACE) -f $(VALUES) --dry-run --debug; \
	fi

uninstall:
	helm uninstall $(CHART_NAME) -n $(NAMESPACE) || true


