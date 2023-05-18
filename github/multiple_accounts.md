# How to use multiple accounts on GitHub

Sometimes, we have multiple GitHub accounts with different SSH Keys.
How to use select which key to use depending on the repository we work with ?

According to this [StackOverflow answer](https://stackoverflow.com/a/61150726), we can use the Match option from SSH!

We can add the following configuration in our `.ssh\config`:

```
Match host github.com exec "[ $(git config user.email) = user@perso.org ]"
    IdentitiesOnly yes
    IdentityFile ~/.ssh/rsa_perso
```

Notes:
1. I didn't add the following:

```
Host github.com
    IdentityFile ~/.ssh/other_key
```

Indeed, if added in the configuration file, this identity will always be included during key selection for SSH.
It means that if the names of the two files are close enough the other identity will be used therefore ruining the intended effect.
For instance, if the files are named `id_rsa` and `id_rsa_perso`, `id_rsa` will be used instead of `id_rsa_perso`.

2. `IdentitiesOnly yes` makes sure that no SSH keys registered in the SSH Agent will be used during key selection.
