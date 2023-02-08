# wtclient

is a command-line client for uploading and downloading from WeTransfer. It can be used when you need to create some automation
that involves WeTransfer - you can use it to download and upload transfers. Downloading transfers and uploading email
transfers (the ones that provide download notifications) requires having a Plus account - get yours at https://wetransfer.com/plus

Uploading link transfers is permitted for free users as well.

## Choosing the application for your operating system

Use the binary provided in the ZIP archive that is fitting with your OS - if you use Windows grab the `-windows` one, on macOS use the `-darwin` one.
The Linux client is compiled to run on both 64 and 32 bit Linux distros (we build on Ubuntu 14.04 LTS).

## Running the client

Open your shell and go to the directory hosting the `wtclient` binaries. Start the one suitable for your operating system and pass it the `--help` flag:

    ./wtclient --help

This shows the supported commands. The commands and the arguments are specified first, then the flags. For example, to upload some files and get
a short URL:

    ./wtclient upload my-file.docx another-file.docx

## Running the client as a Plus user without logging in

If you'd like to run the client without having to login each time:

First login normally but with the `--show-token` flag.

    ./wtclient login --show-token

When you complete the login process, the CLI will give you a Plus Token. Treat this like
an API key / secret -- do not commit it to version control, etc. Now when you use the client
you can pass the token as an environment variable, like so:

    WT_API_PLUS_USER_TOKEN=your-token-here ./wtclient ...

Please note that at this time there is only one token per Plus account.

## Changelog

### r2019-04-12-2

* Clarify `--show-token` flag on login for Plus users

### r2019-04-12-1

* Clarify `--show-token` flag on login

### r2019-02-11-1

* Make sure we do not send the Plus token when doing the Plus user login with email/password

### r2018-10-01-1

* Permit free link transfers without Plus login

### r2018-05-22-1

* Skew fix and retries

### r2018-05-01-1

* Fix int overflow on uploading larger files

### r2018-02-26-1

* Fix the bug with the download progress bar never leaving the 0% state and hanging until the download completed

### r2018-01-31-1

* Implement multipart PUT uploads to bring client in line with website upload flow

### r2017-11-13-1

* Added `--show-token` to `login` so that the obtained Plus token can be show in the terminal. This token can then be supplied externally as an
  environment variable (which helps building scripts on top of wtclient)
* Improve handling of directory upload attempts (explain better why we do not support them)
* Implement clock skew handling so that clients with way-off clock still can use the API

### r2017-06-27-1

* Fixed a possible goroutine deadlock in the parallel uploaders, which could occur when more than 1 goroutine stops with an error

### r2017-06-26-1

* Fixed path sanitization on Windows (it will correctly compute base paths now) (#9)
* Fixed uploads smaller than 5MB being "padded" to 5MB due to incorrect buffer sizing (#10)
* Avoid panicking too often, improve error passthru
* Explicitly refuse empty files
