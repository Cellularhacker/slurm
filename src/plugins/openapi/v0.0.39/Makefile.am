# Makefile for openapi/v0.0.39 plugin

AUTOMAKE_OPTIONS = foreign
CLEANFILES = *.bino

EXTRA_DIST = LICENSE-openapi.json

REF = openapi.json

PLUGIN_FLAGS = -module -avoid-version --export-dynamic

AM_CPPFLAGS = -DSLURM_PLUGIN_DEBUG -I$(top_srcdir) -I$(top_srcdir)/src/common $(JSON_CPPFLAGS)

BIN_REF = $(REF:.json=.bino)

%.bino: %.json
	$(AM_V_GEN)pushd $(abs_srcdir); $(LD) -r -o "$(abs_builddir)/$*.bino" -z noexecstack --format=binary "$(notdir $<)"; popd
	$(AM_V_at)@OBJCOPY@ --rename-section .data=.rodata,alloc,load,readonly,data,contents "$*.bino"

openapi_ref.lo: $(BIN_REF)
	$(AM_V_at)echo "# $@ - a libtool object file" >"$@"
	$(AM_V_at)echo "# Generated by $(shell @LIBTOOL@ --version | head -n 1)" >>"$@"
	$(AM_V_at)echo "#" >>"$@"
	$(AM_V_at)echo "# Please DO NOT delete this file!" >>"$@"
	$(AM_V_at)echo "# It is necessary for linking the library." >>"$@"
	$(AM_V_at)echo >>"$@"
	$(AM_V_at)echo "# Name of the PIC object." >>"$@"
	$(AM_V_at)echo "pic_object='$(BIN_REF)'" >>"$@"
	$(AM_V_at)echo >>"$@"
	$(AM_V_at)echo "# Name of the non-PIC object" >>"$@"
	$(AM_V_at)echo "non_pic_object=''" >>"$@"
	$(AM_V_at)echo >>"$@"

libopenapi_ref_la_SOURCES =
libopenapi_ref_la_DEPENDENCIES = openapi_ref.lo

pkglib_LTLIBRARIES = openapi_v0_0_39.la
noinst_LTLIBRARIES = libopenapi_ref.la

openapi_v0_0_39_la_SOURCES = \
	api.c api.h diag.c jobs.c nodes.c partitions.c reservations.c

openapi_v0_0_39_la_DEPENDENCIES = $(LIB_SLURM_BUILD)
openapi_v0_0_39_la_LDFLAGS = $(PLUGIN_FLAGS)
openapi_v0_0_39_la_LIBADD = $(libslurmfull_la_LIBADD) openapi_ref.lo
