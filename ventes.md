# Ventes

> Ce microservice a pour objectif d'importer des ventes dans la base de données référentielle du client.

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

Pour importer des ventes, effectuez un appel **HTTP POST** vers l’endpoint dédié en fournissant les données nécessaires.

### url :

> https://api-9a4b7c2d6e1f8g3h0i5j2k7l4m9n6o1p2q.veez.myfeedit.com/sales/:year

### Headers

```http
Authorization: Bearer <yourTokenHere>
Content-Type: application/json
```

---

# <a id="body"></a> Body 📖

Le body passé à la requête doit avoir le format qui suit :

```json
[
  {
    "id": "123456789",
    "codeAdh": "Adh-9876543",
    "quantity": "666",
    "productRef": "00/00/15+40cao"
  },
  {
    "id": "98775654",
    "codeAdh": "Adh-44455",
    "quantity": "774",
    "productRef": "00/10/25+30cad"
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

#### 📤 Processus d'import des ventes

1. **Appel de l'API d'authentification** pour récupérer un **token JWT**.
2. **Envoi d'une requête HTTP POST** à l'endpoint dédié en incluant :
   - Un JSON contenant les détails des ventes.
3. **Réponse de l'API** indiquant le succès ou l'échec de l'import.

#### 📌 Format attendu des données

Chaque objet JSON représentant une vente doit contenir les informations suivantes :

- `id` : id de la vente.
- `codeAdh` : code adhérent du client.
- `quantity` : Quantité vendue.
- `productRef` : Nom du produit concerné.

#### 🔄 Gestion des mises à jour

- **Nouvelle vente** : fournir un `id` jamais utilisé auparavant.
- **Mise à jour d'une vente existante** : réutiliser un `id` déjà inséré.

#### ✅ Exemple de réponse après insertion réussie

```json
{
  "success": true,
  "message": "Successfully inserted 4 new sales into \"sales\" table",
  "requestedYear": "2025",
  "businessEntity": "FEEDIT"
}
```

---

# <a id="faq"></a> FAQ ⁉️

<details>
	<summary>❔ <u>Quels props doit contenir mon objet afin d'être valide ?</u></summary>
	
	Chaque objet doit contenir :
	
- `id` : id de la vente.
- `codeAdh` : code adhérent du client.
- `quantity` : Quantité vendue.
- `productRef` : Nom du produit concerné.
</details>

---

<details>
	<summary> ❔ <u>Comment ajouter une nouvelle vente ?</u></summary>
	
	En donnant à l'objet un ID unique encore jamais inséré au préalable.
</details>

---

<details>
	 <summary>❔ <u>Comment additionner une vente à une existante ?</u></summary>
	
	En donnant à l'objet un ID déjà inséré au préalable.
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
<pre>
{
	"message": "Successfully inserted 7 sales in 2025 for FEEDIT"
}
</pre>
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
