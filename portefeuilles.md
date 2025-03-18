<img src="../../../assets/images/feeditLogo.png" width="100"> <img src="../../../assets/images/nodejsLogo.svg" width="100" align="right">

# Portefeuilles

> Ce microservice a pour objectif de gerer les portefeuilles.

# Table des matières

1. [🔄 Processus d'authentification](#processus-authentification)
2. [1️⃣ Demande de Token](#demande-token)
3. [2️⃣ Utilisation du Token pour les appels API](#utilisation-token)
4. [🛣️ Appel des routes](#appel-route)
5. [📖 Body](#body)
6. [✅ Résumé](#resume)
7. [⁉️ FAQ](#faq)

# Authentification 🪪

> Ce microservice utilise l’authentification **Bearer Token** pour sécuriser les appels API.

## 🔄 <a id="processus-authentification"></a> Processus d'authentification

1. **Le client envoie une requête avec Basic Auth** (username & password encodés en Base64).
2. **Le serveur valide les identifiants et génère un token d'accès** via Keycloak.
3. **Le serveur renvoie un token JWT** qui devra être utilisé dans les appels suivants.
4. **Le client utilise le token dans l'en-tête Authorization en Bearer Token**.

---

## 1️⃣ <a id="demande-token"></a> Demande de Token

### 📌 URL de l'API d'authentification

```sh
POST https://auth.myfeedit.com/realms/veez/protocol/openid-connect/token
```

### 🏷️ En-têtes requis

```http
Content-Type: application/x-www-form-urlencoded
Authorization: Basic <base64(username:password)>
```

### 📥 Corps de la requête (`x-www-form-urlencoded`)

```sh
client_id=<KC_CLIENT_ID>&
grant_type=password&
username=<VOTRE_USERNAME>&
password=<VOTRE_PASSWORD>&
scope=openid email profile&
client_secret=<KC_CLIENT_SECRET>
```

> Le client ID et client Secret doit être fourni par **Feed'it**.

### 💡 Exemple avec `curl`

```sh
curl -X POST "https://auth.myfeedit.com/realms/veez/protocol/openid-connect/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "client_id=my_client_id&grant_type=password&username=raymond.bechard&password=password123&scope=openid email profile&client_secret=my_client_secret"
```

### 📤 Réponse attendue (`JSON`)

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI...",
  "expires_in": 3600,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cC...",
  "token_type": "Bearer"
}
```

---

## 2️⃣ <a id="utilisation-token"></a> Utilisation du Token pour les appels API

Une fois le **access_token** obtenu, toutes les requêtes à l'API doivent inclure cet access token dans l'en-tête **Authorization** en mode Bearer Token.

### 📌 URL de l'API d'import de ventes

```sh
POST https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/products
```

### 🏷️ En-têtes requis

```http
Authorization: Bearer <access_token>
Content-Type: application/json
```

### 💡 Exemple avec `curl`

```sh
curl -X POST "https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/products" \
     -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
     -H "Content-Type: application/json" \
     -d '[
            {
                "id": "123456789",
                "name": "00/00/15+40cao",
                "family": "ENGRAIS SOLIDES",
                "sub-family": "BULKS ACHETES A LA CARTE",
                "unit": "Litre"
            }
        ]
```

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

<details>
     <summary> <b>POST</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/save/:year</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../portfolios/insert/2025`

- Query :
  - `?tc=Yvan Dugrain`.

> 💡 Save a portfolio's simulation for a given tc name.

</details>

---

<!--
## Simulations

<details>
     <summary> <b>GET</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/my-simulations/:year</u></em></summary>

- Params :
  - `year` ex : `https://api-9a4b..../my-simulations/2025`

> 💡 Retrieve all simulations.

</details>

---

<details>
     <summary> <b>POST</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/my-simulations/:year</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`

> 💡 Insert a Simulation.

</details>

---

<details>
     <summary> <b>PUT</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/my-simulations/:year/:simulationId</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `simulationId` ex : `https://api-9a4b..../my-simulations/543548663214`

> 💡 Update a simulation.

</details>

---

<details>
     <summary> <b>DELETE</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/my-simulations/:year/:simulationId</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `simulationId` ex : `https://api-9a4b..../my-simulations/543548663214`

> 💡 Delete a simulation.

</details>

---

<details>
     <summary> <b>DELETE</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/my-simulations-contents/:year/:simulationId</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `simulationId` ex : `https://api-9a4b..../my-simulations/543548663214`

> 💡 Delete content of a simulation.

</details>

---

##### (share)

<details>
     <summary> <b>GET</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/simulations-shared-with-me/:year</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`

> 💡 Get a simulation that has been shared with me.

</details>

---

<details>
     <summary> <b>PUT</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/share-simulation-with-my-team/:year/:id</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `id` (simulationId) ex : `https://api-9a4b..../my-simulations/2025/654654`

> 💡 Share a simulation with my team.

</details>

---

<details>
     <summary> <b>PUT</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/share-simulation-with-my-manager/:year/:id</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `id` (simulationId) ex : `https://api-9a4b..../my-simulations/2025/654654`

> 💡 Share a simulation with my manager.

</details>

---

##### (unshare)

<details>
     <summary> <b>PUT</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/unshare-simulation-with-my-team/:year/:id</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `id` (simulationId) ex : `https://api-9a4b..../my-simulations/2025/654654`

> 💡 Unshare a simulation with my team.

</details>

---

<details>
     <summary> <b>PUT</b> => <em><u>https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/portfolios/unshare-simulation-with-my-manager/:year/:id</u></em></summary>

- Params :

  - `year` ex : `https://api-9a4b..../my-simulations/2025`
  - `id` (simulationId) ex : `https://api-9a4b..../my-simulations/2025/654654`

> 💡 Unshare a simulation with my manager.

</details> -->

---

### Headers

```http
Authorization: Basic <yourTokenHere>
Content-Type: application/json
```

# <a id="body"></a> Body 📖

> 💡 Chaque produit contient la prop rateMb qui sera le taux de marge, "activity" permettra de définir ce qu'apporte ou contient le produit.

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

### ✅ <a id="resume"></a> Résumé

#### 🔐 Sécurité et authentification

- **Authentification en deux étapes** :
  1. **Basic Auth** (envoi du `username` et `password` en Base64).
  2. **Obtention d'un Token JWT** via Keycloak.
- **Utilisation obligatoire du Bearer Token** pour toutes les requêtes après authentification.

#### 📤 Processus d'import des ventes

1. **Appel de l'API d'authentification** pour récupérer un **token JWT**.
2. **Envoi d'une requête HTTP POST** à l'endpoint dédié en incluant :
   - Un JSON contenant les détails des produits.
3. **Réponse de l'API** indiquant le succès ou l'échec de l'import.

#### 📌 Format attendu des données

Chaque objet JSON représentant une vente doit contenir les informations suivantes :

- `tc` : Nom du Tc.
- `siret` : siret à lui attribuer.

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "message": "Successfully inserted portfolio for Yvan Dugrain"
}
```

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
