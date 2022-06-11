.PHONY: all clean

all: client

client: client.c
	$(CC) -O2 -Wall -o $@ $^

clean:
	-rm -f client
	-rm -f *~
