# OpenSSL

Tips and tricks for using `openssl` CLI.

## Generate CSR + Key

```bash
openssl req -new -newkey rsa:2048 -nodes -out <csr_filename> -keyout <key_filename> -subj <subject>
```

## View a site's certificate

```bash
# Example for google.com
url=www.google.com
openssl s_client -showcerts -servername "${url}" -connect "${url}:443" 2>/dev/null </dev/null | \
    openssl x509 -text -noout -in /dev/stdin
```

Optionally, you can parse the output into JSON with `jc`. Useful for shell scripts

```bash
openssl s_client -showcerts -servername "${url}" -connect "${url}:443" 2>/dev/null </dev/null | \
  jc --x509-cert --pretty
```

Also, can use above command to access sites through a proxy with the `-proxy <proxyhost>:<proxyport>` directive.

## Convert P7B to PEM

```bash
openssl pkcs7 -inform DER -in <myfilename>.p7b -print_certs -quiet | sed '/^$/d' > output.cer
```
