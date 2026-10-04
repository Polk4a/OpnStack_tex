# OpenStack et ses services — notes de cours (INSA CVL)

Structure LaTeX d'un cours sur OpenStack, basée sur le template INSA CVL
(`info_quantiq_tex`) : charte rouge/gris, en-tête avec logo, page de titre, sommaire,
glossaire, bibliographie biblatex/APA. **Un chapitre = un fichier `chap_<nom>.tex`.**
**Avancement :** le cours est complet : introduction, 15 chapitres et conclusion, regroupés en **cinq parties**,
suivis d'une **sixième partie d'auto-évaluation** : 120 QCM avec corrigés (`QCM.tex`, voir « Partie VI : QCM »).
Pages dans la version actuelle (209 pages avec le sommaire, la bibliographie et le glossaire) :
introduction (2), présentation générale (10), **Keystone** (15, synthétisé), Nova (8), Placement (7), Glance (7),
Neutron (8), Cinder (9), Swift (8), Horizon (9), Octavia (9), Heat (9), Ironic (10), Magnum (9), Trove (12),
méthodes de déploiement (14), conclusion (6), **partie VI QCM (35 : 24 de questionnaires, 11 de corrigés)**. Les chapitres de service sont synthétiques (10 pages au plus, schémas
conservés) ; Keystone, Trove et le déploiement, plus détaillés, peuvent aller jusqu'à 15 pages ; la conclusion,
jusqu'à 10.

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

## En cas d'erreur de compilation

- `I do not know the key '/tikz/pcli'` : `main.tex` est une ancienne copie, sans le second
  `\tikzset{…}` (styles de schémas). Utiliser le `main.tex` du projet.
- `Undefined control sequence \seqmsg` (ou `\seqrep`) : les chapitres Nova, Placement, Neutron,
  Cinder, Horizon, Octavia, Swift, Heat, Ironic, Magnum, Trove et la conclusion tracent leurs diagrammes de séquence avec ces deux macros, définies dans `main.tex` juste après
  le second `\tikzset`. Si elles manquent, les ajouter :

```latex
\newcommand{\seqmsg}[5]{%
  \draw[msg] (#1,#3) -- (#2,#3) node[midway, above=1pt, font=\footnotesize, align=center, text=black, fill=white, inner sep=1pt] {#5};%
  \ifx\relax#4\relax\else\node[num] at ({#1+(#2>#1?-0.4:0.4)},#3) {#4};\fi}
\newcommand{\seqrep}[5]{%
  \draw[rep] (#1,#3) -- (#2,#3) node[midway, above=1pt, font=\footnotesize, align=center, text=black, fill=white, inner sep=1pt] {#5};%
  \ifx\relax#4\relax\else\node[num] at ({#1+(#2>#1?-0.4:0.4)},#3) {#4};\fi}
```

- Tirets doubles (`--option`) rendus comme un tiret long dans `\texttt`, apostrophes courbes dans
  les listings : voir « Réglages ajoutés au préambule » en fin de fichier.

## Arborescence

```
main.tex              Préambule, informations du document, ordre des chapitres et des parties
pagetitre.tex         Page de titre (motif : services / API REST / infrastructure)
introduction.tex      Introduction (non numérotée, 2 pages) : objectifs, public, plan en 5 parties + conclusion + QCM, parcours de lecture

  Partie I — Fondations
chap_presentation.tex 1.  Présentation générale d'OpenStack  (RÉDIGÉ, 10 pages, 7 schémas)
chap_keystone.tex     2.  Keystone : identité  (RÉDIGÉ, 15 pages)
  Partie II — Le socle IaaS
chap_nova.tex         3.  Nova : calcul  (RÉDIGÉ, 8 pages)
chap_placement.tex    4.  Placement : suivi des ressources  (RÉDIGÉ, 7 pages)
chap_glance.tex       5.  Glance : images  (RÉDIGÉ, 7 pages)
chap_neutron.tex      6.  Neutron : réseau  (RÉDIGÉ, 8 pages)
chap_cinder.tex       7.  Cinder : stockage bloc  (RÉDIGÉ, 9 pages)
chap_swift.tex        8.  Swift : stockage objet  (RÉDIGÉ, 8 pages)
  Partie III — Exploiter le cloud
chap_horizon.tex      9.  Horizon : tableau de bord  (RÉDIGÉ, 9 pages)
chap_octavia.tex      10. Octavia : répartition de charge  (RÉDIGÉ, 9 pages)
chap_heat.tex         11. Heat : orchestration  (RÉDIGÉ, 9 pages)
  Partie IV — Services de plateforme
chap_ironic.tex       12. Ironic : bare metal  (RÉDIGÉ, 10 pages)
chap_magnum.tex       13. Magnum : Kubernetes managé  (RÉDIGÉ, 9 pages)
chap_trove.tex        14. Trove : bases de données (DBaaS)  (RÉDIGÉ, 12 pages)
  Partie V — Mettre en œuvre
chap_deploiement.tex  15. Méthodes de déploiement  (RÉDIGÉ, 14 pages)

conclusion.tex        Conclusion (non numérotée)  (RÉDIGÉE, 6 pages)

  Partie VI — Auto-évaluation
QCM.tex               120 QCM (8 par chapitre, chapitres 1 à 15) imprimables + corrigés  (RÉDIGÉ, 35 pages)

glossaire.tex         Entrées du glossaire (173 entrées : présentation, Keystone, Nova, Placement, Glance, Neutron, Cinder, Horizon, Octavia, Swift, Heat, Ironic, Magnum, Trove, déploiement et conclusion)
sources.bib           Bibliographie (254 entrées @online : documentation OpenStack, NIST, KVM, Proxmox, Kubernetes, puis chaque service, le déploiement et la conclusion)
images/               logo_insa.png à y déposer
```

