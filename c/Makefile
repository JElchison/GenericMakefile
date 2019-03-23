#### PROJECT SETTINGS ####
# The name of the executable to be created
BIN_NAME := hello
# Compiler used
CC = gcc
# C standard
CSTD_GCC = gnu11
CSTD_CPPCHECK = c11
# Extension of source files used in the project
SRC_EXT = c
# Path to the source directory, relative to the makefile
SRC_PATH = .
# Space-separated pkg-config libraries used by this project
LIBS =
# Add additional include paths
INCLUDES = -I $(SRC_PATH)

# General compiler flags
COMPILE_FLAGS = -std=$(CSTD_GCC) -Wall -Wextra -Werror -pedantic \
    -Wformat=2 \
    -Wswitch-default -Wswitch-enum -Wcast-align -Wpointer-arith \
    -Wbad-function-cast -Wstrict-overflow=5 -Wstrict-prototypes -Winline \
    -Wundef -Wnested-externs -Wcast-qual -Wshadow -Wunreachable-code \
    -Wlogical-op -Wfloat-equal -Wstrict-aliasing=2 -Wredundant-decls \
    -Wold-style-definition -Wmissing-prototypes \
    -fno-common -fstrict-aliasing
# 64-bit compiler flags
BIT64_COMPILE_FLAGS = -m64
# 32-bit compiler flags
BIT32_COMPILE_FLAGS = -m32
# Additional dynamic-link settings
DYNAMIC_COMPILE_FLAGS =
# Additional static-link settings
STATIC_COMPILE_FLAGS =
# Additional release-specific flags
RELEASE_COMPILE_FLAGS = -D NDEBUG -Os -fno-ident -flto
# Additional debug-specific flags
DEBUG_COMPILE_FLAGS = -D DEBUG -O0 -fident -ggdb3 -fno-omit-frame-pointer

# General linker settings
LINK_FLAGS =
# 64-bit linker flags
BIT64_LINK_FLAGS = -m64
# 32-bit linker flags
BIT32_LINK_FLAGS = -m32
# Additional dynamic linker settings
DYNAMIC_LINK_FLAGS =
# Additional static linker settings
STATIC_LINK_FLAGS = -static
# Additional release-specific linker settings
RELEASE_LINK_FLAGS = -s -Wl,--build-id=none -flto -ffunction-sections -fdata-sections -Wl,--gc-sections
# Additional debug-specific linker settings
DEBUG_LINK_FLAGS = -lmcheck
#### END PROJECT SETTINGS ####

# Optionally you may move the section above to a separate config.mk file, and
# uncomment the line below
# include config.mk

# Generally should not need to edit below this line

# Obtains the OS type, either 'Darwin' (OS X) or 'Linux'
UNAME_S:=$(shell uname -s)

# Function used to check variables. Use on the command line:
# make print-VARNAME
# Useful for debugging and adding features
print-%: ; @echo $*=$($*)

# Shell used in this makefile
# bash is used for 'echo -en'
SHELL = /bin/bash
# Clear built-in rules
.SUFFIXES:

# Append pkg-config specific libraries if need be
ifneq ($(LIBS),)
	COMPILE_FLAGS += $(shell pkg-config --cflags $(LIBS))
	LINK_FLAGS += $(shell pkg-config --libs $(LIBS))
endif

# Verbose option, to output compile and link commands
export V := false
export CMD_PREFIX := @
ifeq ($(V),true)
	CMD_PREFIX :=
endif

# Combine compiler and linker flags
64-dynamic-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(DYNAMIC_COMPILE_FLAGS) $(RELEASE_COMPILE_FLAGS)
64-dynamic-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(DYNAMIC_LINK_FLAGS) $(RELEASE_LINK_FLAGS)
64-dynamic-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(DYNAMIC_COMPILE_FLAGS) $(DEBUG_COMPILE_FLAGS)
64-dynamic-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(DYNAMIC_LINK_FLAGS) $(DEBUG_LINK_FLAGS)
64-static-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(STATIC_COMPILE_FLAGS) $(RELEASE_COMPILE_FLAGS)
64-static-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(STATIC_LINK_FLAGS) $(RELEASE_LINK_FLAGS)
64-static-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(STATIC_COMPILE_FLAGS) $(DEBUG_COMPILE_FLAGS)
64-static-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(STATIC_LINK_FLAGS) $(DEBUG_LINK_FLAGS)
32-dynamic-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(DYNAMIC_COMPILE_FLAGS) $(RELEASE_COMPILE_FLAGS)
32-dynamic-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(DYNAMIC_LINK_FLAGS) $(RELEASE_LINK_FLAGS)
32-dynamic-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(DYNAMIC_COMPILE_FLAGS) $(DEBUG_COMPILE_FLAGS)
32-dynamic-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(DYNAMIC_LINK_FLAGS) $(DEBUG_LINK_FLAGS)
32-static-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(STATIC_COMPILE_FLAGS) $(RELEASE_COMPILE_FLAGS)
32-static-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(STATIC_LINK_FLAGS) $(RELEASE_LINK_FLAGS)
32-static-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(STATIC_COMPILE_FLAGS) $(DEBUG_COMPILE_FLAGS)
32-static-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(STATIC_LINK_FLAGS) $(DEBUG_LINK_FLAGS)

