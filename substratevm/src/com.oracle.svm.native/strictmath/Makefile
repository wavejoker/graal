#
# Copyright (c) 2014, 2017, Oracle and/or its affiliates. All rights reserved.
# DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
#
# This code is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License version 2 only, as
# published by the Free Software Foundation.
#
# This code is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# version 2 for more details (a copy is included in the LICENSE file that
# accompanied this code).
#
# You should have received a copy of the GNU General Public License version
# 2 along with this work; if not, write to the Free Software Foundation,
# Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
#
# Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
# or visit www.oracle.com if you need additional information or have any
# questions.
#
LIBRARY_NAME = strictmath

# Some directory names.
LIBM_SRC_DIR = fdlibm/src
LIBM_INCLUDE = fdlibm/include

TARGETOS ?= $(shell uname -s)
ifeq ($(TARGETOS),Linux)
    OS:=linux
endif
ifeq ($(TARGETOS),Darwin)
    OS:=darwin
endif
ifeq ($(TARGETOS),CYGWIN_NT-10.0)
    OS:=windows
endif

# How to make object files from source files.
ifeq (${OS},windows)
CFLAGS := /I $(LIBM_INCLUDE) /O1 /D _LITTLE_ENDIAN
OBJ_EXT := obj
CC := CL
else
# The flags to be used when compiling C sources.
CFLAGS := -I$(LIBM_INCLUDE) -fPIC -O1 -D _LITTLE_ENDIAN
OBJ_EXT := o
endif

# A list of all the libm function source files.
LIBM_SRCS = $(wildcard $(LIBM_SRC_DIR)/*.c)
LIBM_OBJS = $(LIBM_SRCS:$(LIBM_SRC_DIR)/%.c=%.$(OBJ_EXT))

TARGET_DIR = ../../../clibraries/$(OS)-amd64
TARGET = $(TARGET_DIR)/lib$(LIBRARY_NAME).a

# The first target is what gets made by default.
$(TARGET) : StrictMath.$(OBJ_EXT) $(LIBM_OBJS)
	mkdir -p $(TARGET_DIR)
	ar -cr $@ $^

StrictMath.$(OBJ_EXT) : StrictMath.c $(LIBM_INCLUDE)/fdlibm.h $(LIBM_INCLUDE)/jfdlibm.h
	$(CC) -c $(CFLAGS) $<

%.$(OBJ_EXT) : $(LIBM_SRC_DIR)/%.c $(LIBM_INCLUDE)/fdlibm.h $(LIBM_INCLUDE)/jfdlibm.h
	$(CC) -c $(CFLAGS) $<

# How to clean up.
clean :
	$(RM) $(LIBM_OBJS)
	$(RM) StrictMath.$(OBJ_EXT)
	$(RM) $(TARGET)

# Use "make dependencies" to discover all the dependencies.
dependencies :
	@ $(CC) -MM $(CFLAGS) $(LIBM_SRCS) StrictMath.c

# The declaration of which targets do not result in files.
.PHONY : clean dependencies
