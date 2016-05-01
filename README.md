# Tools

## `rvc` - Ruby Version Check

Checks current Ruby version is an up-to-date stable version. Latest stable versions
are scraped at runtime from the official downloads page at https://www.ruby-lang.org/en/downloads/.

Exits with non-zero status code when you ought to update Ruby. Useful
for continuous integration and monitoring.

Example usage and output:

```bash
# Failure
$ ./rvc
Ruby version: 2.2.0
FAIL: Update Ruby to one of ["2.3.1", "2.2.5", "2.1.9"]

# Success
$ ./rvc
Ruby version: 2.3.1
PASS: Ruby up-to-date
```

[Download the `rvc` script](rvc) or copy the one-liner:

```bash
curl -s https://www.ruby-lang.org/en/downloads/ | pup ':parent-of(:contains("Stable releases")) a[href*="ruby-lang.org/pub/ruby/"] text{}' | tr '\n' ' ' | ruby -e 'current = RUBY_VERSION; puts "Ruby version: #{current}"; stables = gets.gsub("Ruby", "").split; update = !stables.include?(current); puts update ? "FAIL: Update Ruby to one of #{stables}" : "PASS: Ruby up-to-date"; exit !update'
```
