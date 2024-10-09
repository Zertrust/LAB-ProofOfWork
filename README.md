# Documentation d'utilisation : Application de Preuve de Travail (PoW)

Cette application permet de générer et de vérifier des preuves de travail (PoW) via la ligne de commande. Elle se compose de trois fichiers principaux :
- `pow_generation.rs` : Génère un bloc de preuve de travail en fonction d'une difficulté et d'un motif donné.
- `pow_verification.rs` : Vérifie qu'un bloc de preuve de travail est valide en comparant le nonce, les données, et l'empreinte hash générée.
- `main.rs` : Gère l'interface utilisateur pour miner ou vérifier un bloc de PoW via des arguments de ligne de commande.

## Compilation

Pour compiler l'application, assurez-vous d'avoir installé Rust sur votre machine. Placez tous les fichiers dans un même projet Rust et compilez-le avec `cargo build --release` :

```bash
cargo build --release
```

## Utilisation

L'application peut être exécutée avec deux commandes principales : `mine` pour générer une preuve de travail, et `verify` pour vérifier une preuve de travail.

### 1. Miner un bloc (Preuve de Travail)

La commande `mine` permet de générer un bloc en spécifiant les données à inclure, un motif à répéter pour la difficulté, et un niveau de difficulté (nombre de répétitions du motif).

#### Syntaxe :

```bash
./votre_exécutable mine <data> <pattern> [difficulty]
```

- `<data>` : Données à inclure dans le bloc (ex: "Hello, world!").
- `<pattern>` : Motif utilisé pour déterminer la difficulté (ex: "00").
- `[difficulty]` : Niveau de difficulté, optionnel. Si non fourni, la difficulté par défaut est de 1.

#### Exemple :

```bash
./votre_exécutable mine "Hello, world!" "00" 4
```

Dans cet exemple, l'application va générer un bloc où le hash commence par "0000", car la difficulté est de 4.

#### Résultat :

Si la génération réussit, l'application affiche les informations suivantes :
- Le motif attendu.
- Le niveau de difficulté.
- Le timestamp utilisé pour générer le bloc.
- Le nonce trouvé.
- L'empreinte hash correspondante.
- Le temps pris pour miner le bloc.

### 2. Vérifier une preuve de travail (PoW)

La commande `verify` permet de vérifier si un bloc est valide. Il vous faut fournir le nonce, les données utilisées, le timestamp, et l'empreinte hash attendue.

#### Syntaxe :

```bash
./votre_exécutable verify <nonce> <data> <timestamp> <expected_hash>
```

- `<nonce>` : Nonce trouvé lors du minage.
- `<data>` : Données utilisées lors du minage (doivent correspondre exactement).
- `<timestamp>` : Timestamp utilisé lors du minage.
- `<expected_hash>` : Hash attendu (celui obtenu lors du minage).

#### Exemple :

```bash
./votre_exécutable verify 3361 "Hello, world!" 1620895600 "0000abc123..."
```

Dans cet exemple, l'application va recalculer l'empreinte hash avec le nonce, les données et le timestamp fournis, et vérifier si l'empreinte hash correspond au hash attendu.

#### Résultat :

- Si la preuve de travail est valide, l'application affiche : `PoW valide !`
- Si elle échoue (données incorrectes, nonce invalide, hash expiré), un message d'erreur est affiché expliquant la raison de l'échec.

---

## Détails Techniques

### 1. Fichier `pow_generation.rs`

Ce fichier contient la fonction de génération de preuve de travail `mine_block`. Il utilise l'algorithme SHA-256 pour créer une empreinte à partir de la concaténation des données, du timestamp, et du nonce. Le but est de trouver un nonce tel que le hash généré commence par un certain motif répété, en fonction du niveau de difficulté.

### 2. Fichier `pow_verification.rs`

Ce fichier permet de vérifier une preuve de travail à l'aide de la fonction `verify_pow`. Elle compare l'empreinte hash générée à partir des données, du nonce, et du timestamp fournis avec le hash attendu. De plus, il vérifie si le hash est encore valide en comparant le timestamp actuel avec celui utilisé lors du minage, prenant en compte une durée maximale de validité.

### 3. Fichier `main.rs`

Ce fichier gère les arguments de la ligne de commande pour déterminer si l'utilisateur souhaite miner ou vérifier un bloc. Il récupère les paramètres nécessaires, appelle les fonctions appropriées et affiche les résultats ou les erreurs éventuelles.

---