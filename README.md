# Intro

Un petit challenge pas trop compliqué, qui m'aura demandé plus de temps à adapter ma source qu'a chercher.

# Le chall

Notre petit camarade de bureau se croit malin en nous mettant au défi de répondre à une question soit disant impossible, lui donner l'heure précise du jour.

Comme nous sommes des techniciens, nous savons qu'il existe une heure commune à tout les équipements, l'EPOCH.

Cette petite merveille est normalement syncronisée et identique partout. Nous allons donc nous en servir pour lui donner.

# Le script

Je vous présente Evil cat, le pourfendeur de collègues. Grace à lui, nous allons renvoyer la bonne réponse à notre collègue, et on aura même le temps pour un petit café !

```python
#!/usr/bin/env python3
from pwn import *
import time
import re

#context.log_level = "debug"
url = "challengecybersec.fr"
port = 6660
r = remote(url,port)
t = int(time.time())
regex = "DGSESIEE{.*?}"

def sockClose():
    r.close()

def Exploit():
    info("Evil cat miaule très fort")
    r.sendafter(":\n",str(t))
    a = r.recvline()
    s = re.findall(regex, str(a))
    if not str(s):
        warn("Evil cat ne trouve pas son jouet")
    else:
        for answer in s:
            success("Evil cat est content : %s" % str(answer))
            success("Evil cat range sa proie dans Flag.txt")
            f = open("Flag.txt","w")
            f.write(answer)
            f.close()

info("Evil cat se réveille")
try:
    Exploit()
except:
    warn("Evil cat vient de se casser les dents")
finally:
    info("Evil cat va se coucher")
    sockClose()
```

La petite subtilité se trouve dans l'initialisation de la variable **t**, en effet, par défaut `time.time()` retourne un nombre flottant, et ce n'est pas la réponse attendue, en passant sur un entier, nous avons la bonne réponse.

Une fois sa proie attrapée, Evil cat la stock dans Flag.txt, afin de la partager avec vous ! Il est trop adorable ce chat !

# Remerciement

[La DGSE](https://www.defense.gouv.fr/dgse)

[L'ESIEE (en voilà une école que j'aurais aimé connaitre plus jeune)](https://www.esiee.fr/)

[Damien Letoffe, pour m'avoir invité dans l'équipe des touristes.](https://www.linkedin.com/in/damien-l-51b8b5b7/)
