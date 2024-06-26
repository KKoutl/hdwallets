[![PyPI version](https://img.shields.io/pypi/v/hdwallets)](https://pypi.org/project/hdwallets)
[![Build Status](https://github.com/hukkin/hdwallets/workflows/Tests/badge.svg?branch=master)](https://github.com/hukkin/hdwallets/actions?query=workflow%3ATests+branch%3Amaster+event%3Apush)
[![codecov.io](https://codecov.io/gh/hukkin/hdwallets/branch/master/graph/badge.svg)](https://codecov.io/gh/hukkin/hdwallets)

# hdwallets

A basic implementation of the [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) specification for hierarchical deterministic wallets.

A fork of https://github.com/darosior/python-bip32 with some notable changes:

- [`base58`](https://pypi.org/project/base58/) dependency removed.
  All interfaces input and output raw bytes instead of base58 strings.
- Replaced [`coincurve`](https://pypi.org/project/coincurve/) dependency with [`ecdsa`](https://pypi.org/project/ecdsa/)
- Distributes type information ([PEP 561](https://www.python.org/dev/peps/pep-0561/))

## Usage

```python
>>> import base58
>>> from hdwallets import BIP32, HARDENED_INDEX
>>> bip32 = BIP32.from_seed(bytes.fromhex("01"))
# Specify the derivation path as a list ...
>>> bip32.get_xpriv_from_path([1, HARDENED_INDEX, 9998])
b"\x04\x88\xad\xe4\x037\x01)\x0f\x00\x00'\x0e7\xf9\xe7)\x8dCJ\x8b\xfb\xc2j#\xeb\xc0++\xdf}I\x80\xdfr\xef6\xad0\xf7K\x0ceE\xea\x00\xb3D8\x0b\x0e\xf4-\x9a\xe6\x91\xe9\x82\xe8\xbf\x9a\x97\x15\xfe?\x17\xdc[\xf7\xc5\xfb?\xbezaz\\\xb9"
# ... Or in usual m/the/path/
>>> bip32.get_xpriv_from_path("m/1/0'/9998")
b"\x04\x88\xad\xe4\x037\x01)\x0f\x00\x00'\x0e7\xf9\xe7)\x8dCJ\x8b\xfb\xc2j#\xeb\xc0++\xdf}I\x80\xdfr\xef6\xad0\xf7K\x0ceE\xea\x00\xb3D8\x0b\x0e\xf4-\x9a\xe6\x91\xe9\x82\xe8\xbf\x9a\x97\x15\xfe?\x17\xdc[\xf7\xc5\xfb?\xbezaz\\\xb9"
>>> bip32.get_xpub_from_path([HARDENED_INDEX, 42])
b"\x04\x88\xb2\x1e\x02\x11\xd4\xbb(\x00\x00\x00*\xaf?\xc3\x1bb)\x1d\x9e$\x91\xda\xc2b\x8e\x1fm\x7f6\x8c(\x8e'2.\x99-\xf2\xa1\x83\xd7F\x18\x03bB\xb0\xe5\x0b\xb8$\x97\xf0\xf3\xe47\xea\xd6\xd4\xa0\xe3~-#\xbf\t\xf5\x19\xb7\xd1\x06b\xb0\xac\xc5\xd4"
# You can also use "h" or "H" to signal for hardened derivation
>>> bip32.get_xpub_from_path("m/0h/42")
b"\x04\x88\xb2\x1e\x02\x11\xd4\xbb(\x00\x00\x00*\xaf?\xc3\x1bb)\x1d\x9e$\x91\xda\xc2b\x8e\x1fm\x7f6\x8c(\x8e'2.\x99-\xf2\xa1\x83\xd7F\x18\x03bB\xb0\xe5\x0b\xb8$\x97\xf0\xf3\xe47\xea\xd6\xd4\xa0\xe3~-#\xbf\t\xf5\x19\xb7\xd1\x06b\xb0\xac\xc5\xd4"
# You can use pubkey-only derivation
>>> bip32 = BIP32.from_xpub(base58.b58decode_check("xpub6AKC3u8URPxDojLnFtNdEPFkNsXxHfgRhySvVfEJy9SVvQAn14XQjAoFY48mpjgutJNfA54GbYYRpR26tFEJHTHhfiiZZ2wdBBzydVp12yU"))
>>> bip32.get_xpub_from_path([42, 43])
b'\x04\x88\xb2\x1e\x04\xf4p\xd4>\x00\x00\x00+h\xcf\xc2\xd1\xbe\x0c\\-:\x9fpDy\\x\xd5E\xc1\x988\xb1\xe2X\xd1\xba\xb1\xeac\x96\xb04\x8f\x02\xaf?<\xbe>\x92\xcc\xc1fq~\xa9\xcd\xcb\x10\xd5\x15]K\xd6\x10+\xdb\xa8\xb4\xedo\xd2hc\xf9x'
>>> bip32.get_xpub_from_path("m/42/43")
b'\x04\x88\xb2\x1e\x04\xf4p\xd4>\x00\x00\x00+h\xcf\xc2\xd1\xbe\x0c\\-:\x9fpDy\\x\xd5E\xc1\x988\xb1\xe2X\xd1\xba\xb1\xeac\x96\xb04\x8f\x02\xaf?<\xbe>\x92\xcc\xc1fq~\xa9\xcd\xcb\x10\xd5\x15]K\xd6\x10+\xdb\xa8\xb4\xedo\xd2hc\xf9x'
>>> bip32.get_pubkey_from_path("m/1/1/1/1/1/1/1/1/1/1/1")
b'\x02\x0c\xac\n\xa8\x06\x96C\x8e\x9b\xcf\x83]\x0c\rCm\x06\x1c\xe9T\xealo\xa2\xdf\x195\xebZ\x9b\xb8\x9e'
```

## Installation

```sh
pip install hdwallets
```

## Interface

All public keys below are compressed.

All `path` below are a list of integers representing the index of the key at each depth.

#### BIP32

##### from_seed(seed)

__*staticmethod*__

Instantiate from a raw seed (as `bytes`). See
[bip-0032's master key generation](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#master-key-generation).

##### from_xpriv(xpriv)

__*staticmethod*__

Instantiate with an encoded serialized extended private key (as `str`) as master.

##### from_xpub(xpub)

__*staticmethod*__

Instantiate with an encoded serialized extended public key (as `str`) as master.

You'll only be able to derive unhardened public keys.

##### get_extended_privkey_from_path(path)

Returns `(chaincode (bytes), privkey (bytes))` of the private key pointed by the path.

##### get_privkey_from_path(path)

Returns `privkey (bytes)`, the private key pointed by the path.

##### get_extended_pubkey_from_path(path)

Returns `(chaincode (bytes), pubkey (bytes))` of the public key pointed by the path.

Note that you don't need to have provided the master private key if the path doesn't include an index `>= HARDENED_INDEX`.

##### get_pubkey_from_path(path)

Returns `pubkey (bytes)`, the public key pointed by the path.

Note that you don't need to have provided the master private key if the path doesn't include an index `>= HARDENED_INDEX`.

##### get_xpriv_from_path(path)

Returns `xpriv (str)` the serialized and encoded extended private key pointed by the given path.

##### get_xpub_from_path(path)

Returns `xpub (str)` the serialized and encoded extended public key pointed by the given path.

Note that you don't need to have provided the master private key if the path doesn't include an index `>= HARDENED_INDEX`.

##### get_master_xpriv(path)

Equivalent to `get_xpriv_from_path([])`.

##### get_master_xpub(path)

Equivalent to `get_xpub_from_path([])`.
