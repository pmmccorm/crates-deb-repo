Given a list of crates, a private key (with associated email), and a ubuntu distro:
  1 build and package them as .debs
  2 create a repo for these debs and host it on github pages
  3 repeat weekly

The latest versions will be fetched from crates.io. The github pages need to be enabled for the repository and the
private key and email need to be stored as secrets (named private_key and private_email).

## Create the public/private keys

RSA, 4096, never expires, no password, save the email:
```
gpg --full-gen-key
```

Export that secret key:
```
gpg --armor --export-secret-keys $EMAIL > private.asc
```

Store the private key and email in your github secrets. These will need to be passed into the deb-repo action.

## Add repo

Add the following like to apt sources:

```
deb https://pmmccorm.github.io/crates-deb-repo/ .
```

And grab the public key and add it to the trusted keys:

```
curl https://pmmccorm.github.io/crates-deb-repo/public.gpg > /etc/apt/trusted.gpg.d/crates-deb-repo.gpg
```

## TODO

Create proper structure for dist and components

Support multiple dists. Maybe through job matrix? Will have to join .deb artifacts.

Can we do this w/o having to pass around the email?

Debug symbols / packages ?

Check current versions and no-op on no new versions.

Support crate version specifications eg: mcfly<1.0 foo=1.1

Shorten the readme (which gets copied wholesale into package description). This bloats the Packages

## See also

Notes see: https://assafmo.github.io/2019/05/02/ppa-repo-hosted-on-github.html
