Vault
=====

.. versionadded:: 0.3.0

`Vault <https://www.vaultproject.io/>`_ "secures, stores, and tightly controls
access to tokens, passwords, certificates, API keys, and other secrets in modern
computing." It is currently included as a technology demo in
Mantl.

Variables
---------

.. data:: vault_default_port

   Port for Vault to listen on

   default: ``8200``

.. data:: vault_command_options

   Extra options to pass to Vault at startup

   default: ``--ca-cert=/etc/pki/CA/ca.cert``

.. data:: vault_init_json

   Initial JSON configuration for Vault

   default: ``{"secret_shares": 4, "secret_threshold": 3}``
