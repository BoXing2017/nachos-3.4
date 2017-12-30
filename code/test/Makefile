ifndef MAKEFILE_TEST
define MAKEFILE_TEST
yes
endef

# Machine dependent stuff
include ../Makefile.dep

real_bin_dir := $(bin_dir)
CC = $(GCCDIR)gcc 
AS = $(GCCDIR)as 
LD = $(GCCDIR)ld

targets = halt shell matmult sort exec

all = $(targets:%=$(bin_dir)/%) 

all_coff = $(targets:%=$(obj_dir)/%.coff)
all_noff = $(all:%=%.noff)
all_flat = $(all:%=%.flat)

all: $(all_noff) $(all_flat)

$(targets): % : $(bin_dir)/%
	ln -sf $(bin_dir)/$@ $@

CFILES = $(targets:%=%.c)

SFILES = start.s

include ../Makefile.common

INCDIR =-I../userprog -I../threads
CFLAGS = -G 0 -c $(INCDIR)

coff2noff = ../bin/$(real_bin_dir)/coff2noff
coff2flat = ../bin/$(real_bin_dir)/coff2flat

$(all_coff): $(obj_dir)/%.coff: $(obj_dir)/start.o $(obj_dir)/%.o
	@echo ">>> Linking" $(obj_dir)/$(notdir $@) "<<<"
	$(LD) $(LDFLAGS) $^ -o $(obj_dir)/$(notdir $@)

$(all_noff): $(bin_dir)/%.noff: $(obj_dir)/%.coff
	@echo ">>> Converting to noff file:" $@ "<<<"
	$(coff2noff) $^ $@
	ln -sf $@ $(notdir $@)


$(all_flat): $(bin_dir)/%.flat: $(obj_dir)/%.coff
	@echo ">>> Converting to flat file:" $@ "<<<"
	$(coff2flat) $^ $@
	ln -sf $@ $(notdir $@)


%.s: %.c
	@echo ">>> Compiling .s file for" $< "<<<"
	$(CC) $(CFLAGS) -S -c -o $@ $<
endif # MAKEFILE_TEST
