# Patch for Qt 5.15.8

## Building
Requirements:
* Linux environment - this version was tested on 20.04.2 LTS (GNU/Linux 5.4.0-67-generic x86_64)
* See https://wiki.qt.io/Building_Qt_5_from_Git for a full list of requirements for building Qt

### Building wolfSSL
Please follow [README](https://github.com/wolfSSL/osp/blob/master/qt/README_v515.md).

### Building Qt 5.15.8 with wolfSSL
1. Prepare Qt 5.15.8

2. Apply 5.15 patch to Qt 5.15.8
```
wget https://raw.githubusercontent.com/wolfSSL/osp/master/qt/wolfssl-qt-515.patch
cd /path/to/qt5158/qtbase
git apply -v ../wolfssl-qt-515.patch
```
3. Apply 5.15.8 patch

Get patch folder from [osp/qt/5.15.8](https::/github.com/wolfSSL/osp/qt/5.15.8/)

cp all contents in the folder to /path/to/qt5158/

Run shell command

```
$cd /path/to/qt5158
$v5158_path.sh
```

4. Add unit test program(Optional):

   4-1. Clone the OSP repository to get unit test program

   ```
   git clone https://github.com/wolfssl/osp.git /path/to/clone-osp-folder
   ```

   4-2. Copy unit test patch

   ```
   cd /path/to/qt5158

   cp /path/to/clone-osp-folder/5.15.8/v5158_wolf_unitest.patch ./

   ```
   4-3. Apply patch

   ```
   cd qtbase
   patch ./qtbase/tests/auto/network/ssl/ssl.pro ../v5158_wolf_unitest.patch
   ```

   4-4. Copy unit test folder and certificate files

   ```
   copy /path/to/osp-repo/qt/qssl_wolf folder to /path/to/qt5/qtbase/tests/auto/network/ssl
   copy crt, key and pem files from /path/to/qt5158/qtbase/tests/auto/network/ssl/qsslsocket/certs to \
        /path/to/qt5158/qtbase/tests/auto/network/ssl/qssl_wolf/certs folder
   ```
5. Configure build and install
Please follow [README](https://github.com/wolfSSL/osp/blob/master/qt/README_v515.md)

Note1: <strong>For wolfSSL configuration, you need to add `-DALLOW_INVALID_CERTSIGN` for CFLAG Option to make `qsslcertificate` tests passed. </strong>
This needs because later versions than wolfSSL v5.6.4 doesn't allow a certificate which has `CA:FALSE` and `Certificate Sign` set. As such certificate, there is qtbase/tests/auto/network/ssl/qsslcertificate/verify-certs/test-ocsp-good-cert.pem for `qsslcertificate` test.

```
comments from asn.c
/*
 *   https://datatracker.ietf.org/doc/html/rfc5280#section-4.2.1.9
 *   If the cA boolean is not asserted, then the keyCertSign bit in the
 *   key usage extension MUST NOT be asserted.
 */
```

## Running tests
Please follow [README](https://github.com/wolfSSL/osp/blob/master/qt/README_v515.md)
