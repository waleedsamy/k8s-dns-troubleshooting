### k8s dns troubleshooting based on [https://rsmitty.github.io/KubeDNS-Tweaks/](https://rsmitty.github.io/KubeDNS-Tweaks/)

```bash
# Run dnsmasq
docker run --rm --name dnsmasq -it \
  gcr.io/google_containers/kube-dnsmasq-amd64:1.4 \
  --no-daemon \
  --cache-size=1000 \
  --no-resolv \
  --server=80.81.244.235 \
  --server=/cluster.local/127.0.0.1#10053 \
  --server=/in-addr.arpa/127.0.0.1#10053 \
  --server=/ip6.arpa/127.0.0.1#10053 \
  --log-facility=- \
  --log-queries \
  --address=/de.infra.svc.cluster.local/de.svc.cluster.local/de.cluster.local/de.tibet.traffics-switch.de/

# send dns queries to dnsmasq
docker exec -it dnsmasq apk add --update procps curl bind-tools; docker exec -it dnsmasq sh
 $ dig google.com @127.0.0.1
 $ dig cosmo-dev.traffics-switch.de @127.0.0.1
 $ dig cosmo-staging.traffics-switch.de @127.0.0.1
 $ dig connector-dev.traffics-switch.de @127.0.0.1
 $ dig connector-staging.traffics-switch.de @127.0.0.1
 $ dig elasticsearchlogs01.tibet.traffics-switch.de @127.0.0.1
 $ dig elasticsearchlogs01.tibet.traffics-switch.de.infra.svc.cluster.local @127.0.0.1

 # dump dnsmasq stats
 $ kill -s USR1 1
```
