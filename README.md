# CrUX to CSV
Queries the CrUX API and saves the results in stdout or a CSV file.

## Motivation
Currently, historical data for an origin is only available through expensive BigQuery queries or using the [CrUX dashboard](https://web.dev/chrome-ux-report-data-studio-dashboard/). Saving the data to CSV allows you to store it cheaply, run queries against it and create visualizations.

## Prerequisites

**Ubuntu / Debian**
```sh
$ sudo apt update
$ sudo apt install -y jq
```

**OS X**
```
$ brew install jq
```

## Usage

**Basic usage**
```sh
./crux https://web.dev ${CRUX_API_KEY}
```


**Include histogram data (grouped as Good, NI and Poor)**
```sh
./crux https://web.dev ${CRUX_API_KEY} --full
```


**Save results to file**
```sh
./crux https://web.dev ${CRUX_API_KEY} --output ./results.csv --append
```


**Run for multiple origins**
```sh
./crux https://developers.google.com ${CRUX_API_KEY} --output ./results.csv --append
./crux https://developer.mozilla.org ${CRUX_API_KEY} --output ./results.csv --append
```

See help for usage instructions.
```sh
./crux --help
```

## Cron

You can configure a Cron job to execute the script daily and append the results to a CSV file.

**Example**
```
0 1 * * * /path/to/crux https://web.dev ${CRUX_API_KEY} --output /home/kevinfarrugia/results.csv --append > /dev/null 2>&1
```

## CrUX API key

To call the CrUX API you require your own (free) API key which may be obtained from [https://developers.google.com/](https://developers.google.com/web/tools/chrome-user-experience-report/api/guides/getting-started#APIKey).

## License and Copyright

This software is released under the terms of the [MIT license](https://github.com/kevinfarrugia/crux_csv/blob/main/LICENSE).
