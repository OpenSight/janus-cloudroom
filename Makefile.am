ACLOCAL_AMFLAGS = -I m4

# FIXME: These should be enabled once the code is safe for them. That requires
# some fairly big refactoring though, which can wait.
# AM_CFLAGS += -Wshadow -Wstrict-aliasing=2


AM_CFLAGS = -I. -I$(includedir)/janus/plugins -I$(includedir)/janus

EXTRA_DIST = $(NULL)
CLEANFILES = $(NULL)

confdir = $(sysconfdir)/janus
conf_DATA = $(NULL)

plugindir = $(libdir)/janus/plugins
plugin_LTLIBRARIES = $(NULL)


SUBDIRS = html
dist_html_DATA = README.md


%.sample: %.sample.in
	$(MKDIR_P) $(@D)
	$(AM_V_GEN) sed -e "\
	    s|[@]confdir[@]|$(confdir)|;\
	    s|[@]certdir[@]|$(certdir)|;\
	    s|[@]plugindir[@]|$(plugindir)|;\
	    s|[@]transportdir[@]|$(transportdir)|;\
	    s|[@]eventdir[@]|$(eventdir)|;\
	    s|[@]recordingsdir[@]|$(recordingsdir)|;\
	    s|[@]demosdir[@]|$(demosdir)|;\
	    s|[@]streamdir[@]|$(streamdir)|; \
	    s|[@]luadir[@]|$(luadir)|; \
	    s|[@]duktapedir[@]|$(duktapedir)|" \
	$< > $@ || rm $@








.PHONY: FORCE
FORCE:




##
# Plugins
##

plugins_cflags = \
	$(AM_CFLAGS) \
	$(PLUGINS_CFLAGS) \
	$(NULL)

plugins_libadd = \
	$(PLUGINS_LIBS) \
	$(NULL)





if ENABLE_PLUGIN_MIXROOM
plugin_LTLIBRARIES += plugins/libjanus_mixroom.la
plugins_libjanus_mixroom_la_SOURCES = plugins/janus_mixroom.c
plugins_libjanus_mixroom_la_CFLAGS = $(plugins_cflags)
plugins_libjanus_mixroom_la_LDFLAGS = $(plugins_ldflags)
plugins_libjanus_mixroom_la_LIBADD = $(plugins_libadd)
conf_DATA += conf/janus.plugin.mixroom.jcfg.sample
EXTRA_DIST += conf/janus.plugin.mixroom.jcfg.sample
endif






##
# Configuration
##

configs:
	$(MKDIR_P) $(DESTDIR)$(confdir)
	$(foreach config,$(conf_DATA),cp "$(CURDIR)/$(config)" "$(DESTDIR)$(confdir)/$(notdir $(basename $(config) .sample))";)

##
# Extra cleanup
##

clean-local:
	
