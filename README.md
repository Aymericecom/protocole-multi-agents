# Un cerveau, plusieurs mains

**Un protocole de travail multi-agents.** Comment faire travailler un agent orchestrateur et ses sous-agents sur une même tâche sans que la réflexion se perde à chaque passage de relais.

Deux parties :

- **I — Le protocole.** Ce qui circule entre les agents, qui décide, où le travail s'arrête. Valable quel que soit l'outil.
- **II — Le mettre en place quand on n'a rien.** Trois prompts à copier, pour partir d'un dossier vide.

Issu d'une pratique réelle, généralisé : il ne dépend d'aucun outil, d'aucun modèle et d'aucun métier en particulier.

---

# Partie I — Le protocole

## 01 · Le problème : découper le travail en agents découpe aussi la mémoire

Quand un seul agent fait tout, le résultat est bon pour une raison mécanique : **celui qui regarde, celui qui décide et celui qui écrit la consigne ont la même mémoire.**

Dès qu'on découpe en sous-agents, on découpe cette mémoire avec. Chacun reçoit un brief, exécute, répond « fait ». Ce qu'il a remarqué en passant, ce qui l'a fait hésiter, ce qu'il a écarté — tout cela meurt avec sa session.

Le problème n'est donc pas le nombre de modèles. C'est que **seule la tâche est transmise, jamais la pensée**. Une chaîne de dix agents parfaits produit un travail moins bon qu'un agent seul, parce qu'à chaque relais elle repart d'une page presque blanche.

> **Le symptôme à reconnaître.** La chaîne « fait moins bien » que quand vous faisiez tout vous-même, et personne ne sait dire où ça s'est dégradé. C'est toujours ça : la réflexion n'a pas circulé.

## 02 · La règle : le journal est le cerveau, les modèles sont les mains

Un chantier = **un seul fichier `JOURNAL.md`**, à la racine du dossier de travail. Ce n'est pas un compte rendu : c'est la pensée du chantier, tenue en commun.

> **Règle unique — personne n'agit sans l'avoir lu en entier, personne ne finit sans y avoir écrit.**

« En entier » est littéral : pas un résumé, pas un extrait, pas un brief tiré du journal. Résumer, c'est refaire exactement ce qu'on cherche à éviter — filtrer la pensée pour ne garder que la tâche.

**Ce que chaque sous-agent fait, dans cet ordre :**

1. Il **lit tout le journal**. Il hérite ainsi de ce que les précédents ont vu, décidé et raté.
2. Il fait sa tâche.
3. Il **écrit son entrée**, cinq rubriques, toujours les mêmes.

Il ne dit jamais « fait ». **Un résultat sans VU ni DOUTE est renvoyé** — c'est la seule sanction du protocole, et elle suffit.

## 03 · Le format : cinq rubriques, jamais quatre

La forme n'est pas décorative. Chaque rubrique force une chose que les agents ne font pas spontanément.

**VU**
Ce qu'il a *regardé* et ce qu'il y a vu, en faits. Pas d'adjectif sans l'observation qui va avec. Force à regarder au lieu de supposer.

**DÉCIDÉ**
Ce qu'il fait *et pourquoi* — une phrase de raisonnement, pas une case cochée. C'est ce qui rend la décision rejouable par le suivant.

**PRODUIT**
Les fichiers, *avec leur chemin*, et la *consigne exacte* utilisée. Une consigne qui a marché et qu'on n'a pas notée est perdue.

**POUR LE SUIVANT**
Ce qu'il doit savoir, quelles références lui passer, et surtout *ce qu'il ne doit pas refaire*. C'est la rubrique qui économise le plus de temps.

**DOUTE**
Ce qui l'a surpris ou qu'il n'a pas pu vérifier. *Vide interdit* : « rien » doit être justifié. C'est là que sont les défauts qui coûtent cher plus tard.

### Le gabarit, à recopier tel quel

```
## [hh:mm] <agent> · <étape>
VU : sortie/lot3-07.txt — les 4 champs obligatoires sont remplis, mais
     le champ « source » reprend l'identifiant de l'élément 6.
DÉCIDÉ : reprendre l'élément 7 seul, en repartant de la sortie de
     l'élément 6 comme référence. Un seul changement à la fois.
PRODUIT : sortie/lot3-07-v2.txt — consigne exacte : « … »
POUR LE SUIVANT : les éléments 1 à 6 sont acquis, ne pas les rejouer.
     Passer lot3-06 en référence à l'agent de contrôle.
DOUTE : je n'ai pas pu vérifier la source de l'élément 5 — à contrôler
     avant la livraison.
```

