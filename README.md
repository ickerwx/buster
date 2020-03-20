# buster - A Simple Directory Brute-Forcer

`buster` was written because I needed to do resource brute-forcing against web applications that always returned 200 when an unknown resource was requested. Lucky for me, the response size was always the same and I could use it to filter false positives. I wasn't aware of any tool that could take response size into account when running, so I wrote this little script.

## Usage

```
./buster -h 
usage: buster [-h] -u URL -w WORDLIST [-s SIZE] [-t THREADS] [-a USERAGENT] [-f] [-r] [-v]

Brute force web directories, ignore responses of a certain size

optional arguments:
  -h, --help            show this help message and exit
  -u URL, --url URL     URL to scan, will be prefixed to each wordlist line
  -w WORDLIST, --wordlist WORDLIST
                        path to wordlist, one word per line
  -s SIZE, --size SIZE  If a reply is this size, ignore it. Pass multiple values comma-separated (100,123,3123)
  -t THREADS, --threads THREADS
  -a USERAGENT, --useragent USERAGENT
                        Set User-Agent
  -f, --addslash        append a slash to each request
  -r, --redirects          Follow redirects
  -v, --verbose         be more verbose, not used atm.
```

You will need to provide `-u` and `-w`, the rest is optional.

## Example Run

```
./buster -t 100 -u https://demo.example.net -w ../seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt
[200] https://demo.example.net/users (129)
[200] https://demo.example.net/source (130)
[401] https://demo.example.net/groups (130)
[403] https://demo.example.net/sources (131)
[200] https://demo.example.net/models (130)
(...snip...)
```

It will print `[return code] URL (response size)`.

## TODO

[ ] write results into a file, for now `cut` and piping is okay, though