
.PHONY: fetch extract configure build patch
all:: fetch extract configure build

BASE_URL=
VERSION=
PACKAGE_NAME=
CODEC=tar.gz
BASE_NAME=$(PACKAGE_NAME)-$(VERSION)
FILE_NAME=$(BASE_NAME).$(CODEC)
FILE_URL=$(BASE_URL)/$(FILE_NAME)

TARGET_DIR=$(VERSION)
EXTRACT_CMD=tar -C $(TARGET_DIR) -xzf
SRC_DIR=$(TARGET_DIR)/$(PACKAGE_NAME)-$(VERSION)
MODIFIED_DIR=$(TARGET_DIR)/$(PACKAGE_NAME)-$(VERSION)-patched
PATCH_DIR=patch/$(VERSION)

PWD=$(shell pwd)

#
# customize this
#
CONFIGURE_ARGS= --prefix=$(PWD)/install


fetch: 
	if [ ! -e $(FILE_NAME) ]; then wget $(FILE_URL); fi

extract:
	mkdir -p $(TARGET_DIR)
	$(EXTRACT_CMD) $(FILE_NAME)

configure:
	cd $(SRC_DIR) && ./configure $(CONFIGURE_ARGS)

build:
	cd $(SRC_DIR) && make 

custom:
	patch -p0 < $(PATCH_DIR)/$(BASE_NAME).patch

patch:
	mkdir -p $(PATCH_DIR)
	if [ -e $(MODIFIED_DIR) ]; then diff -u $(SRC_DIR) $(MODIFIED_DIR) | tee $(PATCH_DIR)/$(BASE_NAME).patch; fi


test:
	make -C $(SRC_DIR) test

clean:
	rm -rf *.$(CODEC) $(SRC_DIR) $(PATCH_DIR)
