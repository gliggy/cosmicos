
COSMIC_SITE=$(PWD)/../site/cosmicos

HAXELANG=js
#HAXELANG=php
#HAXELANG=cpp

default: audio eval scrawl

audio:
	haxe -$(HAXELANG) CosmicAudio.$(HAXELANG) -main cosmicos.Sound

eval:
	haxe -$(HAXELANG) CosmicEval.$(HAXELANG) -main cosmicos.Evaluate

scrawl:
	haxe -$(HAXELANG) SpiderScrawl.$(HAXELANG) -main cosmicos.SpiderScrawl

ogham:
	haxe -$(HAXELANG) OghamStyle.$(HAXELANG) -main cosmicos.OghamStyle

font: scrawl
	mkdir -p vectors
	NODE_PATH=$(PWD) node ./font/generate_glyphs.js
	cd vectors && ../font/glyphs_to_svg.sh
	cd font && fontcustom compile

font-install:
	test -e $(COSMIC_SITE)/_config.yml
	cp -v font/app/assets/fonts/fontcustom_*.woff $(COSMIC_SITE)/fonts/cosmic_spider.woff
	cp -v font/app/assets/fonts/fontcustom_*.eot $(COSMIC_SITE)/fonts/cosmic_spider.eot
	cp -v font/app/assets/fonts/fontcustom_*.ttf $(COSMIC_SITE)/fonts/cosmic_spider.ttf
	cp -v font/app/assets/fonts/fontcustom_*.svg $(COSMIC_SITE)/fonts/cosmic_spider.svg

glyph:
	NODE_PATH=$(PWD) node ./font/msg2glyphs.js ../msg/unwrapped.txt | grep "<br" | sed "s|^|<span>|" | sed "s|<br|</span><br|" | tee ../msg/glyph.txt
	head -n 25 ../msg/glyph.txt > ../msg/glyph_head.txt

glyph-install:
	test -e $(COSMIC_SITE)/_config.yml
	cp -v ../msg/glyph*.txt $(COSMIC_SITE)/_includes

oghams:
	NODE_PATH=$(PWD) node ./font/msg2ogham.js ../msg/unwrapped.txt | grep "<br" | sed "s|^|<span>|" | sed "s|<br|</span><br|" | tee ../msg/glyph.txt
	head -n 25 ../msg/glyph.txt > ../msg/glyph_head.txt


yaml:
	# npm install -g json2yaml
	json2yaml assem2.json > assem2.yaml
	test -e $(COSMIC_SITE)/_config.yml
	cat $(COSMIC_SITE)/_config_base.yml assem2.yaml | sed "s/^---/msg:/" > $(COSMIC_SITE)/_config.yml

primer:
	NODE_PATH=$(PWD) node ./assemble/primer.js assem.json

console:
	echo "Do: cos = require('cos')"
	NODE_PATH=$(PWD):$(PWD)/assemble node


test-eval:
	NODE_PATH=$(PWD) node ./tests/test_eval.js

test: test-eval
	NODE_PATH=$(PWD) node ./tests/test_audio.js
	NODE_PATH=$(PWD) node ./tests/test_scrawl.js
	rm -f node.txt node.wav tests/test_scrawl.png

