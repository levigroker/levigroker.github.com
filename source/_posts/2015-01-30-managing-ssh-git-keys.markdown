---
layout: post
title: "Managing ssh git keys"
date: 2015-01-30 11:26:32 -0700
comments: false
categories: 
---
Like many developers, I would guess, I have many `ssh` keys. Keys for various services such as [github](https://github.com), [bitbucket](https://bitbucket.org), and others, each with the potential for multiple accounts. Managing these keys when using git on the command line can be a pain, but I've got a decent working solution.

ssh-agent
===

`ssh-agent` is a process that sits around and helps `ssh` do its thing on your behalf. There are several helper apps which allow you to configure its behavior, but specifically we are interested in `ssh-add`.

`ssh-add` is your friend, especially on Mac OS X.

It can be used to store credentials which, upon a connection attempt, `ssh` will use. `ssh` will try all the credentials in the list until it finds one which works.

You can get a listing of what keys are already "known" by the agent with `ssh-add -l`:

``` bash
	Moorea:~ levi$ ssh-add -l
	2048 7d:81:16:54:ae:cb:9b:da:b6:65:75:32:40:c6:1e:ac /Users/levi/.ssh/progroker_id_rsa (RSA)
	1024 b5:86:9e:05:d3:ba:f1:57:71:90:d0:2e:a9:94:1a:47 /Users/levi/.ssh/id_dsa (DSA)
	2048 d4:f4:8c:be:2d:cd:5d:06:55:4a:24:bd:c1:9c:83:2b /Users/levi/.ssh/id_rsa_bitbucket (RSA)
	Moorea:~ levi$ 
```

And here's the awesome-sauce... you can add keys **whose credentials get stored in the keychain** with `ssh-add -K <path_to_private_key>`:

``` bash
Moorea:~ levi$ ssh-add -K /Users/levi/.ssh/id_rsa_bitbucket
Enter passphrase for /Users/levi/.ssh/id_rsa_bitbucket: 
Passphrase updated in keychain: /Users/levi/.ssh/id_rsa_bitbucket
Identity added: /Users/levi/.ssh/id_rsa_bitbucket (/Users/levi/.ssh/id_rsa_bitbucket)
Moorea:~ levi$ 
```

So, if your `ssh` key has a passphrase on it, that passphrase is now stored in your Mac OS X keychain, and when `ssh` is looking for credentials to make the connection with it can find the key *and* use it without you needing to enter your passphrase again.

Win.

HTTPS
===

Further, for bonus points, when you're not using `ssh`, but rather https, you can still use the keychain...

Being on Mac OS X, we have the `osxkeychain` which can be used to store and retrive the kind of information we need for authentication.

To set this up, add (or ensure it is already there) the `helper` line in the `[credential]` section of your `~/.gitconfig` file:

```
    [credential]
            helper = osxkeychain
            useHttpPath = true
```

This will allow git to query the OS keychain when it is looking for stored credentials.

If you have multiple accounts on the same system, you may want to tie a set of credentials to the full repository path, so you can access different repositories with different credentials. That's what the `useHttpPath = true` line does.

Now, once you authorize a connection to a specific repository those credentials will be stored in your keychain:

{% img /images/posts/2015-01-30-managing-ssh-git-keys/git_keychain.png 'git keychain entries' 'git keychain entries' %}

Win a second time.