# Build and output paths
64-dynamic-release: export BUILD_PATH := build/64/dynamic/release
64-dynamic-release: export BIN_PATH := bin/64/dynamic/release
64-dynamic-debug: export BUILD_PATH := build/64/dynamic/debug
64-dynamic-debug: export BIN_PATH := bin/64/dynamic/debug
64-static-release: export BUILD_PATH := build/64/static/release
64-static-release: export BIN_PATH := bin/64/static/release
64-static-debug: export BUILD_PATH := build/64/static/debug
64-static-debug: export BIN_PATH := bin/64/static/debug
32-dynamic-release: export BUILD_PATH := build/32/dynamic/release
32-dynamic-release: export BIN_PATH := bin/32/dynamic/release
32-dynamic-debug: export BUILD_PATH := build/32/dynamic/debug
32-dynamic-debug: export BIN_PATH := bin/32/dynamic/debug
32-static-release: export BUILD_PATH := build/32/static/release
32-static-release: export BIN_PATH := bin/32/static/release
32-static-debug: export BUILD_PATH := build/32/static/debug
32-static-debug: export BIN_PATH := bin/32/static/debug

# Find all source files in the source directory, sorted by most
# recently modified
ifeq ($(UNAME_S),Darwin)
	SOURCES = $(shell find $(SRC_PATH) -name '*.$(SRC_EXT)' | sort -k 1nr | cut -f2-)
else
	SOURCES = $(shell find $(SRC_PATH) -name '*.$(SRC_EXT)' -printf '%T@\t%p\n' \
						| sort -k 1nr | cut -f2-)
endif

# fallback in case the above fails
rwildcard = $(foreach d, $(wildcard $1*), $(call rwildcard,$d/,$2) \
						$(filter $(subst *,%,$2), $d))
ifeq ($(SOURCES),)
	SOURCES := $(call rwildcard, $(SRC_PATH), *.$(SRC_EXT))
endif

# Set the object file names, with the source directory stripped
# from the path, and the build path prepended in its place
OBJECTS = $(SOURCES:$(SRC_PATH)/%.$(SRC_EXT)=$(BUILD_PATH)/%.o)
# Set the dependency files that will be used to add header dependencies
DEPS = $(OBJECTS:.o=.d)

# Macros for timing compilation
ifeq ($(UNAME_S),Darwin)
	CUR_TIME = awk 'BEGIN{srand(); print srand()}'
	TIME_FILE = $(dir $@).$(notdir $@)_time
	START_TIME = $(CUR_TIME) > $(TIME_FILE)
	END_TIME = read st < $(TIME_FILE) ; \
		$(RM) $(TIME_FILE) ; \
		st=$$((`$(CUR_TIME)` - $$st)) ; \
		echo $$st
else
	TIME_FILE = $(dir $@).$(notdir $@)_time
	START_TIME = date '+%s' > $(TIME_FILE)
	END_TIME = read st < $(TIME_FILE) ; \
		$(RM) $(TIME_FILE) ; \
		st=$$((`date '+%s'` - $$st - 86400)) ; \
		echo `date -u -d @$$st '+%H:%M:%S'`
endif

# Version macros
USE_VERSION := false
VERSION := ""
# If this isn't a git repo or the repo has no tags, git describe will return non-zero
ifeq ($(shell git describe --always > /dev/null 2>&1 ; echo $$?), 0)
	USE_VERSION := true
	VERSION := $(shell git describe --tags --long --dirty --always)
	VERSION_MAJOR := $(shell echo $(VERSION) | \
		sed -r 's/^(v?([0-9]+)(\.([0-9]+)(\.([0-9]+))?)?(-.*)?\-([0-9]+)\-)?g?([0-9a-f]{7}.*)$$/\2/')
	VERSION_MINOR := $(shell echo $(VERSION) | \
		sed -r 's/^(v?([0-9]+)(\.([0-9]+)(\.([0-9]+))?)?(-.*)?\-([0-9]+)\-)?g?([0-9a-f]{7}.*)$$/\4/')
	VERSION_PATCH := $(shell echo $(VERSION) | \
		sed -r 's/^(v?([0-9]+)(\.([0-9]+)(\.([0-9]+))?)?(-.*)?\-([0-9]+)\-)?g?([0-9a-f]{7}.*)$$/\6/')
	VERSION_REVISION := $(shell echo $(VERSION) | \
		sed -r 's/^(v?([0-9]+)(\.([0-9]+)(\.([0-9]+))?)?(-.*)?\-([0-9]+)\-)?g?([0-9a-f]{7}.*)$$/\8/')
	VERSION_HASH := $(shell echo $(VERSION) | \
		sed -r 's/^(v?([0-9]+)(\.([0-9]+)(\.([0-9]+))?)?(-.*)?\-([0-9]+)\-)?g?([0-9a-f]{7}.*)$$/\9/')
	ifneq (,$(findstring debug,$(BUILD_PATH)))
		override CFLAGS := $(CFLAGS) \
			-D VERSION_MAJOR=$(VERSION_MAJOR) \
			-D VERSION_MINOR=$(VERSION_MINOR) \
			-D VERSION_PATCH=$(VERSION_PATCH) \
			-D VERSION_REVISION=$(VERSION_REVISION) \
			-D VERSION_HASH=\"$(VERSION_HASH)\"
	endif