Une entrée par étape, dans l'ordre, horodatée, jamais réécrite après coup. Un journal qu'on nettoie perd sa valeur : les erreurs et les hésitations sont précisément ce qu'on transmet.

## 04 · La circulation : les sous-agents ne se parlent jamais entre eux

Le sens de circulation est contre-intuitif et il est structurant : **l'orchestrateur est le seul point de contact.** Il dispatche une tâche, le sous-agent l'exécute et rend son résultat. Le sous-agent ne pose pas de question à un autre sous-agent et ne prend aucune initiative hors de son périmètre.

```
ORCHESTRATEUR
lit tout · décide · valide · escalade
   │
   ├── Préparer    (script)
   ├── Produire    (agent)
   ├── Contrôler   (agent)
   └── Ranger      (script)

✕ aucune liaison entre les exécutants
  ils ne se voient pas, ne se lisent pas, ne s'attendent pas
```

Si le producteur a besoin du résultat du contrôleur, c'est l'orchestrateur qui le lui transmet. Deux sous-agents qui se coordonnent directement sur les mêmes fichiers, c'est du chaos garanti.

**Ce que l'orchestrateur fait — et ne fait plus :**

- **Il passe le journal, pas un brief.** Le sous-agent reçoit : le journal complet + une seule tâche + les fichiers de référence.
- **Il tient la porte entre deux maillons.** Rien ne part vers l'étape suivante tant qu'il n'a pas lu lui-même l'entrée VU du maillon précédent.
- **Il relit les DOUTE** avant de continuer. C'est le seul endroit où les problèmes apparaissent avant de coûter.
- **Il n'exécute rien de mécanique lui-même**, et n'en fait pas exécuter par un agent ce qu'un script fait mieux.

> **Pour ce qui traverse le temps.** Une tâche qui doit survivre à la session en cours ne se transmet pas de vive voix : elle va dans une boîte partagée — un fichier par tâche, avec un statut dedans (`à-faire` / `en-cours` / `fait` / `besoin-humain`).

## 05 · Le casting : agent ou script ?

La leçon la plus chère du protocole : un harnais d'exécution **tue tout sous-agent silencieux au bout de quelques minutes**, et le travail en cours est perdu. Un agent qui attend une API, qui boucle ou qui dort entre deux tours mourra en route.

| Ce que fait l'étape | Qui | Pourquoi |
|---|---|---|
| attendre, boucler, répéter une commande, déplacer des fichiers | **Un script** | Il ne cale pas, ne coûte aucun jeton, et reprend où il en était. |
| trancher, décrire, contrôler, escalader | **Un agent** | C'est le seul endroit où un modèle apporte quelque chose. |

*Un agent qui passe son temps à attendre n'est pas un agent : c'est une pause très chère qui meurt en route.*

### Quel modèle pour quelle tâche

| Nature de la tâche | Modèle |
|---|---|
| Orchestrer, décider, valider — sans rien exécuter | Le plus capable |
| **Regarder un livrable et rendre un verdict** | Le plus capable |
| Rédiger une consigne à partir d'une référence | Intermédiaire |
| Mécanique pure : indexer, formater, déplacer | Le plus léger |

> **L'exception qui se paie.** La règle générale est de redescendre chaque agent sur le modèle le moins cher qui sait faire. **Le contrôle qualité est l'exception.** Un verdict engage toute la dépense d'aval, alors qu'une analyse ratée se rattrape. Un contrôle fait par un modèle qui perçoit mal le livrable est un contrôle qui laisse tout passer — c'est la cause n°1 des chaînes qui se dégradent en silence.

Corollaire : celui qui rédige une consigne à partir d'une référence doit être **celui qui vient de regarder cette référence**. Jamais un gabarit rempli par un petit modèle qui n'a pas vu.

## 06 · Les portes : où le travail s'arrête, et ce que « vérifier » veut dire

Entre deux maillons, une porte. Ce ne sont pas des lenteurs : un livrable raté qui passe la porte pollue tout ce qui vient après.

