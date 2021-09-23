# Nuclei Vulnerability Scanner

Nuclei is used to send requests across targets based on a template leading to zero false positives and providing fast scanning on large number of hosts. Nuclei offers scanning for a variety of protocols including TCP, DNS, HTTP, File, etc. With powerful and flexible templating, all kinds of security checks can be modelled with Nuclei.

## Installation

### Install Using Go-Lang

	# apt install golang

	# GO111MODULE=on go get -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei

	# cp ~/go/bin/nuclei /usr/bin/

	# # nuclei -version

### Install Using brew

	# brew install nuclei

### Install Using docker

	# docker pull projectdiscovery/nuclei:latest

### Install Using Github Repo

	# git clone https://github.com/projectdiscovery/nuclei.git; \

	# cd nuclei/v2/cmd/nuclei; \

	# go build; \

	# mv nuclei /usr/local/bin/; \

	# nuclei -version;

### Updating Template

Nuclei has had built-in support for automatic update/download templates since version v2.4.0. Nuclei-Templates project provides a community-contributed list of ready-to-use templates that is constantly updated.

	# nuclei -ut

	# nuclei -update-templates

### Usage

Simple Scan with Default Templates `-t, -templates string[].`

	# nuclei -u https://example.com

Use a custom Template

	# nuclei -u https://example.com -t ~/nuclei-templates/cves/

Perform List Scan

	# nuclei -list urls.txt

Template Scan

	# nuclei -u https://example.com -tags cve

	# nuclei -u https://example.com -tags config -t ~/nuclei-templates/cves/exposures/

	# nuclei -w ~/nuclei-templates/workflows/wordpress-workflow.yaml -severity critical,high -list http_urls.txt

Exclude Template

	# nuclei -list urls.txt -exclude-templates cves/2020/CVE-2020-XXXX.yaml

	# nuclei -l urls.txt -t cves/ -etags xss

## Reference 

- [nuclei](https://github.com/projectdiscovery/nuclei#install-nuclei)