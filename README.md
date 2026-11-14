# Groupe 10 – Projet Data Mining 2026-2027

> Cette première partie décrit le travail réalisé par le groupe 10. Les consignes d'origine du projet sont conservées sans modification plus bas, après la ligne de séparation.

**Membres :** ABBAS Yasmina, *(à compléter)*

**Avancement :** les dossiers `ecology` et `logistics` sont analysés avec les méthodes des cours 1 à 6. Les dossiers `health` et `public_records` ne servent pour l'instant que de passerelles entre les deux. Voir [Reste à faire](#reste-à-faire).

## Contenu du dépôt

| Fichier | Contenu |
|---|---|
| `Base_notebook_to_complete.ipynb` | Rapport principal. **Partie 1** : travail initial du groupe (exploration, nettoyage), réorganisé et commenté étape par étape. **Parties 2 à 11** : dossier `ecology`, cours 1 à 6, test de l'hypothèse, synthèse. **Annexe** : usage de l'IA générative. |
| `Analyse_logistics_cours.ipynb` | Dossier `logistics` : vérification des deux notebooks ci-dessous, nettoyage corrigé, cours 1 à 6, synthèse, passerelle vers `health`, annexe IA. Il lit les fichiers bruts et n'écrit rien sur le disque. |
| `Clean_logistics.ipynb`, `analyse_logistics.ipynb` | Premiers notebooks `logistics` (nettoyage, puis analyse), conservés sans modification. Leur vérification se trouve dans la partie 1 de `Analyse_logistics_cours.ipynb`. |
| `data/` | Données fournies, non modifiées. |

Trois documents PDF ont aussi été produits, en dehors du dépôt : une synthèse du dossier ecology, une synthèse des dossiers ecology et logistics, et une méthodologie étape par étape avec schémas.

## Exécuter les notebooks

`requirements.txt` ne contient que l'environnement Jupyter. Les bibliothèques d'analyse s'installent ensuite avec `pip` :

```bash
conda create --name datamining-projet --file requirements.txt
conda activate datamining-projet
pip install pandas numpy scipy scikit-learn matplotlib seaborn mlxtend networkx pm4py statsmodels
```

Versions testées : pandas 3.0.5, numpy 2.5.3, scipy 1.18.1, scikit-learn 1.9.1, matplotlib 3.11.2, seaborn 0.13.2, mlxtend 0.25.0, networkx 3.7, pm4py 2.7.23.12, statsmodels 0.15.0.

| Notebook | Exécution complète | Remarque |
|---|---|---|
| `Base_notebook_to_complete.ipynb` | environ 2 min 30 | 224 cellules |
| `Analyse_logistics_cours.ipynb` | environ 45 s | 101 cellules |
| `analyse_logistics.ipynb` | quelques secondes | exécuter d'abord `Clean_logistics.ipynb`, qui crée `data/logistics/clean/` |

## Démarche

La même démarche, en entonnoir, a été appliquée aux deux dossiers :

1. **Explorer large** : appliquer dans l'ordre les méthodes de chaque cours à toutes les tables du dossier, sans hypothèse de départ.
2. **Repérer les signaux faibles** qui résistent aux contrôles : un site qui chute, un rapport qui ne suit pas le modèle type, deux flux d'expéditions trop réguliers.
3. **Relier les sources** par leurs clés : site, mois, numéro d'enregistrement, numéro d'expédition.
4. **Tester une hypothèse précise**, avec des témoins et une mesure du hasard (test placebo, permutations).
5. **Conclure prudemment** : une coïncidence systématique n'est pas une preuve.

Quelques principes ont guidé les décisions :

- tout lire en texte, puis convertir chaque colonne explicitement ;
- considérer qu'une absence peut être une information ;
- ne pas confondre valeur aberrante et anomalie ;
- toujours comparer à un témoin et mesurer la part du hasard ;
- garder les résultats négatifs ;
- ne citer que des chiffres issus d'une sortie exécutée.

