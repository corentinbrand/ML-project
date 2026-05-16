# Relecture `notebook_final_python.ipynb` — Checklist (révisée après discussion)

Lecture seule. 4 axes : **[PROF]** exactitude · **[DEV]** code · **[FIL]** fil rouge + forme · **[SUJET]** conformité.
Statut : ✅ à faire (validé en discussion) · ❌ abandonné (décidé de ne pas faire) · ❓ optionnel.

---

## ✅ À FAIRE — Bloquant

- [ ] **B1 — Cell 165 [SUJET][FIL]** : remplacer la note brouillon « Dire à peu près les mêmes perfs… » par une vraie interprétation chiffrée du réseau de neurones (classification).
- [ ] **B2 — Classification, après cell 167 [SUJET][FIL]** : le graphe comparatif ROC+AUC des 12 modèles EXISTE déjà (cell 167). Ce qui manque = l'**interprétation**. Insérer entre cell 167 et cell 168 une cellule markdown qui :
  - décrit le graphe : courbes ROC quasi superposées, écart total faible (AUC 0.760 → 0.807, soit 0.047) ;
  - donne le classement : linéaires pénalisés (ElasticNet/Ridge/LASSO) en tête à 0.807, gros peloton 0.79-0.81 (NN 0.803, GB 0.802, AdaBoost 0.799, KNN 0.794, RF 0.791, Bagging 0.787, SVM 0.783), arbres seuls en retrait (CART 0.760, élagué 0.764) ;
  - conclut : aucun modèle ne domine, la performance ne départage pas → critère = interprétabilité ;
  - choisit le meilleur modèle : régression logistique LASSO (ou logistique RFECV-sélectionnée, ~12 variables, perf comparable cf cell 108) = top AUC + parcimonie/lisibilité. CART seul = plus lisible visuellement mais pire AUC, mauvais compromis.
  - Style : factuel (chiffres réels), pas de conditionnel, même ton que les autres interprétations, pas de gras inline ni `;`.
