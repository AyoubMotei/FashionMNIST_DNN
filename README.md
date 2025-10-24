# Classification d'Articles de Mode avec un Réseau Neuronal Dense (DNN)

## 1. Introduction du Projet

### Contexte
Ce projet a été réalisé en tant que **Proof of Concept (PoC)** pour une équipe d'e-commerce/mode. L'objectif était de valider un pipeline minimal de Deep Learning (chargement des données, construction de modèle, entraînement, évaluation) capable de classer des articles vestimentaires à partir d'images.

### Objectifs Clés
- Construire un **Réseau Neuronal Dense (DNN)** minimaliste et reproductible
- Atteindre une **Précision (Accuracy) Test > 80%**
- Documenter le pipeline, justifier les choix techniques, et proposer un benchmark SOTA (State-of-the-Art) via Hugging Face

## 2. Organisation du Travail et Planification (Jira Simulation)

Le projet a été planifié sur **5 jours**. Le travail a été structuré en phases logiques pour assurer la traçabilité et le respect des délais.



| ID Tâche | Description de la Tâche | Statut | Estimation (Jours) |
|----------|-------------------------|--------|---------------------|
| **DL-01** | [Data] Chargement, Inspection et Normalisation de Fashion MNIST | ✅ Terminé | 1 |
| **DL-02** | [Model] Construction et Compilation de l'Architecture DNN | ✅ Terminé | 1 |
| **DL-03** | [Train] Entraînement du Modèle avec EarlyStopping | ✅ Terminé | 1 |
| **DL-04** | [Eval] Évaluation finale de la Précision sur l'ensemble de Test | ✅ Terminé | 1 |
| **DL-05** | [Doc] Rédaction de la Synthèse des choix et Benchmark Hugging Face | ✅ Terminé | 1 |



## 3. Pipeline Technique : Chargement et Préparation des Données

Le jeu de données utilisé est **Fashion MNIST**, composé de 60 000 images d'entraînement et 10 000 images de test (28 × 28 pixels en niveaux de gris) réparties en 10 classes d'articles de mode.

### 3.1. Inspection des Données
- **Forme d'Entrée** : (60000, 28, 28) pour l'entraînement
- **Plage de Pixels Initiale** : [0, 255] (valeurs entières)

### 3.2. Normalisation des Pixels (Feature Scaling)

L'étape de normalisation est essentielle pour optimiser l'apprentissage du réseau.

- **Action** : Division de toutes les valeurs de pixels par 255.0
- **Rationale** : Cette mise à l'échelle place toutes les entrées dans l'intervalle [0, 1]. Cela garantit une convergence plus rapide et plus stable de l'optimiseur Adam en évitant des gradients trop importants.

## 4. Architecture, Entraînement et Évaluation du Modèle

### 4.1. Construction du Modèle DNN

Nous avons utilisé une architecture séquentielle simple, comme exigé par le PoC.

| Couche Keras | Sortie (Output Shape) | Fonction d'Activation | Rôle et Justification |
|--------------|----------------------|----------------------|----------------------|
| Flatten | (None, 784) | N/A | Convertit la matrice 2D (28 × 28) en un vecteur 1D pour l'entrée du DNN |
| Dense | (None, 128) | relu | Couche cachée. relu introduit la non-linéarité, permettant d'apprendre des relations complexes dans les données |
| Dense | (None, 10) | softmax | Couche de sortie. Nécessaire pour la classification multi-classes : convertit les 10 logits en une distribution de probabilité |

### 4.2. Compilation et Entraînement

| Paramètre | Valeur Choisie | Justification |
|-----------|----------------|---------------|
| **Optimiseur** | adam | Méthode de descente de gradient adaptive, reconnue pour sa rapidité et son efficacité |
| **Loss** | sparse_categorical_crossentropy | Fonction de coût standard pour la classification multi-classes avec des labels entiers (0 à 9) |
| **Métriques** | accuracy | Métrique principale de surveillance du projet (objectif > 80%) |
| **Callback** | EarlyStopping(monitor='val_loss', patience=5) | Bonus implémenté. Arrête l'entraînement si la perte de validation cesse de s'améliorer pendant 5 époques, prévenant ainsi l'Overfitting (surapprentissage) |

### 4.3. Résultat Final et Performance

| Métrique | Valeur Observée | Statut de l'Objectif |
|----------|-----------------|----------------------|
| **Précision (Accuracy) sur l'Ensemble de Test** | **88.11%** | ✅ **Objectif Atteint (> 80%)** |
| **Perte (Loss) Finale sur l'Ensemble de Test** | 0.35 | Faible, confirme la bonne convergence du modèle |

## 5. Synthèse Professionnelle et Benchmark (Hugging Face)

Le PoC est validé, mais il est crucial de le positionner par rapport aux solutions SOTA pour les prochaines étapes de l'entreprise.

### 5.1. Identification du Service IA Existant

| Critère | Détail |
|---------|--------|
| **Nom du Service IA** | Hugging Face Hub (plateforme) / Vision Transformers (ViT) (architecture SOTA) |
| **Plan Tarifaire (Sommaire)** | Open-Source/Gratuit (modèles téléchargeables). Le coût est principalement celui de l'infrastructure cloud (GPU/TPU) pour l'inférence et le fine-tuning |
| **Avantages SOTA** | Performance supérieure (~99% typique), robustesse sur images réelles (Transfer Learning) |
| **Inconvénients SOTA** | Plus lourd en mémoire, temps d'inférence plus long, nécessite un GPU en production |

### 5.2. Synthèse Comparative (PoC vs. SOTA)

| Caractéristique | Modèle DNN Simple (PoC) | Modèle SOTA (Hugging Face / ViT) |
|-----------------|-------------------------|----------------------------------|
| **Objectif Principal** | Valider le pipeline Data → Métrique | Atteindre la meilleure précision possible |
| **Précision (Fashion MNIST)** | **88.11%** | ~99% |
| **Complexité** | Très Faible (Idéal pour un PoC rapide) | Très Élevée (Nécessite plus de ressources) |

**Recommandation Business** : Le PoC est un succès. Pour passer à une application en production gérant la complexité des photos réelles (couleurs, arrière-plans), l'investissement dans le Transfer Learning (CNN/ViT) est recommandé.

## 6. Conclusion et Étapes Futures

Ce projet a permis de valider un pipeline de Deep Learning fonctionnel et performant. Le modèle DNN a atteint l'objectif de précision avec succès.

### Prochaines Étapes pour l'Équipe :

1. **Migration vers CNN** : Remplacer l'architecture DNN par un Convolutional Neural Network (CNN) pour exploiter la structure spatiale des images et améliorer la précision

2. **Transfer Learning** : Utiliser des modèles pré-entraînés (ex: ResNet) via Hugging Face pour atteindre une performance proche du 100% sur un jeu de données de production

3. **Analyse d'Erreur** : Calculer la Matrice de Confusion pour identifier spécifiquement les classes que le modèle confond.
