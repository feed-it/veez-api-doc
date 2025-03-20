# Portefeuilles

> Ce microservice a pour objectif de gerer les portefeuilles.

# Table des matières

1. [🛣️ Appel des routes](#appel-route)
2. [📖 Body](#body)
3. [✅ Résumé](#resume)
4. [⁉️ FAQ](#faq)

---

# Authentification 🪪

Ce microservice utilise l’authentification **Bearer Token** pour sécuriser les appels API.

[La documentation Authentification se trouve ici](authentification.md)

---

# <a id="appel-route"></a> Appel des routes 🛣️

## Portfolios

<details>
     <summary> <b>GET</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios</u></em></summary>

- Params :
  - `year` ex : `https://api-9a4b..../portfolios/2025`

> 💡 Retrieve all portfolios.

</details>

---

<details>
     <summary> <b>POST</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/insert/:year</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../portfolios/insert/2025`

- Query :
  - `?mode=insert-only` (only insert new datas).
  - `?mode=flush-tc` (delete already present datas from tc's portfolios before new data insertion).
  - `?mode=flush-all` (delete all datas (for a given year) before new data insertion).

> 💡 Insert a portfolio.

</details>

---

### Headers

```http
Authorization: Basic <yourTokenHere>
Content-Type: application/json
```

---

# <a id="body"></a> Body 📖

Le body passé à la requête doit avoir le format qui suit :

```json
[
  {
    "tc": "Yvan Dugrain",
    "siret": "123456789"
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

#### 📤 Processus d'import des portefeuilles

1. **Appel de l'API d'authentification** pour récupérer un **token JWT**.
2. **Envoi d'une requête HTTP POST** à l'endpoint dédié en incluant :
   - Un JSON contenant les portefeuilles.
3. **Réponse de l'API** indiquant le succès ou l'échec de l'import.

#### 📌 Format attendu des données

Chaque objet JSON représentant un portefeuille doit contenir les informations suivantes :

- `tc` : Nom du Tc.
- `siret` : siret à lui attribuer.

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "message": "Successfully inserted portfolio for Yvan Dugrain"
}
```

---

# <a id="faq"></a> FAQ ⁉️

<details>
    <summary>❔ <u>Quels props doit contenir mon objet afin d'être valide ?</u></summary>
    
    Chaque objet doit contenir :
    
- `tc` : Nom du Tc.
- `siret` : siret à lui attribuer.
</details>

---

<details>
    <summary>❔<u> Qui dois-je contacter en cas de besoin ?</u></summary>
    
    L'équipe Feed'it se fera un plaisir de répondre a toutes les questions !
</details>

---

<details>
<summary>❔ <u>Comment savoir si ça a fonctionné ?</u></summary>
    
    Une réponse sera fournie une fois l'insertion terminée.

```json
{
  "message": "Successfully inserted portfolio for Yvan Dugrain"
}
```

</details>

---

<details>
    <summary>❔ <u>Existe-t-il une route pour retrouver tous les portfolios insérés ?</u></summary>
    
    OUI ! La route =>
    GET https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/:year
</details>

---

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
