include ../common.mak

# LDC: disable on Windows (gcc cmdline...)
ifneq (,$(findstring win,$(OS)))
TESTS:=
else
TESTS:=array allocator new
endif

.PHONY: all clean

# osx32 does not link properly, nobody uses it anyway...
ifeq ($(OS)$(MODEL), osx32)

all:

clean:

else
all: $(addprefix $(ROOT)/,$(addsuffix .done,$(TESTS)))

$(ROOT)/%.done : $(ROOT)/%
	@echo Testing $*
	$(QUIET)$(TIMELIMIT)$(ROOT)/$* $(RUN_ARGS)
	@touch $@

# LDC: libdl required by ldc.sanitizers_optionally_linked with version=SupportSanitizers
$(ROOT)/%: $(SRC)/%.cpp $(SRC)/%_test.d
	mkdir -p $(dir $@)
	$(QUIET)$(DMD) $(DFLAGS) -main -unittest -c -of=$(ROOT)/$*_d.o $(SRC)/$*_test.d
	$(QUIET)$(CXX) $(CXXFLAGS_BASE) -o $@ $< $(ROOT)/$*_d.o $(DRUNTIME) $(LDL) -lpthread

clean:
	rm -rf $(GENERATED)
endif
