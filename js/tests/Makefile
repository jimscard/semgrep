EMCC_DEFAULTS = \
   -sALLOW_MEMORY_GROWTH=1 \
   -sEXPORTED_RUNTIME_METHODS=UTF8ToString,stringToUTF8,stringToAscii,lengthBytesUTF8,getValue,setValue \
   -sMODULARIZE

include ../libyaml/Makefile.include
include ../libpcre/Makefile.include

default: test

clean:
	rm -rf dist

test: dist/test.js dist/test.wasm
	node index.test.js

dist/test.js dist/test.wasm: ../libyaml/dist/libyaml.o ../libpcre/dist/libpcre.a
	#SEMGREP_JS_TEST_BUILD=1 make -C .. languages # Enable this once https://github.com/llvm/llvm-project/pull/67715 is released (or if you've built clang yourself)
	make -C .. languages
	mkdir -p dist
	emcc \
		-O3 \
		$^ \
		$(EMCC_DEFAULTS) \
		-sEXPORTED_FUNCTIONS=_malloc,_free,$(YAML_EXPORTED_METHODS),$(PCRE_EXPORTED_METHODS) \
		-o $(word 1,$@)
