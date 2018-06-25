OTT_LOC    = ../ott

SOURCE_OTT = $(OTT_LOC)/*.ott

OTT_OPTS  = -tex_show_meta false
OTT_FILES  = $(OTT_LOC)/syntax.ott

MAIN = doc

RULESFILE = ott-rules.tex

SECTION_LOC = sections

AUTOGEN = $(SECTION_LOC)/systemfc.mng $(SECTION_LOC)/systemfcpro.mng $(SECTION_LOC)/defer_type_error.mng $(SECTION_LOC)/systemfc_kind_equality.mng $(SECTION_LOC)/zero_cost_coercion.mng $(SECTION_LOC)/levity_poly.mng

WARN_MSG = "%%% !!! WARNING: AUTO GENERATED. DO NOT MODIFY !!! %%%\n"

all: $(MAIN).pdf

$(MAIN).pdf: $(MAIN).mng $(wildcard *.tex) $(AUTOGEN) $(MAIN).bib $(SECTION_LOC)/introduction.tex $(SECTION_LOC)/implementation.tex
	@latexmk -pdf $(MAIN).mng

$(RULESFILE): $(OTT_FILES)
	ott -tex_wrap false $(OTT_OPTS) -o $@ $^
	@perl -pi -e 'print $(WARN_MSG) if $$. == 1' $@
	@if grep '<<no parses (' $@ >/dev/null 2>&1 && \
		[ -z "$(DONTSTOP)" ]; then \
			echo; \
			echo "***** OTT PARSE ERROR(S) *****"; \
			grep -n '<<no parses (' $@; \
			$(RM) $@; \
			exit 1; \
	fi >&2

%.mng: %.tex $(RULESFILE)
	ott -tex_wrap false $(OTT_OPTS) -tex_filter $*.tex $*.mng $(OTT_FILES)
	@perl -pi -e 'print $(WARN_MSG) if $$. == 1' $@
	@if grep '<<no parses (' $@ >/dev/null 2>&1 && \
		[ -z "$(DONTSTOP)" ]; then \
			echo; \
			echo "***** OTT PARSE ERROR(S) *****"; \
			grep -n '<<no parses (' $@; \
			$(RM) $@; \
			exit 1; \
	fi >&2

export TEXINPUTS := .:./Styles//:${TEXINPUTS}
export BSTINPUTS := .:./Styles//:${BSTINPUTS}
export SHELL := /bin/bash

all: $(MAIN).pdf

clean:
	@latexmk -c
	@rm -Rf $(MAIN).pdf auto/ *.dvi *.aux *.log *.bbl *.blg *.toc *.out *.fls *.haux *.fdb_latexmk *.lof *.lot *.loe *~

distclean: clean
	$(RM) doc.pdf
	$(RM) *-eps-converted-to.pdf

