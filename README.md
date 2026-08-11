# Nginx Log Analyzer

A simple Bash-based command-line tool for analyzing Nginx access logs.

This project was created to practice basic shell scripting and common Unix command-line utilities such as `awk`, `sort`, `uniq`, `head`, `grep`, and `sed`.

## Features

The analyzer reads an Nginx access log and reports:

* Top 5 IP addresses with the most requests
* Top 5 most requested paths
* Top 5 response status codes
* Top 5 user agents

## Requirements

* macOS or Linux
* Bash
* Standard Unix command-line utilities

No external dependencies are required.

## Project Structure

```text
nginx-log-analyzer/
├── log-analyzer.sh
├── nginx-access.log
└── README.md
```

## Installation

Clone the repository:

```bash
git clone <YOUR-REPOSITORY-URL>
cd nginx-log-analyzer
```

Make the script executable:

```bash
chmod +x log-analyzer.sh
```

## Getting the Sample Log

Download the sample Nginx access log:

```bash
curl -L -o nginx-access.log "https://gist.githubusercontent.com/nilbuild/e66c3b9ea89a1a030d3b739eeeef22d0/raw/77fb3ac837a73c4f0206e78a236d885590b7ae35/nginx-access.log"
```

## Usage

Run the analyzer using the default log file:

```bash
./log-analyzer.sh
```

You can also specify a different log file:

```bash
./log-analyzer.sh /path/to/access.log
```

## Example Output

```text
========================================
        NGINX LOG ANALYZER
========================================
Log file: nginx-access.log

Top 5 IP addresses with the most requests:
   1000 45.76.135.253
    600 142.93.143.8
     50 178.128.94.113
     30 43.224.43.187
     20 192.168.1.10

Top 5 most requested paths:
   1000 /api/v1/users
    600 /api/v1/products
     50 /api/v1/orders
     30 /api/v1/payments
     20 /api/v1/reviews

Top 5 response status codes:
   1000 200
    600 404
     50 500
     30 401
     20 304

Top 5 user agents:
   ...
```

## How It Works

### IP Addresses

The first field of an Nginx access log contains the client IP address.

```bash
awk '{print $1}' nginx-access.log
```

The results are then counted and sorted:

```bash
awk '{print $1}' nginx-access.log | sort | uniq -c | sort -nr | head -5
```

### Requested Paths

The HTTP request is contained inside quotation marks. `awk -F'"'` is used to extract it, and another `awk` extracts the requested path.

```bash
awk -F'"' '{print $2}' nginx-access.log | awk '{print $2}'
```

Then the requests are counted:

```bash
awk -F'"' '{print $2}' nginx-access.log | awk '{print $2}' | sort | uniq -c | sort -nr | head -5
```

### Response Status Codes

The HTTP response status code is extracted from the appropriate log field:

```bash
awk '{print $9}' nginx-access.log
```

Then it is counted and sorted:

```bash
awk '{print $9}' nginx-access.log | sort | uniq -c | sort -nr | head -5
```

### User Agents

The User-Agent is located in the last quoted field of the standard Nginx combined log format:

```bash
awk -F'"' '{print $6}' nginx-access.log
```

The results can then be counted:

```bash
awk -F'"' '{print $6}' nginx-access.log | sort | uniq -c | sort -nr | head -5
```

## Commands Practiced

This project focuses on the following shell utilities:

| Command | Purpose                             |
| ------- | ----------------------------------- |
| `awk`   | Extract fields and process log data |
| `sort`  | Sort extracted values               |
| `uniq`  | Count duplicate values              |
| `head`  | Limit the output to the top results |
| `grep`  | Search and filter text              |
| `sed`   | Transform and manipulate text       |

## Learning Goals

This project demonstrates basic skills in:

* Bash scripting
* Linux/macOS command-line tools
* Text processing
* Log analysis
* Unix pipelines
* Input validation
* Working with structured log files

## Future Improvements

Possible improvements include:

* Add command-line options such as `--top` and `--file`
* Add colored terminal output
* Support different log formats
* Add date/time filtering
* Analyze HTTP methods
* Detect suspicious IP addresses
* Generate reports in JSON or CSV
* Add automated tests
* Add GitHub Actions CI

## License

This project is intended for educational and practice purposes.

