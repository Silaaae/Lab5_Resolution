# Lab5_Resolution
Rapport — OWASP UnCrackable Level 2IntroductionCe laboratoire a pour objectif d'analyser une application Android protégée et d'en extraire le secret caché. Contrairement au Level 1, le secret n'est pas stocké dans le code Java mais dans une bibliothèque native compilée libfoo.so, chargée au runtime via JNI.
Étape 1 — Installation et lancement de l'APKL'APK a été installée sur l'émulateur Android via la commande suivante :
adb install UnCrackable-Level2.apkL
application affiche une interface simple composée d'un champ de saisie et d'un bouton de validation.
<img width="162" height="348" alt="image" src="https://github.com/user-attachments/assets/b4b05c25-36fe-45ba-a9da-4b979519c138" />
L'application affiche une interface simple composée d'un champ de saisie et d'un bouton de validation.
Étape 3 et 4 — Analyse statique avec JADX
L'APK a été ouverte dans JADX afin d'analyser le code Java décompilé. La structure suivante a été identifiée :
<img width="1293" height="730" alt="image" src="https://github.com/user-attachments/assets/a01443bf-78fe-4811-9335-b091a70bc52c" />
Dans MainActivity, on observe que la chaîne saisie par l'utilisateur est transmise à une méthode externe pour vérification :
La vérification n'est donc pas effectuée directement dans MainActivity.
<img width="917" height="497" alt="image" src="https://github.com/user-attachments/assets/f74f8379-e4e8-48ab-a0cf-0484d91c5607" />
Étape 5 — Analyse de la classe CodeCheck
La classe CodeCheck contient deux éléments essentiels :
<img width="1012" height="432" alt="image" src="https://github.com/user-attachments/assets/75c539b3-9934-4ff0-9dbe-8bda524ec581" />
Étape 5 — Analyse de la classe CodeCheck
La classe CodeCheck contient deux éléments essentiels :
<img width="1012" height="432" alt="image" src="https://github.com/user-attachments/assets/d836e495-e776-4515-9fc7-dd2cea4a5060" />
 Étape 6 — Extraction de l'APK

L'APK a été extraite comme une archive ZIP :
et j'ai trouve un ensemble de fichier au sein de lib principalement on est interessée par libfoo.so
lib/
    ├── arm64-v8a/libfoo.so
    ├── armeabi-v7a/libfoo.so
    ├── x86/libfoo.so
    └── x86_64/libfoo.so
  <img width="1295" height="736" alt="image" src="https://github.com/user-attachments/assets/e9fa366e-1318-4410-9781-cc0b7bbdd33e" />

La bibliothèque `libfoo.so` a été importée dans Ghidra. L'analyse automatique a été lancée, permettant à Ghidra de décompiler les fonctions, identifier les symboles et produire un pseudo-code lisible.
  <img width="292" height="782" alt="image" src="https://github.com/user-attachments/assets/fd97b41f-a49d-4f79-bce8-9e9ed0b75883" />
<img width="1262" height="568" alt="image" src="https://github.com/user-attachments/assets/e1d8a119-f453-41da-aa81-f84f6ed07e55" />
Étape 9 — Lecture du pseudo-code
Le pseudo-code de la fonction CodeCheck_bar révèle une comparaison avec strncmp :
<img width="1917" height="966" alt="image" src="https://github.com/user-attachments/assets/110da8ae-a82a-4e39-ba92-3f5292a86db7" />
<img width="450" height="491" alt="image" src="https://github.com/user-attachments/assets/31ab17c3-dacc-4a72-a7f8-4ce3727c76c8" />

tape 10 — Valeur hexadécimale

Dans certaines versions du binaire, la chaîne peut être stockée sous forme hexadécimale :

6873696620656874206c6c6120726f6620736b6e616854
donc on a devcode avec python :
<img width="1412" height="362" alt="image" src="https://github.com/user-attachments/assets/9739c0d1-4a7a-44c1-93e4-d48a275fa6a6" />

Ce challenge illustre l'importance de l'analyse du code natif dans les applications Android. Le secret était   invisible depuis JADX seul. L'utilisation de Ghidra a permis de décompiler cette bibliothèque et d'identifier la chaîne secrète Thanks for all the fish.


