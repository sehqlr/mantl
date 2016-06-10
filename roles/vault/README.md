Vault
=====

Version Added:: 0.3.0

[Vault](https://www.vaultproject.io/) "secures, stores, and tightly
controls access to tokens, passwords, certificates, API keys, and other
secrets in modern computing." It is currently included as a technology
demo in Mantl.

Variables
---------

> vault\_default\_port
>
> > Port for Vault to listen on
> >
> > default: `8200`
>
> vault\_command\_options
>
> > Extra options to pass to Vault at startup
> >
> > default: `-insecure`
>
> vault\_init\_json
>
> > Initial JSON configuration for Vault
> >
> > default: `{"secret_shares": 4, "secret_threshold": 3}`
