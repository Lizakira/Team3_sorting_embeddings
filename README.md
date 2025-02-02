# Clustering et Labélisation des Emails avec LLM Llama 3

## Introduction
Ce projet vise à structurer et analyser un jeu de données d'environ **500 000 emails** provenant du dataset Enron. L'objectif est double :
1. **Regrouper les emails par thématique** (à l'aide de K-Means et HDBSCAN).
2. **Labéliser automatiquement** les emails par **topic principal** et détecter les emails de nature **juridique** à l'aide d'un **LLM (Llama 3)**.

Les images ci-dessous illustrent l'architecture et le pipeline de clustering :
- **Flux du traitement des emails**
 ![image](https://github.com/user-attachments/assets/a44cb133-5c8b-49ae-9f09-0ee3332aba62)

- **Diagramme de l'architecture du clustering et de la labélisation**
  ![image](https://github.com/user-attachments/assets/7b847dec-f75b-4d6f-a953-3d23687d167b)


## 1. Prétraitement des Emails
Les emails ont été filtrés et nettoyés avant le clustering :
- **Suppression des spams** (-30% du volume initial).
- **Suppression des doublons** (-50% du volume restant).
- **Nettoyage des métadonnées et extraction des corps de mails.**

Au final, **150 000 emails ont été retenus** pour le clustering.

## 2. Clustering des Emails
### **2.1 Clustering K-Means (2000 Clusters)**
- Utilisation de **K-Means** pour créer **2000 groupes** (calculé grace à la technique du coude pour un clustering optimal) basés sur la similarité textuelle des emails.
- Chaque cluster regroupe des emails traitant d'un sujet similaire.

### **2.2 Clustering HDBSCAN (329 Clusters)**
- Un second clustering a été appliqué aux centroïdes des clusters K-Means afin de **réduire le bruit et fusionner des clusters proches**.
- HDBSCAN a généré **329 clusters finaux**, représentant des **grands groupes thématiques homogènes**.

## 3. Labélisation des Emails avec LLM
### **3.1 Labélisation des Topics (K-Means Clusters)**
- **Objectif :** Identifier un **thème principal** pour chaque cluster K-Means.
- **Méthode :**
  - Sélection des **10 emails les plus proches du centroïde** de chaque cluster.
  - Envoi de ces **20 000 emails** au **LLM (Llama 3)** avec un **prompt d'analyse thématique**.
  - Attribution du **topic extrait par le LLM** à tous les emails du cluster correspondant.

### **3.2 Classification Juridique des Emails (HDBSCAN Clusters)**
- **Objectif :** Détecter les emails à contenu juridique.
- **Méthode :**
  - Sélection des **20 emails les plus proches du centroïde** de chaque cluster HDBSCAN.
  - Envoi de ces **6 580 emails** au **LLM** avec un **prompt spécifique pour identifier les contenus légaux** (ex : contrats, conformité, litiges).
  - Attribution du label **"juridique / non juridique"** à tous les emails du cluster correspondant.

## 4. Stockage des Résultats
Les emails labélisés ont été sauvegardés au **format Parquet** pour une exploitation optimale.

## 5. Avantages et Limites
### **Avantages :**
- **Automatisation massive** : Identification rapide des sujets et des contenus sensibles.
- **Approche hybride optimisée** : Clustering + LLM permet une analyse plus robuste.
- **Meilleure gestion des risques** : Identification automatique des emails à caractère juridique.

### **Limites et Défis :**
- **Consommation de ressources** : Traitement long et coûteux en puissance de calcul.
- **Sélection d'un sous-ensemble d'emails** : Impossible de passer tous les emails au LLM.
- **Hétérogénéité de certains clusters** : Peut impacter la précision des topics et du label juridique.

## 6. Perspectives d'Amélioration
- **Optimisation du prompt LLM** pour réduire les erreurs de classification.
- **Filtrage des emails analysés** pour une meilleure efficacité.
- **Utilisation d'un LLM open-source en local** pour réduire les coûts d'exécution.


---

Ce README présente l'approche adoptée pour **structurer, classifier et labéliser les emails Enron**. L'utilisation conjointe de **K-Means, HDBSCAN et Llama 3** permet une **classification automatisée et robuste**, rendant l'analyse des emails plus efficace et pertinente.

