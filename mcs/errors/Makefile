thisdir = errors
SUBDIRS = 
include ../build/rules.make

with_mono_path = MONO_PATH="$(topdir)/class/lib/$(PROFILE)$(PLATFORM_PATH_SEPARATOR)$$MONO_PATH"

ilasm = $(topdir)/class/lib/$(PROFILE)/ilasm.exe
ILASM = $(with_mono_path) $(RUNTIME) $(RUNTIME_FLAGS) $(ilasm)

DISTFILES = \
	CONTRIBUTORS_README 	\
	errors.txt 		\
	$(wildcard *.cs)	\
	$(wildcard *.il)	\
	$(wildcard *.inc)	\
	$(wildcard *.pub)	\
	$(wildcard *.snk)	\
	$(wildcard *.response)	\
	$(wildcard known-issues-*)	\
	$(wildcard dlls/first/*.cs)	\
	$(wildcard dlls/second/*.cs)

TEST_SUPPORT_FILES = \
	CS0012-lib.dll CS0012-2-lib.dll CS0012-3-lib.dll CS0012-4-lib.dll CS0012-5-lib.dll CS0012-6-lib.dll CS0012-9-lib.dll CS0012-10-lib.dll CS0012-11-lib.dll CS0012-12-lib.dll CS0012-13-lib.dll CS0012-14-lib.dll CS0012-15-lib.dll CS0012-16-lib.dll CS0012-17-lib.dll CS0012-18-lib.dll CS0029-26-lib.dll \
	CS0103-2-lib.dll CS0118-2-lib.dll CS0122-8-lib.dll CS0122-10-lib.dll CS0122-14-lib.dll CS0122-15-lib.dll CS0122-19-lib.dll CS0122-35-lib.dll CS0122-36-lib.dll CS0143-lib.dll CS0144-3-lib.dll CS0165-19-lib.dll \
	CS0205-3-lib.dll CS0229-3-lib.dll CS0229-4-lib.dll CS0266-25-lib.dll \
	CS0315-2-lib.dll \
	CS0425-7-lib.dll CS0433-lib.dll CS0433-2-lib.dll \
	CS0506-3-lib.dll CS0507-7-lib.dll CS0507-8-lib.dll CS0534-3-lib.dll CS0534-4-lib.dll CS0534-6-lib.dll CS0571-3-lib.dll CS0571-5-lib.dll CS0571-6-lib.dll \
	CS0612-2-lib.dll CS0618-2-lib.dll CS0619-8-lib.dll CS0619-17-lib.dll CS0619-32-lib.dll CS0619-33-lib.dll CS0619-36-lib.dll CS0619-42-lib.dll \
	CS0619-43-lib.dll CS1546-lib.dll CS0619-51-lib.dll CS1509-module.dll CS1681-2-lib.dll \
	CS0730-lib.dll \
	CS1540-15-lib.dll CS1540-17-lib.dll CS1542-lib.dll CS1577-lib.dll \
	CS1607-3-lib.dll CS1683-lib.dll CS1684-lib.dll CS1685-2-lib.dll \
	dlls/first/CS1701-lib.dll dlls/second/CS1701-lib.dll CS1701-lib.dll dlls/first/CS1702-lib.dll dlls/second/CS1702-lib.dll CS1702-lib.dll dlls/first/CS1705-lib.dll dlls/second/CS1705-lib.dll CS1705-lib.dll \
	CS2015-lib.dll CS3005-16-lib.dll CS3013-module.dll CSExternAlias-lib.dll \
	CSFriendAssembly-lib.dll \
	dlls/first/CS1703-lib.dll dlls/first/CS1704-lib.dll \
	dlls/second/CS1703-lib.dll dlls/second/CS1704-lib.dll

# mention all targets
all-local $(STD_TARGETS:=-local):

VALID_PROFILE := $(filter net_4_5, $(PROFILE))
ifdef VALID_PROFILE

qcheck: run-mcs-tests 

# again, run-test is when the tests actually happen, so
# don't compile on make test.

test-local:
	@:

run-test-local: clean-local
	$(MAKE) run-mcs-tests

# Required to test parser correctly
# -compiler-options:-v

TEST_PATTERN = 'v4'
TESTER_OPTIONS = -compiler-options:"-v -d:NET_4_0;NET_4_5"

COMPILER = $(topdir)/class/lib/$(BUILD_TOOLS_PROFILE)/mcs.exe
TESTER = MONO_RUNTIME='$(RUNTIME)' $(TEST_RUNTIME) $(RUNTIME_FLAGS) $(LOCAL_RUNTIME_FLAGS) $(topdir)/class/lib/$(PROFILE)/compiler-tester.exe

run-mcs-tests: $(TEST_SUPPORT_FILES)
	$(TESTER) -mode:neg -files:$(TEST_PATTERN) -compiler:$(COMPILER) -issues:known-issues-$(PROFILE) -log:$(PROFILE).log $(TESTER_OPTIONS) $(TOPTIONS)

endif

clean-local:
	rm -f *.exe *.dll *.log *.mdb dummy.xml *.junk dlls/first/*.dll dlls/second/*.dll

dist-local: dist-default

csproj-local install-local uninstall-local:

CS1701-lib.dll : CS1701-lib.cs
	$(CSCOMPILE) /target:library /warn:0 /r:dlls/first/CS1701-lib.dll /out:$@ $<

CS1702-lib.dll : CS1702-lib.cs
	$(CSCOMPILE) /target:library /warn:0 /r:dlls/first/CS1702-lib.dll /out:$@ $<

CS1705-lib.dll : CS1705-lib.cs
	$(CSCOMPILE) /target:library /warn:0 /r:dlls/first/CS1705-lib.dll /out:$@ $<

%-lib.dll: %-lib.cs
	$(CSCOMPILE) /target:library /warn:0 /out:$@ $<

%-module.dll: %-module.cs
	$(CSCOMPILE) /target:module /out:$@ $<

%-lib.dll: %-lib.il
	$(ILASM) /dll /out:$@ $<
