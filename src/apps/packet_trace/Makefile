MODULE := packet_trace

CC ?= gcc
AR ?= ar
OPENAPPS_OUTPATH ?= .
CFLAGS += -Wall -g -I. -I../../public -I../../../public -I./api -I../../sb_plugin/include -I../../../vendor/cjson -I../../../platform

# NOOP - no-operation, used to suppress "Nothing to do for ..." messages.
NOOP  ?= @:

export OUT_PACKET_TRACE=$(OPENAPPS_OUTPATH)/$(MODULE)
export LIBS_PACKET_TRACE=$(MODULE).a

searchdirs = $(realpath $(OPENAPPS_BASE)/src/apps/packet_trace/api)

export CPATH += $(searchdirs)
OBJECTS_PACKET_TRACE := $(notdir $(patsubst %.c,%.o,$(shell find . -name "*.c")))

export VPATH += $(dir $(shell find . -name "*.c"))

$(OUT_PACKET_TRACE)::
	@echo Making PT
	@mkdir -p $(OUT_PACKET_TRACE) 

$(OUT_PACKET_TRACE)/%.o : %.c
	$(CC) $(CFLAGS) -c  $< -o $@ 

# target for .a 
$(OUT_PACKET_TRACE)/$(LIBS_PACKET_TRACE): $(patsubst %,$(OUT_PACKET_TRACE)/%,$(subst :, ,$(OBJECTS_PACKET_TRACE))) 
	@cd $(OUT_PACKET_TRACE) && $(AR) rvs $(MODULE).a $(OBJECTS_PACKET_TRACE)  

#default target
packettrace all: $(OUT_PACKET_TRACE) $(OUT_PACKET_TRACE)/$(LIBS_PACKET_TRACE) 
	$(NOOP)

clean-packettrace clean: 
	@echo Cleaning PT
	@rm -f $(OUT_PACKET_TRACE)/*.o $(OUT_PACKET_TRACE)/*.a
	@rmdir $(OUT_PACKET_TRACE) 2>/dev/null; true

#target to print all exported variables
debug-packettrace dump-variables: 
	@echo "OUT_PACKET_TRACE=$(OUT_PACKET_TRACE)"
	@echo "LIBS_PACKET_TRACE=$(LIBS_PACKET_TRACE)"
	@echo "OBJECTS_PACKET_TRACE=$(OBJECTS_PACKET_TRACE)"