**Fil conducteur.** Le plan suit l'ordre où une demande de machine virtuelle sollicite les services : la carte
d'ensemble et l'identité (I), puis le socle IaaS dans l'ordre calcul, choix de l'hôte, image, réseau, volume,
objet (II), puis ce qui exploite le cloud (III) et les services bâtis sur le socle (IV), enfin l'installation et
l'exploitation (V). Chaque chapitre de service s'ouvre par une phrase qui le relie au précédent ; la conclusion
reprend la carte du chapitre 1 avec les numéros de chapitre.

**Parties.** La partie VI (`\part{Auto-évaluation}\label{part:qcm}`) est écrite en tête de `QCM.tex`, qui est incluse
par `main.tex` juste après la conclusion. Pour les parties I à V, chaque `\part{…}\label{part:…}` et son `\partdesc{…}` sont écrits en tête du premier chapitre de la
partie, juste avant le `\section` : `chap_presentation` (`part:fondations`), `chap_nova` (`part:socle`),
`chap_horizon` (`part:exploiter`), `chap_ironic` (`part:plateformes`) et `chap_deploiement`
(`part:miseenoeuvre`). Si l'on déplace un chapitre qui porte une partie, déplacer aussi la partie. La classe
`article` ne saute pas de page avant `\part` ; comme chaque chapitre commence en haut de page (`\include`), le
bandeau y est placé. Le format du bandeau (`\titleformat{\part}`), de l'entrée de sommaire (`\l@part`) et de la
macro `\partdesc` est défini dans `main.tex`.

L'ordre (et donc le numéro) des chapitres est celui des `\include` de `main.tex` :
pour réordonner ou retirer un chapitre, déplacer ou supprimer une ligne.
`\includeonly{chap_keystone}` (ligne commentée) permet de ne compiler qu'un chapitre ; les renvois vers les autres
chapitres restent alors indéfinis (`??`) jusqu'à la compilation complète.

## Partie VI : QCM (`QCM.tex`)

`QCM.tex` est un fichier importé par `\include{QCM}` (ligne placée après `\include{conclusion}` dans `main.tex` ; la
supprimer retire toute la partie). Il ne contient ni préambule ni `\begin{document}`. Le fichier se compose, dans l'ordre :
les macros (en tête, sous `\makeatletter`), le bandeau de la partie VI et son mode d'emploi, les 15 blocs de
questions (un par chapitre, dans l'ordre du cours), la page de bilan, puis la section « QCM : corrigés ».

**Contenu.** Chaque chapitre a 8 questions à 4 propositions (A à D) : 3 faciles (1 point, une seule bonne réponse),
2 intermédiaires (2 points), 2 avancées (3 points) et 1 de maîtrise (4 points, 2 ou 3 bonnes réponses : il faut cocher
exactement les bonnes). Un chapitre vaut 17 points, le cours 255. Les 105 questions à réponse unique ont des bonnes
réponses réparties à peu près également (A 27, B 27, C 25, D 26) ; aucun chapitre n'a la même suite de lettres qu'un
autre, et la question 8 a 2 bonnes réponses dans 11 chapitres, 3 dans 4.

**Sur papier.** Chaque proposition est précédée d'une case `\square` à cocher au crayon ; chaque question est dans un
`minipage` (elle ne se coupe pas entre deux pages) ; une ligne « Score : …/17 » figure en tête de chaque chapitre ;
la page de bilan (dernière page du questionnaire) reprend les 15 scores, le total sur 255 et trois cases
« à revoir / acquis / maîtrisé » (0–8, 9–13, 14–17 points). Pour imprimer : pages 157 à 180 pour le questionnaire,
pages 181 à 191 pour les corrigés, à garder à part (les numéros de page sont donnés par le mode d'emploi, qui les
calcule avec `\pageref`). Les chapitres s'enchaînent sans saut de page, un titre de chapitre n'étant jamais
isolé en bas de page (macro `\qcm@need`) ; le premier chapitre commence sur une page neuve.

**Corrigés.** La section « QCM : corrigés » commence par un tableau des bonnes réponses (chapitre en ligne,
questions 1 à 8 en colonnes, niveaux en en-tête), puis donne pour chaque question, dans l'ordre : le numéro (`Q 3.4` =
chapitre 3, question 4, pour ne pas confondre avec les numéros de section), « Réponse : B. » ou « Réponses : A, C. »,
l'explication, et « *Voir* section 3.4 (p. 57) » vers le passage du cours (les renvois sont des `\ref` et `\pageref` :
ils suivent la numérotation du document).

