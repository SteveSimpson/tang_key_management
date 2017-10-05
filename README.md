# tang_key_management

A MariaDB Encryption Key Management Plugin for Tang server

The beauty of this schema is that neither the MariaDB server, or the Tang server know
the secret key encryption key. It takes both to derive it & that key is never actually
on the wire.


## Project Goals

The initial goal of this project is to make a secure network key management replacement
for the file_key_management_plugin for MariaDB. This will have the same capabilities as
the that plugin but instead of storing the key in a simple on disk, it will use the
clevis / tang key management system.

The first iteration will not include automated key rotation for MariaDB. However, rotation
of the key signing key is possible by using the backup registration program to re-register
the server after a new tang key has been generated.

The first iteration should work with a primary & backup server as shown below. (May look at
more slots later.)

## Project Structure

This should be a drop in replacement for the file_key_management_plugin in MariaDB.

Configuration would simply be creating the key:

```
maria-register-tang <server> <filename1>
maria-register-backup-tang <server> <filename1> <filename2>
```

where 

- **server** would be something like *tang1.example.com:6789*
- **filenameN** would tyipcally be */var/lib/mysql/maria-tangN.jwe*

The registration program, *maria-register-tang*, will create & encrypt a master key that
will encrypted with the key encrypting key derived by clevis/tang.

The backup registration program, *maria-register-backup-tang*, will decrypt the master key
then re-encrypt it with a second key-encrypting key derived by clevis/tang.

### Settings to configure for MariaDB

```
tang_key_management_filename_1 <filename1>
tang_key_management_filename_2 <filename2>
tang_key_management_encryption_algorithm aes_ctr
```