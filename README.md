# OpenStack et ses services — notes de cours (INSA CVL)

Structure LaTeX d'un cours sur OpenStack, basée sur le template INSA CVL
(`info_quantiq_tex`) : charte rouge/gris, en-tête avec logo, page de titre, sommaire,
glossaire, bibliographie biblatex/APA. **Un chapitre = un fichier `chap_<nom>.tex`.**
**Avancement :** introduction/présentation (squelette), **Keystone** (complet, ~39 pages),
**Nova** (8 pages) et **Placement** (7 pages) rédigés. Les autres chapitres de service et la
conclusion sont encore vides (titre + `\label`) ; ils seront rédigés selon les mêmes règles
(voir « Conventions des chapitres rédigés »). Keystone est le chapitre « exhaustif » ; les
suivants sont plus synthétiques (10 pages au plus, schémas conservés).

## Compilation

```bash
pdflatex main.tex
biber main            # bibliographie (biblatex + biber, style APA)
pdflatex main.tex
pdflatex main.tex     # 2 passes : sommaire, renvois, glossaire, figures TikZ
```

Il faut une distribution TeX Live/MiKTeX complète et `biber`.
**À ajouter :** `images/logo_insa.png` (le logo n'est pas dans le dépôt du template ;
la compilation échoue sans lui).
**À renseigner :** `\docprof` dans le bloc « Informations du document » de `main.tex`.

## Arborescence

```
main.tex              Préambule, informations du document, ordre des chapitres
pagetitre.tex         Page de titre (motif : services / API REST / infrastructure)
introduction.tex      Introduction (non numérotée) : présentation du cours et plan
chap_presentation.tex 1.  Présentation générale d'OpenStack (sous-sections seulement)
chap_keystone.tex     2.  Keystone : identité  (RÉDIGÉ, ~39 pages)
chap_nova.tex         3.  Nova : calcul  (RÉDIGÉ, 8 pages)
chap_placement.tex    4.  Placement : suivi des ressources  (RÉDIGÉ, 7 pages)
chap_glance.tex       5.  Glance : images
chap_neutron.tex      6.  Neutron : réseau
chap_cinder.tex       7.  Cinder : stockage bloc
chap_horizon.tex      8.  Horizon : tableau de bord
chap_octavia.tex      9.  Octavia : répartition de charge
chap_swift.tex        10. Swift : stockage objet
chap_heat.tex         11. Heat : orchestration
chap_ironic.tex       12. Ironic : bare metal
chap_magnum.tex       13. Magnum : Kubernetes managé
chap_trove.tex        14. Trove : bases de données (DBaaS)
chap_deploiement.tex  15. Méthodes de déploiement (hors service)
conclusion.tex        Conclusion (non numérotée, vide)
glossaire.tex         Entrées du glossaire (~54 entrées : Keystone, Nova, Placement)
sources.bib           Bibliographie (42 entrées @online : documentation OpenStack, Keystone, Nova, Placement, Trove)
images/               logo_insa.png à y déposer
```

L'ordre (et donc le numéro) des chapitres est celui des `\include` de `main.tex` :
pour réordonner ou retirer un chapitre, déplacer ou supprimer une ligne.
`\includeonly{chap_keystone}` (ligne commentée) permet de ne compiler qu'un chapitre.

## Correspondance avec les supports de cours

| Chapitre | À reprendre depuis |
|---|---|
| Présentation générale | Introduction_OpenStack : cloud, IaaS, architecture, multi-nœuds, scénario « création d'une VM », comparaisons |
| Keystone | Introduction (identité, authentification/autorisation) ; Chapitre 5 partie 1 (domaines, fédération SAML2/OIDC) |
| Nova, Placement (rédigés) ; Glance, Horizon, Octavia | Introduction_OpenStack (une partie chacun) |
| Neutron | Introduction (concepts, ports, security groups) ; Chapitre 2 (provider/self-service, OVS, OVN, ML2) |
| Cinder | Introduction ; Chapitre 3 partie 1 (multi-backend, volume types, QoS, snapshots, backups) |
| Swift | Chapitre 3 partie 2 |
| Heat | Chapitre 3 partie 3 |
| Ironic, Magnum | Chapitre 5 parties 2 et 3 |
| Trove | Chapitre 5 partie 4 ; présentation Trove (groupe 5) |
| Déploiement | Chapitre 4 (DevStack, Kolla-Ansible, manuel/bare-metal) |

## Outils du template

- Encadrés : `definition`, `resultat`, `remarque`, `aretenir`, `savoirfaire`, `exercice`,
  `correction` (voir `main.tex`).
- Styles TikZ du template pour les schémas d'architecture : `svc` (service), `svcgris`
  (composant secondaire), `bus` (barre API/bus), `flux` (flèche).
