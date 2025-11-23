Pour écrire les pages 
cd /Users/destri_c/Documents/doc_DDC/doc_Don_corps

git add --all pour ajouter tous les fichers au commit
git commit -m "added _README.txt3" : crée un commit added _README.txt3
git push main main : synchronise les deux reposit



pour lancer l'environnement virtuel 
cd /Users/destri_c/Documents/doc_DDC/doc_Don_corps
source ../venv_mkdocs/bin/activate.csh

Pour en sortir : deactivate

dans l'environnemnt virtuel 
- Pour généréer les pages à la volée : ../venv_mkdocs/bin/mkdocs serve

- Pour publier les pages sur git hub : ../venv_mkdocs/bin/mkdocs gh-deploy -r main

