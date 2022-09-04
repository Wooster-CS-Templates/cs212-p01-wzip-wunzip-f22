wunzip:
	gcc wunzip.c -Wall -Werror -o wunzip

test:
	./test-wunzip.sh

gh-test: clean wunzip test

clean:
	rm -f wunzip
	rm -rf tests-out