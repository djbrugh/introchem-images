SOURCE = beaker-endothermic

make:
		pdflatex $(SOURCE).tex -output-format=pdf
		make clean

clean:
		rm -fr *.aux *.log *.svg

svg:
		pdf2svg $(SOURCE).pdf $(SOURCE).svg

png:
		make
		make svg
		inkscape -z -d300 -w512 --export-png=$(SOURCE).png $(SOURCE).svg
		make clean
		rm -fr $(SOURCE).pdf

