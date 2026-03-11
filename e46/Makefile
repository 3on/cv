TEX      := cv
TEXBIN   := /Library/TeX/texbin
XELATEX  := $(shell command -v xelatex 2>/dev/null || echo $(TEXBIN)/xelatex)
TLMGR    := $(shell command -v tlmgr   2>/dev/null || echo $(TEXBIN)/tlmgr)

PACKAGES := smartdiagram pgf biblatex biber logreq unicode-math \
            textpos metalogo tex-gyre lato xifthen ifmtarg parskip xstring

.PHONY: all install clean open preview

all: install $(TEX).pdf

$(TEX).pdf: $(TEX).tex friggeri-cv.cls bibliography.bib
	$(XELATEX) -interaction=nonstopmode -halt-on-error $(TEX).tex || true
	$(XELATEX) -interaction=nonstopmode -halt-on-error $(TEX).tex
	@test -s $(TEX).pdf || (echo "ERROR: $(TEX).pdf was not produced"; exit 1)

install:
	@if ! command -v xelatex >/dev/null 2>&1 && [ ! -x $(TEXBIN)/xelatex ]; then \
		echo "==> Installing BasicTeX via Homebrew (this may take a few minutes)..."; \
		brew install --cask basictex; \
		eval "$$(/usr/libexec/path_helper -s)"; \
	fi
	@echo "==> Ensuring TeX packages are up to date..."
	@sudo $(TLMGR) update --self 2>/dev/null || true
	@sudo $(TLMGR) install $(PACKAGES) 2>/dev/null || true

clean:
	rm -f $(TEX).aux $(TEX).log $(TEX).bcf $(TEX).run.xml \
	      $(TEX).bbl $(TEX).blg $(TEX).out $(TEX).pdf

open: all
	open $(TEX).pdf

preview: open
