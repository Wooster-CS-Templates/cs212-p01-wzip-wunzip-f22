wzip:
	gcc wzip.c -Wall -Werror -o wzip

test:
	./test-wzip.sh

gh-test: clean wzip test

clean:
	rm -f wzip
	rm -rf tests-out