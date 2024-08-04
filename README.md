# Static Site for "A Treasury of World's Fair Art & Architecture"

## Building the static site

These steps build the static site by harvesting the dynamic site and then
applying updates.

### Crawl the dynamic site

```sh
# Writes to digital.lib.umd.edu directory
scripts/wget.sh
```

### Reorganize the file layout

```sh
# Static site ends up in html/worldsfairs
scripts/reorg.py
```

### Download Solr results

Create data/solr.json and data/inventory.csv

```sh
ssh -L 8983:localhost:8983 fedora.lib.umd.edu

curl 'http://localhost:8983/solr/fedora/select?fl=pid,handle,handlehttp,doType,displayTitle,hasPart,dmIdentifier,wfFair_facet,wfContent_facet&fq=((isMemberOfCollection:umd\:2))%20AND%20((doStatus:Complete))&indent=on&q=*:*&rows=378&wt=json' > data/solr.json

scripts/inventory.py
```

### Build the new static browse.html

```sh
scripts/browse.py > html/worldsfairs/browse.html
```

### Fix embedded images which are too large

```sh
scripts/fiximages.py
```
