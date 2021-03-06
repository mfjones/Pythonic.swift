PWD=$(shell pwd)
MODULE_NAME=Pythonic
TEST_APPLICATION_SWIFT=$(MODULE_NAME)-test
TEST_APPLICATION_PYTHON=$(MODULE_NAME)-test.py
LIB_NAME=lib$(MODULE_NAME).dylib
LIB_PATH=$(PWD)/$(LIB_NAME)
SWIFT_MODULE_PATH=$(PWD)/$(MODULE_NAME).swiftmodule

module: clean
	xcrun -sdk macosx swift -emit-library -o $(LIB_PATH) -Xlinker -install_name -Xlinker @rpath/$(LIB_NAME) -emit-module -emit-module-path $(SWIFT_MODULE_PATH) -module-name $(MODULE_NAME) -module-link-name $(MODULE_NAME) $(MODULE_NAME).swift $(MODULE_NAME).*.swift

clean:
	rm -f $(LIB_NAME) $(MODULE_NAME).swiftdoc $(MODULE_NAME).swiftmodule $(TEST_APPLICATION_SWIFT) $(TEST_APPLICATION_PYTHON)

test: module
	(printf "#!/usr/bin/env python2.7\nimport math\nimport re\nimport sys\nimport time\n"; egrep ^assert Pythonic-test.swift | sed 's/nil/None/g' | sed 's/true/True/g' | sed 's/false/False/g' | sed 's/!/not /g') > $(TEST_APPLICATION_PYTHON)
	chmod +x $(TEST_APPLICATION_PYTHON)
	./$(TEST_APPLICATION_PYTHON) arg1 arg2 < Makefile && echo "All tests successfully passed when executing as Python code."
	xcrun -sdk macosx swift $(TEST_APPLICATION_SWIFT).swift -o $(TEST_APPLICATION_SWIFT) -I $(PWD) -L $(PWD) -Xlinker -rpath -Xlinker @executable_path/
	./$(TEST_APPLICATION_SWIFT) arg1 arg2 < Makefile && echo "All tests successfully passed when executing as Swift code."
