# Mini-Site Statique - Pipeline Jenkins CI/CD

## Description
Ce projet est un **mini-site statique** développé pour mettre en pratique les notions de **CI/CD avec Jenkins**.  
Le site contient plusieurs sections, un design moderne et un bouton interactif "Hello World" en JavaScript.  
Il sert d'exemple concret pour illustrer un pipeline complet : récupération du code, build, test et déploiement.

---

## Contenu du projet
- `index.html` : page principale du site avec sections et interactions
- `style.css` : styles et design du site
- `script.js` : interactions JS (bouton "Hello World")
- `images/` : dossier contenant les images utilisées
- `Jenkinsfile` : définition du pipeline Jenkins pour ce projet
- `README.md` : documentation du projet

---

## Pipeline Jenkins
Le pipeline est défini dans le fichier `Jenkinsfile` et comporte les étapes suivantes :

1. **Checkout** : récupération automatique du code depuis GitHub
2. **Build** : préparation du site (possibilité de minifier CSS/JS et optimiser les images)
3. **Test** : vérification que tous les fichiers nécessaires sont présents
4. **Deploy** : copie des fichiers dans `C:\mini-site-deploy` pour un accès local
5. **Post stage** : confirmation de la réussite du pipeline

**Statut du pipeline :** ✅ `SUCCESS`

---

## Déploiement
Pour visualiser le site après le pipeline Jenkins :  
1. Aller dans le dossier : `C:\mini-site-deploy`  
2. Ouvrir le fichier `index.html` dans un navigateur  


---

## Lien GitHub
[https://github.com/dhahri372/mini-site-jenkins](https://github.com/dhahri372/mini-site-jenkins)

---

## Auteur
**Malek Dhahri** – Projet  CI/CD avec Jenkins
