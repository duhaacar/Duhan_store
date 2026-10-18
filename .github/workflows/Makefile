#---------------------------------------------------------------------------------
# Duha_store - PSL1GHT Makefile
#   make       -> .elf / .self
#   make pkg   -> Duha_store.pkg
#---------------------------------------------------------------------------------
.SUFFIXES:
#---------------------------------------------------------------------------------

ifeq ($(strip $(PSL1GHT)),)
$(error "PSL1GHT ortam değişkeni ayarlı değil. export PSL1GHT=<yol>")
endif

include $(PSL1GHT)/ppu_rules

TARGET		:=	Duha_store
BUILD		:=	build
SOURCES		:=	source
DATA		:=	data
INCLUDES	:=	include

TITLE		:=	Duha Store
APPID		:=	DUHASTORE
CONTENTID	:=	UP0001-$(APPID)_00-0000000000000000
ICON0		:=	$(CURDIR)/ICON0.PNG
SFOXML		:=	$(CURDIR)/sfo.xml

CFLAGS		=	-O2 -Wall -mcpu=cell $(MACHDEP) $(INCLUDE)
CXXFLAGS	=	$(CFLAGS)
LDFLAGS		=	$(MACHDEP) -Wl,-Map,$(notdir $@).map

LIBS		:=	-lfont -ltiny3d -lsimdmath -lgcm_sys -lio -lsysutil -lrt -llv2 -lfreetype -lz -lm

ifneq ($(BUILD),$(notdir $(CURDIR)))

export OUTPUT	:=	$(CURDIR)/$(TARGET)
export VPATH	:=	$(foreach dir,$(SOURCES),$(CURDIR)/$(dir)) \
					$(foreach dir,$(DATA),$(CURDIR)/$(dir))
export BUILDDIR	:=	$(CURDIR)/$(BUILD)
export DEPSDIR	:=	$(BUILDDIR)

CFILES		:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.c)))
BINFILES	:=	$(foreach dir,$(DATA),$(notdir $(wildcard $(dir)/*.*)))

export OFILES	:=	$(addsuffix .o,$(BINFILES)) $(CFILES:.c=.o)
export INCLUDE	:=	$(foreach dir,$(INCLUDES),-I$(CURDIR)/$(dir)) \
					$(LIBPSL1GHT_INC) \
					-I$(CURDIR)/$(BUILD)
export LIBPATHS	:=	$(LIBPSL1GHT_LIB)

.PHONY: $(BUILD) clean pkg

$(BUILD):
	@[ -d $@ ] || mkdir -p $@
	@$(MAKE) --no-print-directory -C $(BUILD) -f $(CURDIR)/Makefile

clean:
	@echo clean ...
	@rm -rf $(BUILD) $(OUTPUT).elf $(OUTPUT).self $(OUTPUT)*.pkg

pkg: $(BUILD)
	@echo Creating PKG ...
	@$(SELF_NPDRM) $(OUTPUT).elf $(BUILDDIR)/pkg/USRDIR/EBOOT.BIN $(CONTENTID) >/dev/null
	@$(SFO) --title "$(TITLE)" --appid "$(APPID)" -f $(SFOXML) $(BUILDDIR)/pkg/PARAM.SFO
	@$(PKG) --contentid $(CONTENTID) $(BUILDDIR)/pkg/ $(OUTPUT).pkg >/dev/null
	@cp $(ICON0) $(BUILDDIR)/pkg/ICON0.PNG

else

DEPENDS	:=	$(OFILES:.o=.d)

$(OUTPUT).self: $(OUTPUT).elf
$(OUTPUT).elf:	$(OFILES)

-include $(DEPENDS)

endif
