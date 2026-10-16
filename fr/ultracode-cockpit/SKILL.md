---
name: ultracode-cockpit
description: À utiliser dès que l'utilisateur évoque ultracode, les Dynamic Workflows, les agents parallèles ou sa consommation de tokens, ou qu'il lance une tâche lourde (recherche profonde, refonte, génération ou exploration massive). Impose un triage en paliers, un devis avant toute dépense importante, et la redescente après usage.
allowed-tools: Read, Grep, Glob, Bash, WebSearch, WebFetch
---

# UltraCode Cockpit — toute la puissance, la main sur la manette

*Full power. Your hands on the throttle.*

## Principe — le poste de pilotage

Ultracode est un réacteur : jusqu'à 1 000 agents par run qui codent, cherchent et se vérifient. Le problème n'est pas la puissance — c'est que moi, l'IA, **je ne sens pas la dépense**. Lancer un agent ou seize, c'est le même geste, la même absence de douleur. Et détail décisif : quand ultracode est activé, Claude Code **coupe ses propres garde-fous natifs** (l'avertissement « Large workflow » et l'approbation au premier lancement en mode auto) — le mode le plus dépensier est celui où plus rien n'alerte. Ce skill est la prothèse du sens qui me manque : il installe l'humain dans le cockpit.

Doctrine : **une dépense n'est bonne que si elle change le résultat.** Le même résultat avec un agent qu'avec dix ? On déploie un. L'effort maximal sur une tâche simple ne produit pas une meilleure réponse — juste une réponse plus chère.

## §0 — RÉFÉRENTIEL (vérifié sur la doc officielle le 16/10/2026) ⚠️ seule section périssable

Tout ce qui peut changer chez Anthropic vit ICI et nulle part ailleurs. Source : code.claude.com/docs/en/workflows.
- **Déclenchement ponctuel** (une tâche) : le mot-clé `ultracode` dans le prompt, ou une demande en clair (« use a workflow »).
- **Mode session** : `/effort ultracode` → Claude orchestre un workflow pour chaque tâche substantielle, au niveau d'effort courant. Au lancement : `claude --effort ultracode` (règle aussi l'effort sur xhigh ; v2.1.203+). **Arrêt : `/effort ultracode off`.** Portée : la session en cours.
- **Limites** : 1 000 agents max par run ; **16 simultanés par défaut** (modulable 1-256 via `CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS`).
- **Taille des workflows** : `/config` → « Dynamic workflow size » : `small` (<5 agents) · `medium` (<10, défaut) · `large` (<50) · `unrestricted`. C'est un objectif donné à Claude, pas un plafond dur.
- **Garde-fous natifs** : avertissement « Large workflow » à >25 agents ou >1,5 M tokens projetés — **désactivé quand ultracode est ON**, tout comme l'approbation au premier lancement en mode auto.
- **Pilotage en vol** : `/workflows` → phases, nombre d'agents, tokens par phase, temps ; `p` pause · `x` stop · `s` sauvegarder le script en commande réutilisable.
- **Workflow fourni** : `/deep-research` (recherche croisée multi-sources ; requiert WebSearch).
- **Niveaux d'effort** : `low · medium · high · xhigh · max` (commande `/effort`).
- **Désactivation complète** : toggle « Dynamic workflows » dans `/config` · `"disableWorkflows": true` (settings.json) · `CLAUDE_CODE_DISABLE_WORKFLOWS=1`.
- **Accès** : tous les plans payants (sur Pro : activer la ligne « Dynamic workflows » dans `/config`).

## Étape 1 — Pre-flight (le scan d'adaptation)

- **Léger, toujours** : où suis-je ? Claude Code (commandes disponibles) ou claude.ai (autodiscipline de délégation). Le reste du skill s'exécute selon ce profil.
- **Complet, au palier 3 seulement** : avant de prescrire, je vérifie que les éléments du §0 existent encore — commande testée, ou vérification sur la doc officielle en ligne. Le garde-fou ne doit pas coûter plus cher que ce qu'il garde.
- **MODE PATCH** — si la réalité ne correspond plus au §0 : je le dis clairement, je rédige le **bloc Référentiel de remplacement complet, daté du jour**, et j'indique à l'humain où le coller : dans son skill (compte Claude) ET dans le dépôt GitHub — les deux copies restent jumelles. S'il utilise le skill public : proposer le patch en issue ou pull request. Je n'applique jamais le patch moi-même : j'écris le correctif, l'humain l'applique.

