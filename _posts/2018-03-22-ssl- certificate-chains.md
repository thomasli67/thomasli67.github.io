---
layout: post
title:  "SSL 证书链"
date:   2018-03-22 16:02:00 +0800
categories:
 - SSL
tags:
 - SSL
 - Certificate
---



### 查看已安装配置好证书的nginx证书链

   ```
  openssl s_client -connect api.chemchina.com:443
   ```
   返回    
   ```
   CONNECTED(00000003)
depth=0 C = CN, ST = \E5\8C\97\E4\BA\AC, L = \E5\8C\97\E4\BA\AC, O = \E4\B8\AD\E5\9B\BD\E5\8C\96\E5\B7\A5\E9\9B\86\E5\9B\A2\E5\85\AC\E5\8F\B8, OU = \E7\AE\A1\E7\90\86\E4\B8\8E\E4\BF\A1\E6\81\AF\E9\83\A8, CN = *.chemchina.com
verify error:num=20:unable to get local issuer certificate
verify return:1
depth=0 C = CN, ST = \E5\8C\97\E4\BA\AC, L = \E5\8C\97\E4\BA\AC, O = \E4\B8\AD\E5\9B\BD\E5\8C\96\E5\B7\A5\E9\9B\86\E5\9B\A2\E5\85\AC\E5\8F\B8, OU = \E7\AE\A1\E7\90\86\E4\B8\8E\E4\BF\A1\E6\81\AF\E9\83\A8, CN = *.chemchina.com
verify error:num=21:unable to verify the first certificate
verify return:1
---
Certificate chain
 0 s:/C=CN/ST=\xE5\x8C\x97\xE4\xBA\xAC/L=\xE5\x8C\x97\xE4\xBA\xAC/O=\xE4\xB8\xAD\xE5\x9B\xBD\xE5\x8C\x96\xE5\xB7\xA5\xE9\x9B\x86\xE5\x9B\xA2\xE5\x85\xAC\xE5\x8F\xB8/OU=\xE7\xAE\xA1\xE7\x90\x86\xE4\xB8\x8E\xE4\xBF\xA1\xE6\x81\xAF\xE9\x83\xA8/CN=*.chemchina.com
   i:/C=US/O=GeoTrust Inc./CN=GeoTrust SSL CA - G3
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIHBTCCBe2gAwIBAgIQYxFV5ndbO0MAKmSmEIolETANBgkqhkiG9w0BAQsFADBE
MQswCQYDVQQGEwJVUzEWMBQGA1UEChMNR2VvVHJ1c3QgSW5jLjEdMBsGA1UEAxMU
R2VvVHJ1c3QgU1NMIENBIC0gRzMwHhcNMTcwMzEzMDAwMDAwWhcNMjAwNjExMjM1
OTU5WjCBiTELMAkGA1UEBhMCQ04xDzANBgNVBAgMBuWMl+S6rDEPMA0GA1UEBwwG
5YyX5LqsMSEwHwYDVQQKDBjkuK3lm73ljJblt6Xpm4blm6Llhazlj7gxGzAZBgNV
BAsMEueuoeeQhuS4juS/oeaBr+mDqDEYMBYGA1UEAwwPKi5jaGVtY2hpbmEuY29t
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvw8JOKl4X9dy4syd4Uot
W4mcZrHQrnpNpkDTij3IsGWXGsguudd+XmGwX76tjnfnWaqpVUHPeQfg41mLgGHc
B3A9CyqtHnZT8TsJpjgte5rUjtVfOep+C3zJBL7T8F757lIXyJzAjH9tEOZTrtAr
6tpF/vVEK7D0mPt6fM8Ti2SVV4ecNJjALbrh3I8O7IHuX9BFFS2gwUthI2kYTLdj
fZ9zbDJUc6haNm3L0SBnsL6k69jzmr68IkqbBPdFU6uKJhfjC0QzX0KV19K6ohOY
MK3P47auujzHIygk1EvpjuSb+lslRfrSNqJV0IWn7+BJmYVW9fcmY8AvK8cvhQ2w
YQIDAQABo4IDqzCCA6cwKQYDVR0RBCIwIIIPKi5jaGVtY2hpbmEuY29tgg1jaGVt
Y2hpbmEuY29tMAkGA1UdEwQCMAAwDgYDVR0PAQH/BAQDAgWgMCsGA1UdHwQkMCIw
IKAeoByGGmh0dHA6Ly9nbi5zeW1jYi5jb20vZ24uY3JsMIGdBgNVHSAEgZUwgZIw
gY8GBmeBDAECAjCBhDA/BggrBgEFBQcCARYzaHR0cHM6Ly93d3cuZ2VvdHJ1c3Qu
Y29tL3Jlc291cmNlcy9yZXBvc2l0b3J5L2xlZ2FsMEEGCCsGAQUFBwICMDUMM2h0
dHBzOi8vd3d3Lmdlb3RydXN0LmNvbS9yZXNvdXJjZXMvcmVwb3NpdG9yeS9sZWdh
bDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwHwYDVR0jBBgwFoAU0m/3
lvSFP3I8MH0j2oV4m6N8WnwwVwYIKwYBBQUHAQEESzBJMB8GCCsGAQUFBzABhhNo
dHRwOi8vZ24uc3ltY2QuY29tMCYGCCsGAQUFBzAChhpodHRwOi8vZ24uc3ltY2Iu
Y29tL2duLmNydDCCAfcGCisGAQQB1nkCBAIEggHnBIIB4wHhAHYA3esdK3oNT6Yg
i4GtgWhwfi6OnQHVXIiNPRHEzbbsvswAAAFaxa4PQQAABAMARzBFAiEApKgdBIVj
l67jivV6k65wWH25Rfg2SSFo1lnUUiUDppECIG8qZwc/1qCHm2hO1aUWXPcnJ4kB
9n7Yi/ujo4dzMSvrAHcApLkJkLQYWBSHuxOizGdwCjw1mAT5G9+443fNDsgN3BAA
AAFaxa4PfAAABAMASDBGAiEAq9aOKg3F+JvlX+11jSyDa1RxpRN57mbjm+j01MNm
N3sCIQCTG2fiXGA6FGCL2ecwJEocgj3+d3b9mcrv5JSkmOhVOwB2AO5Lvbd1zmC6
4UJpH6vhnmajD35fsHLYgwDEe4l6qP3LAAABWsWuEToAAAQDAEcwRQIhAIXEU/HO
yS/MNY1uicMCoJj1kubzyIh9DUh5ngirsBK/AiA2rLP1aF6M/86cEB3dC6jDr7VQ
PIdsnaFPmnjzG9Ik4gB2ALx44d/F9jxoRkkzTaEPoV8JeWkgCcCBtPP2kX8+2bil
AAABWsWuEDoAAAQDAEcwRQIhANK+OnoFWc0sSaIVrBKBcol3dKZ+fbRED7Ot5fxF
zGuyAiBZN97PKxORzfUFo9GaujIwaEau79fNjvoHE8Hv50q4ezANBgkqhkiG9w0B
AQsFAAOCAQEAL1F2sLqyT5p0V9glROPT16K7bImmF6sAHCNRF59SbECbOH4w8dcD
7/7KopUXvOMkeQ8tpfBleq97j8DVaofxpNVMWH7H7spMdiSAIXpBnFp/lKxSV2cq
ClWd3AHfCBiY/b/QoRYkckhApsFYgHYleI5F3BpdBtCg6xnIxDc9CNdLa59fSJJ8
bP2XkwD4xoabp9F+YIu6n93VHgza/70ArDQr7WVu75eZPGN4CYoHBmcyHZFjeRws
Im/4nSRbT/ChWhU+Bt4NowCGCNy+coX+pLL+8wBgi+CGhHNKQebPWvUzEes/pxUt
FmDYeF4hO1orXJ6VwQudo92XwsoRZeFBOQ==
-----END CERTIFICATE-----
subject=/C=CN/ST=\xE5\x8C\x97\xE4\xBA\xAC/L=\xE5\x8C\x97\xE4\xBA\xAC/O=\xE4\xB8\xAD\xE5\x9B\xBD\xE5\x8C\x96\xE5\xB7\xA5\xE9\x9B\x86\xE5\x9B\xA2\xE5\x85\xAC\xE5\x8F\xB8/OU=\xE7\xAE\xA1\xE7\x90\x86\xE4\xB8\x8E\xE4\xBF\xA1\xE6\x81\xAF\xE9\x83\xA8/CN=*.chemchina.com
issuer=/C=US/O=GeoTrust Inc./CN=GeoTrust SSL CA - G3
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 2471 bytes and written 415 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: C6DEF4CE40D8D1021775EE7ACEF075DDA9A8FD4D079D516829AC2DE057057D0A
    Session-ID-ctx:
    Master-Key: FF734CA1EBD1563DC2C4D08DDCA647B059DC3639D127E67F51F627EDC91A9B8D100365D1B54B3913CDA18380EA28DBA0
    Key-Arg   : None
    Krb5 Principal: None
    PSK identity: None
    PSK identity hint: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - c0 d9 5b 26 b5 b5 85 91-50 33 e9 fa ea a4 e8 53   ..[&....P3.....S
    0010 - 79 3d fe 90 aa 13 0a 8d-8c e1 7f c0 59 c7 78 e2   y=..........Y.x.
    0020 - a5 a3 9f 42 b6 6d ed 0e-7d 4e 18 4e 0e 81 e2 47   ...B.m..}N.N...G
    0030 - 12 94 aa e1 fc 87 04 bb-51 dc 64 07 a2 18 9d 29   ........Q.d....)
    0040 - b3 8b 2a 7e 1f 29 3b fe-66 a6 2c 67 37 19 ea 05   ..*~.);.f.,g7...
    0050 - 1b d8 91 d8 3a bb 63 16-a8 34 4a 96 9d 14 01 6d   ....:.c..4J....m
    0060 - 17 9a 63 61 e7 55 bf 64-a5 48 1a 3b 0b 0d ba ae   ..ca.U.d.H.;....
    0070 - 6d 7c 0c c7 2b 80 25 55-95 ac 72 40 70 41 7f cf   m|..+.%U..r@pA..
    0080 - 94 8a cd b7 37 ee 60 a3-b0 05 3f 32 cb ec a0 6a   ....7.`...?2...j
    0090 - 41 bc fb 9d fb 89 23 7f-c8 08 d8 6f ce 2f 78 f8   A.....#....o./x.
    00a0 - 3b b2 69 77 ce 79 79 bd-6e dc d0 11 66 f3 17 86   ;.iw.yy.n...f...

    Start Time: 1521705084
    Timeout   : 300 (sec)
    Verify return code: 21 (unable to verify the first certificate)