**Écrire ou modifier une question.** Une question s'écrit une seule fois ; la macro l'imprime dans le questionnaire
et range son corrigé en mémoire pour la section des corrigés :

```latex
\qcmchap{chap:nova}{Nova : le service de calcul}      % une fois par chapitre, titre = titre du \section
\qcmq{f}{Énoncé ?}                                     % niveau : f, i, a ou m
  {Proposition A}{Proposition B}{Proposition C}{Proposition D}{B}   % réponse(s) : « B » ou « A, C »
  {Explication de la bonne réponse (et du distracteur le plus tentant).}
  {\qr{sec:nova-role} et \qrf{fig:nova-env}}          % renvois au cours
```

- Un chapitre = `\qcmchap` puis exactement 8 `\qcmq` dans l'ordre f, f, f, i, i, a, a, m (les barèmes et le tableau
  en dépendent).
- Renvois : `\qr{sec:…}` (section), `\qrc{chap:…}` (chapitre), `\qrf{fig:…}` (figure), `\qrt{tab:…}` (tableau) ; 1 à 3
  par question, sans « Voir » ni point final (ajoutés par la macro). Les labels doivent exister : un label inconnu
  s'affiche `??` et sort en `undefined reference` au log.
- Dans les arguments : pas de `\verb`, de `lstinline`, de `\gls`, de `\parencite` ni d'environnement ; `\_`, `\%`, `\#`,
  `\&` pour les caractères spéciaux ; `\texttt{…}` pour les commandes et noms techniques. L'explication ne cite jamais
  une proposition par sa lettre (cite son contenu), afin de pouvoir réordonner les propositions sans la modifier.
- Ajouter un chapitre au cours : ajouter son bloc `\qcmchap` + 8 `\qcmq` à la bonne place dans `QCM.tex` (l'ordre des
  blocs fixe celui du tableau, du bilan et des corrigés), puis mettre à jour les nombres « 120 questions » et « 255 points »
  (mode d'emploi de `QCM.tex`, barre de la partie VI de l'introduction).
- Retirer un chapitre : supprimer son bloc ; supprimer aussi son `\include` dans `main.tex` (les renvois du
  bloc doivent exister).
- Chaque question a été vérifiée à l'aveugle contre le texte du chapitre (réponse trouvée sans voir la clé,
  puis comparée), les renvois ont été relus dans les sous-sections citées, et la bonne réponse n'est plus
  systématiquement la plus longue. Si un chapitre est modifié, relire les questions qui s'y appuient : ce sont les
  sections citées dans leurs renvois.

**Pièges de mise en œuvre.** Avec `babel` en français, `:` est un caractère actif dans le corps du document : une macro
définie dans `QCM.tex` ne peut pas utiliser `\@for…:=` (« File ended while scanning use of `\@for` »). Les boucles de
`QCM.tex` sont donc écrites sans `:=` (`\@whilenum` et une récursion sur la liste de chapitres). Les clés internes
(`qcm@c@<chapitre>-<n>`) sont formées avec `\detokenize` pour la même raison. Les corrigés s'écrivent sous un
`\begingroup\emergencystretch=3em` pour éviter les débordements dus aux `\texttt` longs. Les numéros de question et de page
des corrigés ne sont corrects qu'après deux compilations complètes ; avec `\includeonly`, les renvois vers les chapitres
non compilés restent `??`.

## Correspondance avec les supports de cours

