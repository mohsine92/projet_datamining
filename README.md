# Projet de data mining — Domaine des Trois Rivières

## Préparation des données

Le [notebook de préparation](notebooks/Pr%C3%A9paration%20des%20donn%C3%A9es.ipynb) fournit une base commune aux analyses du groupe. Les données d'origine sont dans `data/raw/` et les exports sont dans `data/processed/`.

### Reproduire les exports

L'exécution a été vérifiée avec Python 3.13.17. Depuis la racine du dépôt, créer un environnement virtuel et installer les dépendances. Sous macOS ou Linux, utiliser les commandes suivantes.

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Ouvrir le notebook dans VS Code avec l'extension Jupyter, sélectionner le noyau Python de `.venv`, puis exécuter toutes les cellules dans l'ordre. Le répertoire de travail du notebook doit être `notebooks/`. Créer le dossier `data/processed/` s'il est absent.

### Données préparées

| Export | Contenu |
| --- | --- |
| [`parcelles_clean.csv`](data/processed/parcelles_clean.csv) | 220 parcelles distinctes après retrait de trois lignes strictement identiques. |
| [`historique_clean.csv`](data/processed/historique_clean.csv) | 1 338 observations de rendement et de prix pour 1 320 couples parcelle–millésime, de 2019 à 2024. |

Les identifiants concordent entre les deux fichiers. Les 8 expositions et les 8 pluviométries manquantes concernent 16 parcelles distinctes. Les valeurs manquantes sont conservées. La colonne `rendement_nul_a_verifier` signale P128, P166 et P184, dont les rendements historiques sont nuls sur les six millésimes.

### Couples parcelle–millésime en conflit

P44, P101 et P115 ont chacune deux observations pour chacun des six millésimes. Dans les 18 couples concernés, les deux lignes donnent des rendements et des prix différents. Les 36 lignes sont conservées dans l'export commun, car la source ne permet pas de choisir une observation.

Pour les analyses qui exigent une observation par parcelle et par millésime, les **217 séries complètes sans conflit** (1 302 observations) servent de base de référence. En complément, une moyenne calculée **par couple parcelle–millésime** sur les 18 couples en conflit permet de tester la sensibilité des résultats à cette hypothèse.

### Valeurs atypiques et unités

P91, P100 et P145 ont un âge renseigné de 150 ans, contre un maximum de 58 ans pour les autres parcelles. Ces valeurs sont conservées. Sur l'export parcelles, la corrélation de Pearson entre l'âge des vignes et le rendement est de −0,15 sur les 220 parcelles et de −0,23 lorsque ces trois lignes sont écartées du calcul. Pour les analyses où l'âge intervient, comparer les résultats avec et sans ces observations.

L'azote est renseigné en g/ha. Pour l'exprimer en kg/ha, diviser les valeurs par 1 000. Les imputations, agrégations et choix de variables sont documentés dans les analyses qui les utilisent.