| Situation | Action |
|---|---|
| Tâche mécanique, résultat vérifiable | L'agent exécute, l'orchestrateur vérifie |
| Décision de goût ou de jugement | **Toujours** validation humaine avant de continuer |
| Information manquante | L'agent **demande**, il n'invente jamais |
| Résultat douteux | Statut `besoin-humain`, on s'arrête |

> **La définition qui coûte trois jours quand elle manque.** « Vérifier » ne veut pas dire lire le verdict d'un sous-agent. Avant tout dispatch fondé sur un jugement, l'orchestrateur **ouvre lui-même le livrable et le compare à la référence**. Un verdict en texte ne vaut pas vérification : le passage par le texte a détruit l'information même sur laquelle porte le jugement.

> **Un verdict ne se propage jamais.** Une consigne se propage, un gabarit se propage. Un verdict, non — il porte sur un livrable précis. Même deux éléments identiques se re-regardent chacun contre sa propre référence.

> **Le garde-fou qui tient vraiment.** Écrire « il faut valider avant de continuer » dans un document ne tient pas. Mettez la contrainte dans l'outil : que le script refuse le statut « validé » sans le nom d'un humain dedans. Une règle qu'on peut oublier de lire sera oubliée.

## 07 · Les itérations : trois règles pour ce qui se produit par essais successifs

Elles se ressemblent et elles disent la même chose sous trois angles : **ce qui n'est pas verrouillé sera réinventé.** Deux productions indépendantes ne partagent rien — pas la mise en page, pas le ton, pas les détails qu'on croyait acquis.

1. **Un seul changement par essai.** Jamais deux consignes nouvelles dans le même passage. Si un élément résiste, on lui consacre un passage isolé, pour lui seul.
2. **La sortie précédente est toujours la première référence.** C'est elle qui verrouille tout ce qui ne doit pas bouger.
3. **Rien de mémoire.** Ce qui doit rester identique se *joint*, il ne se décrit pas. Un élément récurrent se donne par son fichier de référence, pas par une phrase qui le résume.

> **Et la règle qui économise le plus : on ne rejoue jamais ce qui est acquis.** Un défaut localisé se corrige sur la sortie ratée — on ne relance pas toute l'étape en espérant que ça tombe mieux. Relancer, c'est remettre en jeu ce qui était déjà bon.

## 08 · L'humain : quand la chaîne attend quelqu'un, elle le dit tout de suite

Une chaîne qui s'arrête en silence sur une question est une chaîne à l'arrêt pour la journée. Le coût n'est pas la question — c'est le délai avant que quelqu'un s'en aperçoive.

- **Dès qu'un blocage ne peut être levé que par l'humain, on le prévient immédiatement**, sans attendre qu'il repasse. Un canal qui arrive sur son téléphone, pas une ligne dans un fichier qu'il ouvrira peut-être.
- **Le message porte l'information**, pas « j'ai une question » : ce qui bloque, ce qu'on attend de lui, et ce qu'on fera de sa réponse — lisible sans rien ouvrir.
- **On ne pose pas une question et on n'attend pas.** On pose, puis on continue sur tout ce qui n'en dépend pas, et on relève régulièrement.
- **Ce qui doit être jugé va sous ses yeux**, à chaque fois, sans qu'il ait à le demander ni à chercher où c'est rangé.

## 09 · Le format des agents : dimensionner, et savoir quand arrêter

Un sous-agent long décroche. Pas parce qu'il est mauvais — parce que sa fenêtre s'épuise et qu'il finit par répondre n'importe quoi, avec assurance.

- **Des lots courts.** Deux ou trois éléments par agent, pas quinze. Mieux vaut cinq agents qui finissent qu'un seul qui meurt à mi-course.
- **Écriture sur disque au fil de l'eau.** Chaque élément traité est écrit immédiatement. Un agent qui garde tout pour son rapport final ne rendra rien s'il tombe.
- **Un compte rendu à chaque itération**, jamais un rapport final. On veut voir la dérive au troisième élément, pas au quinzième.
- **Ne pas corriger un agent dans son dos.** Modifier ses fichiers pendant qu'il travaille produit des conflits invisibles : on lui envoie un message, ou on l'arrête.

> **La règle anti-acharnement.** On fait le point à intervalle régulier, et **si ça stagne, on change de méthode — on ne réessaie pas.** Au troisième échec sur le même défaut, on arrête d'itérer et on va chercher l'information qui manque auprès de la personne qui l'a. Trois essais ratés se ressemblent : c'est la consigne qui est fausse, pas la chance.

