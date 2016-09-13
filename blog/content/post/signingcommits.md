+++
author = "fabianehlert"
date = "2016-09-13"
draft = false
title = "Signing GitHub Commits"
tags = ["gpg","sign","commit","git","dev","cool"]
image = "images/essential/unitednations.jpg"
comments = false
share = true
+++

> This one is going to be a more technical post on how to sign commits. I will write a separate blog post about key signing and GPG in particular later.

> This _guide_ is written for everyone. It doesn't matter if you have never used GPG before or you are the creator of it.

### Why sign commits at all?
While git cryptographically is secure, you cannot be sure that work really comes from a specific person that claims to be the source of it.
This is where signing commits comes in handy. Signing commits gives certainty that commits really come from a trusted source, i.e. _you_.

And also the badge you get looks really cool 😉
<img src="../signingcommits/GitHub-verifiedcommit.jpg" alt="Twitter" style="width: 700px;"/>

### 1.Creating a key with GPGTools
You first need to create your own personal key which will be used later to sign your commits. If you have one already, you can just skip this whole step.

I recommend to download the GPG Suite from [https://gpgtools.org](https://gpgtools.org) because it offers a super intuitive way to create keys.

> GPGTools is a lot more than an app to create a key you can sign commits with. But I won't be covering all the features of it here. Taken from their website, it's basically a tool to encrypt, decrypt, sign and verify files or messages.

Once installed and open, you can click the button to create a new key in GPGTools which you will have to assign your name and mail address which you're using with your GitHub account with. You don't have to check _upload key to server_.
<br>Lastly you have to give it a passphrase. This should be a long and secure code only you know.
<br>Then hit create and you have successfully created a GPG key! 🎉

### 2.GitHub preparation
Open your GitHub account settings and navigate to "SSH and GPG keys". Then press on "New GPG key". This is where you insert your public GPG key.
<br>You get your public key by exporting your just created key from GPGTools and opening the exported file in a text editor.
<br>You should see this header `-----BEGIN PGP PUBLIC KEY BLOCK-----` and a rather unreadable block of numbers, letters and special characters. That means you've successfully exported your public key 👍 Copy the complete content shown in the text editor and paste it in the GitHub textbox asking for your public key and hit _Add GPG key_.

### 3.Configuring Git
So now that GitHub knows which public key is yours you have to configure Git on your machine to sign commits with the same key.

In Terminal list all your keys with
`gpg --list-secret-keys --keyid-format LONG`

Look out for the key assigned to your GitHub mail address and copy the key written after `sec length/yourkey YYYY-MM-DD ...`
<br>`yourkey` should be a key with numbers and letters like this `3AA5C34371567BD2`

Assign your key to the global Git configuration on your computer
<br>`git config --global user.signingkey yourkey`

>

[https://help.github.com/articles/telling-git-about-your-gpg-key/](https://help.github.com/articles/telling-git-about-your-gpg-key/)

### 4.Commit
> Note that at some point you might be required to enter your passphrase from step 1.
> GPGTools also  offers to store your passphrase in the macOS keychain, so you don't always have to enter it manually.

And now you're ready to commit your first _signed_ commit.

In Terminal `cd` to your project that is under Git control and commit your changes like this `git commit -S -m "A signed commit 🎉"`

See the additional `-S` flag?!
You don't have to add this if you run<br>`git config --global commit.gpgsign true` to have all commits signed by default. However it is still recommended to add `-S` when committing.

Finally you can push (`git push`) this commit to GitHub to see the Badge added to your commit by GitHub 🤗

[https://help.github.com/articles/signing-commits-using-gpg/](https://help.github.com/articles/signing-commits-using-gpg/)

> **NOTE:** It is also possible to setup commit signing individually for each repository. When running the terminal commands, just make sure to be in the correct directory of the repo you'd like to configure (`pwd`) and then leave out `--global` in the `git` commands.

### Autosign with Tower
If you're using Tower and love commiting from there instead of going via Terminal (I do understand you! I ❤️ Tower too 😉), I can tell you, you don't have to relinquish Tower. There is an easy way to have your commits from there signed as well.

In Terminal just do
<br>`echo no-tty >> ~/.gnupg/gpg.conf`
<br>and
<br>`git config --global gpg.program /usr/local/bin/gpg`

Taken from [https://aaronparecki.com/2016/07/29/10/git-tower](https://aaronparecki.com/2016/07/29/10/git-tower)
<br>(Link [provided](https://twitter.com/gittower/status/772684408428134400) by [@gittower](https://twitter.com/gittower))

---

I recommend you also check git's official guide to signing commits: [https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work](https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work)
