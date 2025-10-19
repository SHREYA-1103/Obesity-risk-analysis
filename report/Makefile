# ================================================
# Makefile for Modular LaTeX Report Project
# ================================================

TARGET = main
SRC_DIRS = . settings sections
TEX_FILES := $(shell find $(SRC_DIRS) -name '*.tex')
BIB_FILES := $(wildcard *.bib)

# Default rule: build the PDF
all: $(TARGET).pdf

$(TARGET).pdf: $(TEX_FILES) $(BIB_FILES)
	@echo "Building $(TARGET).pdf ..."
	pdflatex -interaction=nonstopmode $(TARGET).tex
	@if grep -q "Citation" $(TARGET).log; then \
		echo "Running BibTeX..."; \
		bibtex $(TARGET); \
		pdflatex -interaction=nonstopmode $(TARGET).tex; \
		pdflatex -interaction=nonstopmode $(TARGET).tex; \
	else \
		echo "No citations found, skipping BibTeX."; \
	fi
	@echo "Build finished: $(TARGET).pdf"

clean:
	@echo "Cleaning auxiliary files..."
	rm -f *.aux *.log *.bbl *.blg *.toc *.out *.lof *.lot *.fls *.fdb_latexmk
	@echo "Clean done."

cleanall: clean
	@echo "Removing PDF..."
	rm -f $(TARGET).pdf

watch:
	@echo "Watching for changes (press Ctrl+C to stop)..."
	latexmk -pdf -pvc $(TARGET).tex

view:
	@echo "Opening PDF..."
	@if command -v xdg-open >/dev/null 2>&1; then xdg-open $(TARGET).pdf; \
	elif command -v open >/dev/null 2>&1; then open $(TARGET).pdf; \
	else echo "Please open $(TARGET).pdf manually."; fi

.PHONY: all clean cleanall watch view
