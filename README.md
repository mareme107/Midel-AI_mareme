# Midel-AI
## Projet Machine learning ISEP2 2025 : Détection de transactions frauduleuses sur la plateforme de vente xente
### Objectifs : 
Le projet consiste a entrainer un modèle d'apprentissage automatique permettant de détecter des transsactions frauduleuses dans la plateforme de vente Xente.



### Description : 


Xente est une application de commerce électronique et de services financiers utilisée par plus de 10 000 clients en Ouganda. 
L'ensemble de données utilisées contient environ 140 000 transactions réalisées entre le 15 novembre 2018 et le 15 mars 2019. Un des défis majeurs associé à ce projet consiste au déséquilibre très marqué entre les classes frauduleuses et non frauduleuses avec seulement 0.2% de fraudes dans les données d'entrainement.



### Structure du dépot : 
Le dépot Github contient les dossiers: donnees, config, Dashboard, docs, models et Notebook.

- données : contient les données utiliséses pour l'apprentissage du modèle, il contient également un fichier qui décrit les variables qui y sont présentes.
- Notebooks : Les différents notebooks correspondant à chaque étape du projet.
- Models : contient l'ensemble de tous les différents modèles entrainés au format dill
- Dashboard: Il s'agit du dossier contenant la structure permettant le déploiement du modèle.

Pour l'éxécution du code il faut vous assurer d'installer toutes les dépendances et packages considérés dans le dossier configuration (config). Le script principal en notebook contient toutes les parties de la collection à la modélisation et choix du meilleur modèle passant par l'EDA et les features. Les sous scripts sont séparés pour faciliter la lecture mais se retrouvent dans le notebook principal.


## Choix du Modèle : LightGBM_SMOTE


Pour sélectionner le meilleur modèle de classification dans le cadre de ce projet, on a procédé à une comparaison rigoureuse de plusieurs algorithmes, en respectant une méthodologie cohérente et exhaustive.

### 1. Sélection initiale des modèles

On a testé cinq familles de modèles représentatives, couvrant à la fois des approches simples et avancées :

    Modèle simple paramétrique : Régression logistique
    Modèles simples non paramétriques : k-plus proches voisins (KNN), Arbre de décision
    Modèle ensembliste parallèle : Random Forest
    Modèle ensembliste séquentiel : LightGBM
    
L'objectif était de comparer différentes philosophies d'apprentissage automatique, en particulier sur une tâche déséquilibrée, et de ne pas me limiter à des modèles complexes sans justification.

### 2. Construction d’une baseline pour chaque modèle

Pour chacun de ces modèles, on a d’abord construit une baseline, c’est-à-dire une version initiale du modèle avec des hyperparamètres par défaut, sans rééquilibrage de la base. Cela nous a permis d’avoir un premier aperçu des performances intrinsèques de chaque algorithme.


### 3. Optimisation systématique de chaque modèle

Après cette étape de baseline, chaque modèle a été entraîné et optimisé selon une procédure rigoureuse :
Utilisation de GridSearchCV pour explorer un espace d’hyperparamètres étendu
Validation croisée stratifiée, pour garantir la robustesse des performances malgré un déséquilibre de classe.
Maximisation du F1-score comme métrique principale, car elle permet de prendre en compte à la fois la précision et le rappel, ce qui est crucial dans un contexte de déséquilibre.

### 4. Stratégies de rééquilibrage des classes

Le jeu de données étant déséquilibré, on a intégré plusieurs stratégies de rééquilibrage dans le processus de modélisation :
SMOTE (Synthetic Minority Oversampling Technique)
Sous-échantillonnage (undersampling)
Pondération des classes
Données originales (sans rééquilibrage)

### 5. Comparaison et sélection du meilleur modèle

Une fois tous les modèles optimisés, nous avons enregistré pour chacun la version entraînée avec ses meilleurs hyperparamètres ainsi que sa meilleure méthode de rééquilibrage, selon le F1-score obtenu en validation croisée.

À l’issue de cette procédure exhaustive, les meilleures performances brutes en termes de F1-score ont été obtenues par les modèles Arbre de Décision et Random Forest, mais uniquement lorsqu’ils étaient entraînés sur la base de données originale non rééquilibrée. Cela peut s'expliquer par le fait que ces modèles, sensibles aux structures des données, peuvent être "surpris" ou faussés par les méthodes de rééchantillonnage qui modifient la distribution initiale des classes.

Cependant, notre objectif étant précisément de rééquilibrer la base de données afin de mieux représenter les classes minoritaires, nous avons fait le choix de privilégier un modèle dont les meilleures performances sont obtenues avec une méthode explicite de rééquilibrage.

C’est pourquoi nous avons retenu comme modèle final le LightGBM combiné avec SMOTE. Bien qu’il arrive en troisième position en termes de F1-score global, c’est le meilleur modèle parmi ceux ayant intégré une stratégie de rééquilibrage efficace, tout en offrant de bonnes performances, une grande rapidité d’entraînement.

Ce choix représente un compromis pertinent entre performance, robustesse face au déséquilibre, et capacité de généralisation.


### Conclusion

Le modèle LightGBM combiné avec la méthode de rééquilibrage SMOTE, optimisé avec les meilleurs hyperparamètres, s’est imposé comme le meilleur compromis entre performance, capacité à gérer le déséquilibre des classes, et puissance de modélisation. Bien qu’il ne soit pas celui qui a obtenu le F1-score le plus élevé en valeur absolue, il est celui qui performe le mieux dans un cadre rigoureusement rééquilibré, ce qui correspond pleinement à nos objectifs.