---
closed

   ```

### 使用网上的证书检查器，发现“服务器缺少中间证书”   
```
SSL服务器证书安装检查器api.f5.chemchina.com:443 获取的证书链如下：
错误：域名与证书不匹配
警告：服务器证书需要在2018年9月13日前替换成新版本，请联系代理商免费替换
证书1
证书使用者：	*.chemchina.com
证书颁发者：	GeoTrust SSL CA - G3
有效期：	从 2017/3/13 到 2020/6/12
匹配域名：	*.chemchina.com
chemchina.com
签名算法：	sha256RSA
公钥长度：	2048位 (RSA 算法)
SHA1指纹：	87 46 78 f0 cb 61 fb bb c2 b6 48 4c d2 65 a0 65 36 12 aa 25
SHA256指纹：	61 17 ff d4 2d 3d 52 79 41 4d a3 29 8e e1 05 2b 88 7b 0b 3c 1d 23 a8 b3 51 d9 1c 4c 6a 74 f5 16
证书来源：	服务器返回的证书
状态：	正常

证书2
证书使用者：	GeoTrust SSL CA - G3
证书颁发者：	GeoTrust Global CA
有效期：	从 2013/11/6 到 2022/5/21
签名算法：	sha256RSA
公钥长度：	2048位 (RSA 算法)
SHA1指纹：	5a ea ee 3f 7f 2a 94 49 ce ba fe ec 68 fd d1 84 f2 01 24 a7
SHA256指纹：	07 45 41 ec df 88 ed 99 2e d5 ad e3 ec dd ef 27 a2 6b a1 b4 44 80 a1 95 c0 a8 da da e2 52 1d 8e
证书来源：	缺失证书
状态：	错误： 服务器缺少中间证书

