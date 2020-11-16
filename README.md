# OneLineArmy
We call it as one line but it has a couple of lines....don't Think me bad ;)

### Before we start !
Tools I've used here :
- Httpx
- Assetfinder
- Subfinder
- Amass
- Gau
- Waybackurls
- ffuf
- qsreplace
- Linkfinder
- Secretfinder

## Paste these in your .bashrc file

### Grab all the subdoamins with this

Useage : subsof domain
```bash
subsof(){
        echo $1 | assetfinder -subs-only | sort -u >> asub.txt;
        echo $1 | subfinder -silent | sort -u >> subf.txt;
        cat asub.txt subf.txt | sort -u >> sub.txt ;
        rm -rf asub.txt subf.txt;
        amass enum -nf sub.txt -d $1 -noalts -passive >> amass.txt;
        cat subs.txt amass.txt | sort -u >> subs.txt;
        rm -rf amass.txt sub.txt;
        cat subs.txt | httpx -silent | sed 's/https\?:\/\///' >> alive-subs.txt
}
```

### Get js files with the output of gau or waybackurls or something else

Useage : getjs allurls.txt

```bash
getjs(){
        cat $1 | grep '\.js$' | httpx -status-code -mc 200 -content-type -no-color -silent | grep 'application/javascript' | sed -e 's/\[application\/javascript]//g' | sed 's/\[200]//g' | tee -a js.txt
}
```
### OpenRedirect Oneliner :)
```bash
waybackurls YOUR_TARGET | sort -u | qsreplace https://google.com > redirffuf.txt; ffuf -u FUZZ -w redirffuf.txt -mr google.com
```
### Dirsearch Alias

Useage : dirsearch -u evil.com
```bash
alias dirsearch="python3 /path/to/dirsearch/dirsearch.py -x 301,302,304,400,401,404,500 -e php,sql,html,js,json,asp,aspx,pl,zip,txt,tar,jsp,swf,log,rar"
```

### Grab all Urls and Endpoints with Gau , Waybacurls and httpx

Useage : grab_url domain

```bash
grab_url(){
          echo $1 | gau -subs | sort -u | tee -a gau.txt
          echo $1 | waybackurls| sort -u | tee -a waybacks.txt;
          cat gau.txt waybacks.txt | sort -u |allurls.txt
}
```
### Grab urls and Seek for Sensitive Info. from OTX

Useage: otx domain.site

Thanks [@Virdoex_hunter](https://twitter.com/Virdoex_hunter)

```bash
otx()
{
    gron "https://otx.alienvault.com/otxapi/indicator/hostname/url_list/$1?limit=100&page=1" | grep "\burl\b" | gron --ungron | jq | grep http | tr -d '"' | tr -d 'url:' | tee -a otx.txt;
}
```

### Give your js files as input here to get urls and juicy contents 
 #### Nothing fancy...Just,merging 2 tools

Useage : jsfun doamin/*.js

```bash
jsfun(){
        mkdir -p js;
        cat $1 | while read url; do linkfinder -d -i $url -o cli;done >> js/linkfinder.txt;
        cat $1 | while read url; do secretfinder -i $url -o cli;done >> js/secrets.txt;
}
  ```
