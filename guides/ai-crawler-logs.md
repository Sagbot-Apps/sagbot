# See which AI crawlers really visit your site

Your robots.txt says which AI crawlers may read your site. Your access log says which ones did, how often, and what they got back. This guide shows where to find that log, how to count AI crawler visits with a few shell commands, and how to tell a real crawler from one that only borrows its name.

[Check your robots.txt with SagBot](https://sagbot.app/) · [Full guide on sagbot.app](https://sagbot.app/guides/ai-crawler-logs) · [README](../README.md#guides)

## Why it matters who actually visits

An AI assistant can only quote a page one of its crawlers has read. If none of them has fetched your pricing page, no answer will cite it, however well it’s written.

Your robots.txt, and the AI bots table in a SagBot report, say which crawlers may read your site. They don’t say which ones came. Your server’s access log does: every request, with the address it came from, the page, the status code and the user agent that names the crawler.

## Which AI crawlers exist, and why they come

AI crawlers come for one of three reasons. Training crawlers collect pages to train models. Search crawlers build the index an assistant searches when it answers. User agents fetch a page live because someone just asked the assistant about it. Visits from the last two are the ones that can lead to a citation.

OpenAI sends `GPTBot` for training, `OAI-SearchBot` for search and `ChatGPT-User` for live requests. Anthropic sends `ClaudeBot`, `Claude-SearchBot` and `Claude-User`; Perplexity sends `PerplexityBot` and `Perplexity-User`. Meta uses `meta-externalagent`, `meta-webindexer` and `meta-externalfetcher`, Amazon uses `Amazonbot`, `Amzn-SearchBot` and `Amzn-User`, and Mistral and DuckDuckGo send `MistralAI-User` and `DuckAssistBot`. `Bytespider` (ByteDance) and `CCBot` (Common Crawl, an open archive many models are trained on) collect training data.

Two names you won’t find in a log: `Google-Extended` and `Applebot-Extended` are robots.txt rules, not crawlers. Google fetches pages with `Googlebot` and Apple with `Applebot`; the Extended rules only decide whether what they fetched may be used for AI. Google’s agent for live user requests, `Google-Agent`, does appear by name.

## 1. Find your access log

On your own server the log is a plain text file. nginx writes `/var/log/nginx/access.log`. Apache writes `/var/log/apache2/access.log` on Debian and Ubuntu, and `/var/log/httpd/access_log` on Red Hat, Rocky and Alma. Older days are rotated into numbered or dated files next to it, the older ones compressed as `.gz`.

On shared or managed hosting, look in the hosting panel for raw access logs and download them. Some hosts keep only a few days, so save them regularly if you want a longer view.

Behind Cloudflare, open AI Crawl Control for your domain in the dashboard. It’s on every plan and shows requests per AI crawler, their status codes and the most requested paths, without any log files; on the free plan its metrics cover the last 24 hours. For the raw request lines, Logpush exports them to storage you choose. Cloudflare only sees hostnames that are proxied (the orange cloud); DNS-only records go straight to your server.

On Vercel, the project’s Logs page shows recent requests with their user agent, for an hour on Hobby and a day on Pro. A log drain (Pro and Enterprise) sends every request, including static files, to storage you choose.

## 2. Check that the log keeps the user agent

The user agent is what names the crawler, so every line needs it. nginx and Apache both have a format called `combined` that ends each line with the referrer and the user agent in quotes. nginx uses it unless you choose another; Apache defines it, and your `CustomLog` line decides whether it’s used.

Look at the last lines of the log with the first command below. If each one ends with a quoted browser or bot name, you’re set. If lines end with the status and a byte count, the server writes the Common Log Format, which has no user agent: set `combined` on the `access_log` line (nginx) or the `CustomLog` line (Apache), as below, and reload the server.

The commands in the next steps assume the `combined` layout. If you use your own `log_format` or `LogFormat`, keep `$http_user_agent` or `%{User-Agent}i` as the last quoted field, or adjust the commands.

```sh
tail -n 3 /var/log/nginx/access.log
```

```sh
access_log /var/log/nginx/access.log combined;
```

```sh
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
CustomLog ${APACHE_LOG_DIR}/access.log combined
```

## 3. Count visits per AI crawler

Paste the first block into a shell on the server. It sets `BOTS` to the AI crawler names from SagBot’s AI bots table and counts requests per crawler in the current log. Change the path if yours differs, for example `/var/log/apache2/access.log`.

The second block does the same across the rotated files, compressed or not; run it in the same shell so `BOTS` is still set. Training crawlers often lead the list. Visits from search and user agents are the ones tied to answers, so it’s worth noticing when they’re missing.

Prefer not to count by hand? [SagBot’s AI crawler log analyzer](https://sagbot.app/tools/ai-crawler-log-analyzer): it reads the log in your browser and does not send it to SagBot.

```sh
BOTS='GPTBot|OAI-SearchBot|ChatGPT-User|ClaudeBot|Claude-SearchBot|Claude-User'
BOTS="$BOTS|PerplexityBot|Perplexity-User|Google-Agent|Applebot|Bytespider|CCBot"
BOTS="$BOTS|meta-externalagent|meta-webindexer|meta-externalfetcher|Amazonbot"
BOTS="$BOTS|Amzn-SearchBot|Amzn-User|MistralAI-Training|MistralAI-Index"
BOTS="$BOTS|MistralAI-User|DuckAssistBot"
awk -F'"' '{print $6}' /var/log/nginx/access.log \
  | grep -Eo "$BOTS" | sort | uniq -c | sort -rn
```

```sh
zgrep -hE "${BOTS:?}" /var/log/nginx/access.log* \
  | awk -F'"' '{print $6}' | grep -Eo "$BOTS" | sort | uniq -c | sort -rn
```

## 4. Verify that a crawler is genuine

Anyone can send a request whose user agent says `GPTBot`, and scrapers do. To trust a visit, check the address it came from against the operator’s own list. The first command below lists the addresses that claimed to be `GPTBot`; swap in any other name.

OpenAI, Anthropic, Perplexity, Amazon, Apple and Common Crawl publish their crawler IP ranges in their bot documentation. Google and Bing can also be checked by reverse DNS: the address must resolve to a `googlebot.com`, `google.com` or `googleusercontent.com` name for Google, or `search.msn.com` for Bing, and that name must resolve back to the same address. The second block shows the check with Google’s own example address. Apple (`applebot.apple.com`) and Common Crawl (`crawl.commoncrawl.org`) pass the same test.

Meta and ByteDance publish no address list at the time of writing, so their visits can be counted but not verified.

```sh
awk '/GPTBot/ {print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn
```

```sh
host 66.249.66.1
host crawl-66-249-66-1.googlebot.com
```

## 5. Find pages that fail AI crawlers

A crawler that gets a 404 or a 500 leaves with nothing to quote, and a page that keeps failing may drop out of its index.

The block below lists the paths that answered AI crawlers with 404 or any 5xx, most frequent first; it needs `BOTS` from the count step in the same shell. Redirect moved pages to their new address, fix the errors, and take dead links out of your sitemap and internal links.

```sh
awk -F'"' -v bots="${BOTS:?}" '$6 ~ bots {
  split($3, f, " "); split($2, r, " ")
  if (f[1] == 404 || f[1] >= 500) print f[1], r[2]
}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -20
```

## What logs can’t show you

A page served from a CDN cache never reaches your server, so your own log undercounts. Use the CDN’s logs or crawler view for the full picture.

Cloudflare only sees proxied hostnames. A hostname set to DNS only shows no visits in AI Crawl Control, however many crawlers reach it.

The user agent alone can be faked, and the Common Log Format doesn’t record it at all.

Logs show visits, not citations. A crawler reading a page doesn’t mean an assistant will quote it; how often you’re cited is a separate measurement.

## Check who may read your site

A free scan reads your robots.txt and shows which AI crawlers it lets in and which it blocks, next to the rest of your site’s AI readiness.
