#
# Devilspie2 po makefile
# $Id$
#

include ../Make.vars

SOURCES = config.c devilspie2.c script.c script_functions.c xutils.c error_strings.c

XG_ARGS = --keyword=_ --keyword=N_ -w 80 --package-name=${NAME} --package-version=${VERSION} --msgid-bugs-address=devspam@moreofthesa.me.uk
LANGUAGES = sv fr pt_BR nl ru fi ja it ka

LANGUAGES_MO = $(foreach currlang,$(LANGUAGES),$(currlang).mo)
LANGUAGES_PO = $(foreach currlang,$(LANGUAGES),$(currlang).po)

# Quiet!
$(eval $(call make_V,GETTEXT))
$(eval $(call make_V,MSGMERGE))
$(eval $(call make_V,MSGFMT,,$(LANGUAGES_PO)))

all: build

${NAME}.pot:
	$(V_GETTEXT)xgettext ${XG_ARGS} --directory=../src/ ${SOURCES} -d ${NAME} -o ./${NAME}.pot

update-pot: ${NAME}.pot

%.pox: %.po
	$(V_MSGMERGE)msgmerge -o $@ --previous $< ${NAME}.pot

build: $(LANGUAGES_MO)

$(LANGUAGES_MO): $(LANGUAGES_PO)
	$(V_MSGFMT)
	$(V_at)for po in ${LANGUAGES}; \
	do msgfmt -v --statistics -c -o t-$${po} $${po}.po && mv -f t-$${po} $${po}.mo; \
	done


install: all install-dirs
	for po in ${LANGUAGES}; \
	do install -m 0644 $${po}.mo ${LOCALEDIR}/$${po}/LC_MESSAGES/${NAME}.mo; \
	done

install-dirs:
	for po in ${LANGUAGES}; \
	do install -d ${LOCALEDIR}/$${po}/LC_MESSAGES; \
	done

uninstall:
	for po in ${LANGUAGES}; \
	do rm -f  ${LOCALEDIR}/$${po}/LC_MESSAGES/${NAME}.mo; \
	done

clean-build:
	rm -f *.mo

clean-pox:
	rm -f *.pox

clean: clean-build
	rm -f *~

.PHONY: all build install install-dirs uninstall clean clean-pox clean-build update-pot

