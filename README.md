# i2pd-tools

This repository contains tools that supplement i2pd.

Notice: git submodules are used so make sure to clone this repository recursively

    git clone --recursive https://github.com/purplei2p/i2pd-tools

## Building

### Dependencies

* boost chrono
* boost date-time
* boost filesystem
* boost program-options
* libssl
* zlib1g

```bash

depend="libboost-chrono-dev \
    libboost-date-time-dev \
    libboost-filesystem-dev \
    libboost-program-options-dev \
    libboost-system-dev \
    libboost-thread-dev \
    libssl-dev \
    zlib1g-dev"
kernel=`uname -a`

case "$kernel" in
*ubuntu*)
	sudo apt install $depend;;
*debian*)
	sudo aptitude install $depend;;
*gentoo*)
	sudo emerge --deep --newuse dev-libs/boost dev-libs/openssl;;

*)
	echo "Just install libboost and libopenssl dev packages on your pc";;
esac
```

### Building

    make

## Tools included

### routerinfo

print information about a router info file

#### usage


print ip and port for router info excluding ipv6

    ./routerinfo -p ~/.i2pd/netDb/r6/routerInfo-blah.dat

print iptables firewall rules to allow 1 nodes in netdb through firewall including ipv6 addresses

    ./routerinfo -6 -f ~/.i2pd/netDb/r6/routerInfo-blah.dat

### keygen

Generate an i2p private key

#### Usage

Make a DSA-SHA1 destination key

    ./keygen privkey.dat

Make an destination key with a certain key type

    ./keygen privkey.dat <number>

or

    ./keygen privkey.dat <key name>


| key name             | number |
| -------------------- | ------ |
| DSA-SHA1             | 0      |
| ECDSA-SHA256-P256    | 1      |
| ECDSA-SHA384-P384    | 2      |
| ECDSA-SHA512-P521    | 3      |
| RSA-SHA256-2048      | 4      |
| RSA-SHA384-3072      | 5      |
| RSA-SHA512-4096      | 6      |
| EDDSA-SHA512-ED25519 | 7      |

### vain

Vanity generation adress.

#### Usage

./vain --usage

#### Time to Generate on a 2.70GHz Processor
| characters| time to generate (approx.) |
| -------------------- | --------------- |
|         1 	       | ~0.082s	 |
|         2	       | ~0.075s	 |
|         3	       | ~0.100s	 |
|         4	       | ~0.394s	 |
|         5	       | ~6.343s	 |
|         6	       | ~1m-5m	 	 |
|         7	       | ~30m	 	 |

### keyinfo

Prints information about an i2p private key

#### Usage

Print just the b32 address for this key

     ./keyinfo privatekey.dat

... just the base64 address

    ./keyinfo -d privatekey.dat

Print all info about the public key

    ./keyinfo -v privatekey.dat

### regaddr

Generate authentication string to register a domain on stats.i2p

    ./regaddr domain.dat domain.i2p > auth_string.txt
    cat auth_string.txt

Send output of auth_string to http://stats.i2p/i2p/addkey.html
To register a subdomain, use [`regaddr_3ld`](#regaddr_3ld)

### regaddr_3ld

Generate authentication string to register a subdomain on stats.i2p in 3 steps

    ./regaddr_3ld step1 sub_domain.dat sub.domain.i2p > step1.txt
    ./regaddr_3ld step2 step1.txt domain.dat domain.i2p > step2.txt
    ./regaddr_3ld step3 step2.txt sub_domain.dat > step3.txt
    cat step3.txt

Send output from step3 to http://stats.i2p/i2p/addkey.html
