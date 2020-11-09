# sign_verify_cmds

Commands to generate and verify signatures

Signature Envelope Generation:

openssl smime -sign -in Plaintext.txt -signer PK.crt -inkey PK.key > Signed-Plaintext.txt

openssl smime -verify -in Signed-Plaintext.txt -signer PK.crt -noverify

================

PECoff / EFI File signature appending:

sbsign --key PK.key --cert PK.crt --output HW_Signed.efi HW_Unsigned.efi

diff -y <(xxd HW_Signed.efi) <(xxd HW_Unigned.efi) | colordiff

sbverify --cert PK.crt HW_Signed.efi

===============

RPM Signature inserted before header:

rpm -Kv hello.rpm

rpm --define "_gpg_name Acme Corp" --addsign hello.rpm

xxd -c 2 US_hello.rpm | cut -c15- > hu

==============

Detached Signatures:

openssl smime -sign -binary -in Plaintext.txt -signer PK.crt -inkey PK.key -outform der -out file.p7b

openssl smime -verify -binary -inform der -in file.p7b -content Plaintext.txt -noverify

=============