证书3
证书使用者：	GeoTrust Global CA
证书颁发者：	GeoTrust Global CA
有效期：	从 2002/5/21 到 2022/5/21
签名算法：	sha1RSA
公钥长度：	2048位 (RSA 算法)
SHA1指纹：	de 28 f4 a4 ff e5 b9 2f a3 c5 03 d1 a3 49 a7 f9 96 2a 82 12
SHA256指纹：	ff 85 6a 2d 25 1d cd 88 d3 66 56 f4 50 12 67 98 cf ab aa de 40 79 9c 72 2d e4 d2 b5 db 36 a7 3a
证书来源：	浏览器内置的根证书
状态：	正常

```

### 为nginx配置中间证书
把中间证书加到服务器证书文件的后面
```
cat chain.crt >> server-ssl.crt

```

### 再检查
```
openssl s_client -connect api.chemchina.com:443
```
这时返回
```
CONNECTED(00000003)
depth=2 C = US, O = GeoTrust Inc., CN = GeoTrust Global CA
verify return:1
depth=1 C = US, O = GeoTrust Inc., CN = GeoTrust SSL CA - G3
verify return:1
depth=0 C = CN, ST = \E5\8C\97\E4\BA\AC, L = \E5\8C\97\E4\BA\AC, O = \E4\B8\AD\E5\9B\BD\E5\8C\96\E5\B7\A5\E9\9B\86\E5\9B\A2\E5\85\AC\E5\8F\B8, OU = \E7\AE\A1\E7\90\86\E4\B8\8E\E4\BF\A1\E6\81\AF\E9\83\A8, CN = *.chemchina.com
verify return:1
---
Certificate chain
 0 s:/C=CN/ST=\xE5\x8C\x97\xE4\xBA\xAC/L=\xE5\x8C\x97\xE4\xBA\xAC/O=\xE4\xB8\xAD\xE5\x9B\xBD\xE5\x8C\x96\xE5\xB7\xA5\xE9\x9B\x86\xE5\x9B\xA2\xE5\x85\xAC\xE5\x8F\xB8/OU=\xE7\xAE\xA1\xE7\x90\x86\xE4\xB8\x8E\xE4\xBF\xA1\xE6\x81\xAF\xE9\x83\xA8/CN=*.chemchina.com
   i:/C=US/O=GeoTrust Inc./CN=GeoTrust SSL CA - G3
 1 s:/C=US/O=GeoTrust Inc./CN=GeoTrust SSL CA - G3
   i:/C=US/O=GeoTrust Inc./CN=GeoTrust Global CA
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIHBTCCBe2gAwIBAgIQYxFV5ndbO0MAKmSmEIolETANBgkqhkiG9w0BAQsFADBE
MQswCQYDVQQGEwJVUzEWMBQGA1UEChMNR2VvVHJ1c3QgSW5jLjEdMBsGA1UEAxMU
R2VvVHJ1c3QgU1NMIENBIC0gRzMwHhcNMTcwMzEzMDAwMDAwWhcNMjAwNjExMjM1
OTU5WjCBiTELMAkGA1UEBhMCQ04xDzANBgNVBAgMBuWMl+S6rDEPMA0GA1UEBwwG
5YyX5LqsMSEwHwYDVQQKDBjkuK3lm73ljJblt6Xpm4blm6Llhazlj7gxGzAZBgNV
BAsMEueuoeeQhuS4juS/oeaBr+mDqDEYMBYGA1UEAwwPKi5jaGVtY2hpbmEuY29t
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvw8JOKl4X9dy4syd4Uot
W4mcZrHQrnpNpkDTij3IsGWXGsguudd+XmGwX76tjnfnWaqpVUHPeQfg41mLgGHc
B3A9CyqtHnZT8TsJpjgte5rUjtVfOep+C3zJBL7T8F757lIXyJzAjH9tEOZTrtAr
6tpF/vVEK7D0mPt6fM8Ti2SVV4ecNJjALbrh3I8O7IHuX9BFFS2gwUthI2kYTLdj
fZ9zbDJUc6haNm3L0SBnsL6k69jzmr68IkqbBPdFU6uKJhfjC0QzX0KV19K6ohOY
MK3P47auujzHIygk1EvpjuSb+lslRfrSNqJV0IWn7+BJmYVW9fcmY8AvK8cvhQ2w
YQIDAQABo4IDqzCCA6cwKQYDVR0RBCIwIIIPKi5jaGVtY2hpbmEuY29tgg1jaGVt
Y2hpbmEuY29tMAkGA1UdEwQCMAAwDgYDVR0PAQH/BAQDAgWgMCsGA1UdHwQkMCIw
IKAeoByGGmh0dHA6Ly9nbi5zeW1jYi5jb20vZ24uY3JsMIGdBgNVHSAEgZUwgZIw
gY8GBmeBDAECAjCBhDA/BggrBgEFBQcCARYzaHR0cHM6Ly93d3cuZ2VvdHJ1c3Qu
Y29tL3Jlc291cmNlcy9yZXBvc2l0b3J5L2xlZ2FsMEEGCCsGAQUFBwICMDUMM2h0
dHBzOi8vd3d3Lmdlb3RydXN0LmNvbS9yZXNvdXJjZXMvcmVwb3NpdG9yeS9sZWdh
bDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwHwYDVR0jBBgwFoAU0m/3
lvSFP3I8MH0j2oV4m6N8WnwwVwYIKwYBBQUHAQEESzBJMB8GCCsGAQUFBzABhhNo
dHRwOi8vZ24uc3ltY2QuY29tMCYGCCsGAQUFBzAChhpodHRwOi8vZ24uc3ltY2Iu
Y29tL2duLmNydDCCAfcGCisGAQQB1nkCBAIEggHnBIIB4wHhAHYA3esdK3oNT6Yg
i4GtgWhwfi6OnQHVXIiNPRHEzbbsvswAAAFaxa4PQQAABAMARzBFAiEApKgdBIVj
l67jivV6k65wWH25Rfg2SSFo1lnUUiUDppECIG8qZwc/1qCHm2hO1aUWXPcnJ4kB
9n7Yi/ujo4dzMSvrAHcApLkJkLQYWBSHuxOizGdwCjw1mAT5G9+443fNDsgN3BAA
AAFaxa4PfAAABAMASDBGAiEAq9aOKg3F+JvlX+11jSyDa1RxpRN57mbjm+j01MNm
N3sCIQCTG2fiXGA6FGCL2ecwJEocgj3+d3b9mcrv5JSkmOhVOwB2AO5Lvbd1zmC6
4UJpH6vhnmajD35fsHLYgwDEe4l6qP3LAAABWsWuEToAAAQDAEcwRQIhAIXEU/HO
yS/MNY1uicMCoJj1kubzyIh9DUh5ngirsBK/AiA2rLP1aF6M/86cEB3dC6jDr7VQ
PIdsnaFPmnjzG9Ik4gB2ALx44d/F9jxoRkkzTaEPoV8JeWkgCcCBtPP2kX8+2bil
AAABWsWuEDoAAAQDAEcwRQIhANK+OnoFWc0sSaIVrBKBcol3dKZ+fbRED7Ot5fxF
zGuyAiBZN97PKxORzfUFo9GaujIwaEau79fNjvoHE8Hv50q4ezANBgkqhkiG9w0B
AQsFAAOCAQEAL1F2sLqyT5p0V9glROPT16K7bImmF6sAHCNRF59SbECbOH4w8dcD
7/7KopUXvOMkeQ8tpfBleq97j8DVaofxpNVMWH7H7spMdiSAIXpBnFp/lKxSV2cq
ClWd3AHfCBiY/b/QoRYkckhApsFYgHYleI5F3BpdBtCg6xnIxDc9CNdLa59fSJJ8
bP2XkwD4xoabp9F+YIu6n93VHgza/70ArDQr7WVu75eZPGN4CYoHBmcyHZFjeRws
Im/4nSRbT/ChWhU+Bt4NowCGCNy+coX+pLL+8wBgi+CGhHNKQebPWvUzEes/pxUt
FmDYeF4hO1orXJ6VwQudo92XwsoRZeFBOQ==
-----END CERTIFICATE-----
subject=/C=CN/ST=\xE5\x8C\x97\xE4\xBA\xAC/L=\xE5\x8C\x97\xE4\xBA\xAC/O=\xE4\xB8\xAD\xE5\x9B\xBD\xE5\x8C\x96\xE5\xB7\xA5\xE9\x9B\x86\xE5\x9B\xA2\xE5\x85\xAC\xE5\x8F\xB8/OU=\xE7\xAE\xA1\xE7\x90\x86\xE4\xB8\x8E\xE4\xBF\xA1\xE6\x81\xAF\xE9\x83\xA8/CN=*.chemchina.com
issuer=/C=US/O=GeoTrust Inc./CN=GeoTrust SSL CA - G3
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 3581 bytes and written 415 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: F9986CA4D7B030D79D92429207EAFD3FD8047402E9B8EC5CA8EEAFDBE9BE682E
    Session-ID-ctx:
    Master-Key: 73ADDF8B28959D00044464F126E2D94AE645CE3BFFCFA9EE24749A28E927023947D6932EC28E8DA63A8FBAE8B297DC2D
    Key-Arg   : None
    Krb5 Principal: None
    PSK identity: None
    PSK identity hint: None
    TLS session ticket lifetime hint: 300 (seconds)
    TLS session ticket:
    0000 - 66 1c ca 40 5e 6b 93 1a-8b cc cb b3 05 78 92 a9   f..@^k.......x..
    0010 - 44 99 bc 63 c5 bf 3d 4b-c7 3f 4b 62 59 33 0f 89   D..c..=K.?KbY3..
    0020 - 2d 74 e2 7a b0 a0 c2 2e-99 5e f9 24 b5 03 80 6d   -t.z.....^.$...m
    0030 - e0 a7 b6 57 a0 1a 6c 09-26 54 b7 61 3f 92 fd b0   ...W..l.&T.a?...
    0040 - f6 61 b7 12 d2 3d c7 9f-f0 ad bd 38 b3 65 67 20   .a...=.....8.eg
    0050 - 91 10 7c 47 4e d7 ee c7-34 d0 48 32 51 93 1d 56   ..|GN...4.H2Q..V
    0060 - 92 12 b8 8b 32 7a 01 b5-a9 38 19 4c 18 09 d9 97   ....2z...8.L....
    0070 - 00 15 6e 53 4c 57 da 16-90 d7 0d 84 26 ec ab 47   ..nSLW......&..G
    0080 - 18 bf 2b 65 e0 ad 1c 1c-72 55 ae f9 00 6f 9d fe   ..+e....rU...o..
    0090 - 73 fb 2f 25 5c 9e f5 6f-e4 51 f7 35 7c c4 db 07   s./%\..o.Q.5|...
    00a0 - 84 b3 b3 66 c9 a8 6a 9e-10 1d 48 25 f0 6a c2 33   ...f..j...H%.j.3

    Start Time: 1521707365
    Timeout   : 300 (sec)
    Verify return code: 0 (ok)
---
closed

```


## 参考
[Configuring HTTPS servers](https://nginx.org/en/docs/http/configuring_https_servers.html)    
