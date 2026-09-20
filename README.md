Documentation Technique : Phase I – Étape 1
Projet : MOLakisi AI Tuteur Socratique Edge-AI pour l'EPST en RDC

Auteur et responsable des droits d’auteur/IP : BADPEY BADPEY Alex ( albad-lab )
Contexte de Déploiement : Offline-First, Terminaux Android à ressources contraintes (2 à 4 Go RAM)
Environnement de Travail : WSL2 (Ubuntu) / VS Code — E:/Formation Dev IA ODC/Projets/APV_Molakisi

 Explication Détaillée & Rôle Strategic
La Phase I (Architecture Core, Environnement & Configuration) pose les fondations logicielles, juridiques et structurelles du projet MOLakisi AI.
L'Étape 1 (Initialisation du Projet & Dépendances Négatives) a pour rôle d'isoler l'environnement de développement, d'assurer le découplage strict du code source et de verrouiller le périmètre juridique et technique avant d'intégrer les modules lourds d'IA (poids de modèles SLM quantisés, moteurs RAG vectoriels et wrappers C++ natively linked).

 Objectifs Majeurs de l'Étape 1 :

1.	Définition de l'Identité de l'Application : Structuration du manifeste Expo (app.json) et du point d'entrée universel (index.ts / App.tsx).
2.	Verrouillage de l'Arbre de Dépendances : Prévention absolue des dérives de versions (dependency drift) via un fichier package-lock.json v3 strict.
3.	Garantie de Sécurité & Dépendances Négatives : Exclusion explicite via .gitignore des secrets (.env), environnements virtuels, artefacts de build et fichiers de poids de modèles non versionnés (.bin, .onnx) pour éviter la saturation du dépôt Git.
4.	Sécurisation de la Propriété Intellectuelle : Implémentation d'une licence propriétaire commerciale fermée (Tous Droits Réservés).
5.	Régulation du Compilateur TypeScript : Activation du mode strict ("strict": true) pour garantir l'absence de types implicites (any) lors des échanges de données Edge-AI.


2. Architecture de base et structure arborescente des fichiers (Étape 1)

[ Racine du Projet : molakisi-ai ]
  │
  ├── 📄 app.json              <-- Identifiants uniques (Bundle ID, Scheme) & Métadonnées Expo
  ├── 📄 index.ts              <-- Enregistrement du composant racine dans AppRegistry
  ├── 📄 App.tsx               <-- Composant React Native d'amorçage
  ├── 📄 package.json          <-- Déclarations des dépendances autorisées (TypeScript, Expo, Core UI)
  ├── 📄 package-lock.json     <-- Verrou d'intégrité & arbre de dépendances exact
  ├── 📄 tsconfig.json         <-- Compilateur TypeScript configuré en Mode Strict
  ├── 📄 README.md             <-- Documentation technique et guide d'onboarding
  ├── 🔒 LICENSE               <-- Contrat de Licence Propriétaire Commerciale Fermée (albad-lab)
  ├── 🛑 .gitignore            <-- Filtre des dépendances négatives & exclusions d'artefacts/IA
  │
  └── 📁 src/                  <-- Socle d'architecture modulaire
      └── 📁 types/
          └── 📄 index.ts      <-- Typages globaux stricts (Languages, ExecutionMode, Message)

3. Procédure d'Exécution Pas à Pas (Setup & Configuration)

Étape 1.1 : Initialisation de la structure de l’application Expo / React Native
Génération du squelette applicatif sous React Native / Expo alimenté par le moteur TypeScript :

npx create-expo-app@latest molakisi-ai --template blank-typescript

Étape 1.2 : Installation des Dépendances Core Stabilisées
Alignement des liaisons natives via le gestionnaire Expo pour éviter les ruptures de Native Bridge :

npx expo install react-native-screens react-native-safe-area-context react-native-gesture-handler react-native-reanimated @react-native-async-storage/async-storage react-native-svg lucide-react-native




Étape 1.3 : Configuration du Filtrage & Dépendances Négatives (.gitignore)
Définition des règles d'exclusion pour bloquer les fichiers volumineux, secrets, artefacts natifs et poids de modèles IA : 

# Dépendances & Cache
node_modules/
.expo/
dist/
web-build/
# Environnement & Secrets
*.env
*.env.local

# Fichiers Binaires & Modèles AI (Dépendances Négatives)
*.bin
*.onnx
*.gguf
*.tflite
# Logs & OS
*.log
.DS_Store
Thumbs.db

