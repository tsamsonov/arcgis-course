HTML = docs
BUILD = build
OUTPUT_FILENAME = index
METADATA = metadata.yml
CHAPTERS = chapters/*.md
TOC = --toc --toc-depth=2
TEMPLATE = --template template.html
IMAGES_FOLDER = images
LATEX_CLASS = report
MATH_FORMULAS = --webtex
CSS_FILE = template.css
CSS_ARG = --css=$(CSS_FILE)
ARGS = $(TOC) $(MATH_FORMULAS) $(TEMPLATE) $(CSS_ARG)

all: book

book: epub html pdf

clean:
	rm -r $(BUILD)

epub: $(BUILD)/epub/$(OUTPUT_FILENAME).epub

html: $(HTML)/$(OUTPUT_FILENAME).html

pdf: $(BUILD)/pdf/$(OUTPUT_FILENAME).pdf

$(BUILD)/epub/$(OUTPUT_FILENAME).epub: $(METADATA) $(CHAPTERS)
	mkdir -p $(BUILD)/epub
	pandoc $(ARGS) -S --epub-metadata=$(METADATA) --epub-cover-image=$(COVER_IMAGE) -o $@ $^

$(HTML)/$(OUTPUT_FILENAME).html: $(METADATA) $(CHAPTERS)
	pandoc $(ARGS) --standalone --to=html5 -o $@ $^
	cp $(CSS_FILE) $(HTML)/$(CSS_FILE)

$(BUILD)/pdf/$(OUTPUT_FILENAME).pdf: $(METADATA) $(CHAPTERS)
	mkdir -p $(BUILD)/pdf
	pandoc $(ARGS) -V documentclass=$(LATEX_CLASS) -o $@ $^
