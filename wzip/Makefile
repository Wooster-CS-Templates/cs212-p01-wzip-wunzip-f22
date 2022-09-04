wzip:
	gcc wzip.c -Wall -Werror -o wzip

test:
	./test-wzip.sh

clean:
	rm -f wzip
	rm -rf tests-out

gh-test: clean wzip test