## 10 · La boucle : ce qui a été appris doit sortir de la session

C'est l'étape que presque tout le monde saute, et sans elle la chaîne ne progresse jamais : chaque session repart de zéro et on repaie les mêmes erreurs.

> **Règle de capitalisation — le correctif va dans la définition de l'agent, pas dans la conversation**, sinon il meurt avec la session. Si un agent s'est trompé et qu'on lui a expliqué pourquoi, cette explication appartient désormais à son fichier de rôle.

En fin de chantier, un dernier maillon décide de ce qui remonte dans la base commune :

- ✅ un process qui a marché → il devient une procédure réutilisable ;
- ✅ une erreur résolue → elle s'écrit **avec sa signature**, pour être reconnue la prochaine fois ;
- ✅ une décision et sa raison → dans la note du projet concerné ;
- ❌ le déroulé de la conversation, les essais ratés, l'évident → on jette.

Il ne garde pas tout. Une base qui grossit sans filtre devient illisible, et un agent qui doit lire cinq cents fichiers pour trouver une information est un agent lent.

> **Et pour savoir où on en est : l'avancement se compte sur ce qui se juge, pas sur ce qui se construit.** Un système bien monté qui n'a encore rien produit de regardable est à zéro. La plomberie n'est pas le produit.

---

# Partie II — Le mettre en place quand on n'a rien

Pas d'éditeur de code, pas de base de connaissances, pas de scripts existants. Juste Claude Code et un dossier vide. Le système ne se construit pas à l'avance : **on fait le travail une fois à la main, et on le fait écrire.**

## 11 · Le matériel

| Il faut | Comment |
|---|---|
| **Claude Code** | L'application de bureau (Mac ou Windows) est le chemin le plus court : elle installe tout, terminal compris. Sinon, dans un terminal : `npm install -g @anthropic-ai/claude-code`, puis la commande `claude`. |
| **Un dossier** | Un dossier par chantier. On ouvre Claude Code **dans ce dossier** — c'est là qu'il lit et écrit. |
| **Rien d'autre** | Pas d'éditeur de code, pas d'abonnement supplémentaire, pas de base de connaissances à préparer. |

> **Le contresens de départ.** On croit qu'il faut d'abord construire une base de connaissances, puis automatiser. C'est l'inverse. **La base de connaissances est un sous-produit du premier chantier fait à la main** — elle s'écrit toute seule le jour où on demande à Claude de raconter ce qu'on vient de faire ensemble.

### Deux outils facultatifs, et ce qu'ils apportent vraiment

Tout ce que produit le système est du **texte brut dans un dossier** — des fichiers `.md`. Ça se lit avec n'importe quoi, y compris l'aperçu du Finder. Deux outils rendent la relecture plus confortable ; ni l'un ni l'autre n'est nécessaire pour commencer.

| Outil | Nécessaire | Ce qu'il apporte |
|---|---|---|
| **Claude Code** | **Oui** | C'est lui qui fait le travail. |
| **Obsidian** | Non | Ouvre le dossier tel quel, sans rien importer. Les liens `[[entre notes]]` deviennent cliquables, les images s'affichent dans le texte, la recherche porte sur tout le dossier d'un coup. C'est le confort de relecture du journal et des procédures, quand elles commencent à se répondre entre elles. |
| **Cursor** ou **VS Code** | Non | L'arborescence des fichiers à gauche et un terminal au même endroit : on voit les fichiers apparaître et changer pendant que ça tourne. Utile quand la chaîne produit beaucoup, inutile au départ. |

**Deux pièges d'Obsidian, appris à l'usage :**

- **Ouvrir le dossier de travail comme racine**, ni au-dessus ni en dessous. Ouvert trop haut, on navigue dans des dossiers sans rapport ; ouvert trop bas, la moitié des notes est hors du périmètre et les liens ne mènent nulle part.
- **Le dossier `.claude/` restera invisible** — Obsidian masque tout ce qui commence par un point. Les fichiers de rôle des agents ne se relisent donc pas depuis Obsidian : c'est Claude Code, ou l'éditeur, pour ceux-là.

