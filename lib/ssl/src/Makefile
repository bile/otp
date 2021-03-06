#
# %CopyrightBegin%
#
# Copyright Ericsson AB 1999-2012. All Rights Reserved.
#
# The contents of this file are subject to the Erlang Public License,
# Version 1.1, (the "License"); you may not use this file except in
# compliance with the License. You should have received a copy of the
# Erlang Public License along with this software. If not, it can be
# retrieved online at http://www.erlang.org/.
#
# Software distributed under the License is distributed on an "AS IS"
# basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
# the License for the specific language governing rights and limitations
# under the License.
#
# %CopyrightEnd%
#

#

include $(ERL_TOP)/make/target.mk
include $(ERL_TOP)/make/$(TARGET)/otp.mk

# ----------------------------------------------------
# Application version
# ----------------------------------------------------
include ../vsn.mk
VSN=$(SSL_VSN)

# ----------------------------------------------------
# Release directory specification
# ----------------------------------------------------
RELSYSDIR = $(RELEASE_PATH)/lib/ssl-$(VSN)

# ----------------------------------------------------
# Common Macros
# ----------------------------------------------------

BEHAVIOUR_MODULES= \
        ssl_session_cache_api

MODULES= \
	ssl \
	ssl_alert \
	ssl_app \
	ssl_dist_sup\
	ssl_sup \
	inet_tls_dist \
	ssl_certificate\
	ssl_certificate_db\
	ssl_cipher \
	ssl_connection \
	ssl_connection_sup \
	ssl_debug \
	ssl_handshake \
	ssl_manager \
	ssl_session \
	ssl_session_cache \
	ssl_record \
	ssl_ssl2 \
	ssl_ssl3 \
	ssl_tls1 \
	ssl_tls_dist_proxy

INTERNAL_HRL_FILES = \
	 ssl_debug.hrl \
	 ssl_alert.hrl ssl_cipher.hrl ssl_handshake.hrl ssl_internal.hrl \
	 ssl_record.hrl

ERL_FILES= \
	$(MODULES:%=%.erl) \
	$(BEHAVIOUR_MODULES:%=%.erl)


TARGET_FILES= $(MODULES:%=$(EBIN)/%.$(EMULATOR))

BEHAVIOUR_TARGET_FILES= $(BEHAVIOUR_MODULES:%=$(EBIN)/%.$(EMULATOR))

APP_FILE= ssl.app
APPUP_FILE= ssl.appup

APP_SRC= $(APP_FILE).src
APP_TARGET= $(EBIN)/$(APP_FILE)
APPUP_SRC= $(APPUP_FILE).src
APPUP_TARGET= $(EBIN)/$(APPUP_FILE)

# ----------------------------------------------------
# FLAGS
# ----------------------------------------------------
EXTRA_ERLC_FLAGS = +warn_unused_vars
ERL_COMPILE_FLAGS += -I$(ERL_TOP)/lib/kernel/src \
	-pz $(EBIN) \
	-pz $(ERL_TOP)/lib/public_key/ebin \
	$(EXTRA_ERLC_FLAGS) -DVSN=\"$(VSN)\"


# ----------------------------------------------------
# Targets
# ----------------------------------------------------

$(TARGET_FILES): $(BEHAVIOUR_TARGET_FILES)

debug opt: $(TARGET_FILES) $(APP_TARGET) $(APPUP_TARGET) 

clean:
	rm -f $(TARGET_FILES) $(APP_TARGET) $(APPUP_TARGET) 
	rm -f errs core *~

$(APP_TARGET):	$(APP_SRC) ../vsn.mk
	$(vsn_verbose)sed -e 's;%VSN%;$(VSN);' $< > $@

$(APPUP_TARGET):	$(APPUP_SRC) ../vsn.mk
	$(vsn_verbose)sed -e 's;%VSN%;$(VSN);' $< > $@

docs:


# ----------------------------------------------------
# Release Target
# ---------------------------------------------------- 
include $(ERL_TOP)/make/otp_release_targets.mk

release_spec: opt
	$(INSTALL_DIR) "$(RELSYSDIR)/src"
	$(INSTALL_DATA) $(ERL_FILES) $(INTERNAL_HRL_FILES) "$(RELSYSDIR)/src"
	$(INSTALL_DIR) "$(RELSYSDIR)/ebin"
	$(INSTALL_DATA) $(BEHAVIOUR_TARGET_FILES) $(TARGET_FILES) $(APP_TARGET) \
	$(APPUP_TARGET) "$(RELSYSDIR)/ebin"

release_docs_spec:

# ----------------------------------------------------
# Dependencies
# ----------------------------------------------------
$(EBIN)/inet_tls_dist.$(EMULATOR): ../../kernel/include/net_address.hrl  ../../kernel/include/dist.hrl  ../../kernel/include/dist_util.hrl
$(EBIN)/ssl.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl ssl_handshake.hrl ../../public_key/include/public_key.hrl
$(EBIN)/ssl_alert.$(EMULATOR): ssl_alert.hrl ssl_record.hrl  
$(EBIN)/ssl_certificate.$(EMULATOR): ssl_internal.hrl ssl_alert.hrl ssl_handshake.hrl ../../public_key/include/public_key.hrl
$(EBIN)/ssl_certificate_db.$(EMULATOR): ssl_internal.hrl ../../public_key/include/public_key.hrl ../../kernel/include/file.hrl
$(EBIN)/ssl_cipher.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl ssl_handshake.hrl ssl_alert.hrl ../../public_key/include/public_key.hrl
$(EBIN)/ssl_connection.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl ssl_handshake.hrl ssl_alert.hrl ../../public_key/include/public_key.hrl
$(EBIN)/ssl_handshake.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl ssl_handshake.hrl ssl_alert.hrl ../../public_key/include/public_key.hrl
$(EBIN)/ssl_manager.$(EMULATOR): ssl_internal.hrl ssl_handshake.hrl ../../kernel/include/file.hrl
$(EBIN)/ssl_record.$(EMULATOR):  ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl ssl_handshake.hrl ssl_alert.hrl
$(EBIN)/ssl_session.$(EMULATOR):  ssl_internal.hrl ssl_handshake.hrl
$(EBIN)/ssl_session_cache.$(EMULATOR):  ssl_internal.hrl ssl_handshake.hrl
$(EBIN)/ssl_session_cache_api.$(EMULATOR):  ssl_internal.hrl ssl_handshake.hrl
$(EBIN)/ssl_ssl3.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl
$(EBIN)/ssl_tls1.$(EMULATOR): ssl_internal.hrl ssl_record.hrl ssl_cipher.hrl


