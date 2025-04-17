# Verifying downloads

the other day i needed to verify my download to ensure it was not tampered with during transit, a common way to do this is to use `gpg` to verify the download using the pgp certificate and download signature.

## Step 1: import the certificate
### Step 1: import the certificate

the website where you downloded the package should come with a download signature and an OpenPGP certificate. you can import the certificate like so:

```bash
gpg --import ThomasV.asc
```

which should give the following output

```bash
gpg: key 2BD5824B7F9470E6: "Thomas Voegtlin (https://electrum.org) <thomasv@electrum.org>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
```

## Step 2: download the download signature

after importing the certificate, you need to also download the download signature used to sign the download. The file extension is also `.asc`.

## Step 3: verify the download

now that you have both certificate and download signature, you can finally verify the download like so:

```bash
gpg --verify electrum-4.5.8-x86_64.AppImage.asc
```

which should give this output

```bash
gpg: assuming signed data in 'electrum-4.5.8-x86_64.AppImage'
gpg: Signature made Wed 23 Oct 2024 03:13:28 PM +08
gpg:                using RSA key 637DB1E23370F84AFF88CCE03152347D07DA627C
gpg: Can't check signature: No public key
gpg: Signature made Wed 23 Oct 2024 02:03:11 PM +08
gpg:                using RSA key 0EEDCFD5CAFB459067349B23CA9EEEC43DF911DC
gpg: Can't check signature: No public key
gpg: Signature made Wed 23 Oct 2024 11:26:58 AM +08
gpg:                using RSA key 6694D8DE7BE8EE5631BED9502BD5824B7F9470E6
gpg: Good signature from "Thomas Voegtlin (https://electrum.org) <thomasv@electrum.org>" [unknown]
gpg:                 aka "ThomasV <thomasv1@gmx.de>" [unknown]
gpg:                 aka "Thomas Voegtlin <thomasv1@gmx.de>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 6694 D8DE 7BE8 EE56 31BE  D950 2BD5 824B 7F94 70E6
```

## Download has been corrupted

in the event a download has been tampered during transmission, the output should show a message "BAD signature..."

```bash
gpg: Signature made Sa 23 Dez 2017 17:13:01 CET
gpg:                using RSA key EE1B0E6B2D8387B7
gpg: BAD signature from "Scott R. Shinn <scott@atomicorp.com>" [unknown]
```

## References:

1. [How to verify a file using an asc signature file](https://serverfault.com/a/1045515)
