

[Source Video](https://www.youtube.com/watch?v=VH4gXcvkmOY)

[Download Documentation Original Source](https://www.patreon.com/file?h=109937722&m=339848105)

[How TLS/SSL Certificates Works Theoretically? Video](https://www.youtube.com/watch?v=LJDsdSh1CYM)

[How TLS/SSL Certificates Works Mathematically? Video](https://www.youtube.com/watch?v=qph77bTKJTM)

---
> ssl: secure sockets layer.

> tls: transport layer security(more secure ssl).

> ca: certificate authority.

> csr: certificate sign request.

> rsa: Rivest–Shamir–Adleman(creators of rsa hashing).

> pem: Privacy Enhanced Mail.

> aes: Advanced Encryption Standard.

> key: private key.

> crt: certificate.

> (.pem = .key), (.pem = .crt),(.pem = .csr):  just different extensions but same file content but we change ".pem" to tell the difference between files.

---
# Self-Signed Certificates:

![](assets/how-ssl-tls-files-are-used.svg)
## 1. Make Your Own Certificate Authority(CA) Public Key:

![](assets/create-self-signed-certificate.svg)

> Notes: 
> 
> - Save .key & .crt keys somewhere safe.
> 
> - These keys will be used to sign future public keys(.crt) for your service/s.
> 
> - ca-public-key.crt key will be added to clients as a trusted certificate authority public key later.

**1.1. Create working directory `~/certs`:**
```bash
mkdir ~/certs
cd ~/certs
```

**1.2. Generate RSA private encrypted key using a password:**

>Note: each time you try to sign another key you will be asked to enter this password.

>Note: nobody can sign or create keys to be trusted by your clients unless they have that password, even if they have the private key(ca-private.key).

```bash
openssl genrsa -aes256 -out ca-private.key 4096
```

>**genrsa:** generate an RSA private key.
>
>**-aes256:** AES-256 encryption algorithm. 
>
> **ca-private.key:** your certificate authority private key name.
> 
> **-out:** the output filename containing your AES-256 4096 bit long encrypted password.
> 
>**4096:** size of the RSA private key in bits.

**1.3. Enter your password:**
```text
Enter PEM pass phrase: <YOUR_PASSWORD>
Verifying - Enter PEM pass phrase: <YOUR_PASSWORD>
```

`ls` output:
```text
ca-private.key
```

**1.4. Create a self-signed certificate from the private key:**
```bash
openssl req -x509 -sha256 -days 3650 -key ca-private.key -out ca-public-key.crt
```

> **req:** create key signing request.
> 
> **-x509:** tells OpenSSL to create a **self-signed certificate** (instead of generating a certificate signing request (CSR)).
> 
> **-sha256:** specifies the **SHA-256** hash algorithm to use when signing the certificate.
> 
> **-days:** how long the certificate will stay valid.
> 
> **-key:** private key you just generated with AES algorithm.
> 
> **-out:** output certificate authority public key that will be distributed to the clients connecting to your server.


**1.5. Enter the password you used to create `ca-private.key` file and file the prompt:**

>Note: Enter all the info required otherwise your generated keys won't be trusted and have conflicts.

```text
Enter pass phrase for ca-key.pem: <YOUR_PASSWORD>
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]: EG
State or Province Name (full name) []: Giza
Locality Name (eg, city) [Default City]: Giza
Organization Name (eg, company) [Default Company Ltd]: my_company
Organizational Unit Name (eg, section) []: my_department
Common Name (eg, your name or your server's hostname) []: *.my_company.com
Email Address []: example@example.com
```

`ls` output:
```text
ca-private.key  ca-public-key.crt
```


**Verify the certificate(optional)**
```bash
openssl x509 -noout -text -in ca-public-key.crt
```

---
## 2. Using a self signed key to sign another:

![](assets/sign-a-key-with-self-signed-certificate.svg)

> Note: same .key and .crt can be used for multiple services sharing same domain(DNS)/subdomain or IP. 

**2.1. Generate a new private key:**

>Don't use password unless the service supports encrypted keys.

**Generate private key `without` password:**
```bash
openssl genrsa -out service-private.key 4096
```

**Generate private key  `with` password:**
```bash
openssl genrsa -aes256 -out service-private.key 4096
```

**2.2. Create a CSR from the private key:**
```bash
openssl req -new -key service-private.key -out service-crt-request.csr
```

**2.3. Create an `extfile` (extras file) with all the alternative names and ip addresses:**

>Note: without these extra configs even local services won't trust your certificate over SSl/TLS.

```bash
echo "subjectAltName=DNS:localhost,DNS:centos5.local,IP:127.0.0.1,IP:10.0.10.101" >> extfile.cnf
```

`ls` output:
```text
ca-private.key  ca-public-key.crt  extfile.cnf  service-crt-request.csr  service-private.key
```

**2.4. Sign the CSR with the CA certificate:**
```bash
openssl x509 -req -sha256 -in service-crt-request.csr -CA ca-public-key.crt -CAkey ca-private.key -CAcreateserial -out service-signed-public-key.crt -days 3650 -extfile extfile.cnf
```

> **-extfile:** extras file(containing alternative names and ip addresses ...etc).

`ls` output:
```text
ca-private.key  ca-public-key.crt  ca-public-key.srl  extfile.cnf  service-crt-request.csr  service-private.key  service-signed-public-key.crt
```

> `.srl`: file extension refers to a **Serial List** file, which stores serial numbers for certificates issued by the you CA.
>
> Note: Don't delete it to keep track of future certificates serial number. 

**Verify the certificate(optional)**
```bash
openssl verify -CAfile ca-public-key.crt -verbose service-signed-public-key.crt
```

> **Now you can add the .csr and the .key to your service.**

![](assets/ssl-tls-files-location.svg)

---
## 3. Install the CA Cert as a trusted root CA on clients:

### On Debian & Derivatives:

- Move the CA certificate (`ca-public-key.crt`) into 
```
/usr/local/share/ca-certificates/
```

- Update the Cert Store
  
```bash
sudo update-ca-certificates
```

Refer the documentation [here](https://wiki.debian.org/Self-Signed_Certificate) and [here.](https://manpages.debian.org/buster/ca-certificates/update-ca-certificates.8.en.html)

---
### On Fedora:

- Move the CA certificate (`ca-public-key.crt`) to ```

```text
/etc/pki/ca-trust/source/anchors/
``` 

or 

```text
/usr/share/pki/ca-trust-source/anchors/
```

- Now run (with sudo if necessary)

```bash
sudo update-ca-trust
```

Refer the documentation [here.](https://docs.fedoraproject.org/en-US/quick-docs/using-shared-system-certificates/)

---
### On Arch:

System-wide – Arch(p11-kit)
(From arch wiki)
Run (As root)

```bash
trust anchor --store ca-public-key.crt
```

- The certificate will be written to /etc/ca-certificates/trust-source/myCA.p11-kit and the "legacy" directories automatically updated.
- If you get "no configured writable location" or a similar error, import the CA manually:
- Copy the certificate to the /etc/ca-certificates/trust-source/anchors directory.

Run (As root)

```bash 
update-ca-trust
```

wiki page  [here](https://wiki.archlinux.org/title/User:Grawity/Adding_a_trusted_CA_certificate)

---
### On Windows:

Assuming the path to your generated CA certificate as `C:\ca-public-key.crt`, run:

```powershell
Import-Certificate -FilePath "C:\ca-public-key.crt" -CertStoreLocation Cert:\LocalMachine\Root
```

- Set `-CertStoreLocation` to `Cert:\CurrentUser\Root` in case you want to trust certificates only for the logged in user.

OR

In Command Prompt, run:

```sh
certutil.exe -addstore root C:\ca-public-key.crt
```

- `certutil.exe` is a built-in tool (classic `System32` one) and adds a system-wide trust anchor.

---
### On Android:

The exact steps vary device-to-device, but here is a generalised guide:

1. Open Phone Settings
2. Locate `Encryption and Credentials` section. It is generally found under `Settings > Security > Encryption and Credentials`
3. Choose `Install a certificate`
4. Choose `CA Certificate`
5. Locate the certificate file `ca-public-key.crt` on your SD Card/Internal Storage using the file manager.
6. Select to load it.
7. Done!

---
## Certificate Formats:

X.509 Certificates exist in Base64 Formats **PEM (.pem, .crt, .ca-bundle)**, **PKCS#7 (.p7b, p7s)** and Binary Formats **DER (.der, .cer)**, **PKCS#12 (.pfx, p12)**.

> .pem = .key = .crt = just different extensions but same file content.

**Convert Certs:**

| COMMAND                                                | CONVERSION |
| ------------------------------------------------------ | ---------- |
| `openssl x509 -outform der -in cert.pem -out cert.der` | PEM to DER |
| `openssl x509 -inform der -in cert.der -out cert.pem`  | DER to PEM |
| `openssl pkcs12 -in cert.pfx -out cert.pem -nodes`     | PFX to PEM |

---