| Chapitre | À reprendre depuis |
|---|---|
| Présentation générale (rédigée) | Introduction_OpenStack : virtualisation, IaaS/PaaS/SaaS, cloud privé/public/hybride, architecture, multi-nœuds et quatre réseaux, scénario « création d'une VM », mini-TP, comparaison KVM/Proxmox/vSphere/Kubernetes ; définition du cloud du NIST (SP 800-145), site et guide d'installation d'OpenStack, pages de KVM, Proxmox VE et Kubernetes |
| Keystone (rédigé, synthétisé) | Introduction (identité, authentification/autorisation) ; Chapitre 5 partie 1 (domaines, fédération SAML2/OIDC) ; documentation officielle de Keystone |
| Nova, Placement, Glance, Octavia (rédigés) | Introduction_OpenStack (une partie chacun ; Octavia : partie 12) ; le reste vient de la documentation officielle |
| Horizon (rédigé) | Introduction (partie 10 : Horizon n'est qu'un client des API) ; le reste vient de la documentation officielle d'Horizon et du guide de sécurité |
| Neutron (rédigé) | Introduction (concepts, ports, security groups) ; Chapitre 2 (provider/self-service, OVS, OVN, ML2) |
| Cinder (rédigé) | Introduction ; Chapitre 3 partie 1 (multi-backend, volume types, QoS, snapshots, backups) |
| Swift (rédigé) | Chapitre 3 partie 2 ; le reste vient de la documentation officielle de Swift |
| Heat (rédigé) | Chapitre 3 partie 3 ; le reste vient de la documentation officielle de Heat |
| Ironic (rédigé) | Chapitre 5 partie 2 ; le reste vient de la documentation officielle d'Ironic |
| Magnum (rédigé) | Chapitre 5 partie 3 ; le reste vient de la documentation officielle de Magnum et de magnum-capi-helm |
| Trove (rédigé) | Chapitre 5 partie 4 ; présentation Trove (groupe 5) ; le reste vient de la documentation officielle de Trove |
| Déploiement (rédigé) | Chapitre 4 (DevStack, Kolla-Ansible, manuel/bare-metal) ; le reste vient de la documentation officielle de DevStack, Kolla, Kolla-Ansible, OpenStack-Ansible, OpenStack-Helm, du guide d'installation, du guide de haute disponibilité et de la résolution SLURP |
| QCM (rédigés) | Le texte des 15 chapitres uniquement : chaque bonne réponse et chaque explication doivent pouvoir se retrouver dans le chapitre, vers lequel pointe le renvoi du corrigé |
| Conclusion (rédigée) | Les chapitres du cours ; registre des projets officiels, annonce OpenInfra/Linux Foundation, page de l'examen COA, guide du contributeur |

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

Règles TikZ à connaître : ne pas nommer un style `cap` (clé existante de TikZ, « line cap » : erreur
« requires a value ») ; ne pas ajouter `!80!black` à une couleur qui contient déjà un `!` (`insarouge!45`) ;
ne pas mettre `\\` dans un groupe `{}` imbriqué d'un nœud
`align=center` ; préférer un nœud explicite (`\node[etiqf] at (x,y) {…}`) à `node` placé après la
destination d'un `to[...]` (le nœud se retrouve alors sur la destination, pas sur la courbe).

Règles LaTeX à connaître : dans un item de liste (`enumerate`, `itemize`), un long `\texttt{…}`
(chemin de fichier, identifiant) ne se coupe pas, même avec `\allowbreak` (avec ce préambule, babel-french
et `enumitem`) : le mettre dans un paragraphe, ou le scinder en plusieurs `\texttt`. Dans un nom
d'option, écrire `\_\allowbreak` après chaque tiret bas. Pour que les tableaux de dépannage restent
près de leur texte, on peut les placer avec `\begin{table}[H]` (paquet `float`). Pour garder un
exercice et son corrigé sur la même page, on peut les envelopper dans un `minipage` (chapitre Swift,
exercice 10.3), mais pas si le corrigé contient un listing : le listing y est coupé par un trou
(`correction` est un `tcolorbox` coupable). On insère alors un `\clearpage` avant la sous-section
des exercices (chapitre Octavia) ; si cela laisse une page presque vide, on peut plutôt insérer un
`\newpage` juste avant le seul corrigé concerné (chapitre Trove, exercice 14.2). Un listing long
placé dans un `minipage` juste après des flottants qui remplissent la page provoque un
`Overfull \vbox` : le placer après un paragraphe de texte (il passe alors en entier à la page
suivante), ou ne pas le mettre dans un `minipage` (chapitre Trove, fichier `trove.conf`). Pour
un paragraphe qui déborde à cause de noms de fichiers ou de sections en `\texttt`, on ajoute
`\_\allowbreak` ou `\emergencystretch=3em` localement.