Étape 1.4 : Mise en Place de la Licence Propriétaire Commerciale
Substitution de la licence Open-Source par défaut par le contrat de Licence Propriétaire Fermée :
Texte brut/Texte non chiffré
Copyright (c) 2026 BADPEY BADPEY Alex (albad-lab). Tous droits réservés.
Le présent logiciel et ses fichiers de code source associés sont la propriété exclusive 
de BADPEY BADPEY Alex. Toute reproduction, distribution, modification ou rétro-ingénierie, 
partielle ou totale, sans autorisation écrite préalable est strictement interdite.

Étape 1.5 : Sécurisation du Compilateur TypeScript (tsconfig.json)
Activation du contrôle strict pour interdire l'usage de types implicites any et forcer la 
vérification des valeurs nulles :

JSON
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}

Étape 1.6 : Typage des Entités Core (src/types/index.ts)

Création du contrat de données principal garantissant l'interopérabilité entre les couches UI, SQLite-VSS et Inférence Edge :
TypeScript
export type Language = 'lingala' | 'swahili' | 'tshiluba' | 'kikongo' | 'french';
export type ExecutionMode = 'edge_offline' | 'cloud_online';
export interface Message {
  id: string;
  sender: 'user' | 'tutor';
  text: string;
  audioUri?: string;
  timestamp: number;
  modeUsed: ExecutionMode;
}

## 4. Directive par Corps de Métier (Guide d'Onboarding)

•	Développeurs Full-Stack / Mobile : Développer uniquement dans le périmètre TypeScript strict. Toute installation de package doit impérativement passer par npx expo install pour garantir la cohérence avec le moteur Metro.
•	Ingénieurs DevOps / Sécurité : Vérifier systématiquement la non-inclusion des modèles d'IA (.onnx, .bin) et des variables d'environnement (.env) dans l'arbre Git (git status). Maintenir l'intégrité de package-lock.json.
•	Data Scientists / AI Engineers : S'assurer que le contrat de données défini dans src/types/index.ts respecte les contraintes de mémoire et de structure de données nécessaires pour l'inférence locale (Sherpa-ONNX / Whisper-Tiny) et le RAG (SQLite-VSS).
•	UI/UX Designers : Exploiter la charte graphique officielle de MOLakisi AI :
o	Vert Émeraude (primary) : #0D9488
o	Bleu Nuit (secondary) : #1E3A8A
o	Jaune Chaleureux (accent) : #F59E0B
gito	Fond Neutre (bgLight) : #F8FAFC


## 5. Matrice de Tests, Vérifications & Assurance Qualité

| Intitulé du Test | Commande / Méthode | Critère de Succès | Statut |
| :--- | :--- | :--- | :---: |
| **Vérification de Compilation TS** | `npx tsc --noEmit` | Aucune erreur de typage relevée (0 errors). | **PASS** |
| **Test d'Amorçage du Serveur** | `npx expo start` | Serveur Metro Bundler fonctionnel sans avertissement. | **PASS** |
| **Audit des Fichiers Exclus (.gitignore)** | `git status` | Exclusion effective de `node_modules/`, `.expo/`, ainsi que des artefacts `.env` / `.onnx`. | **PASS** |
| **Contrôle d'Intégrité de Licence** | Inspection du fichier `LICENSE` | Absence totale de mentions MIT / Open-Source. Présence de la mention propriétaire `albad-lab`. | **PASS** |
| **Vérification des Dépendances Core** | `npm ls react-native-screens` | Arbre de dépendances résolu et verrouillé sans conflits de pairs (*peer dependencies*). | **PASS** |


## 6. Résultats Obtenus & Préparation à l'Étape 2

1.	Environnement Isolé & Stabilisé : Squelette React Native / Expo propre, léger et typé en TypeScript strict.
2.	Arbre de Dépendances Intègre : Fichier package-lock.json v3 opérationnel pour garantir la reproductibilité sur n'importe quel poste de développement.
3.	Sécurisation IP & Confidentialité : Protection juridique commerciale fermée et filtre Git opérationnel contre la fuite de secrets ou de données volumineuses.
4.	Prêt pour la Phase I - Étape 2 : Le socle est parfaitement stabilisé pour accueillir NativeWind v4 (Tailwind CSS), la structure modulaire complète des dossiers /src et l'interface du Tuteur Socratique (SocraticTutorScreen.tsx).
