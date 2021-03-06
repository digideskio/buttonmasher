# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
include ../Makefile.config

APPS = frontend backend visualizer loader

SOURCES := $(shell find . -name '*.yaml')
OBJECTS := $(addprefix .tmp/,$(SOURCES))

default:
	@echo "Deploy everything: make deploy"
	@echo "You will need to search and replace hybrid-cloud-rocks if you have your own project"

ez: $(addsuffix -ez,$(CLUSTER_REGIONS))

%-ez: %-vars
	kubectl config set-context $(cluster_name) \
		--cluster=$(cluster_context) \
		--user=$(cluster_context)

federation-deploy:
	cat federated/*.yaml | sed -e 's|FEDERATION_NAME|$(FEDERATION_NAME)|g' | kubectl --context=federation-cluster apply -f -

deploy: $(addsuffix -deploy,$(CLUSTER_REGIONS))

%-deploy: %-vars
	kubectl --context=$(cluster_context) apply -f deployment/

status: $(addsuffix -status,$(CLUSTER_REGIONS))
	kubectl --context=federation-cluster get svc
	kubectl --context=federation-cluster get clusters

%-status: %-vars
	kubectl --context=$(cluster_context) get deployments
	kubectl --context=$(cluster_context) get svc

demo-all: ez federation-deploy deploy
