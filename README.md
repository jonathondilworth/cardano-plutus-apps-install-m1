## PPP: Local Playground (MacOS M1, 2020)

*Hopefully, this helps solve any issues surrounding the use of MacOS M1 machines in time for those people to get involved in the course as soon as possible.*

*Note: I have [included a set of audioless videos](#videos) that demonstrate the process that is outlined in §3; as perhaps it's helpful to see, rather than to read.*

#### 1. Introduction

The purpose<sup><a href="fn1">1</a></sup> of this document is to assist plutus pioneer students encountering problems with [plutus-apps](https://github.com/input-output-hk/plutus-apps) build procedures due to their preference to use MacOS as a primary operating system for the course. The overarching goal is to ensure that individuals who choose to use the M1 RISC chips from Apple are not penalised for their choice of hardware, situating all enrollees on an equal footing.

Having spent several hours pouring over this myself, I could not provide adequate assistance to any struggling peers - a couple of hours sleep later, I thought I ought to share at least the procedure that seemed to work for me.

#### 2. Reccomended Reading

* Renzwo provided the initial materials (specifically for M1 chips) that seemed to solve the build issues for me: [https://github.com/renzwo/cardano-plutus-apps-install-m1](https://github.com/renzwo/cardano-plutus-apps-install-m1) - I pretty much followed these instructions but also restarted my machine several times.
* There appears to be some variance in what works on different builds, XiTouch has produced a StackExchange post on some of the issues students encountered today: [https://cardano.stackexchange.com/questions/6287/lessons-learned-setting-up-plutus-playground-feedback-welcome](https://cardano.stackexchange.com/questions/6287/lessons-learned-setting-up-plutus-playground-feedback-welcome)


#### 3. The Procedure That Seemed To Work For Me

*Taken mainly from Renzwos repo.*

This worked for me, as you will see in the video links below. Hopefully, it'll work for you, but there are no guarantees.

**3.1. Installing Nix**

```
sh <(curl -L https://nixos.org/nix/install) --darwin-use-unencrypted-nix-store-volume
```

**3.2. Close Your Terminal Window / Restart Your Machine**

As has been explained with the [post by XiTouch](https://cardano.stackexchange.com/questions/6287/lessons-learned-setting-up-plutus-playground-feedback-welcome).

**3.3. Modify Your Nix Configuration**

The file can be found here:

```
sudo vim /etc/nix/nix.conf
```

Copy & pasted from Renzwo, this is my exact config:

```
build-users-group = nixbld

substituters        = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/
trusted-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=

system = x86_64-darwin
extra-platforms = x86_64-darwin aarch64-darwin

sandbox = false
extra-sandbox-paths = /System/Library/Frameworks /System/Library/PrivateFrameworks /usr/lib /private/tmp /private/var/tmp /usr/bin/env
experimental-features = nix-command
extra-experimental-features = flakes
```

**3.4. Restart Your Machine**

Seriously.

**3.5. Cloning plutus-apps**

Do not clone iohk/plutus; you want plutus-apps, which **can be found here: [https://github.com/input-output-hk/plutus-apps](https://github.com/input-output-hk/plutus-apps)**

```
git clone https://github.com/input-output-hk/plutus-apps
```

**3.6. Change Directory**

```
cd plutus-apps/
```

**3.7. Git Checkout**

Checkout to the appropriate commit. It may have changed by this time, so please double-check: [https://github.com/input-output-hk/plutus-apps](https://github.com/input-output-hk/plutus-apps)

```
git checkout 7f53f18dfc788bf6aa929f47d840efa1247e11fd
```

**3.8. Playground Server Build**

Attempt to build the server (good luck).

```
nix-build -A plutus-playground.server
```
**3.9. (Optional) Restart Your Machine**

I don't think I restarted the first time around, but after reading the StackExchange post, I would close my terminal window and restart the machine.

**3.10. Change Directory**

Back To plutus-apps:

```
cd ~/plutus-apps/
```

*This is where we deviate a little.*

**3.11. Start A Nix Shell**

```
nix-shell
```

**3.12. Build Client**

```
nix-build -A plutus-playground.client
```

**3.13. Open A New Nix Shell**

Go ahead and open a new terminal window, change Directory to:

```
cd ~/plutus-apps
```

and start a nix-shell:

```
nix-shell
```

**3.14. Change Directory (& start server)**

To the server:

```
cd plutus-playground-server
```
and

```
plutus-playground-server
```

**3.15. Patience is a virtue**

Just wait for the server, don't interrupt it because you think it's doing nothing; you should see something like this:

```
[nix-shell:~/Documents/Source/plutus-apps/plutus-playground-server]$ plutus-playground-server
plutus-playground-server: for development use only
[Info] Running: (Nothing,Webserver {_port = 8080, _maxInterpretationTime = 80s})
Initializing Context
Initializing Context
Warning: GITHUB_CLIENT_ID not set
Warning: GITHUB_CLIENT_SECRET not set
Warning: JWT_SIGNATURE not set
Interpreter ready
```

**3.16. Change Back To Other Terminal Window & NPM**

*global*

```
sudo npm install -g npm
```

**3.17. Start The Client**

```
cd plutus-playground-client
```

```
npm run start
```

<h3 id="videos">Recordings Of The Procedure</h3>

*These are really boring, but in case you would rather watch the procedure:*

1. [Video One](https://youtu.be/OB0OeveN6Ao)
2. [Video Two](https://youtu.be/QVTmbi1U39Q)
3. [Video Three](https://youtu.be/qyqfmYUqjP8)

#### credit where credit's due

Thank you, Renzwo, for the materials in the initial repo. I would have likely been banging my head against the wall for a little while longer otherwise. Cheers!
