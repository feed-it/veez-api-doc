# Produits

> Ce microservice a pour objectif d'importer un référentiel produit dans la base de données du client.

# Table des matières

1. [🛣️ Appel de la route](#appel-route)
2. [📖 Body](#body)
3. [✅ Résumé](#resume)
4. [⁉️ FAQ](#faq)
5. [tableau de référence](#tableau-de-correspondance)

---

# Authentification 🪪

Ce microservice utilise l’authentification **Bearer Token** pour sécuriser les appels API.

[La documentation Authentification se trouve ici](authentification.md)

---

# <a id="appel-route"></a> Appel de la route 🛣️

Pour importer un référentiel, effectuez un appel **HTTP POST** vers l’endpoint dédié en fournissant les données nécessaires.

### url :

> https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/products/products-families

### Headers

```http
Authorization: Bearer <yourTokenHere>
Content-Type: application/json
```

---

# <a id="body"></a> Body 📖

> 💡 Chaque ligne de référentiel produit est insérée sous forme d'objet json.

Le body passé à la requête doit avoir le format qui suit :

```json
[
  {
    "productRef": "TEST000079",
    "family": "PHYTOSANITAIRES",
    "subFamily": "ESPACES VERTS",
    "unit": "LITRE"
  }
]
```

    Si aucune autre prop n'est ajoutée en suivant le tableau de correspondance ci-dessous, toutes les props seront initialisées à 0 par défaut.

---

# ✅ <a id="resume"></a> Résumé

#### 🔐 Sécurité et authentification

- **Authentification en deux étapes** :
  1. **Basic Auth** (envoi du `username` et `password` en Base64).
  2. **Obtention d'un Token JWT** via Keycloak.
- **Utilisation obligatoire du Bearer Token** pour toutes les requêtes après authentification.

#### 📤 Processus d'import des produits

1. **Appel de l'API d'authentification** pour récupérer un **token JWT**.
2. **Envoi d'une requête HTTP POST** à l'endpoint dédié en incluant :
   - Un JSON contenant les détails des ventes.
   - Un **ID unique** pour chaque ligne de vente.
3. **Réponse de l'API** indiquant le succès ou l'échec de l'import.

#### 📌 Format attendu des données

Chaque objet JSON représentant une vente doit contenir les informations suivantes :

- `productRef` : identitfiant du produit.
- `family` : famille de produit.
- `subFamily` : Sous-famille du produit.
- `unit` : Unités (litres, tonnes etc...).

#### 🔄 Gestion des mises à jour

- **Nouvelle reference** : Insertion normale.
- **Mise à jour d'une nouvelle reference** : ajouter une ou plusieurs props suivant le tableau de référence pour updater un rate.

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "message": "Successfully inserted 16462 products families."
}
```

---

# <a id="faq"></a> FAQ ⁉️

<details>
  <summary>❔ <u>Quels props doit contenir mon objet afin d'être valide ?</u></summary>
  
  Chaque objet doit contenir :

- Un **name**
- Un **family**
- Un **subFamily**
- Une **unit**
</details>

---

<details>
	<summary> ❔ <u>Qui dois-je contacter en cas de besoin ?</u></summary>
    L'équipe Feed'it se fera un plaisir de répondre a toutes les questions !

</details>

---

# <a id="tableau-de-correspondance"></a>🌿📈 tableau de correspondance

| prop                             | type                                        |
| -------------------------------- | ------------------------------------------- |
| rateRealCaOad                    | OAD (CHIFFRE D'AFFAIRES)                    |
| rateRealTonFertilizers           | FERTILISANTS (TONNES)                       |
| rateRealMbFertilizers            | FERTILISANTS (MARGE BRUTE)                  |
| rateRealTonAmendments            | AMENDEMENTS (TONNES)                        |
| rateRealMbAmendments             | AMENDEMENTS (MARGE BRUTE)                   |
| rateRealCaHerbi                  | HERBICIDES (CHIFFRE D'AFFAIRES)             |
| rateRealMbHerbi                  | HERBICIDES (MARGE BRUTE)                    |
| rateRealCaFongi                  | FONGICIDES (CHIFFRE D'AFFAIRES)             |
| rateRealMbFongi                  | FONGICIDES (MARGE BRUTE)                    |
| rateRealCaInsect                 | INSECTICIDES (CHIFFRE D'AFFAIRES)           |
| rateRealMbInsect                 | INSECTICIDES (MARGE BRUTE)                  |
| rateRealCaPhyto                  | PHYTOSANITAIRES (CHIFFRE D'AFFAIRES)        |
| rateRealMbPhyto                  | PHYTOSANITAIRES (MARGE BRUTE)               |
| rateRealCaSeedsOleo              | SEMENCES OLÉAGINEUSES (CHIFFRE D'AFFAIRES)  |
| rateRealMbSeedsOleo              | SEMENCES OLÉAGINEUSES (MARGE BRUTE)         |
| rateRealCaSeedsCorn              | SEMENCES MAÏS (CHIFFRE D'AFFAIRES)          |
| rateRealMbSeedsCorn              | SEMENCES MAÏS (MARGE BRUTE)                 |
| rateRealCaSeedsForage            | SEMENCES FOURRAGÈRES (CHIFFRE D'AFFAIRES)   |
| rateRealMbSeedsForage            | SEMENCES FOURRAGÈRES (MARGE BRUTE)          |
| rateRealCaSeedsCer               | SEMENCES CÉRÉALIÈRES (CHIFFRE D'AFFAIRES)   |
| rateRealMbSeedsCer               | SEMENCES CÉRÉALIÈRES (MARGE BRUTE)          |
| rateRealCaOligo                  | OLIGO-ÉLÉMENTS (CHIFFRE D'AFFAIRES)         |
| rateRealMbOligo                  | OLIGO-ÉLÉMENTS (MARGE BRUTE)                |
| rateRealCaSeeds                  | SEMENCES (CHIFFRE D'AFFAIRES)               |
| rateRealMbSeeds                  | SEMENCES (MARGE BRUTE)                      |
| rateRealUnitN                    | ENGRAIS AZOTÉS (UNITÉ)                      |
| rateRealMbN                      | ENGRAIS AZOTÉS (MARGE BRUTE)                |
| rateRealUnitP                    | ENGRAIS PHOSPHORÉS (UNITÉ)                  |
| rateRealMbP                      | ENGRAIS PHOSPHORÉS (MARGE BRUTE)            |
| rateRealUnitK                    | ENGRAIS POTASSIQUES (UNITÉ)                 |
| rateRealMbK                      | ENGRAIS POTASSIQUES (MARGE BRUTE)           |
| rateRealCaInputs                 | INTRANTS (CHIFFRE D'AFFAIRES)               |
| rateRealMbInputs                 | INTRANTS (MARGE BRUTE)                      |
| rateRealTonCollectResale         | COLLECTE REVENTE (TONNES)                   |
| rateRealMbCollectResale          | COLLECTE REVENTE (MARGE BRUTE)              |
| rateRealTonCollectSeeds          | COLLECTE SEMENCES (TONNES)                  |
| rateRealMbCollectSeeds           | COLLECTE SEMENCES (MARGE BRUTE)             |
| rateRealTonCollect               | COLLECTE (TONNES)                           |
| rateRealMbCollect                | COLLECTE (MARGE BRUTE)                      |
| rateRealTonCollectOleo           | COLLECTE OLÉAGINEUX (TONNES)                |
| rateRealMbCollectOleo            | COLLECTE OLÉAGINEUX (MARGE BRUTE)           |
| rateRealTonCollectCorn           | COLLECTE MAÏS (TONNES)                      |
| rateRealMbCollectCorn            | COLLECTE MAÏS (MARGE BRUTE)                 |
| rateRealTonCollectCer            | COLLECTE CÉRÉALES (TONNES)                  |
| rateRealMbCollectCer             | COLLECTE CÉRÉALES (MARGE BRUTE)             |
| rateRealHaCollectSeeds           | COLLECTE SEMENCES (HECTARES)                |
| rateRealCaPal                    | PALISSAGE (CHIFFRE D'AFFAIRES)              |
| rateRealMbPal                    | PALISSAGE (MARGE BRUTE)                     |
| rateRealCaEqts                   | ÉQUIPEMENTS (CHIFFRE D'AFFAIRES)            |
| rateRealMbEqts                   | ÉQUIPEMENTS (MARGE BRUTE)                   |
| rateRealCaEqtsVeges              | ÉQUIPEMENTS LÉGUMES (CHIFFRE D'AFFAIRES)    |
| rateRealMbEqtsVeges              | ÉQUIPEMENTS LÉGUMES (MARGE BRUTE)           |
| rateRealCaEqtsOrchards           | ÉQUIPEMENTS VERGERS (CHIFFRE D'AFFAIRES)    |
| rateRealMbEqtsOrchards           | ÉQUIPEMENTS VERGERS (MARGE BRUTE)           |
| rateRealCaEqtsIrri               | ÉQUIPEMENTS IRRIGATION (CHIFFRE D'AFFAIRES) |
| rateRealMbEqtsIrri               | ÉQUIPEMENTS IRRIGATION (MARGE BRUTE)        |
| rateRealCaEqtsTools              | OUTILLAGE (CHIFFRE D'AFFAIRES)              |
| rateRealMbEqtsTools              | OUTILLAGE (MARGE BRUTE)                     |
| rateRealCaEqtsCattle             | ÉQUIPEMENTS ÉLEVAGE (CHIFFRE D'AFFAIRES)    |
| rateRealMbEqtsCattle             | ÉQUIPEMENTS ÉLEVAGE (MARGE BRUTE)           |
| rateRealTonMineralFeedComplement | ALIMENT MINÉRAL (TONNES)                    |
| rateRealMbMineralFeedComplement  | ALIMENT MINÉRAL (MARGE BRUTE)               |
| rateRealTonFeedAdditive          | ADDITIFS ALIMENTAIRES (TONNES)              |
| rateRealMbFeedAdditive           | ADDITIFS ALIMENTAIRES (MARGE BRUTE)         |
| rateRealMbService                | SERVICES (MARGE BRUTE)                      |
| rateRealCaService                | SERVICES (CHIFFRE D'AFFAIRES)               |
| rateRealTonAnimalFeed            | ALIMENTATION ANIMALE (TONNES)               |
| rateRealMbAnimalFeed             | ALIMENTATION ANIMALE (MARGE BRUTE)          |
| rateRealTonCompleteAnimalFeed    | ALIMENT COMPLET (TONNES)                    |
| rateRealMbCompleteAnimalFeed     | ALIMENT COMPLET (MARGE BRUTE)               |
| rateRealTonBeefFeed              | ALIMENT BOVINS (TONNES)                     |
| rateRealMbBeefFeed               | ALIMENT BOVINS (MARGE BRUTE)                |
| rateRealTonSheepFeed             | ALIMENT OVINS (TONNES)                      |
| rateRealMbSheepFeed              | ALIMENT OVINS (MARGE BRUTE)                 |
| rateRealTonChickenFeed           | ALIMENT VOLAILLES (TONNES)                  |
| rateRealMbChickenFeed            | ALIMENT VOLAILLES (MARGE BRUTE)             |
| rateRealTonRawMaterial           | MATIÈRES PREMIÈRES (TONNES)                 |
| rateRealMbRawMaterial            | MATIÈRES PREMIÈRES (MARGE BRUTE)            |
| rateRealCaFeedComplement         | COMPLÉMENT ALIMENTAIRE (CHIFFRE D'AFFAIRES) |
| rateRealMbFeedComplement         | COMPLÉMENT ALIMENTAIRE (MARGE BRUTE)        |
| rateRealMbPa                     | PA (MARGE BRUTE)                            |
| rateRealMb                       | MARGE BRUTE TOTALE                          |

```txt

 :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
 ::::::::::::::::.       ::::::::::::::::::::::::::::::::::.    ::.....:::::::::::::::::::::::::::::::::
 :::::::::::::::  @@@@@@-:::::::::::::::::::::::::::::::::: @@@ .:.@@@:::.    .:::::::::::::::::::::::::
 ::::::::::::::: @@@   . ::::::::::::::::::::::::::::::::::  @@ .:.:@#.:: %@@%.::    .::::::::::::::::::
 ::::::::::::::: @@  ...:::::::::::::::::::::::::::::::::::  @@ .:..@+.:-.-@@..::.@@@ ::::::::::::::::::
 :::::::::::-.   @@    .::.........::::::......::::::        @@ .:.*@@.::.    .   .@     .::::::::::::::
 :::::::::::- @@@@@@@@@....@@@@@@%..:::.*%@@%%#.:::.  @@@@@@@@@ .::....::.+@@..@@@@@@@@@@.::::::::::::::
 :::::::::::-    @@    ..@@@....#@@..:-##:....##+.. #@@%    @@@ .::::::::. %% .   :@     .::::::::::::::
 ::::::::::::::. @@  ::.@@=......=@#.:*#.......*#. .@@  .::  @@ .::::::::. %% .:. @@. ..::::::::::::::::
 ::::::::::::::: @@  -:.@@@@@@@@@@@@.:*#%@@@@@@%%: :@@  :::  @@ .::::::::. %% .:. @@: ::::::::::::::::::
 ::::::::::::::: @@  ::.@@:..........:*#.........: :@@  ::   @@  ::::::::. %% .:. @@. ::::::::::::::::::
 ::::::::::::::: @@: -:..@@@.......=..+##.......-:. @@:    @@@@  -:::::::. %% .:: @@:     ::::::::::::::
 ::::::::::::::: @@@ -::..@@@@@@@@@@.:..#%%%%%%@%::  @@@@@@@ @@@ ::::::::..@@=.:: #@@@@@@ ::::::::::::::
 :::::::::::::::    .-:::............:::.........:::             -::::::::    .::.        ::::::::::::::
 :::::::::::::::::.::::::::::::::::::::::::::::::::::::...::::::::::::::::::::::::::...:::::::::::::::::
 :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

```