- Code : styles `listings` `terminal`, `darkterminal`, `verbatimlike`, macro `\prompt`.
- Renvois : `\label`/`\ref` préfixés (`chap:`, `sec:`, `fig:`, `ex:`). Un chapitre a pour
  label `chap:<nom>` (ex. `\ref{chap:keystone}`).
- Glossaire : `\gls{iaas}` ; bibliographie : `\parencite{openstack-docs}`.

## Schémas TikZ (style de la page 5 de la présentation Trove)

Les styles sont définis dans `main.tex` (second `\tikzset`) et réutilisables dans tous les
chapitres. Chaque schéma est un `figure[!ht]` avec `\adjustbox{max width=\linewidth}` autour du
`tikzpicture` (il s'adapte donc à la largeur du texte), une légende qui explique les
numéros, et un `\label{fig:<service>-<sujet>}`.

| Style | Usage |
|---|---|
| `pcli` | client / acteur (pastille blanche, bord rouge) |
| `pfoc` | service au centre de la figure (rouge plein) |
| `psvc` | autres services OpenStack (pêche) |
| `pdat` | données, objets stockés (rose) |
| `pgris` | infrastructure, composant externe (gris) |
| `etiqf` | étiquette de flèche (petite, grise) |
| `fluxo`, `fluxr`, `fluxd` | flèches orange (appel de service), rouge (requête client), pointillée (asynchrone ou externe) |
| `num` | pastille rouge numérotée (étapes 1, 2, 3 de la légende) |
| `callout` | bandeau pêche sous la figure : le message à retenir |
| `acteur`, `vie`, `msg`, `rep` | diagrammes de séquence |

Pour un diagramme de séquence : `acteur` (boîte en tête), `vie` (ligne de vie pointillée) et les
macros `\seqmsg{x1}{x2}{y}{num}{texte}` (requête, flèche pleine) et `\seqrep{x1}{x2}{y}{num}{texte}`
(réponse, flèche pointillée). Exemple complet : figure « Connexion fédérée par navigateur » dans
`chap_keystone.tex`.

Règles TikZ à connaître : ne pas mettre `\\` dans un groupe `{}` imbriqué d'un nœud
`align=center` ; préférer un nœud explicite (`\node[etiqf] at (x,y) {…}`) à `node` placé après la
destination d'un `to[...]` (le nœud se retrouve alors sur la destination, pas sur la courbe).

## Conventions des chapitres rédigés

- Un chapitre = une `\section{…}\label{chap:<nom>}` ; sous-sections `sec:<nom>-<sujet>`,
  figures `fig:<nom>-…`, tableaux `tab:<nom>-…`, exercices `ex:<nom>-…`.
- Chapitres synthétiques (Nova, Placement et suivants) : 10 pages au plus ; mêmes sections, mais
  une figure par idée clé (environnement, architecture, séquence, ordonnancement/états), 3 exercices
  courts et un récapitulatif sans schéma de synthèse.
- Contenu type : rôle du service et place dans OpenStack (schéma structure + interactions) ;
  concepts ; architecture interne ; configuration et commandes ; exploitation et diagnostic ;
  sécurité et limites ; exercices corrigés ; récapitulatif (schéma de synthèse, `aretenir`,
  tableau de commandes, `savoirfaire`).
- Exercices : environnement `exercice` suivi d'un environnement `correction` ; ils se numérotent
  `section.n` automatiquement.
- Code : `\begin{lstlisting}[style=terminal]` pour les sessions shell (lignes précédées de `$ `),
  `\begin{lstlisting}` seul pour les fichiers de configuration ou le JSON. Les blocs courts
  sont placés dans un `minipage` (ils ne se coupent pas entre deux pages).
- Sources : `\parencite{clé}` ; chaque affirmation technique renvoie à la documentation officielle
  (fichier `sources.bib`). Les entrées `@online` portent `urldate` ; faute de date de
  publication, biblatex-apa affiche « s. d.-a », « s. d.-b »… (comportement normal d'APA pour
  plusieurs documents du même auteur sans date).
- Glossaire : un terme est défini dans `glossaire.tex` puis cité avec `\gls{…}`.

## Réglages ajoutés au préambule

- `microtype` sans protrusion/expansion/kerning (il casse les ligatures dans `\texttt`) et
  `\DisableLigatures` pour la police à chasse fixe : `--option` s'affiche correctement.
- `upquote=true` dans `\lstset` : apostrophes droites dans les listings (commandes shell, SQL).
- `\emergencystretch=2em` : évite la plupart des débordements de ligne.
- Paquets ajoutés : `adjustbox`, `tikz` (bibliothèques `positioning`, `arrows.meta`,
  `fit`, `backgrounds`, `calc`, `shapes.geometric`, etc. : voir `main.tex`).
