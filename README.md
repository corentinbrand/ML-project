# Projet Machine Learning - Risque cardiovasculaire


![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![R](https://img.shields.io/badge/R-4.3.3-276DC3?logo=r&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-F37626?logo=jupyter&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-Modelling-F7931E?logo=scikitlearn&logoColor=white)

Projet de Machine Learning autour d'un jeu de données synthétique de santé cardiovasculaire.  
L'objectif est de construire, comparer et interpréter plusieurs modèles supervisés sur deux tâches complémentaires :

- **classification** : prédire `Heart_Disease_Risk`, c'est-à-dire le risque d'accident cardiaque.
- **régression** : prédire `Cholesterol_LDL`, le taux de LDL-cholestérol.

Le projet combine analyse exploratoire, préparation des données, modélisation, validation croisée, comparaison de modèles et discussion interprétabilité/performance.

> Les données sont synthétiques et l'étude a un objectif pédagogique. Les résultats ne constituent pas un outil médical.

---

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Résultats principaux](#résultats-principaux)
- [Structure du dépôt](#structure-du-dépôt)
- [Données](#données)
- [Méthodologie](#méthodologie)
- [Modèles étudiés](#modèles-étudiés)
- [Reproduire le projet](#reproduire-le-projet)
- [Lecture conseillée](#lecture-conseillée)
- [Points forts du projet](#points-forts-du-projet)
- [Limites et pistes d'amélioration](#limites-et-pistes-damélioration)

---

## Vue d'ensemble

Le jeu de données contient **15 000 patients** et **19 colonnes**. Après suppression de l'identifiant `Patient_ID`, les variables décrivent notamment l'âge, le genre, la taille, le poids, l'IMC, la pression artérielle, les taux de cholestérol, la glycémie, le tabagisme, l'activité physique, les antécédents familiaux, le stress et le sommeil.

Deux problèmes sont traités :

| Problème | Variable cible | Type | Métriques principales |
|---|---:|---|---|
| Risque cardiovasculaire | `Heart_Disease_Risk` | Classification binaire | Accuracy, balanced accuracy, F1-score, ROC-AUC |
| LDL-cholestérol | `Cholesterol_LDL` | Régression | RMSE, MAE, R² |

La classe cible de classification est raisonnablement équilibrée : environ **56.4 %** de patients à faible risque et **43.6 %** à risque élevé.

---

## Résultats principaux

### Classification du risque cardiaque

Les modèles obtiennent des performances proches : les courbes ROC sont quasiment superposées, avec des AUC globalement comprises entre **0.760** et **0.807**.

| Famille de modèles | Résultat observé |
|---|---|
| Régressions logistiques pénalisées | Meilleures AUC, autour de **0.807** pour LASSO, Ridge et Elastic Net |
| SVM / réseau de neurones / boosting | Très bons résultats, proches des meilleurs modèles |
| Random Forest / Bagging | Performances correctes, mais interprétabilité plus limitée |
| CART seul | Très lisible, mais AUC plus faible, autour de **0.760** |

**Conclusion classification :** aucun modèle ne domine brutalement. Le meilleur compromis retenu est un modèle linéaire pénalisé ou sélectionné, car il garde une performance proche du maximum tout en restant interprétable. Les variables les plus utiles sont notamment le tabagisme, les antécédents familiaux, l'activité physique, l'âge, la pression artérielle systolique et la glycémie.

### Régression du LDL-cholestérol

La régression donne une conclusion plus nette : les modèles linéaires suffisent largement.

| Modèle | RMSE test | MAE test | R² test | Lecture |
|---|---:|---:|---:|---|
| Lasso | **9.782** | 7.718 | **0.6946** | Meilleur score, modèle parcimonieux |
| Régression linéaire complète | 9.788 | 7.728 | 0.6942 | Référence simple et très solide |
| Ridge | 9.788 | 7.729 | 0.6943 | Même niveau que le modèle complet |
| Régression linéaire sélectionnée | 9.790 | 7.726 | 0.6941 | 4 variables retenues seulement |
| SVR linéaire | 9.796 | 7.738 | 0.6937 | Très proche des modèles linéaires |
| Gradient Boosting | 9.822 | 7.757 | 0.6921 | Bon, mais pas meilleur |
| Random Forest | 9.874 | 7.807 | 0.6888 | Stable, mais plus coûteux |
| CART régression | 9.901 | 7.819 | 0.6871 | Interprétable par seuils |
| MLP | 10.222 | 8.073 | 0.6665 | Correct, mais pas compétitif ici |
| kNN régression | 12.469 | 9.927 | 0.5038 | Moins adapté |

**Conclusion régression :** `Cholesterol_Total` porte l'essentiel du signal prédictif pour `Cholesterol_LDL`. Un modèle linéaire parcimonieux, notamment le Lasso ou la régression sélectionnée, offre le meilleur compromis entre précision, simplicité et interprétabilité.

---

## Structure du dépôt

```text
ML-project/
├── README.md
├── RELECTURE_notebook_final.md
├── renv.lock
├── divers/
│   ├── healthcare_synthetic_data.csv
│   ├── ML-Project-4MA-2526.pdf
│   └── supports de cours et documents PDF
├── notebook_finaux/
│   └── notebook_final_python.ipynb
├── notebook finaux/
│   ├── notebook_final_python.ipynb
│   ├── notebook_final_R.ipynb
│   ├── Slides_oral.ipynb
│   ├── outputs_python/
│   └── outputs_R/
├── analyses_python/
│   ├── Analyse_exploratoire.ipynb
│   ├── Analyse_exploratoire_Chris.ipynb
│   ├── Reg_Lin.ipynb
│   ├── kNN_SVM_gus.ipynb
│   ├── CART_AgrDMod_python_gus.ipynb
│   ├── CART_AgrDMod_python_gus_cholesterol.ipynb
│   ├── réseaux_neurones.ipynb
│   └── contingence.ipynb
└── analyses_R/
    └── fichier_Romain.ipynb
```

Le notebook principal à lire est :

```text
notebook_finaux/notebook_final_python.ipynb
```

Les dossiers `analyses_python/` et `analyses_R/` gardent les analyses intermédiaires utilisées pendant la construction du projet.

---

## Données

Fichier utilisé :

```text
divers/healthcare_synthetic_data.csv
```

Variables principales :

| Variable | Description |
|---|---|
| `Age` | âge du patient |
| `Gender` | genre encodé numériquement |
| `Height_cm`, `Weight_kg`, `BMI` | caractéristiques morphologiques |
| `Systolic_BP`, `Diastolic_BP` | pression artérielle |
| `Cholesterol_Total`, `Cholesterol_LDL`, `Cholesterol_HDL` | mesures de cholestérol |
| `Fasting_Blood_Sugar` | glycémie à jeun |
| `Smoking_Status` | statut tabagique |
| `Alcohol_Consumption` | niveau de consommation d'alcool |
| `Physical_Activity_Level` | niveau d'activité physique |
| `Family_History` | antécédents familiaux |
| `Stress_Level` | niveau de stress |
| `Sleep_Hours` | heures de sommeil |
| `Heart_Disease_Risk` | cible de classification |

Prétraitements appliqués :

- suppression de `Patient_ID`, identifiant sans valeur prédictive.
- conversion des variables qualitatives en catégories.
- one-hot encoding des variables catégorielles.
- séparation apprentissage/test.
- standardisation pour les modèles sensibles aux échelles : kNN, SVM/SVR, réseaux de neurones et modèles pénalisés.

---

## Méthodologie

Le projet suit une démarche complète :

1. **Chargement et contrôle qualité** : dimensions, types, valeurs manquantes.
2. **Analyse univariée** : distributions, boxplots, modalités des variables qualitatives.
3. **Analyse bivariée** : corrélations, liens avec les cibles, comparaisons par groupes.
4. **ACP** : lecture globale de la structure des variables quantitatives.
5. **Préparation des données** : encodage, split train/test, standardisation.
6. **Modélisation classification** : comparaison des approches sur `Heart_Disease_Risk`.
7. **Modélisation régression** : comparaison des approches sur `Cholesterol_LDL`.
8. **Validation croisée** : choix des hyperparamètres.
9. **Comparaison finale** : métriques test, stabilité, coût d'entraînement, interprétabilité.

---

## Modèles étudiés

### Classification

- Régression logistique simple.
- Régression logistique LASSO.
- Régression logistique Ridge.
- Régression logistique Elastic Net.
- Sélection de variables par RFECV.
- kNN.
- SVM linéaire et noyau RBF.
- CART.
- CART élagué.
- Bagging.
- Random Forest.
- AdaBoost.
- Gradient Boosting.
- Réseau de neurones MLP.

### Régression

- Régression linéaire complète.
- Régression linéaire avec sélection de variables.
- Ridge.
- Lasso.
- CART de régression.
- CART élagué.
- Bagging Regressor.
- Random Forest Regressor.
- AdaBoost Regressor.
- Gradient Boosting Regressor.
- kNN Regressor.
- SVR.
- MLP Regressor.

---

## Reproduire le projet

### 1. Cloner le dépôt

```bash
git clone <url-du-repo>
cd ML-project
```

### 2. Créer un environnement Python

Linux/macOS :

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows PowerShell :

```powershell
python -m venv .venv
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

### 3. Installer les dépendances Python

```bash
pip install numpy pandas matplotlib seaborn scipy scikit-learn jupyter
```

### 4. Lancer le notebook principal

```bash
jupyter notebook notebook_finaux/notebook_final_python.ipynb
```

### 5. Option R

Le dépôt contient un `renv.lock` pour l'environnement R. Pour restaurer l'environnement R depuis R :

```r
install.packages("renv")
renv::restore()
```

Selon l'environnement local, les notebooks R peuvent nécessiter des packages complémentaires comme `tidyverse`, `ggplot2`, `FactoMineR`, `factoextra`, `glmnet`, `pROC`, `ROCR` ou `IRkernel`.

---

## Lecture conseillée

Pour comprendre le projet rapidement :

1. Lire ce README pour avoir la vue globale.
2. Ouvrir `notebook_finaux/notebook_final_python.ipynb`.
3. Parcourir l'introduction, la synthèse exploratoire, puis les deux conclusions de modélisation.
4. Utiliser les notebooks de `analyses_python/` pour retrouver les essais détaillés par famille de modèles.
5. Ouvrir `notebook finaux/Slides_oral.ipynb` pour retrouver le plan de présentation orale.

---

## Points forts du projet

- Analyse exploratoire complète avant la modélisation.
- Comparaison de nombreuses familles d'algorithmes.
- Validation croisée pour éviter le choix arbitraire des hyperparamètres.
- Discussion explicite du compromis biais/variance.
- Comparaison entre performance brute et interprétabilité.
- Cohérence entre l'analyse exploratoire et les résultats de modélisation.
- Même logique appliquée à une tâche de classification et à une tâche de régression.

---

## Limites et pistes d'amélioration

- Les données sont synthétiques : les conclusions ne doivent pas être généralisées directement à un contexte médical réel.
- Certaines méthodes coûteuses, comme le SVR et les réseaux de neurones, ont été optimisées sur des grilles volontairement raisonnables pour garder un notebook exécutable.
- La performance classification reste modérée : le risque cardiaque est multifactoriel et aucune méthode ne sépare parfaitement les classes.
- Une étape future utile serait de factoriser le code en scripts réutilisables et d'ajouter un fichier `requirements.txt`.
- Un rapport exporté en HTML ou PDF depuis le notebook final rendrait la consultation encore plus simple pour un lecteur externe.

---

## Conclusion

Ce projet montre qu'un modèle plus complexe n'est pas automatiquement meilleur.  
Pour le risque cardiovasculaire, plusieurs modèles atteignent des performances proches et le choix final dépend fortement du besoin d'interprétabilité. Pour la prédiction du LDL-cholestérol, les modèles linéaires dominent nettement : le signal principal est déjà porté par `Cholesterol_Total`, et un modèle parcimonieux suffit à expliquer environ **69 %** de la variabilité observée sur l'échantillon test.

En bref : beaucoup de modèles testés, mais une conclusion simple et propre. La performance compte, l'explicabilité aussi.
