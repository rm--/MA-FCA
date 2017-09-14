.PHONY: appendix
THESIS_FILE_NAME = thesis
MD_FILES = $(wildcard content/*.md | sort)
BUILD_DIR = build
APPENDIX_DIR = appendix

LATEX_ENGINE = xelatex

XELATEX_OPTS=\
	-no-pdf\
	-output-directory=$(BUILD_DIR)

PANDOC_OPTS=\
	--from=markdown\
	--to=latex\
	--listing\
	--smart\
	--chapters\
	--number-sections\
	--biblatex

thesis: tex
	$(LATEX_ENGINE) $(XELATEX_OPTS) $(BUILD_DIR)/$(THESIS_FILE_NAME).tex
	makeindex -s $(BUILD_DIR)/$(THESIS_FILE_NAME).ist -o $(BUILD_DIR)/$(THESIS_FILE_NAME).gls $(BUILD_DIR)/$(THESIS_FILE_NAME).glo
	biber --output_directory=$(BUILD_DIR) $(THESIS_FILE_NAME)
	$(LATEX_ENGINE) $(XELATEX_OPTS) $(BUILD_DIR)/$(THESIS_FILE_NAME).tex
	$(LATEX_ENGINE) -output-directory=$(BUILD_DIR) $(BUILD_DIR)/$(THESIS_FILE_NAME).tex
	mv $(BUILD_DIR)/$(THESIS_FILE_NAME).pdf .

tex: appendix
	pandoc $(PANDOC_OPTS)\
		--latex-engine=$(LATEX_ENGINE)\
		--template=config/template.tex\
		--include-in-header=config/listings-setup.tex\
		config/metadata.md $(MD_FILES)\
		--output $(BUILD_DIR)/$(THESIS_FILE_NAME).tex

appendix:
	pandoc $(PANDOC_OPTS)\
		--latex-engine=$(LATEX_ENGINE)\
		content/appendix/APPENDIX.md\
		--output content/include/APPENDIX.tex

clean:
	rm -f $(THESIS_FILE_NAME).pdf
	rm -fr $(BUILD_DIR)/*

fast: tex
	$(LATEX_ENGINE) -output-directory=$(BUILD_DIR) $(BUILD_DIR)/$(THESIS_FILE_NAME).tex
	mv $(BUILD_DIR)/$(THESIS_FILE_NAME).pdf .


open:
ifeq ($(shell uname),Darwin)
	open thesis.pdf &
else
	xdg-open thesis.pdf &
endif

clear-biber:
	rm -rf `biber --cache`


help:
	@echo "Use 'make <target>' by choosing one of the following as <target>:"
	@echo "           clean           to remove all thesis related output files"
	@echo "           clear-biber     to clear biber cache"
	@echo "           fast            to build fast w/o bib/glossar etc linking"
	@echo "           help            to show all possible targets"
	@echo "           open            to open thesis PDF"
	@echo "           tex             to build LaTeX files from markdown"
	@echo "           thesis          to build a new thesis version"
