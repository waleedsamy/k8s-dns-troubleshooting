### k8s dns troubleshooting

```
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

docker exec -it dnsmasq apk add --update procps curl bind-tools; docker exec -it dnsmasq sh
 $ dig google.com @127.0.0.1
 $ dig cosmo-dev.traffics-switch.de @127.0.0.1
 $ dig cosmo-staging.traffics-switch.de @127.0.0.1
 $ dig connector-dev.traffics-switch.de @127.0.0.1
 $ dig connector-staging.traffics-switch.de @127.0.0.1
 $ dig elasticsearchlogs01.tibet.traffics-switch.de @127.0.0.1
 $ dig elasticsearchlogs01.tibet.traffics-switch.de.infra.svc.cluster.local @127.0.0.1

 $ kill -s USR1 1 # dump stats

docker run -it --rm --name kube-dns gcr.io/google_containers/kubedns-amd64:1.9 --domain=cluster.local --dns-port=10053 --config-map=kube-dns --v=2
```
