# Produits

> Ce microservice a pour objectif d'importer des ventes dans la base de données du client.

# Table des matières

1. [🛣️ Appel de la route](#appel-route)
2. [📖 Body](#body)
3. [✅ Résumé](#resume)
4. [⁉️ FAQ](#faq)

---

# Authentification 🪪

Ce microservice utilise l’authentification **Bearer Token** pour sécuriser les appels API.

[La documentation Authentification se trouve ici](authentification.md)

---

# <a id="appel-route"></a> Appel de la route 🛣️

Pour importer des produits, effectuez un appel **HTTP POST** vers l’endpoint dédié en fournissant les données nécessaires.

### url :

> https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/products

### Headers

```http
Authorization: Bearer <yourTokenHere>
Content-Type: application/json
```

---

# <a id="body"></a> Body 📖

> 💡 Chaque ligne de produit est insérée sous forme d'objet json.

Le body passé à la requête doit avoir le format qui suit :

```json
[
  {
    "productRef": "456323789",
    "productName": "00/00/15+40cao",
    "family": "SEMENCES ET PLANTS",
    "subFamily": "SEMENCES BLES PRINTEMPS",
    "unit": "Litres"
  },
  {
    "productRef": "987453",
    "productName": "00/00/20+6MGO+27CAO",
    "family": "ENGRAIS SOLIDES",
    "subFamily": "BULKS ACHETES A LA CARTE",
    "unit": "Tonnes"
  }
]
```

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

Chaque objet JSON représentant un produit doit contenir les informations suivantes :

- `productRef` : identitfiant du produit.
- `productName` : nom du produit.
- `family` : famille de produit.
- `subFamily` : Sous-famille du produit.
- `unit` : Unités (litres, tonnes etc...).

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "message": "Successfully inserted 16462 products in 2024."
}
```

---

# <a id="faq"></a> FAQ ⁉️

<details>
  <summary>❔ <u>Quels props doit contenir mon objet afin d'être valide ?</u></summary>
  
  Chaque objet doit contenir :

- Un **ID** (défini par le client)
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
