# Git Tips and Tricks

## Configuring SSH key with Git
### Step 1 : Generate the SSH Keys

Open git bash and type command `ssh-keygen -t rsa -Cbiplab.nayak@company.com `

    bipnayak@LIN19000307 MINGW64 ~/AppData/Local/Programs/Git
    $ ssh-keygen -t rsa -C "biplab.nayak@capgemini.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/c/Users/bipnayak/.ssh/id_rsa):
    Created directory '/c/Users/bipnayak/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /c/Users/bipnayak/.ssh/id_rsa.
    Your public key has been saved in /c/Users/bipnayak/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:mD7Bkuqv6NLnZ6F8Ix2yL+nQiWgxsQjfH+TCg6+sSN8 biplab.nayak@company.com
    The key's randomart image is:
    +---[RSA 2048]----+
    |                 |
    |                 |
    |..    .          |
    |o.o+ = o         |
    |.+o B B S        |
    | .o=.Boo         |
    |.++.+*+o         |
    |=+ooX *.         |
    |*o=B+E..         |
    +----[SHA256]-----+

This will generate 2 keys in `~/.ssh` directory. ine is the public key (`id_rsa.pub`) and other one is private key(`id_rsa`).

### Step 2 : Copy the text of public key configure it in your github / bitbucket account.
For github : Settings > SSH and GPG keys > New SSH key
For bitbucket : Manage Account > SSH Keys > Add key

### CAUTION : DONOT SHARE PRIVATE KEY TO ANYONE.

## Manage multiple SSH Keys :

* Create SSH keys with different names for each account.
* Import the public keys to th erespective accounts
* Add the private key to ssh agent. you might need to start the agent before adding it. otherwiise it will throw some error.

For eg.

    bipnayak@LIN19000307 MINGW64 /d/test
    $ ssh-add /c/Users/bipnayak/.ssh/github
    Could not open a connection to your authentication agent.
    
    bipnayak@LIN19000307 MINGW64 /d/test
    $ eval `ssh-agent -s`
    Agent pid 9172
    
    bipnayak@LIN19000307 MINGW64 /d/test
    $ ssh-add /c/Users/bipnayak/.ssh/github
    Identity added: /c/Users/bipnayak/.ssh/github (/c/Users/bipnayak/.ssh/github)
    
    bipnayak@LIN19000307 MINGW64 /d/test
    $ git clone git@github.com:biplabnayak/poc.git
    Cloning into 'poc'...
    remote: Counting objects: 172, done.
    remote: Total 172 (delta 0), reused 0 (delta 0), pack-reused 172
    Receiving objects: 100% (172/172), 72.67 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (20/20), done.
    


## Change a commit message after push