> **Ce qui rend le système durable : il ne dépend d'aucun outil.** Le journal, les procédures, les rôles : du texte. Changer d'éditeur, de modèle ou d'assistant ne casse rien — ce qui a été appris reste lisible, et se relit dans dix ans.

## 12 · La méthode : trois temps, et le troisième se répète

1. **Faire le travail une fois, entièrement à la main, avec Claude Code.** Sans chercher à automatiser quoi que ce soit. On corrige, on râle, on refait — c'est exactement ce qu'on veut : chaque correction est une règle qui sera capturée. *Ne pas fermer la fenêtre à la fin.*
2. **Coller le prompt A dans la même conversation.** Claude relit tout ce qu'on vient de faire ensemble et en tire la procédure, les rôles, les scripts et le journal. Le dossier vide devient un système en une réponse.
3. **Les fois d'après, coller le prompt B** pour lancer un chantier, et le **prompt C** chaque fois que quelque chose rate. Le système se corrige lui-même au lieu de refaire les mêmes erreurs.

> **Pourquoi le premier temps ne se saute pas.** Une procédure écrite avant d'avoir fait le travail est une procédure inventée : elle décrit ce qu'on imagine, pas ce qui arrive. **Toute la valeur est dans les endroits où on a corrigé Claude** — et ces endroits n'existent que si on a vraiment fait le travail.

## 13 · Les prompts

Rien à adapter, sauf les crochets. Ils sont écrits pour n'importe quel travail répétable.

### Prompt A — faire écrire le système

À coller **dans la conversation où le travail vient d'être fait**, sans avoir rien fermé.

```
Tu viens de m'aider à faire ce travail à la main, du début à la fin. Ne le refais pas.

Ta tâche maintenant : transformer ce qu'on vient de faire en un système que je pourrai
relancer seul, sur le même travail, sans avoir à te réexpliquer quoi que ce soit.

1. Relis toute notre conversation depuis le début et ouvre tous les fichiers qu'on a
   produits. Repère : les étapes réelles dans leur ordre réel ; les endroits où je t'ai
   corrigé ; ce qu'on a dû refaire, et pourquoi.

2. Écris dans ce dossier :
   - CLAUDE.md : comment on travaille ici. Le contexte, mes contraintes, ce qui est
     interdit. C'est le fichier que tu liras en premier à chaque session.
   - SOP.md : la procédure, étape par étape, numérotée. Pour chaque étape : ce qui entre,
     ce qui sort, qui la fait (script ou agent), et comment on sait qu'elle est réussie.
     Ajoute sous chaque étape une rubrique « ce qui a raté et pourquoi » avec les erreurs
     qu'on a réellement commises aujourd'hui.
   - JOURNAL.md : l'entrée d'ouverture d'aujourd'hui d'après ce qu'on a fait, et en bas,
     en commentaire, le gabarit d'entrée à recopier.
   - .claude/agents/<nom>.md : un fichier par rôle que tu as identifié. Sa mission, ce
     qu'il reçoit, ce qu'il rend, ce qu'il n'a pas le droit de faire.
   - un script par étape mécanique : tout ce qui attend, boucle, convertit, déplace,
     renomme ou télécharge.

3. Règles à respecter en écrivant tout ça :
   - Ce qui attend, boucle ou déplace des fichiers = un script, jamais un agent.
   - Ce qui juge, décrit, contrôle ou tranche = un agent.
   - Chaque agent commence par lire JOURNAL.md EN ENTIER et finit par y écrire son
     entrée en cinq rubriques : VU / DÉCIDÉ / PRODUIT / POUR LE SUIVANT / DOUTE.
     « Fait » n'est pas une entrée.
   - Un seul changement par essai, et la sortie précédente sert toujours de référence.
   - Place explicitement les points d'arrêt où je dois regarder avant que ça continue.
   - Les chiffres, formulations et contraintes que je t'ai donnés aujourd'hui se
     recopient exactement. Tu ne les reformules pas, tu ne les arrondis pas.

4. Tu n'inventes rien. Partout où tu ne sais pas parce que je ne te l'ai pas dit, tu
   écris la question dans QUESTIONS.md au lieu de combler le trou.

À la fin : liste-moi les fichiers créés, et lis-moi tes questions.
```

### Prompt B — lancer un chantier

Dans une **nouvelle** conversation, ouverte dans le même dossier.

