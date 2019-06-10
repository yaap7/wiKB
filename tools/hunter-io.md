# Hunter.io

A good python library is `pyhunter` ([https://github.com/VonStruddle/PyHunter](https://github.com/VonStruddle/PyHunter)) but a simple curl request is enough to use the API.

## finding emails from a domain name

A request is limited to 100 results.
And 10 results will consume 1 "request" in your monthly plan.

The free plan is limited to 50 "requests" so you are limited to 500 results (emails) per months.

```
curl 'https://api.hunter.io/v2/domain-search?domain=medium.com&api_key=<API_KEY>&limit=100&offset=0' > liste_emails_0-100.json
curl 'https://api.hunter.io/v2/domain-search?domain=medium.com&api_key=<API_KEY>&limit=100&offset=100' > liste_emails_100-200.json
... and so on.
```

