# Core concepts
## The Tox ID

![](https://raw.githubusercontent.com/ToxClient/wiki/master/_static/public_key.png)

This is a typical Tox ID that you currently give out to friends. It is a public key, nospam value, and checksum concatenated in hexadecimal format. The result is the 76-character string shown above.

![](https://raw.githubusercontent.com/ToxClient/wiki/master/_static/public_key_bd.png)

(*The three parts of the a Tox ID.*)

### Public Key

The public key is internally generated from the `crypto_box_keypair` function by NaCl.

This is explained better [here](http://nacl.cr.yp.to/box.html). In the current implementation of NaCl, it is 32 bytes (64 hexadecimal characters).

In practice, libtoxcore.so generates a new random public/private keypair during initialization and it's up to you to save this identity (tox\_save()) or replace it with a previously saved one (tox\_load())

### `nospam` Value

The `nospam` value is a randomly generated number appended to the key. A friend request sent without knowing the correct `nospam` value will be ignored.

The `nospam` value can be changed at any time without affecting the public key, stopping all requests to your current ID. This makes it effective for fighting spam (its original purpose!).

### Checksum

The checksum is a simple XOR checksum of the public key and `nospam` value. It is used to to quickly verify the integrity of the Tox ID.

## Connecting to the network
Because Tox has no central servers, you will need to know someone who is already "in" the network before you can successfully connect your client.

The Tox core handles this in one of two ways:

-   Automatically bootstrapping to a Tox client that it finds on the LAN, and
-   Bootstrapping to a known peer that your code provides.

> **NOTE** 
> If you would like that data in a more machine-friendly format, an
> unofficial JSON list is available
> [here](https://dist-build.tox.im/Nodefile.json), updated hourly from
> the wiki. Additionally, a script to scrape entries off the wiki is
> available [here](https://github.com/Jman012/Tox-DHTservers-Updater).