```
Lis CLAUDE.md, puis SOP.md, puis JOURNAL.md. En entier, tous les trois, avant d'agir.

Chantier du jour : [ce que je veux obtenir, en une phrase]
Entrées : [les fichiers ou le lien de départ]

Déroule la SOP dans l'ordre, un maillon à la fois. Après chaque maillon tu écris ton
entrée dans JOURNAL.md et tu me montres ce que tu as produit. Aux points d'arrêt de la
SOP tu t'arrêtes vraiment et tu attends ma réponse — tu n'enchaînes pas.

Ce qui attend ou boucle, tu le lances en script. Ce qui juge, tu le fais toi-même, ou tu
le confies à un sous-agent en lui passant le journal complet — jamais un résumé.

Si une information te manque, tu me la demandes. Tu ne la devines pas. Et si tu bloques
sur quelque chose que moi seul peux débloquer, tu me le dis tout de suite, puis tu
continues sur ce qui n'en dépend pas au lieu d'attendre.
```

### Prompt C — quand ça rate

C'est ce prompt qui fait progresser le système. Sans lui, la même erreur revient toutes les semaines.

```
Ce qui vient de rater : [en une phrase, avec le fichier concerné]

Ne corrige pas seulement ce cas précis. Trouve d'abord pourquoi la SOP a laissé passer
ça, et dis-le-moi avant de modifier quoi que ce soit. Ensuite :

- corrige l'étape concernée dans SOP.md, et ajoute l'erreur sous « ce qui a raté et
  pourquoi », avec de quoi la reconnaître la prochaine fois ;
- si c'est un agent qui s'est trompé, la correction va dans son fichier
  .claude/agents/ — pas dans notre conversation, sinon elle meurt avec la session ;
- si la règle peut être contournée par oubli, mets-la dans un script plutôt que dans
  un texte à relire ;
- écris l'entrée correspondante dans JOURNAL.md.

Puis dis-moi en une phrase ce qui a changé dans le système.
```

## 14 · Le résultat : à quoi ressemble le dossier après le prompt A

```
mon-chantier/
├── CLAUDE.md          ← lu en premier, à chaque session
├── SOP.md             ← la procédure + les erreurs déjà commises
├── JOURNAL.md         ← le cerveau du chantier
├── QUESTIONS.md       ← ce qu'il n'a pas deviné : à répondre
├── .claude/agents/    ← un fichier par rôle
│   ├── analyste.md
│   └── controleur.md
├── scripts/           ← tout ce qui attend, boucle ou déplace
└── sorties/           ← le travail produit
```

Ce dossier **est** la base de connaissances. Elle n'a pas été préparée : elle est tombée du premier chantier. Au deuxième, elle est meilleure. Au dixième, elle contient des choses qu'on avait oubliées avoir apprises.

> **Le seul entretien nécessaire :** répondre aux questions de `QUESTIONS.md`, et coller le prompt C à chaque ratage. Deux minutes qui remplacent une réunion de process.

## 15 · Le cas créa

Si le travail à automatiser, c'est de la création publicitaire, le protocole ne change pas d'un mot. Ce qui change, c'est où tombent les points d'arrêt — et sur une chaîne créative, ils tombent toujours au même endroit.

| Maillon | Qui | Porte |
|---|---|---|
| Récupérer et découper les références | Script | — |
| Analyser ce qui fait marcher chaque référence | Agent | — |
| Décider l'angle, le ton, la promesse | **Humain** | **Arrêt** |
| Écrire les consignes de production | Agent | **Arrêt** |
| Produire les déclinaisons | Script | — |
| Contrôler chaque sortie contre sa référence | Agent | **Arrêt** |
| Nommer, ranger, exporter | Script | — |

> **Ce qui ne s'automatise pas, et qu'il ne faut pas essayer : le goût.** Le choix d'un angle, d'une référence, d'un ton — c'est ce qui différencie le travail, donc c'est précisément ce qu'on garde à la main. Automatiser le goût produit du volume moyen très vite : le pire résultat possible.
>
> Et un contrôle de sortie créative se fait par un modèle qui **voit réellement** le livrable. Un contrôle sur une description textuelle laisse tout passer.

*Même découpe pour n'importe quel autre travail répétable : mécanique aux scripts, jugement aux agents, goût à l'humain. Ce sont les mêmes trois colonnes.*

---

Le tester coûte un fichier texte ; ne pas le tester coûte un relais sur deux.
