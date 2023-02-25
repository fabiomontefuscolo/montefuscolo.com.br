---
title: "ICP Brasil client certs setup for Nginx"
date: 2021-04-14T15:53:16-03:00
draft: true
summary: "Use client certificates to authenticate and control access to resources with Nginx."
---

## Getting CA certs from ICP-Brazil

```shell
$ curl http://acraiz.icpbrasil.gov.br/credenciadas/CertificadosAC-ICP-Brasil/ACcompactado.zip -o /tmp/ACcompactado.zip
$ mkdir /etc/ssl/icp-brasil
$ unzip -d /etc/ssl/icp-brasil /tmp/ACcompactado.zip
$ rm /tmp/ACcompactado.zip
```


## Picking correct CA files to Nginx

As of April 2021, 3 files from the downloaded zip are needed to work with Nginx:

* `/etc/ssl/certs/icp-brasil/AC_Certisign_RFB_G5.crt`
* `/etc/ssl/certs/icp-brasil/AC_Secretaria_da_Receita_Federal_do_Brasil_v4.crt`
* `/etc/ssl/certs/icp-brasil/ICP-Brasilv5.crt`

You need to concatenate the 3 files above into a single one Nginx will read. Something like below

```shell
{ \
    cat /etc/ssl/certs/icp-brasil/AC_Certisign_RFB_G5.crt; \
    cat /etc/ssl/certs/icp-brasil/AC_Secretaria_da_Receita_Federal_do_Brasil_v4.crt; \
    cat /etc/ssl/certs/icp-brasil/ICP-Brasilv5.crt; \
} &gt; /etc/ssl/certs/icp-brasil/ICP-Brasil-bundle.crt
```


## Setting up Nginx

These 3 directives below are needed by Nginx to start requesting the client certificate from ICP-Brasil certificate holders.

```nginx
ssl_client_certificate /etc/ssl/certs/icp-brasil/ICP-Brasil-bundle.crt;
ssl_verify_client on;
ssl_verify_depth 2;
```

Variables forwarded to the backend.

```nginx
    fastcgi_param SSL_CIPHER              $ssl_cipher              if_not_empty;
    fastcgi_param SSL_CIPHERS             $ssl_ciphers             if_not_empty;
    fastcgi_param SSL_CLIENT_CERT         $ssl_client_cert         if_not_empty;
    fastcgi_param SSL_CLIENT_ESCAPED_CERT $ssl_client_escaped_cert if_not_empty;
    fastcgi_param SSL_CLIENT_FINGERPRINT  $ssl_client_fingerprint  if_not_empty;
    fastcgi_param SSL_CLIENT_I_DN         $ssl_client_i_dn         if_not_empty;
    fastcgi_param SSL_CLIENT_I_DN_LEGACY  $ssl_client_i_dn_legacy  if_not_empty;
    fastcgi_param SSL_CLIENT_RAW_CERT     $ssl_client_raw_cert     if_not_empty;
    fastcgi_param SSL_CLIENT_S_DN         $ssl_client_s_dn         if_not_empty;
    fastcgi_param SSL_CLIENT_S_DN_LEGACY  $ssl_client_s_dn_legacy  if_not_empty;
    fastcgi_param SSL_CLIENT_SERIAL       $ssl_client_serial       if_not_empty;
    fastcgi_param SSL_CLIENT_V_END        $ssl_client_v_end        if_not_empty;
    fastcgi_param SSL_CLIENT_VERIFY       $ssl_client_verify       if_not_empty;
    fastcgi_param SSL_CLIENT_V_REMAIN     $ssl_client_v_remain     if_not_empty;
    fastcgi_param SSL_CLIENT_V_START      $ssl_client_v_start      if_not_empty;
    fastcgi_param SSL_CURVES              $ssl_curves              if_not_empty;
    fastcgi_param SSL_EARLY_DATA          $ssl_early_data          if_not_empty;
    fastcgi_param SSL_PROTOCOL            $ssl_protocol            if_not_empty;
    fastcgi_param SSL_SERVER_NAME         $ssl_server_name         if_not_empty;
    fastcgi_param SSL_SESSION_ID          $ssl_session_id          if_not_empty;
    fastcgi_param SSL_SESSION_REUSED      $ssl_session_reused      if_not_empty;
```


## Sample Nginx config

```nginx
server {
    listen 80;
    listen 443 ssl;
    server_name testcert.montefuscolo.com.br;

    root /var/www/html;
    access_log /var/log/nginx/testcert.montefuscolo.com.br_access_log;
    error_log /var/log/nginx/testcert.montefuscolo.com.br_error_log;

    ssl_certificate /etc/letsencrypt/live/testcert.montefuscolo.com.br/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/testcert.montefuscolo.com.br/privkey.pem;

    ssl_client_certificate /etc/ssl/certs/icp-brasil/ICP-Brasil-bundle.crt;
    ssl_verify_client on;
    ssl_verify_depth 2;

    index index.html index.htm index.php;

    location ~ \.php(/|$) {
        include fastcgi.conf;

        fastcgi_param SSL_CIPHER              $ssl_cipher              if_not_empty;
        fastcgi_param SSL_CIPHERS             $ssl_ciphers             if_not_empty;
        fastcgi_param SSL_CLIENT_CERT         $ssl_client_cert         if_not_empty;
        fastcgi_param SSL_CLIENT_ESCAPED_CERT $ssl_client_escaped_cert if_not_empty;
        fastcgi_param SSL_CLIENT_FINGERPRINT  $ssl_client_fingerprint  if_not_empty;
        fastcgi_param SSL_CLIENT_I_DN         $ssl_client_i_dn         if_not_empty;
        fastcgi_param SSL_CLIENT_I_DN_LEGACY  $ssl_client_i_dn_legacy  if_not_empty;
        fastcgi_param SSL_CLIENT_RAW_CERT     $ssl_client_raw_cert     if_not_empty;
        fastcgi_param SSL_CLIENT_S_DN         $ssl_client_s_dn         if_not_empty;
        fastcgi_param SSL_CLIENT_S_DN_LEGACY  $ssl_client_s_dn_legacy  if_not_empty;
        fastcgi_param SSL_CLIENT_SERIAL       $ssl_client_serial       if_not_empty;
        fastcgi_param SSL_CLIENT_V_END        $ssl_client_v_end        if_not_empty;
        fastcgi_param SSL_CLIENT_VERIFY       $ssl_client_verify       if_not_empty;
        fastcgi_param SSL_CLIENT_V_REMAIN     $ssl_client_v_remain     if_not_empty;
        fastcgi_param SSL_CLIENT_V_START      $ssl_client_v_start      if_not_empty;
        fastcgi_param SSL_CURVES              $ssl_curves              if_not_empty;
        fastcgi_param SSL_EARLY_DATA          $ssl_early_data          if_not_empty;
        fastcgi_param SSL_PROTOCOL            $ssl_protocol            if_not_empty;
        fastcgi_param SSL_SERVER_NAME         $ssl_server_name         if_not_empty;
        fastcgi_param SSL_SESSION_ID          $ssl_session_id          if_not_empty;
        fastcgi_param SSL_SESSION_REUSED      $ssl_session_reused      if_not_empty;

        fastcgi_pass localhost:4567;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
    }
}
```


## References

* http://nginx.org/en/docs/http/ngx_http_ssl_module.html#variables
* https://gist.github.com/skarllot/9663935
* https://github.com/c0h1b4/autenticacao-ICP-Brasil/blob/master/NGINX/site.nginx
* https://stackoverflow.com/questions/8431528/nginx-ssl-certificate-authentication-signed-by-intermediate-ca-chain