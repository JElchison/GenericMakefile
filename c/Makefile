#### PROJECT SETTINGS ####
# The name of the executable to be created
BIN_NAME := hello
# Compiler used
CC = gcc
# C standard
CSTD = c99
# Extension of source files used in the project
SRC_EXT = c
# Path to the source directory, relative to the makefile
SRC_PATH = .
# Space-separated pkg-config libraries used by this project
LIBS =
# Add additional include paths
INCLUDES = -I $(SRC_PATH)

# General compiler flags
COMPILE_FLAGS = -std=$(CSTD) -Wall -Wextra -Werror -pedantic \
    -Wno-missing-braces -Wno-missing-field-initializers -Wformat=2 \
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
# Additional release-specific flags
RCOMPILE_FLAGS = -D NDEBUG -Os -fno-ident -flto
# Additional debug-specific flags
DCOMPILE_FLAGS = -D DEBUG -O0 -fident -ggdb3 -fno-omit-frame-pointer

# General linker settings
LINK_FLAGS =
# 64-bit linker flags
BIT64_LINK_FLAGS = -m64
# 32-bit linker flags
BIT32_LINK_FLAGS = -m32
# Additional release-specific linker settings
RLINK_FLAGS = -s -Wl,--build-id=none -flto
# Additional debug-specific linker settings
DLINK_FLAGS = -lmcheck
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
64-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(RCOMPILE_FLAGS)
64-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(RLINK_FLAGS)
64-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT64_COMPILE_FLAGS) $(DCOMPILE_FLAGS)
64-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT64_LINK_FLAGS) $(DLINK_FLAGS)
32-release: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(RCOMPILE_FLAGS)
32-release: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(RLINK_FLAGS)
32-debug: export CFLAGS := $(CFLAGS) $(COMPILE_FLAGS) $(BIT32_COMPILE_FLAGS) $(DCOMPILE_FLAGS)
32-debug: export LDFLAGS := $(LDFLAGS) $(LINK_FLAGS) $(BIT32_LINK_FLAGS) $(DLINK_FLAGS)

# Build and output paths
64-release: export BUILD_PATH := build/64/release
64-release: export BIN_PATH := bin/64/release
64-debug: export BUILD_PATH := build/64/debug
64-debug: export BIN_PATH := bin/64/debug
32-release: export BUILD_PATH := build/32/release
32-release: export BIN_PATH := bin/32/release
32-debug: export BUILD_PATH := build/32/debug
32-debug: export BIN_PATH := bin/32/debug

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

# 64-bit release build
.PHONY: 64-release
64-release: dirs
	@echo "Beginning 64-bit release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 64-bit debug build
.PHONY: 64-debug
64-debug: dirs
	@echo "Beginning 64-bit debug build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@# no call to 'strip' target for debug builds
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit release build
.PHONY: 32-release
32-release: dirs
	@echo "Beginning 32-bit release build $(VERSION)..."
	@$(START_TIME)
	@$(MAKE) check --no-print-directory
	@$(MAKE) main --no-print-directory
	@$(MAKE) strip --no-print-directory
	@echo -n "Total build time: "
	@$(END_TIME)

# 32-bit debug build
.PHONY: 32-debug
32-debug: dirs
	@echo "Beginning 32-bit debug build $(VERSION)..."
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

# Strip the binary, including the .comment section which normally contains the GCC version
.PHONY: strip
strip:
	$(CMD_PREFIX)strip --strip-all -R .note -R .comment $(BIN_PATH)/$(BIN_NAME)

# Main rule, checks the executable and symlinks to the output
.PHONY: main
main: $(BIN_PATH)/$(BIN_NAME)
	@echo "Making symlink: $(BIN_NAME) -> $< ..."
	@$(RM) $(BIN_NAME)
	@ln -s $(BIN_PATH)/$(BIN_NAME) $(BIN_NAME)

.PHONY: check
check:
	@cppcheck --enable=all --std=$(CSTD) .

.PHONY: indent
indent:
	@indent -kr $(SOURCES)

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
