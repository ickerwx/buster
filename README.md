# buster - A Simple Directory Brute-Forcer

`buster` was written because I needed to do resource brute-forcing against web applications that always returned 200 when an unknown resource was requested. Lucky for me, the response size was always the same and I could use it to filter false positives. I wasn't aware of any tool that could take response size into account when running, so I wrote this little script.

## Usage

```
./buster -h
usage: buster [-h] -u URL -w WORDLIST [-p PROXY] [-s SIZE] [-t THREADS] [-a USERAGENT] [-f] [-r] [-v]

Brute force web directories, ignore responses of a certain size

optional arguments:
  -h, --help            show this help message and exit
  -u URL, --url URL     URL to scan, will be prefixed to each wordlist line
  -w WORDLIST, --wordlist WORDLIST
                        path to wordlist, one word per line
  -p PROXY, --proxy PROXY
                        send identified resources to this proxy. The proxy must do HTTP and HTTPS.
  -s SIZE, --size SIZE  If a reply is this size, ignore it. Pass multiple values comma-separated (100,123,3123)
  -t THREADS, --threads THREADS
  -a USERAGENT, --useragent USERAGENT
                        Set User-Agent
  -f, --addslash        append a slash to each request
  -r, --redirects       Follow redirects
  -v, --verbose         be more verbose, not used atm.
```

You will need to provide `-u` and `-w`, the rest is optional.

## About the proxy flag

This tool is supposed to do resource discovery, and it would be kind of nice to have everything that has been found available inside a tool like Burpsuite or ZAP. You can specify `-p/--proxy` to make buster send an additional request to every identified resource through your intercepting proxy. This saves a little time and is quite convenient. Example:

```
./buster -t 100 -u https://demo.example.net -w ../seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt -p localhost:8081
```
Everything that is discovered is additionally requested again through the proxy running at `localhost:8081`.

Note: the proxy flag will NOT make use of the proxy while brute-forcing. If you need this, use environment variables like `HTTP_PROXY`. This should work, although I didn't test it.

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