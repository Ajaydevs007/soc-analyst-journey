# Splunk BOTS v1 Attack Timeline

| Phase | Time | Activity | Evidence | Investigation |
|-------|------|----------|----------|---------------|
| 1 | TBD | Web Application Vulnerability Scanning | Source IP **40.80.148.42** generated 17,483 HTTP requests, with 13,415 requests targeting `/joomla/index.php/component/search/`. A total of 1,399 unique query strings indicate automated scanning/fuzzing. | investigations/01-web-application-vulnerability-scanning.md |