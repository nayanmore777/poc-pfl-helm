## [ AWS ] If API Gateway was setup within the same cluster, API Calls to Custom DNS on NLB Won't work due to Hairpinning

1. Edit CORE-DNS Config Map

```sh

kubectl edit cm coredns -n kube-system

```

2. Point Custom DNS to AWS NLB CNAME

```sh

.:53 {
    errors
    health {
        lameduck 5s
        }
    ready
    rewrite name atlasapigateway.acc.ltd atlas-eks-ingress-apigw-nlb-ca11775c332b9f98.elb.ap-south-1.amazonaws.com
    kubernetes cluster.local in-addr.arpa ip6.arpa {
        pods insecure
        fallthrough in-addr.arpa ip6.arpa
    }
    prometheus :9153
    forward . /etc/resolv.conf
    cache 30
    loop
    reload
    loadbalance
}

```

**rewrite name atlasapigateway.acc.ltd atlas-eks-ingress-apigw-nlb-ca11775c332b9f98.elb.ap-south-1.amazonaws.com**
