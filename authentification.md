# Authentification

> Tous les microservices Veez utilisent une authentification de type **Bearer Token** pour sécuriser les appels API.
> Cette page vous explique comment récupérer votre Bearer Token pour ensuite interagir avec l'API Veez.

# Table des matières

- [🔄 Processus d'authentification](#processus-authentification)
- [1️⃣ Demande de Token](#demande-token)
- [2️⃣ Utilisation du Token pour les appels API](#utilisation-token)

---

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

### 📌 Exemple avec l'url de l'API d'import des ventes

```sh
POST https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/sales/:year
```

### 🏷️ En-têtes requis

```http
Authorization: Bearer <access_token>
Content-Type: application/json
```

### 💡 Exemple avec `curl`

```sh
curl -X POST "https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/sales/2025" \
     -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
     -H "Content-Type: application/json" \
     -d '[
  			{
				"id": "123456789",
				"codeAdh": "Adh-9876543",
				"quantity": "666",
				"productRef": "00/00/15+40cao",
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
