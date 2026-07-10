# AdGuard Home block profiles

These files are optional policy profiles for AdGuard Home. They are not loaded by
`values-production.yaml` automatically.

## Files

- `adult-malware-values.yaml`: adult, gambling, malware, phishing, DoH/VPN bypass
  protection lists plus safe search settings.
- `block-youtube-roblox-values.yaml`: strict DNS block for YouTube and Roblox.
- `family-safe-strict-values.yaml`: combines adult/malware protection with strict
  YouTube and Roblox blocking.
- `family-adult-gambling-rules.txt`: adult and gambling rules as a standalone
  DNS blocklist filter.
- `block-youtube-rules.txt`: copy/paste rules for AdGuard Home custom filtering.
- `block-roblox-rules.txt`: copy/paste rules for AdGuard Home custom filtering.

## Helm usage

Use one of these as the last values file so it overrides arrays intentionally:

```bash
helm upgrade --install adguard-home \
  helm/adguard-home/adguard-home-1.4.12.tgz \
  -n adguard-home \
  -f helm/adguard-home/values-production.yaml \
  -f helm/adguard-home/profiles/family-safe-strict-values.yaml
```

The chart only seeds `AdGuardHome.yaml` on first run. For an existing PVC, update
the running AdGuard Home UI/API or patch `/opt/adguardhome/conf/AdGuardHome.yaml`
directly, then restart the pod.

`values-production.yaml` uses the standalone `.txt` profiles as named DNS
blocklist filters from GitHub raw URLs. When these files change, push the same
commit to the GitHub mirror so the AdGuard Home pod can download the public raw
files. In AdGuard Home, open **Filters -> DNS blocklists** to enable/disable:

- `Family Adult Gambling Rules`
- `Block YouTube Strict`
- `Block Roblox Strict`

## Notes

- DNS cannot block every YouTube ad without breaking YouTube playback. The strict
  YouTube profile blocks YouTube itself, including `googlevideo.com`.
- Adult/child-safety filters are large. Watch pod memory after enabling them.
- To prevent bypass, configure clients to use only this DNS server and block
  outbound UDP/TCP 53 plus common DoH endpoints at the network edge.

## Source lists used

- OISD NSFW: `https://nsfw.oisd.nl/`
- HaGeZi NSFW: `https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/nsfw.txt`
- HaGeZi Gambling: `https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/gambling.txt`
- HaGeZi DoH/VPN/Proxy bypass: `https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/doh-vpn-proxy-bypass.txt`
- HaGeZi no-safe-search bypass: `https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/nosafesearch.txt`
- URLhaus hostfile: `https://urlhaus.abuse.ch/downloads/hostfile/`
- Phishing Army extended: `https://phishing.army/download/phishing_army_blocklist_extended.txt`