endif

# 64-bit dynamic release build
.PHONY: 64-dynamic-release
64-dynamic-release: dirs
	@echo "Beginning 64-bit dynamic release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 64-bit static release build
.PHONY: 64-static-release
64-static-release: dirs
	@echo "Beginning 64-bit static release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 64-bit dynamic debug build
.PHONY: 64-dynamic-debug
64-dynamic-debug: dirs
	@echo "Beginning 64-bit dynamic debug build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@# no call to 'strip' target for debug builds
	@echo -n "Total build time: "
	@$(END_TIME)

# 64-bit static debug build
.PHONY: 64-static-debug
64-static-debug: dirs
	@echo "Beginning 64-bit static debug build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@# no call to 'strip' target for debug builds
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit dynamic release build
.PHONY: 32-dynamic-release
32-dynamic-release: dirs
	@echo "Beginning 32-bit dynamic release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit static release build
.PHONY: 32-static-release
32-static-release: dirs
	@echo "Beginning 32-bit static release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit dynamic debug build
.PHONY: 32-dynamic-debug
32-dynamic-debug: dirs
	@echo "Beginning 32-bit dynamic debug build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@# no call to 'strip' target for debug builds
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit static debug build
.PHONY: 32-static-debug
32-static-debug: dirs
	@echo "Beginning 32-bit static debug build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@# no call to 'strip' target for debug builds
	@echo -n "Total build time: "
	@$(END_TIME)

# Create the directories used in the build
.PHONY: dirs
dirs:
	@echo "Creating directories ..."
	@mkdir -p $(dir $(OBJECTS))
	@mkdir -p $(BIN_PATH)

# Removes all build files
.PHONY: clean
clean:
	@echo "Deleting $(BIN_NAME) symlink ..."
	@$(RM) $(BIN_NAME)
	@echo "Deleting directories ..."
	@$(RM) -r build
	@$(RM) -r bin

.PHONY: indent
indent:
	$(CMD_PREFIX)indent -kr $(SOURCES)

# Strip the binary, including the .comment section which normally contains the GCC version
.PHONY: strip
strip:
	$(CMD_PREFIX)strip --strip-all -R .note -R .comment $(BIN_PATH)/$(BIN_NAME)

# Main rule, symlinks to the output
.PHONY: main
main: $(BIN_PATH)/$(BIN_NAME)
	@echo "Making symlink: $(BIN_NAME) -> $< ..."
	@$(RM) $(BIN_NAME)
	@ln -s $(BIN_PATH)/$(BIN_NAME) $(BIN_NAME)

# Link the executable
$(BIN_PATH)/$(BIN_NAME): $(OBJECTS)
	@echo "Linking: $@ ..."
	@$(START_TIME)
	$(CMD_PREFIX)$(CC) $(OBJECTS) $(LDFLAGS) -o $@
	@echo -en "\t Link time: "
	@$(END_TIME)

# Add dependency files, if they exist
-include $(DEPS)

# Source file rules
# After the first compilation they will be joined with the rules from the
# dependency files to provide header dependencies
$(BUILD_PATH)/%.o: $(SRC_PATH)/%.$(SRC_EXT)
	@echo "Compiling: $< -> $@ ..."
	@$(START_TIME)
	$(CMD_PREFIX)$(CC) $(CFLAGS) $(INCLUDES) -MP -MMD -c $< -o $@
	@echo -en "\t Compile time: "
	@$(END_TIME)

.PHONY: check
check:
	$(CMD_PREFIX)cppcheck --check-config --enable=all --std=$(CSTD_CPPCHECK) $(INCLUDES) .
	$(CMD_PREFIX)cppcheck --enable=all --std=$(CSTD_CPPCHECK) $(INCLUDES) . | grep -v 'cannot find all the include files'