## Étape 2 — Triage en 3 paliers (30 secondes)

- **P1 · Main nue** — tâche courante (correction, question, petit script) : solo, zéro sous-agent, effort standard. **Totalement silencieux : pas de devis, pas de préambule, le travail commence.** La discipline doit être invisible sur le chemin courant.
- **P2 · Équipe réduite** — tâche composite : **5 sous-agents maximum** (l'équivalent natif : taille de workflow `small`), mini-devis.
- **P3 · Pleine puissance (ultracode)** — recherche profonde, refonte, exploration massive : devis complet, **jamais lancé sans GO**.

Dans le doute entre deux paliers : je propose le plus bas et je dis en une ligne ce que le palier supérieur apporterait.

## Étape 3 — Devis avant dépense (P2/P3 — 5 lignes maximum)

| Palier | Agents (plafond) | Effort | Classe de coût | Pourquoi |
|---|---|---|---|---|
| P2 ou P3 | n | niveau | S / M / L / XL | une ligne |

Réponses à un mot : **GO · P1 · P2 · P3 · STOP**. L'humain arbitre, j'exécute le palier choisi — jamais plus.
Honnêteté : la classe de coût est un **ordre de grandeur**, pas un compteur. Je ne promets jamais un plafond de tokens que je ne peux pas garantir. **Le silence ne vaut pas accord** : pas de GO, pas de P3. Et je rappelle au besoin : ultracode ON = les alertes natives de Claude Code se taisent — ce devis les remplace.
Conseil de pilote avant un gros run : **tester sur une tranche d'abord** (un dossier, pas tout le repo ; une question étroite, pas un rapport-monde).

## Étape 4 — La règle du brief

Le vrai gaspillage n'est pas le nombre d'agents, c'est l'agent mal briefé. Donc : **aucun sous-agent lancé sans une mission d'une ligne et un livrable attendu** — une conclusion, pas un déballage. Et je ne refais jamais moi-même ce que j'ai délégué : on paie une fois.

## Étape 5 — Tuto d'exécution (une commande à la fois)

- **Claude Code** — l'enchaînement exact, une commande, ce qu'elle doit afficher, vérification, la suivante. Exemple P3 :
  1. `/config` → vérifier « Dynamic workflows » activé, et régler « Dynamic workflow size » au palier du devis (`small` pour ≤5 agents).
  2. `/effort ultracode` → confirmer l'activation.
  3. La tâche, dans le plafond du devis.
  4. `/workflows` → surveiller : agents, tokens par phase ; `p` pour pauser, `x` pour stopper.
- **claude.ai** — pas de commandes : j'applique moi-même la discipline (nombre d'agents plafonné au devis, effort réglé délégation par délégation).

## Étape 6 — Conduite & checkpoint mi-devis

- Le plafond d'agents du devis n'est **jamais** dépassé sans nouveau devis. Une vague à la fois.
- **Checkpoint mi-devis** (anti-coût irrécupérable) : à la moitié de la dépense annoncée — vérifiable dans `/workflows` —, la moitié du résultat est-elle là ? Sinon : STOP, bilan, re-devis. Jamais de fuite en avant pour « sauver » un workflow qui patine.
- **Kill-switch** : « STOP » → arrêt propre (`/workflows` puis `x` si un run tourne), rapport partiel de ce qui a été fait et dépensé.

## Étape 7 — Redescente & rapport de vol

Je ne clos jamais une session ultracode sans :
1. **La redescente** — `/effort ultracode off` (et réajuster `/effort` si le niveau a été monté). Le mode est lié à la session, mais on ne laisse pas un réacteur allumé.
2. **Le rapport de vol** — palier joué, agents réellement lancés vs devis (chiffres de `/workflows`), classe de coût constatée, et la leçon : le triage était-il le bon ?

## Garde-fous permanents

- Je ne désactive jamais une sécurité existante et ne contourne jamais une limite d'Anthropic — je dose à l'intérieur des limites.
- J'annonce mes propres limites quand c'est utile : pas de compteur de tokens en temps réel côté skill, pas de plafond dur — une discipline, pas une promesse. Les jauges réelles sont dans `/workflows`.
- P3 jamais lancé en l'absence de l'humain. Pas de GO implicite.
- Ce skill vieillit proprement : si le §0 est périmé, je le signale et je produis le patch — je ne prescris jamais une commande non vérifiée.
