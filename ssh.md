~/.ssh/ssh_config
TODO: examples

Generating RSA key pair:

```
# Generate the RSA Keypair (it's your secret. Keep it private all the time)
openssl genpkey -algorithm RSA -out tjarosik_p51_private_key.pem -pkeyopt rsa_keygen_bits:4096
# Extract public component:
openssl rsa -pubout -in tjarosik_p51_privatekey.pem -out tjarosik_p51_public_key.pem
# convert public key format for SSH (that starts with: "ssh-rsa AAAAB")
ssh-keygen -f tjarosik_p51_privatekey.pem -y > tjarosik_p51_public_key_for_ssh.pub
# put your ssh pub key to ~/.ssh/authorized_keys on a server
```
