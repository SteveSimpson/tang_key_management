# tang_key_management

A MariaDB Encryption Key Management Plugin for Tang server

The beauty of this schema is that neither the MariaDB server, or the Tang server know
the secret key encryption key. It takes both to derive it & that key is never actually
on the wire.

The registration program will 

## Project Goals

The initial goal of this project is to make a secure network key management replacement
for the file_key_management_plugin for MariaDB. This will have the same capabilities as
the that plugin but instead of storing the key in a simple on disk, it will use the
clevis / tang key management system.

The first iteration will not include automated key rotation at either end, but later releases
will rotate the keys on both sides.

The first iteration should work with a primary & backup server as shown below.

## Project Structure

This should be a drop in replacement for the file_key_management_plugin in MariaDB.

Configuration would simply be creating the key:

```
maria-register-tang <server> <filename>
maria-register-backup-tang <server> <filename1> <filename2>
```

where 

- **server** would be something like *tang1.example.com:6789*
- **filepath** would tyipcally be */var/lib/mysql/maria-tang.jwe*


### Settings to configure for MariaDB

```
tang_key_management_filename_1 <filename1>
tang_key_management_filename_2 <filename2>
tang_key_management_encryption_algorithm aes_ctr
```