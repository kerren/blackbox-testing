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