## Conventions des chapitres rédigés

- Un chapitre = une `\section{…}\label{chap:<nom>}` ; sous-sections `sec:<nom>-<sujet>`,
  figures `fig:<nom>-…`, tableaux `tab:<nom>-…`, exercices `ex:<nom>-…`.
- Chapitres synthétiques (Nova, Placement, Glance, Neutron, Cinder, Swift, Horizon, Octavia, Heat, Ironic, Magnum) : 10 pages au plus (Keystone, Trove et déploiement, chapitres plus détaillés : 15 au plus ; conclusion : 10 au plus) ; mêmes sections, mais
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
- Glossaire : un terme est défini dans `glossaire.tex` puis cité avec `\gls{…}` (`\glspl{…}` pour le
  pluriel). Les clés propres au déploiement : `slurp`, `quorum`, `ansible`, `playbook`, `inventaireansible`
  (et non `inventaire`, qui est celui de Placement), `devstack`, `kolla`, `kollaansible`, `kayobe`,
  `openstackansible`, `openstackhelm`, `lxc`, `venv`, `registre`, `galera`, `rabbitmq`, `keepalived`,
  `aio`, `ceph`, `ntp`, etc.
- Chapitre « Déploiement » : labels préfixés `dep-` (`sec:dep-kolla`, `fig:dep-topo`, `tab:dep-outils`,
  `ex:dep-choix`). Il décrit les versions en vigueur début octobre 2026 (2026.2 *Hibiscus* sortie le
  30 septembre 2026) : à chaque nouvelle version, revoir les branches `stable/2026.x` des commandes
  (sections 15.5 et 15.10), les systèmes pris en charge, les versions de Kubernetes d'OpenStack-Helm
  (tableau 15.3) et les noms de versions (figure 15.10, en fin de section 15.10).
- Chapitres de service : la première sous-section (« Rôle et place de… ») reprend le même plan (définition, problème, rôle ou
  communications, exemple) et se lit seule ; le paragraphe d'ouverture du chapitre contient la phrase de transition qui le relie
  au précédent.
- Chapitre « Présentation » : labels préfixés `pres-` (`sec:pres-architecture`, `fig:pres-archi`, `fig:pres-scenario`,
  `tab:pres-services`, `ex:pres-web`). Les figures `fig:pres-archi` et `fig:pres-scenario` sont citées par le chapitre
  Keystone ; l'ordre des familles du tableau `tab:pres-services` suit celui des parties. Les adresses IP et les noms
  d'interface des figures multi-nœuds sont ceux du support de cours.
- Introduction et conclusion : `\section*`, donc sans numéro de chapitre ; la conclusion remet à zéro les compteurs de figures
  et de tableaux (`C.1`, `C.2`…) au début de `conclusion.tex`.

## Écarts entre le chapitre Déploiement et le support de cours (Chapitre 4)

Le chapitre suit la documentation officielle quand elle contredit le support :

- **TripleO** est retiré du projet (dépôt archivé en 2024) ; il n'est plus présenté comme une méthode
  de déploiement actuelle (Red Hat déploie aujourd'hui par des opérateurs sur OpenShift, RHOSO).
- **Kolla-Ansible** : les images publiées sur Quay.io sont destinées aux essais, pas à la production
  (construire ses propres images avec `kolla-build` et un registre local) ; Swift n'est plus déployé ;
  Ceph n'est pas déployé (seulement connecté) ; la documentation ne décrit pas de retour arrière après
  une mise à niveau.
- **DevStack** : Swift n'est pas activé par défaut ; l'installation se fait sous un compte `stack`
  dédié, pas en `root`.
- **« Bare metal »** : le terme recouvre deux sens, distingués dans une remarque de la section 15.7.

## Réglages ajoutés au préambule

- `microtype` sans protrusion/expansion/kerning (il casse les ligatures dans `\texttt`) et
  `\DisableLigatures` pour la police à chasse fixe : `--option` s'affiche correctement.
- `upquote=true` dans `\lstset` : apostrophes droites dans les listings (commandes shell, SQL).
- `\emergencystretch=2em` : évite la plupart des débordements de ligne.
- Paquets ajoutés : `adjustbox`, `tikz` (bibliothèques `positioning`, `arrows.meta`,
  `fit`, `backgrounds`, `calc`, `shapes.geometric`, etc. : voir `main.tex`).
