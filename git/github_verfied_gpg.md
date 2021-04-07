# GPG Verification for signing your GitHub commits

If you would like to go directly to the official docs, they can be found at https://docs.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account

After some troubleshooting, the steps below are what I did locally and managed to have the expected results.

## macOS (and probably Linux, but never tested)

- `brew install gnupg` This will install the necessary `gpg` tools
- `gpg --full-generate-key`

In the prompts, I used these values:
- Choose the `RSA` key type
- GitHub requires `4096` bits
- Do not expire
- My name as I want it to appear in GitHub
- For the email address, I use GitHub's [private email forwarding](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address#setting-your-commit-email-address-on-github)
- Secure password, which I generated using 1Password

This is complete the generation of the key, now we just need to save it to your GitHub account.

`gpg --list-secret-keys --keyid-format LONG`

```
# ----------------------------------
# sec   4096R/E870EE00B5243537 2017-12-31 [expires: 2021-12-31]
# uid                          John Smith <john.smith@gmail.com>
 # ssb   4096R/F9E3E72EBBFDCFD6 2017-12-31
```

Look for the shortcode (E870EE00B5243537 in this example) of the key you would like to use and then run 

`gpg --armor --export E870EE00B5243537 | pbcopy`

This will automatically put the key into your clipboard. Now you can follow the directions at https://docs.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account to complete the setup with your settings.

After this, we need to configure your local `git` to auto-sign your commits so they can all be verified.

- Update your user email address if you set it to private `git config --global user.email "ID+username@users.noreply.github.com"`
- `git config --global user.signingkey E870EE00B5243537` Adds your shortcode as the signing key
- `git config --global commit.gpgsign true` Defaults to signing your commits automatically
- `git config --global gpg.program gpg` Sets the default gpg parser to use the `gpg` app installed by the original brew command

Lastly, if you use a WebStorm, or other IDE, you may need to set the following to avoid errors.
- `echo 'no-tty' >> ~/.gnupg/gpg.conf`

## Windows

- Download the latest [GPG4Win](https://gpg4win.org/download.html)
- Open up Kleopatra which is installed from GPG4Win
- Click on `New Key Pair`
- Name and Email should be what you would like to show up on GitHub
  - I use GitHub's [private email forwarding](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address#setting-your-commit-email-address-on-github)
- Protect with a password (and I recommend storing it somewhere safe)
- Click the `Advanced Settings` button and make a few changes
  - RSA and + RSA should be `4096`
  - Uncheck the `Valid until` so the key does not expire

Now we jump into the `cmd` or PowerShell and run a few commands

- `gpg --list-secret-keys --keyid-format LONG`
  - Will present you with your keys 
  - ```# ----------------------------------
       # sec   4096R/E870EE00B5243537 2017-12-31 [expires: 2021-12-31]
       # uid                          John Smith <john.smith@gmail.com>
       # ssb   4096R/F9E3E72EBBFDCFD6 2017-12-31
       ```
- `where.exe gpg` to get the path of your gpg.exe, should be similar to `C:\Program Files (x86)\GnuPG\bin\gpg.exe`      
 
Then, in your Git Bash terminal
       
- `git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"` set this as the default gpg parser
- `git config --global commit.gpgsign true` auto-sign the commits
- `git config --global user.signingkey E870EE00B5243537` the key to use to sign the commits
- `echo 'no-tty' >> ~/.gnupg/gpg.conf` This isn't always necessary, but can resolve issues with using IDEs

Please leave an issue in the repo, or submit a PR, if I missed any steps or if there are better ways to accomplish this!

Thanks for reading.
