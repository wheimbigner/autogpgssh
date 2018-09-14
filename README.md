# autogpgssh
Allow management of authorized SSH keys to be automatically updated.

This allows users on a machine to specify a GPG master key fingerprint or fingerprints, and then use authentication-subkeys that have been signed by that master key (and are not expired or revoked) to login.

## WARNING
This project is currently in the very initial stages. While theoretically functional, it almost definitely has security vulnerabilities (including potentially locking you out of your server).

Use this as the starting point for tinkering, NOT for any serious usage.

## Installation and Usage

Install gnupg2 and monkeysphere
Create a user called "gpg" on your intended machine. This user must have a valid home directory.
Copy fetchsshkeys to /usr/local/bin and ensure it is only writeable by root, and world readable/executable
Add the following lines to your SSHD configuration:
    AuthorizedKeysCommand /usr/local/bin/fetchsshkeys '%h'
    AuthorizedKeysCommandUser gpg
Restart sshd
Add a single line containing the GPG fingerprint to /home/youruser/.ssh/authorized_gpgs e.g. `0xD97524B8879F5880`. Multiple master keys can be added by adding additional lines to this file.
Login

## Todo

Cache keys to avoid hammering a keyserver on every login
Verify pubkey authenticity (to ensure a rogue subkey does not exist that has not beet properly signed by the master key, etc.)
Add documentation about what all everything does