- [ ] **B3 — ~~tableau manquant~~** : corrigé après discussion — le graphe comparatif existe (cell 167). Fusionné dans B2 (seule l'interprétation manque). Pas de tableau supplémentaire à créer.
- [ ] **B4 — Classification, juste après B2 [SUJET]** : ajouter une cellule « lien avec l'analyse exploratoire ». Confronter les résultats observés (coefficients LASSO/Ridge, variables RFECV, importances RF) aux prédictions de la **cellule synthèse exploratoire (cell 58)** : phénomène multifactoriel, Smoking_Status / Family_History / Physical_Activity_Level discriminants, Height/HDL/Stress/Sleep faibles. Dire ce qui a été confirmé / infirmé. Court et pertinent. (Nécessitera de relire les sorties réelles de coefficients/importances au moment de la rédaction.)
- [ ] **B5 — Cell 173 [PROF][SUJET]** : `test_size=0.25` → **0.20** (l'énoncé impose 20%).

## ✅ À FAIRE — Important

- [ ] **I1 — Cells 83 et 101 [PROF][FIL]** : remplacer « AVC » dans le **texte** par « risque de maladie cardiaque » / « accident cardiaque ». (Les noms de variables `Y_*_AVC` ne sont PAS modifiés, décidé en discussion.)
- [ ] **I3 — Cell 80 [PROF]** : enlever le `1.-` devant `logitOpt.best_score_`, garder le libellé « Meilleur score » affichant directement `best_score_`.
- [ ] **I4 — Cell 75 [PROF][DEV]** : remplacer la MSE par la binary cross-entropy `log_loss(Y_test_AVC, logit_simple.predict_proba(Xr_test))` (attention : nécessite les probabilités, pas `y_pred`), OU supprimer la ligne (la cell 76 affiche déjà accuracy + matrice de confusion).
- [ ] **I5 — Cells 121, 123, 207 [PROF][FIL]** : réécrire au factuel (résultats réels observés) au lieu du conditionnel « si le meilleur modèle est… ».
- [ ] **I6 — Cell 154 [DEV][PROF]** : supprimer la cell 154 (doublon mort de la 155, output écrasé, aucun graphe). Ajouter une phrase markdown expliquant pourquoi on utilise `staged_predict` + CV manuelle plutôt que `GridSearchCV` (early stopping sur n_estimators à partir d'un seul fit).
- [ ] **I8 — Cell 143 [PROF]** : corriger la comparaison stabilité arbre/bagging. Actuellement l'arbre est fit sur `X_boot` (ré-échantillonné) et le bagging sur `Xr_train` complet → pas à isopérimètre, exagère l'instabilité de l'arbre. Faire varier les deux de la même façon.
- [ ] **I10 — Plusieurs cellules [PROF]** : unifier la métrique d'optimisation des `GridSearchCV`. Actuellement mélange accuracy / roc_auc / balanced_accuracy (cells 80, 89, 96, 104, 114, 118, 130, 142, 147, 153, 163). Choisir une seule métrique de référence (roc_auc recommandé vu le déséquilibre 56/44) ou justifier chaque écart.
- [ ] **I11 — Régression, cells 207-208-209 [PROF][FIL][SUJET]** : restructurer la conclusion régression pour qu'elle soit **symétrique à la classification** :
  - une cellule conclusion (comparaison RMSE/R² réels, classement, niveau de précision, choix du meilleur modèle sous contrainte d'interprétabilité) — factualisée, plus de conditionnel « si… » ;
  - une cellule « lien avec l'analyse exploratoire » : confronter aux prédictions de la cell 58 (Cholesterol_Total massivement dominant r=0.83, Age second, qualitatives nulles pour LDL). Présenter les vraies importances observées, pas « on s'attend à retrouver ».

## ✅ À FAIRE — Mineur (forme)

- [ ] **M1 — Partout [FIL]** : retirer le gras inline (cells 112, 117, 121, 125, 126, 135, 136, 141, 146, 151, 152, 156, 157, 161, 171, 175, 189, 191, 195, 199, 202, 207, 209). Ne garder que les labels type `**Interprétation :**`.
- [ ] **M2 — Partout [FIL]** : remplacer les `;` de fin de puce par `.` (cells 112, 117, 121, 123, 125, 126, 135, 152, 157, 161, 171, 175, 185, 195, 199, 207...).
- [ ] **M3 — Cell 71 [FIL]** : supprimer les retours à la ligne en milieu de phrase (texte à mettre en continu). Mineur.
- [ ] **M7 — Typos [FIL]** : « Classificcation » (129), « importannt » (133), « Cette permet »→« Cela permet » (146), « arrê »/« arrrêt » (77, 83), « renforcel'interprétation » (208, espace manquant), « couteux »→« coûteux » (209).

## ❓ OPTIONNEL (à trancher)

- [ ] **O1 — Numérotation [FIL]** : classification = 5 sections, régression = 12. Harmoniser la granularité du découpage entre les deux moitiés. Optionnel (pas faux car sous des `#` séparés).
- [ ] **O2 — Global [DEV]** : regrouper TOUS les imports dans une seule cellule en tête de notebook, plus aucun import ensuite (vérifier qu'aucun n'est oublié). Tâche différée, à faire en dernier.
- [ ] **O3 — Cell 160 [DEV]** : `plt.ylim(0.68, 0.75)` codé en dur, peu robuste si les scores changent. Optionnel.

## ❌ ABANDONNÉ (décidé en discussion)

- ~~I2 — « C » appelé paramètre de pénalisation~~ : vocabulaire du cours, OK tel quel.
- ~~I7 — Cell 173 recharge le CSV~~ : pas un souci.
- ~~`results`/`store_result` redéfinis cell 174~~ : pas un souci.
- ~~M4 — Ton familier (« logique », « sur-domine »)~~ : humanise le rapport, pas un problème.

## ✅ Conforme (ne pas toucher)

- Analyse exploratoire (cells 0-69), déjà retravaillée.
- Tous les modèles demandés présents pour les 2 problèmes.
- Hyperparamètres optimisés par CV partout.
- Régression : comparaison test + interprétabilité présents (cells 205-207), à factualiser seulement (I11).
- Compromis biais/variance bien traité (section arbres).
