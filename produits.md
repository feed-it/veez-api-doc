# Produits

> Ce microservice a pour objectif d'importer des ventes dans la base de données du client.

# Table des matières

1. [🔄 Processus d'authentification](#processus-authentification)
2. [1️⃣ Demande de Token](#demande-token)
3. [2️⃣ Utilisation du Token pour les appels API](#utilisation-token)
4. [🛣️ Appel de la route](#appel-route)
5. [📖 Body](#body)
6. [✅ Résumé](#resume)
7. [⁉️ FAQ](#faq)

# Authentification 🪪

Ce microservice utilise l’authentification **Bearer Token** pour sécuriser les appels API.

[La documentation Authentification se trouve ici](authentification.md)

# <a id="appel-route"></a> Appel de la route 🛣️

Pour importer des ventes, effectuez un appel **HTTP POST** vers l’endpoint dédié en fournissant les données nécessaires.

### url :

> https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/products

### Headers

```http
Authorization: Basic <yourTokenHere>
Content-Type: application/json
```

# <a id="body"></a> Body 📖

> 💡 Chaque ligne de vente est insérée sous forme d'objet json. Ainsi chaque ligne doit comporter un ID unique afin de pouvoir updater une vente existante (Id déjà inséré précédemment), ou en insérer une nouvelle (Id encore inconnu).

> Le client devra fournir lui même les ID de chaque ligne de vente à importer.

Le body passé à la requête doit avoir le format qui suit :

```json
[
  {
    "id": "456323789",
    "name": "00/00/15+40cao",
    "family": "SEMENCES ET PLANTS",
    "subFamily": "SEMENCES BLES PRINTEMPS",
    "unit": "Litres"
  },
  {
    "id": "987453",
    "name": "00/00/20+6MGO+27CAO",
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

Chaque objet JSON représentant une vente doit contenir les informations suivantes :

- `id` : identitfiant du produit.
- `name` : nom du produit.
- `family` : famille de produit.
- `subFamily` : Sous-famille du produit.
- `unit` : Unités (litres, tonnes etc...).

#### 🔄 Gestion des mises à jour

- **Nouvelle vente** : fournir un `id` jamais utilisé auparavant.
- **Mise à jour d'une vente existante** : réutiliser un `id` déjà inséré.

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "message": "Successfully inserted 16462 products in 2024."
}
```

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

<details>
	<summary> ❔ <u>Comment savoir si ça a fonctionné ?</u></summary>
	Une réponse sera fournie une fois l'insertion terminée.
<pre>	
{
	"success": true,
	"message": "Successfully inserted 4 new sales,
	"requestedYear": "2025",
	"businessEntity": "FEEDIT"
}
		</pre>
</details>

---

```txt

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: ::::::::::::::::. ::::::::::::::::::::::::::::::::::. ::.....::::::::::::::::::::::::::::::::: ::::::::::::::: @@@@@@-:::::::::::::::::::::::::::::::::: @@@ .:.@@@:::. .::::::::::::::::::::::::: ::::::::::::::: @@@ . :::::::::::::::::::::::::::::::::: @@ .:.:@#.:: %@@%.:: .:::::::::::::::::: ::::::::::::::: @@ ...::::::::::::::::::::::::::::::::::: @@ .:..@+.:-.-@@..::.@@@ :::::::::::::::::: :::::::::::-. @@ .::.........::::::......:::::: @@ .:._@@.::. . .@ .:::::::::::::: :::::::::::- @@@@@@@@@....@@@@@@%..:::._%@@%%#.:::. @@@@@@@@@ .::....::.+@@..@@@@@@@@@@.:::::::::::::: :::::::::::- @@ ..@@@....#@@..:-##:....##+.. #@@% @@@ .::::::::. %% . :@ .:::::::::::::: ::::::::::::::. @@ ::.@@=......=@#.:_#......._#. .@@ .:: @@ .::::::::. %% .:. @@. ..:::::::::::::::: ::::::::::::::: @@ -:.@@@@@@@@@@@@.:_#%@@@@@@%%: :@@ ::: @@ .::::::::. %% .:. @@: :::::::::::::::::: ::::::::::::::: @@ ::.@@:..........:_#.........: :@@ :: @@ ::::::::. %% .:. @@. :::::::::::::::::: ::::::::::::::: @@: -:..@@@.......=..+##.......-:. @@: @@@@ -:::::::. %% .:: @@: :::::::::::::: ::::::::::::::: @@@ -::..@@@@@@@@@@.:..#%%%%%%@%:: @@@@@@@ @@@ ::::::::..@@=.:: #@@@@@@ :::::::::::::: ::::::::::::::: .-:::............:::.........::: -:::::::: .::. :::::::::::::: :::::::::::::::::.::::::::::::::::::::::::::::::::::::...::::::::::::::::::::::::::...::::::::::::::::: :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

```

```

```
