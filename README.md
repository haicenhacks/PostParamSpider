# Post-ParamSpider

I developed this tool as a companion to [ParamSpider](https://github.com/devanshbatham/ParamSpider).
ParamSpider searches the wayback machine, looking for web pages with url parameters.
These can be ripe targets for local file includes, xss, and sqlinjection.
Unfortunately, my experience when bug bounty hunting is that many of these urls from the wayback machine are now dead, so I wanted a way to check these hosts to see if they were up, as well as try some simple injections.

# Useage

There are two files that must exist, the results from ParamSpider, and a file containing in-scope domains.
ParamSpider is a passive tool, but Post-ParamSpider is active and will make requests with fuzzed parameters.

`python PostParamSpider.py output/results.txt --in-scope output/scope.txt`


The above command will validate that the url is in scope.
It does not make any requests, and will filter `results.txt` down to only those in scope and save as `analysis/in-scope.txt`

Adding `--host-up` will validate that the host is up, by iterating through the `in-scope.txt` and confirming whether a request to the base url (ex: `blog.haicen.me` is up, when the url found by ParamSpider was `blog.haicen.me/posts/?page=1`)

Adding `--check-payload` will perform web requests on the full url, fuzzing the parameter identified by ParamSpider and searching for reflections and error messages.