## Résultats

### Dossier ecology

- Une table unifiée de 4 992 relevés (64 sites × 78 mois, 2019-2025). L'activité est mesurée par le taux horaire (comptage / heures d'observation).
- Le site KNT-KES chute au printemps 2024, mais pas ses sites témoins : la cause n'est pas climatique.
- Un seul rapport de terrain sort du modèle type (EFR-510088). Il renvoie au registre des accès aux sites. La **Horizon Field Research Cooperative** y détient 18 des 60 dossiers, sur 8 sites. Ses accès ont toujours lieu de nuit, sont souvent confidentiels et utilisent du matériel réfrigéré.
- Après chaque accès d'Horizon, l'activité des chauves-souris baisse de **59 %** en moyenne (test placebo, p ≈ 0,0005).
  - Rien de tel après les accès des 26 autres organisations.
  - Les espèces de comparaison et l'état des appareils ne bougent pas.
  - Les 14 épisodes observables sont tous suivis d'un déficit, mais un seul a été signalé dans un rapport.

### Dossier logistics

- Les « disparitions de marchandises » de l'analyse initiale sont un bruit uniforme : 10,7 % de réceptions incomplètes, dans toutes les catégories et chez tous les fournisseurs.
- Chacun des 18 accès d'Horizon est suivi, **6 heures après**, d'un envoi réfrigéré d'« environmental and wildlife reference materials » vers l'entrepôt **Meridian BioLogistics** de la région. C'est toujours le même transporteur (Northstar Cold Chain), avec un départ à 9 h.
- Dans ces entrepôts, les échantillons biologiques disparaissent le mois de l'arrivée : **−7,3 lots** au bilan, contre +1,2 les autres mois (permutation, p ≈ 0,0001). Les autres catégories ne bougent pas. Chaque envoi est suivi d'un ajustement de stock négatif le dernier jour du mois.
- Trois mémos internes seulement sortent du modèle type, tous écrits par Meridian. Ils décrivent une double comptabilité (« CHAMELEO ») et un « material sink » sur les échantillons biologiques.
- En aval, Meridian livre des lots « cliniques » à l'**Asterion Clinical Institute**. 54 % des procédures MRP-42 d'Asterion (dossier `health`) ont lieu dans les 14 jours qui suivent une livraison, contre 5 % attendus au hasard.

### Hypothèse d'ensemble

L'hypothèse la plus cohérente avec les données est la suivante. Des animaux sont prélevés lors d'accès autorisés, puis acheminés par la chaîne du froid de Meridian. Ils sortent ensuite de la comptabilité ordinaire et alimentent un programme « clinique » d'Asterion.

Les données établissent une coïncidence systématique, spécifique et contrôlée. Elles ne montrent ni le contenu des lots, ni leur usage final. Le récit est fictif, puisque les données sont synthétiques.

## Vérification des notebooks logistics initiaux

| Point | Constat |
|---|---|
| `str_to_num` | Le signe moins est perdu : les 89 ajustements négatifs sur 140 deviennent positifs. |
| Dates | Avec `format="mixed"`, 3 dates au format jj/mm sont lues à l'envers. |
| Catégories | Dans 8 colonnes, les variantes de casse et d'espaces ne sont pas harmonisées. |
| Conclusions | Les « vols » sont un bruit uniforme : 10,1 % de réceptions incomplètes chez Meridian, contre 10,7 % en tout (p = 1,00). Les règles Apriori reposent sur 2 commandes. Le chiffre de « +41 % » n'est calculé nulle part. |
| Points validés | Confrontation au dictionnaire des données, traitement des virgules décimales et des unités, absence de conversion des devises. |

Les corrections sont appliquées dans la partie 2 de `Analyse_logistics_cours.ipynb`.

## Reste à faire

