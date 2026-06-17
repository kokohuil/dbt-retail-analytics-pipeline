# 🛒 dbt Retail Analytics Pipeline

Pipeline de données analytiques complet construit avec **dbt + BigQuery**, sur le dataset e-commerce public [Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

Ce projet illustre les compétences d'un **Analytics Engineer** : modélisation de données en couches, définition de KPIs métier, tests de qualité automatisés et exposition de marts analytiques prêts pour Power BI.

---

## 🏗️ Architecture 3 couches

```
raw_olist (BigQuery)
    │
    ▼
[STAGING]        — Nettoyage, typage, renommage des données brutes
    stg_orders · stg_order_items · stg_customers · stg_products · stg_reviews
    │
    ▼
[INTERMEDIATE]   — Jointures, règles de gestion métier, enrichissement
    int_orders_enriched · int_customer_orders · int_product_performance
    │
    ▼
[MARTS]          — KPIs finaux, matérialisés en tables, prêts pour BI
    mart_sales_performance · mart_customer_rfm · mart_product_analytics
```

---

## 📊 Marts disponibles

| Mart | Description | Usage |
|------|-------------|-------|
| `mart_sales_performance` | KPIs mensuels : CA, panier moyen, croissance MoM, livraison, satisfaction | Dashboard Performance |
| `mart_customer_rfm` | Segmentation RFM : Champions, Fidèles, À risque, Perdus | Dashboard Client |
| `mart_product_analytics` | Performance catégories, classification ABC, part de CA | Dashboard Produit |

---

## 🧪 Qualité des données

Chaque modèle est couvert par des tests dbt :
- `unique` et `not_null` sur toutes les clés primaires
- `accepted_values` sur les colonnes de statut et de segmentation
- Tests de cohérence sur les règles de gestion métier

```bash
dbt test                     # Tous les tests
dbt test --select staging    # Tests staging uniquement
dbt test --select marts      # Tests marts uniquement
```

---

## 🛠️ Stack technique

| Outil | Rôle |
|-------|------|
| **dbt-bigquery** | Modélisation, transformation, tests, documentation |
| **BigQuery** | Entrepôt de données cloud (GCP free tier suffisant) |
| **Power BI** | Visualisation (voir repo `powerbi-retail-dashboard`) |
| **Python** | Ingestion des sources (voir repo `bigquery-data-pipeline`) |

---

## 🚀 Installation & lancement

```bash
# 1. Cloner le repo
git clone https://github.com/<votre-username>/dbt-retail-analytics-pipeline.git
cd dbt-retail-analytics-pipeline

# 2. Installer dbt
pip install dbt-bigquery

# 3. Configurer la connexion BigQuery
cp profiles.yml ~/.dbt/profiles.yml
# Éditer avec votre Project ID GCP

# 4. Installer les packages dbt
dbt deps

# 5. Tester la connexion
dbt debug

# 6. Lancer les modèles
dbt run

# 7. Lancer les tests
dbt test

# 8. Générer la documentation interactive
dbt docs generate && dbt docs serve
```

---

## 📁 Structure du projet

```
dbt-retail-analytics-pipeline/
├── models/
│   ├── staging/          # Nettoyage des données brutes (views)
│   ├── intermediate/     # Règles de gestion métier (views)
│   └── marts/            # KPIs finaux exposés au BI (tables)
├── macros/               # Fonctions SQL réutilisables
├── analyses/             # Requêtes ad-hoc documentées
├── tests/                # Tests personnalisés
├── dbt_project.yml       # Configuration principale
├── packages.yml          # Dépendances (dbt_utils)
└── profiles.yml          # Connexion BigQuery (à adapter)
```

---

## 💡 Choix de conception

- **Matérialisation** : `view` pour staging/intermediate (légèreté), `table` pour les marts (performance BI)
- **Segmentation RFM** : `NTILE(4)` pour un scoring robuste et reproductible sur tout volume de données
- **Classification ABC** : règle Pareto appliquée aux catégories produit pour prioriser les actions assortiment
- **Tests qualité** : bloquants en CI/CD, les anomalies n'atteignent jamais les utilisateurs finaux

---

## 📸 Dashboards Power BI

Voir le repo [`powerbi-retail-dashboard`](https://github.com/kokohuil/powerbi-retail-dashboard) pour les visuels connectés à ces marts.

---

## 👤 Auteur

**William KOUKOUI** — Data Analyst / Analytics Engineer
[LinkedIn](https://www.linkedin.com/in/wi-ko-data-analyst-cdi-paris) · [Email](mailto:william.koukoui.ai@gmail.com)
