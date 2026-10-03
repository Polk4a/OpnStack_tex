# OpenStack et ses services — notes de cours (INSA CVL)

Structure LaTeX d'un cours sur OpenStack, basée sur le template INSA CVL
(`info_quantiq_tex`) : charte rouge/gris, en-tête avec logo, page de titre, sommaire,
glossaire, bibliographie biblatex/APA. **Un chapitre = un fichier `chap_<nom>.tex`.**
Les chapitres de services et la conclusion sont volontairement vides (titre + `\label`).

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
chap_keystone.tex     2.  Keystone : identité
chap_nova.tex         3.  Nova : calcul
chap_placement.tex    4.  Placement : suivi des ressources
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
glossaire.tex         Entrées du glossaire (3 exemples)
sources.bib           Bibliographie (documentation OpenStack, Trove)
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
| Nova, Placement, Glance, Horizon, Octavia | Introduction_OpenStack (une partie chacun) |
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
- Styles TikZ pour les schémas d'architecture : `svc` (service), `svcgris` (composant
  secondaire), `bus` (barre API/bus), `flux` (flèche).
- Code : styles `listings` `terminal`, `darkterminal`, `verbatimlike`, macro `\prompt`.
- Renvois : `\label`/`\ref` préfixés (`chap:`, `sec:`, `fig:`, `ex:`). Un chapitre a pour
  label `chap:<nom>` (ex. `\ref{chap:keystone}`).
- Glossaire : `\gls{iaas}` ; bibliographie : `\parencite{openstack-docs}`.
