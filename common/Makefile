KIND_CLUSTER_NAME ?= kind
NODEPORT_START    ?= 30000
NODEPORT_END      ?= 30010
KIND_CONFIG       ?= cluster-config.yaml

.PHONY: kind-up kind-down kind-config

## Generate kind config with NodePort range
kind-config:
	@echo "Generating $(KIND_CONFIG) with ports $(NODEPORT_START)-$(NODEPORT_END)"
	@echo "kind: Cluster" > $(KIND_CONFIG)
	@echo "apiVersion: kind.x-k8s.io/v1alpha4" >> $(KIND_CONFIG)
	@echo "nodes:" >> $(KIND_CONFIG)
	@echo "- role: control-plane" >> $(KIND_CONFIG)
	@echo "  extraPortMappings:" >> $(KIND_CONFIG)
	@for port in $$(seq $(NODEPORT_START) $(NODEPORT_END)); do \
		echo "  - containerPort: $$port" >> $(KIND_CONFIG); \
		echo "    hostPort: $$port" >> $(KIND_CONFIG); \
		echo "    protocol: TCP" >> $(KIND_CONFIG); \
	done

## Create kind cluster
kind-up: kind-config
	@kind delete cluster --name $(KIND_CLUSTER_NAME) || true
	@kind create cluster --name $(KIND_CLUSTER_NAME) --config $(KIND_CONFIG)

## Delete kind cluster
kind-down:
	@kind delete cluster --name $(KIND_CLUSTER_NAME) || true
	@rm -rf $(KIND_CONFIG)
