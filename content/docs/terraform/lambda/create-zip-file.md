---
title: "Create lambda ZIP file"
---

```bash
mkdir -p $(DIR_LAMBDA_OUTPUT1)
cp $(DIR_LAMBDA1)/*.py $(DIR_LAMBDA_OUTPUT1)
tar czf $(DIR_LAMBDA_OUTPUT1)/ansible.tar $(DIR_ANSIBLE)
pip3 install \
	--platform manylinux2014_x86_64 \
	--implementation cp \
	--python $(PYTHON_VERSION) \
	--only-binary=:all: \
	-r $(DIR_LAMBDA1)/requirements.txt \
	-t $(DIR_LAMBDA_OUTPUT1)
```