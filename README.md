# CrUX to CSV
Queries the CrUX API and saves the results in `stdout` or a CSV file.

## Motivation
Currently, historical data for a URL or origin is only available through BigQuery or using the [CrUX dashboard](https://web.dev/chrome-ux-report-data-studio-dashboard/). Saving the data to CSV allows you to store it cheaply, run queries against it and create visualizations.

## Prerequisites

**Ubuntu / Debian**
```sh
$ sudo apt update
$ sudo apt install -y jq
```

**MacOS**
```
$ brew install jq
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


**Save results to file**
```sh
./crux --output ./results.csv --append https://web.dev ${CRUX_API_KEY}
```


**Run for multiple URLs**
```sh
./crux --output ./results.csv --append https://developers.google.com ${CRUX_API_KEY}
./crux --output ./results.csv --append https://developer.mozilla.org ${CRUX_API_KEY}
```

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
