# Blackbox Testing

This repo outlines the use of StackExchange/blackbox to encrypt secrets such as .env files in the repository. It seems silly that we have to send secret information using other channels. Instead, we should encrypt the data and allow people to see the encrypted data in plain sight. Then only a few people have the ability to decrypt the file.

# Steps Followed

This section outlines the steps followed to set this system up.

## Step 1
Check out the documentation [here](https://github.com/StackExchange/blackbox).

## Step 2
Make sure that you have a `PGP` key setup up, follow [these](https://help.github.com/en/articles/generating-a-new-gpg-key) instructions if not.

I ran `gpg --full-generate-key` and chose the default option (`RSA` and `RSA`), 4096 bits long, does not expire. This is what my output looked like:

```
gpg (GnuPG) 2.2.8; Copyright (C) 2018 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Kerren Ortlepp
Email address: kerren@entrostat.com
Comment: My PGP key used for StackExchange/blackbox
You selected this USER-ID:
    "Kerren Ortlepp (My PGP key used for StackExchange/blackbox) <kerren@entrostat.com>"
```

Running `gpg --list-secret-keys --keyid-format LONG` shows my newly created key (with others).

# Step 3

Add blackbox to the system through `ZSH` (or your preferred method). I use `zplug` so in my `.zshrc` I added the following line:

```
zplug "StackExchange/blackbox"
```

Then I created a new `ZSH` session and installed it.

# Step 4

Now I went into the repository and ran `blackbox_initialize` to start it up. When I initialised, it said that I needed to manually commit the data in using the following command:

```
git commit -m'INITIALIZE BLACKBOX' .blackbox /home/kerren/src/scratch/blackbox-testing/.gitignore
```

# Step 5

Now what I need to do is add the secret file to the repository, in my case I'm going to be using a `.env` file and I'm going to encrypt that so that the data isn't visible to any users on the system unless I've added them as blackbox admins.

First I need to add myself as an admin to the repository, `blackbox_addadmin kerren@entrostat.com` gives the following output:

```
gpg: /home/kerren/src/scratch/blackbox-testing/.blackbox/trustdb.gpg: trustdb created
gpg: key 10DA9C6F20CBCF26: public key "Kerren Ortlepp (My PGP key used for StackExchange/blackbox) <kerren@entrostat.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1


NEXT STEP: You need to manually check these in:
      git commit -m'NEW ADMIN: kerren@entrostat.com' .blackbox/pubring.kbx .blackbox/trustdb.gpg .blackbox/blackbox-admins.txt

```

I then ran `blackbox_register_new_file .env`, which took the `.env` file which I had created and added it to the `.gitignore` and then created a `.env.pgp` which was committed to the repository. This `.env.pgp` is an encrypted file that cannot be opened if you don't have the secret key.
