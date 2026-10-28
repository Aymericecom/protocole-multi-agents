# Journal des actualités Anthropic

Veille sur ce qu'Anthropic publie et qui pourrait un jour toucher le protocole : modèles, outils d'agents, règles d'usage. On ne s'en sert pas aujourd'hui ; ce fichier existe pour le jour où on en aura besoin.

**Règles du fichier** (les mêmes que pour `JOURNAL.md`) :

- Une entrée par actualité, datée, **la plus récente en haut**. Jamais réécrite après coup : si une info se révèle fausse, on ajoute une entrée qui la corrige.
- Chaque entrée porte sa **source**. Pas de source officielle → on l'écrit dans DOUTE.
- Chaque entrée dit **ce que ça change pour le protocole**, ou « rien » avec la raison. Une actualité sans conséquence n'a rien à faire ici.
- On ne garde pas tout : levées de fonds, partenariats commerciaux et annonces sans effet sur la façon de travailler restent dehors (voir la section 10 du README : une base qui grossit sans filtre devient illisible).

### Le gabarit, à recopier tel quel

```
## [aaaa-mm-jj] <titre court>
QUOI : ce qui a été annoncé, en faits.
SOURCE : <lien>
POUR LE PROTOCOLE : quelle section du README ça touche, et ce qu'il faudrait changer — ou « rien », et pourquoi.
DOUTE : ce qui n'est pas vérifié, ou reste à tester sur un vrai chantier.
```

---

## [2026-10-28] Mise en place du journal

QUOI : ouverture du fichier. Relevé des annonces de septembre et octobre 2026 sur anthropic.com/news, filtré sur ce qui touche au travail multi-agents.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : rien pour l'instant. Aucune annonce relevée ne contredit une règle du README.
DOUTE : les fonctionnalités de Claude Code (sous-agents, worktrees, agents en arrière-plan) ne sont relevées ici que d'après des sources secondaires. À vérifier dans les notes de version officielles avant de s'en servir.

## [2026-10-22] Les Agent Skills confiées à l'Agentic AI Foundation

QUOI : Anthropic a donné le format Agent Skills à l'Agentic AI Foundation. Plus de 70 plateformes d'agents l'auraient adopté.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : touche la section 10 (la boucle) et le prompt A. Une procédure réutilisable (`SOP.md`) ou un fichier de rôle pourrait s'écrire au format Skill, et rester lisible par d'autres outils que Claude Code. Ça va dans le sens de « ne dépend d'aucun outil ».
DOUTE : pas encore testé. À essayer sur un chantier avant de changer le prompt A.

## [2026-10-08] Nouvelle version de la politique d'usage

QUOI : Anthropic a publié une nouvelle version de sa Usage Policy, avec un résumé des changements.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : rien directement. À relire si un chantier touche un domaine sensible (cybersécurité, biologie, élections).
DOUTE : le détail des changements n'a pas été lu. À faire avant le premier chantier concerné.

## [2026-10-07] Claude Haiku 5.5

QUOI : sortie de Claude Haiku 5.5, présenté comme un petit modèle rapide et capable pour les gros volumes.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : touche la section 05, ligne « Mécanique pure : indexer, formater, déplacer → le plus léger ». C'est le candidat pour ce rôle. Ne change pas l'exception : le contrôle qualité reste au modèle le plus capable.
DOUTE : pas comparé à Haiku précédent sur une vraie tâche mécanique.

## [2026-09-28] Claude Sonnet 5.5

QUOI : sortie de Claude Sonnet 5.5, plus rapide et moins cher que Sonnet 5.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : section 05, ligne « Rédiger une consigne à partir d'une référence → intermédiaire ». Candidat pour ce rôle, avec le corollaire inchangé : celui qui rédige la consigne doit être celui qui vient de regarder la référence.
DOUTE : rien de testé.

## [2026-09-22] Claude Opus 5.5

QUOI : sortie de Claude Opus 5.5, présenté comme le meilleur en code agentique et en travail intellectuel.
SOURCE : https://www.anthropic.com/news
POUR LE PROTOCOLE : section 05, lignes « Orchestrer, décider, valider » et « Regarder un livrable et rendre un verdict → le plus capable ». C'est le modèle à mettre sur l'orchestrateur et sur le contrôle qualité.
DOUTE : la règle du README est volontairement écrite sans nom de modèle (« le plus capable »). Il ne faut pas l'y ajouter : le nom change tous les six mois, la règle non.

## [2026, avril → juin] Sous-agents de Claude Code : isolation, arrière-plan, imbrication

QUOI : d'après des récapitulatifs de notes de version, Claude Code permettrait de lancer un sous-agent dans un worktree git isolé, de faire tourner des sous-agents en arrière-plan, et de laisser un sous-agent lancer ses propres sous-agents (jusqu'à 5 niveaux).
SOURCE : sources secondaires uniquement, par ex. https://www.havoptic.com/r/claude-code-2.1.172 et https://pricci1.github.io/cc-docs-changelog/posts/2026-05-12-002215.html
POUR LE PROTOCOLE :
- Worktree isolé → section 09, « ne pas corriger un agent dans son dos ». L'isolation empêche deux agents d'écrire sur les mêmes fichiers, ce qui renforce la section 04.
- Sous-agents imbriqués → **contredit la section 04** si on s'en sert sans règle : un sous-agent qui en lance un autre devient un second orchestrateur. Position à garder : l'orchestrateur reste le seul point de contact. Si on autorise l'imbrication un jour, le sous-agent parent devra passer le journal complet à son propre sous-agent, comme l'orchestrateur le fait.
- Arrière-plan → section 05. Ne change pas la règle : ce qui attend reste un script.
DOUTE : rien de vérifié dans la documentation officielle. À confirmer avant de modifier le README.
