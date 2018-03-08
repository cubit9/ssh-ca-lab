# ssh-ca-lab

I found myself in need of a lab to play around with SSH certificate authorities for a recent project, so I built one.  This project will bring up a certificate authority as well as one client and one ssh server.  It also does all the necessary deployment of host, server and signing keys, as well as the signing of said keys.

Once the environment is up and running, the root user on client1 should be able to SSH to server1 without being prompted to accept a new host key or to enter a password.  The magic of a SSH certificate authorityin action!

## Getting Started

Before bringing the environment up via docker-compose, we'll need to generate all of the SSH host, user and signing keys.  We also need to sign our server's host public key and our user's public key in order to take advantadge of the benefits of our certificate authority.

Kick this off by running the build script:
```
sh ./build.sh
```

Next, bring up the environment via docker-compose:
```
docker-compose up
```

## Using The Project

Open a shell on the client container:
```
docker exec -it sshcalab_client1_1 sh
```

From inside the client container, attempt to SSH to the server container:
```
ssh root@server1
```

If you weren't prompted to accept a new host key for server1 or for a password, you've just wittenessed the glory of a SSH certificate authority in action!

## Tearing It Down

If you ever want to modify or rebuild this project, it's import to teardown the old SSH keys we generated for our containers in between builds.  The ```build.sh``` script also modifies client1's Dockerfile directly (to add our CA to known_hosts), so we want to remove those edits as well.

To remove the artificats of the build script, use the ```remove.sh``` script:
```
sh remove.sh
```

## Disclaimer

All SSH keys used in this project are being generated without passkeys, which should certainly not be followed in production.  The containers and scripts generated by this project should be used for learning purposes only and should never be public facing!

## Acknowledgments
All containers used in this project are based on [Alpine Linux SSHD](https://github.com/sickp/docker-alpine-sshd/) by [Adrian B. Danieli](https://github.com/sickp)
