# CrUX to CSV
Queries the CrUX API and saves the results in `stdout` or a CSV file.

## Motivation
Currently, historical data for a URL or origin is only available through BigQuery or using the [CrUX dashboard](https://web.dev/chrome-ux-report-data-studio-dashboard/). Saving the data to CSV allows you to store it cheaply, run queries against it and create visualizations.

## Prerequisites

Requires `curl` and `jq` (`curl` is preinstalled on most systems).

**Ubuntu / Debian**
```sh
$ sudo apt update
$ sudo apt install -y curl jq
```

**MacOS**
```
$ brew install curl jq
```

## Usage

**Basic usage**
```sh
./crux https://web.dev/lcp ${CRUX_API_KEY}
```


**Origin-level data**
```sh
./crux --origin https://web.dev/lcp ${CRUX_API_KEY}
```

This will only return data for the origin and disregard the path, (i.e. https://web.dev)


**Include histogram data (grouped as Good, NI and Poor)**
```sh
./crux --full https://web.dev ${CRUX_API_KEY}
```


**Output as a Markdown table**
```sh
./crux --format md https://web.dev ${CRUX_API_KEY}
```

Accepted values are `csv` (default) and `md`.


**Save results to file**
```sh
./crux --output ./results.csv --append https://web.dev ${CRUX_API_KEY}
```


**Run for multiple URLs**
```sh
./crux --output ./results.csv --append https://developers.google.com ${CRUX_API_KEY}
./crux --output ./results.csv --append https://developer.mozilla.org ${CRUX_API_KEY}
```

For a longer list of URLs, use `--input` (below) instead of invoking the script once per URL.


**Run for a list of URLs from a file**
```sh
./crux --input ./urls.txt ${CRUX_API_KEY}
```

`urls.txt` should contain one URL per line, e.g.
```
https://developers.google.com
https://developer.mozilla.org
```

Note that `<url>` is omitted from the command line in this mode — only the API key is passed. `--input` cannot be combined with `--crawl`.


**Crawl a domain**
```sh
./crux --crawl https://web.dev ${CRUX_API_KEY}
```

Discovers URLs for the domain via `sitemap.xml` (following sitemap indexes), falling back to crawling links from the homepage (up to 2 levels deep) if no sitemap is found, and queries CrUX for each discovered URL, up to 1000 URLs. Only same-host `http(s)` URLs are considered, and duplicates are removed. `<url>` must include a scheme and host, e.g. `https://web.dev`. `--crawl` cannot be combined with `--origin` or `--input`.

To avoid exceeding CrUX API rate limits, all requests (including `--crawl` and `--input` runs) are automatically throttled to 150 queries per minute.


**Verbose output**
```sh
./crux --verbose https://web.dev ${CRUX_API_KEY}
```

Prints the underlying `curl` requests and progress information to help with debugging.


See help for usage instructions.
```sh
./crux --help
```

## Cron

You can configure a Cron job to execute the script daily and append the results to a CSV file.

**Example**
```
0 1 * * * /path/to/crux --output /home/kevinfarrugia/results.csv --append https://web.dev ${CRUX_API_KEY} > /dev/null 2>&1
```

## CrUX API key

To call the CrUX API you require your own (free) API key which may be obtained from [https://developers.google.com/](https://developers.google.com/web/tools/chrome-user-experience-report/api/guides/getting-started#APIKey).

## License and Copyright

This software is released under the terms of the [MIT license](https://github.com/kevinfarrugia/crux_csv/blob/main/LICENSE).
