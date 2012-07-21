################################################################################
# Copyright (c) 2010 HSM.

# @author	HSM
# @file		Makefile
# @date		2008-08-08 08:08 created
# @last		2011-03-29 17:18 updated
# @brief	Makefile template V.1.0

################################################################################
# what target(s) you want
target := a.out
# what library(ies) you want to create
lib := 
# dst must be set explicitly, even it's a dot.
dst := build
src := $(wildcard *.[cC]* */*.[cC]*) 				# c/cpp/cxx source file
#src := $(shell find -iname "*.c*" -type f) 		# require find, independent
src := $(filter-out test% %test.c %test.cpp, $(src))# no module test unit(test_)
#src +=  											# add mannually

obj := $(patsubst %, $(dst)/%.o, $(basename $(src))) # translate to object

#.INTERMEDIATE: $(obj) 								# which obj(s) do not save

#dir := $(wildcard ./src* ./*/src*) 				# makefile dependent
dir := $(shell find -maxdepth 2 -type d) 			# hierachically search
dir := $(filter-out . ./$(dst)%, $(dir)) 			# remove unused
#dir := src/model src/view src/control 				# add mannually

# mkdir for build directories if non-exist
ifeq ($(wildcard $(dst)),)
$(shell mkdir -p $(dst))
$(foreach d, $(dir), $(shell mkdir -p $(dst)/$d))
#or @if ! test -d $(@D); then mkdir -p $(@D); fi
#or @if ! test -d $(dir $@); then mkdir -p $(dir $@); fi
endif

CC = gcc
CXX = g++
CFLAGS = -g -Wall $(addprefix -I, $(filter %inc, $(dir))) \

CXXFLAGS = -g -Wall $(addprefix -I, $(filter %inc, $(dir))) \

LDFLAGS = $(addprefix -L, $(filter %src, $(dir))) \
		  -lboost_system-mt -lboost_thread-mt

VPATH = $(subst $(SPACE) $,:,$(dir))
vpath %.o $(subst $(SPACE) $,:,$(addprefix $(dst)/, $(dir)))

all: $(target)

$(target): $(obj) $(lib)
ifeq ($(filter-out %.c, $(src)),)
	$(LINK.c) $(LDFLAGS) $^ -o $@
else
	$(LINK.cc) $(LDFLAGS) $^ -o $@
endif

$(dst)/%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

$(dst)/%.o:%.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

$(dst)/%.a:
	#make -C $(subdir) lib=$(notdir $@) dst=$(PWD)/$(dst)/$(subdir)

$(dst)/%.so:
	#make -C $(subdir) lib=$(notdir $@) dst=$(PWD)/$(dst)/$(subdir)

#fun.c: def.h
# more specific prerequisite can be appended

.PHONY: clean dist
clean:
	$(RM) $(obj) $(lib) $(target)

dist:
	$(RM) -r $(dst)
	$(RM) $(target)

