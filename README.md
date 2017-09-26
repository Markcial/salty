# salty

[![PyPI version](https://badge.fury.io/py/Salty.svg)](https://badge.fury.io/py/Salty)

Small password management utility made with [PyNaCl][1]

## Usage

The salty library exposes different commands for use via command line or via python scripts.

### Via the command line

The `salty` command has implicit help in case of need.

    usage: salty [-h] {keys,messages,encrypt,decrypt} ...

    Salty password management tool.

    positional arguments:
      {keys,messages,encrypt,decrypt}
                            Sub command list
        keys                Key sub command
        messages            List available messages
        encrypt             Encryption sub command
        decrypt             Decryption sub command

    optional arguments:
      -h, --help            show this help message and exit

If you write down `salty keys` it will list the available encryption keys.

salty keys has a serie of sub command for key management.

  * current: Displays the selected and active key that will be used for encryption
  of the messages.
  * new: Will create a new key
  * set: Will update the key, if the flag `--pos` is sent will set the pointer
  to that position, if the flag `--value` is set, the whole key will be replaced
  by the one sent as a value.

The messages sub command will list the messages by default.

    ~$ salty messages
    Message named (foo) : KWJgu37hoNNgg7MQiEGth284ACeZEwouSAm3jJ7v3qxQwAKVChll-X2OhmAMQHOjF1PbhkFOGz9dDPQ=
    Message named (foo bar) : c6WuwELRau1LweFio-mhL1FCNsIIrx-ryPMZ5K2Ee-ktY5mQd4Sx67grTYNWwj0r4Ne2X7flCQRPWh-vndnDhkkIBHhdi1GXiYk=
    Message named (mysql.password) : 9js0YDdxw2U3-P9MuTvm2Rk-MrCwmX_oAajf0sh-ZjE92Vm3yzmgnOD1EzWDD_AXqqiBqEE=
    Message named (really.secret) : B23TDCYKOzJ7pQ50ehqC27lRFOG-zDccgIDS5PiPSbwlezqyCUY5LWGbavWDGW3r9a36l8LV5ZY05g==

That messages are safe to send, share or publish on github, pastebin, slack etcetera.
The key token must be shared privately for the decryption.

The messages are pairs of key, encrypted values. This way you can check
all the available secrets and use the name as identifier on case you want to
decrypt that concrete message.

The encrypt sub command adds a new secret to the store.

    ~$ salty encrypt ftp.password.vendor "s0m3h4x0rPWD"
    ~$ salty messages                                                                                                                                                                   (375ms)
    Message named (ftp.password.vendor) : FgnF5JRIqUb_DnWvciNyNPdEIVmufBOAJrVhmYytwIuJ0vwOHkAPcWjSkIAig1Q4qygFDA==
    Message named (foo) : KWJgu37hoNNgg7MQiEGth284ACeZEwouSAm3jJ7v3qxQwAKVChll-X2OhmAMQHOjF1PbhkFOGz9dDPQ=
    Message named (foo bar) : c6WuwELRau1LweFio-mhL1FCNsIIrx-ryPMZ5K2Ee-ktY5mQd4Sx67grTYNWwj0r4Ne2X7flCQRPWh-vndnDhkkIBHhdi1GXiYk=
    Message named (mysql.password) : 9js0YDdxw2U3-P9MuTvm2Rk-MrCwmX_oAajf0sh-ZjE92Vm3yzmgnOD1EzWDD_AXqqiBqEE=
    Message named (really.secret) : B23TDCYKOzJ7pQ50ehqC27lRFOG-zDccgIDS5PiPSbwlezqyCUY5LWGbavWDGW3r9a36l8LV5ZY05g==

If you need to check some password you can use the decrypt sub command.

    ~$ salty decrypt ftp.password.vendor
    s0m3h4x0rPWD


### Via python scripts

The salty api is accesible via python scripts too.

    from salty import get_secret
    mysql_password = get_secret('mysql.password')

    print(mysql_password)  # b'some password'

You can check the current key or change it too.

    from salty import current
    print(current()) #  etOh7GYrvadpN9rwUSP0Itv-wlLeONLsPu3C9CmYiAM=
    current('ao-UmLDEPrg4Ypr729kMlx0pPiBwrluVRc4v-DJJPzI=') #  True
    print(current()) #  ao-UmLDEPrg4Ypr729kMlx0pPiBwrluVRc4v-DJJPzI=


Add new secrets.

    from salty import add_secret
    add_secret('delightful.paranoia', 'some words that i might want to be hidden')
    print(secrets()['delightful.paranoia']) #  _Y-4EFLiyUb4ZCR1LA5250bxo5kwCAczf1iQInCO-WgvrmkscTjAkT_-PolJ7DJImblUd9U_nx8D6wa-bu_7P2xQKgxK8iQtCCh9-5Bz_Hxl

And decrypt them back

    from salty import get_secret
    print(get_secret('delightful.paranoia')) #  some words that i might want to be hidden



[1] https://github.com/pyca/pynacl