- [ ] Compléter la liste des membres en tête des notebooks et de ce README.
- [ ] Compléter les deux annexes « Usage de l'IA générative » : « quand et pourquoi », et « ce que nous avons vérifié, modifié ou contesté ».
- [ ] Décider s'il faut fusionner `Analyse_logistics_cours.ipynb` dans `Base_notebook_to_complete.ipynb`, puisque les consignes demandent un rapport unique.
- [ ] Relire et reformuler les interprétations.
- [ ] Explorer pour eux-mêmes les dossiers `health` (procédures MRP-42) et `public_records` (dossiers réglementaires, « Unwabu »).

## Usage de l'IA générative

Plusieurs parties du travail ont été produites avec l'aide d'un assistant d'IA générative, Claude (Anthropic) dont des documents de synthèse.

L'assistant s'est appuyé sur les supports de cours et sur le travail initial du groupe. Chaque chiffre cité a été vérifié sur les sorties des notebooks. Le détail figure dans l'annexe de chaque notebook.

---

# Année 2026-2027 - M2IA/M2DS - UE Data Mining - Projet

Sur ce repository, vous pourrez trouver l’ensemble des éléments
nécessaire au projet de groupe de l’UE Data Mining pour l’année 2026-2027.

Votre objectif est de mettre en évidence le narratif caché à l’intérieur
de ces données à l’aide des notions et méthodes vues au fur et
à mesure de l’UE Data Mining.

/!\ Ce narratif est entièrement fictif. Il a été créé dans un objectif pédagogique et ne reflète aucunement la réalité /!\

Pour ce faire, vous travaillerez en groupe en débattant et
en vous répartissant les tâches à faire.

Le rendu attendu est un rapport, sous la forme d’un notebook jupyter,
qui détaillera vos réflexions, les méthodes et vos conclusions.

Pour cela, le fichier `Base_notebook_to_complete.ipynb` est mis à votre disposition (vous pouvez changer le nom si vous le souhaitez).

Pensez à bien y préciser:

* Le nom de votre groupe
* La liste des membres

Vous serez avant tout évalué sur votre méthode, donc soyez précis, détaillé
et rigoureux dans vos descriptions.

/!\ L’usage de l’IA Générative est fortement déconseillé (ce sont vos cerveaux que l’on souhaite entrainer ici).
Si vous souhaitez tout de même l’utiliser, cela doit être justifié et documenté dans votre rapport /!\

## Jeux de données

Dans le dossier `data/`, vous trouverez les ensembles des jeux de données suivants:

- `data/public_records/` — jeux de données issus d’institutions administratives publiques,
- `data/health/` — jeux de données médicales issus de cliniques et d’hôpitaux,
- `data/ecology/` — jeux de données issus de recherches environnementales,
- `data/logistics/` — jeux de données issus de groupes et d’entreprises privées.

Le fichier `data/data_dictionary.csv` détaille les caractéristiques des variables présentes dans chaque jeu de données.

/!\ Ces jeux de données sont entièrement synthétiques et ont été créé dans un but pédogagique.
Ils ne représentent pas des informations factuelles à propos de personnes réelles,
d’organisations réelles, de gouvernnements réels ou localisations réelles.
Toute ressemblance avec des personnes réelles, physiques ou morales, est totalement involontaire et fortuite. /!\ 
 
## Pour bien commencer

1. Faites un `fork` de ce repository vers un noveau repository pour votre groupe.
2. Clonez le repository de votre groupe sur votre machine.
3. (Installez conda si ce n’est pas déjà fait)
4. Installez les dépendances avec la commande: `conda create --name <envname> --file requirements.txt`
5. Lancez le notebook avec la commande: `jupyter notebook nom-du-notebook.ipynb`
6. Travaillez en groupe et complétez le notebook

## Rendu

Le rendu pourra se faire via:

* une `pull-request`, 
* en m’envoyant une archive .zip contenant votre travail à `antoine.richard@chu-lyon.fr`.
