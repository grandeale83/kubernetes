# Copyright 2016 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FONT := DroidSansMono.ttf

PNGS := $(patsubst %.seqdiag,%.png,$(wildcard *.seqdiag))

.PHONY: all
all: $(PNGS)

.PHONY: watch
watch:
	fswatch *.seqdiag | xargs -n 1 sh -c "make || true"

$(FONT):
	curl -sLo $@ https://googlefontdirectory.googlecode.com/hg/apache/droidsansmono/$(FONT)

%.png: %.seqdiag $(FONT)
	seqdiag --no-transparency -a -f '$(FONT)' $<

# Build the stuff via a docker image
.PHONY: docker
docker:
	docker build -t clustering-seqdiag .
	docker run --rm clustering-seqdiag | tar xvf -

.PHONY: docker-clean
docker-clean:
	docker rmi clustering-seqdiag || true
	docker images -q --filter "dangling=true" | xargs docker rmi

.PHONY: fix-clock-skew
fix-clock-skew:
	boot2docker ssh sudo date -u -D "%Y%m%d%H%M.%S" --set "$(shell date -u +%Y%m%d%H%M.%S)"
