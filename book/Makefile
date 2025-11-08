# Makefile for "Drop Test" Book
# Build system using Pandoc

# Variables
CHAPTERS := $(sort $(wildcard chapters/[0-9]*.md))
APPENDICES := $(sort $(wildcard chapters/appendices/*.md))
ALL_MD := $(CHAPTERS) $(APPENDICES)
METADATA := metadata.yaml
OUTPUT_DIR := output
IMAGES := images
TEMPLATES := templates

# Pandoc options
PANDOC := pandoc
PANDOC_PDF_OPTS := --pdf-engine=xelatex --toc --number-sections
PANDOC_EPUB_OPTS := --toc --epub-cover-image=$(IMAGES)/cover.jpg
PANDOC_HTML_OPTS := --toc --standalone --self-contained
PANDOC_COMMON_OPTS := --resource-path=.:$(IMAGES)

# Targets
.PHONY: all pdf epub html docx clean wordcount help

all: pdf epub html ## Build all formats (PDF, EPUB, HTML)

pdf: $(OUTPUT_DIR)/pdf/book.pdf ## Build PDF

epub: $(OUTPUT_DIR)/epub/book.epub ## Build EPUB

html: $(OUTPUT_DIR)/html/book.html ## Build HTML

docx: $(OUTPUT_DIR)/docx/book.docx ## Build DOCX (for editing)

# Build PDF using XeLaTeX
$(OUTPUT_DIR)/pdf/book.pdf: $(METADATA) $(ALL_MD) | $(OUTPUT_DIR)/pdf
	@echo "Building PDF..."
	$(PANDOC) $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		$(PANDOC_PDF_OPTS) \
		$(PANDOC_COMMON_OPTS)
	@echo "PDF built: $@"

# Build EPUB
$(OUTPUT_DIR)/epub/book.epub: $(METADATA) $(ALL_MD) | $(OUTPUT_DIR)/epub
	@echo "Building EPUB..."
	$(PANDOC) $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		$(PANDOC_EPUB_OPTS) \
		$(PANDOC_COMMON_OPTS)
	@echo "EPUB built: $@"

# Build HTML
$(OUTPUT_DIR)/html/book.html: $(METADATA) $(ALL_MD) | $(OUTPUT_DIR)/html
	@echo "Building HTML..."
	$(PANDOC) $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		$(PANDOC_HTML_OPTS) \
		$(PANDOC_COMMON_OPTS)
	@echo "HTML built: $@"

# Build DOCX
$(OUTPUT_DIR)/docx/book.docx: $(METADATA) $(ALL_MD) | $(OUTPUT_DIR)/docx
	@echo "Building DOCX..."
	$(PANDOC) $(METADATA) $(CHAPTERS) $(APPENDICES) \
		-o $@ \
		--toc \
		$(PANDOC_COMMON_OPTS)
	@echo "DOCX built: $@"

# Create output directories
$(OUTPUT_DIR)/pdf:
	mkdir -p $(OUTPUT_DIR)/pdf

$(OUTPUT_DIR)/epub:
	mkdir -p $(OUTPUT_DIR)/epub

$(OUTPUT_DIR)/html:
	mkdir -p $(OUTPUT_DIR)/html

$(OUTPUT_DIR)/docx:
	mkdir -p $(OUTPUT_DIR)/docx

# Word count
wordcount: ## Count words in all chapters
	@echo "Word count by chapter:"
	@for file in $(CHAPTERS); do \
		echo -n "$$(basename $$file): "; \
		wc -w < $$file; \
	done
	@echo "---"
	@echo -n "Total words: "
	@cat $(CHAPTERS) $(APPENDICES) | wc -w

# Clean generated files
clean: ## Remove all generated output files
	rm -rf $(OUTPUT_DIR)/*
	@echo "Cleaned output directory"

# Help
help: ## Show this help message
	@echo "Available targets:"
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "  %-15s %s\n", $$1, $$2}'

# Check for required tools
check: ## Check if required tools are installed
	@echo "Checking for required tools..."
	@command -v pandoc >/dev/null 2>&1 || { echo "Error: pandoc not found. Install it first."; exit 1; }
	@command -v xelatex >/dev/null 2>&1 || { echo "Warning: xelatex not found. PDF generation will fail."; }
	@echo "All required tools are available."

# Watch mode (requires entr or similar)
watch: ## Watch for changes and rebuild (requires entr)
	@echo "Watching for changes... (press Ctrl-C to stop)"
	ls $(METADATA) $(ALL_MD) | entr -c make pdf

.DEFAULT_GOAL := help
