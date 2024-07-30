FNB := $(shell find . -name "[0-9]*.ipynb" | sort)
FCSS := $(shell find . -name "*.css" | sort)
FILE = U3_Classification_trees

all: $(FILE).pdf

$(FILE).html: $(FNB) $(FCSS)
	nbmerge $(FNB) -o $(@:.html=.ipynb)
	jupyter-nbconvert \
		--TemplateExporter.extra_template_basedirs=../ \
		--template sintpl \
		--to html \
		--embed-images \
		--Application.log_level=0 \
		$(@:.html=.ipynb)

# --Application.show_config=True
# --NbConvertApp.show_config_json=True

$(FILE)_pw.pdf: $(FILE).html
	playwright pdf \
		--browser chromium \
		--channel chrome \
		--wait-for-timeout 3000 \
		--viewport-size "1280, 720" \
		$< $@

# playwright help pdf

define TEX
\documentclass[landscape]{article}\
\usepackage[a4paper,bindingoffset=0mm,left=20mm,right=20mm,top=10mm,bottom=15mm,footskip=12.0pt]{geometry}\
\usepackage{pdfpages,fancyhdr,lastpage}\
\pagestyle{fancyplain}\renewcommand{\headrulewidth}{0pt}\
\lfoot{\textsf{Alfons Juan, Jorge Civera, SIN 2024/25}}\
\cfoot{\textsf{\thepage\space/\space\pageref{LastPage}}}\
\rfoot{\textsf{DSIC, ETSINF, UPV}}\
\begin{document}\
\includepdf[pages=-,pagecommand={}]{$(FILE)_pw.pdf}\
\end{document}
endef

$(FILE).pdf: $(FILE)_pw.pdf
	pdflatex -jobname=$(FILE) "$(TEX)"
	pdflatex -jobname=$(FILE) "$(TEX)"
	-rm $(FILE).aux $(FILE).log

clean:
	-rm -rf $(FILE)*

.PHONY: all

