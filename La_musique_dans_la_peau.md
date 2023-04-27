# Projets : La musique dans la peau 🎸🎸

## Description

L'objectif est de récupérer une base de données existante et d'exécuter des requêtes SQL afin de pouvoir répondre à un exercice. Ici figureront les requêtes nécessaires pour y parvenir.

## Sources

- Base de donnée : https://drive.google.com/file/d/1XG-s84jEJINSnLUgw5TYBCFsuyUX_Ll4/view?pli=1
- Logiciel SQL : SQLite

## Exercice

_Rédige les requêtes SQL permettant d'obtenir les informations ci-dessous. Consigne importante : la requête doit se faire en une seule ligne de SQL et ne doit pas s'appuyer sur d'autres requêtes (notamment pas les requêtes précédentes)._

- **Récupérer tous les albums**
  _SELECT _ FROM albums* : pour avoir la table album
  *SELECT Title FROM albums\* : pour avoir uniquement la colonne des noms d'album

- **Récupérer tous les albums dont le titre contient "Great"**
  _SELECT _ FROM albums WHERE Title LIKE '%Great%'* : pour avoir la liste des album contenant "Great" et "Greatest"
  *SELECT _ FROM albums WHERE (Title LIKE '%Great%') AND (Title NOT LIKE '%Greatest%')_ : pour avoir la liste des album contenant uniquement "Great"

- **Donner le nombre total d'albums contenus dans la base (sans regarder les id bien sûr)**
  _SELECT count(Title) FROM albums_

- **Supprimer tous les albums dont le nom contient "music"**
  _DELETE FROM albums WHERE Title LIKE '%music%'_ : supprime de façon définitive dans la BDD

Cependant, il est nécessaire de requêter _PRAGMA foreign_keys = OFF;_ auparavant pour éviter l'erreur _"FOREIGN KEY constraint failed_ . Cette erreur est une sorte de sécurité qui permet de protéger les données qui peuvent être appelées dans une table liée. Il faut requêter _PRAGMA foreign_keys = ON;_ pour la rétablir

- **Récupérer tous les albums écrits par AC/DC**
  _SELECT DISTINCT Title FROM albums NATURAL JOIN tracks WHERE Composer = 'AC/DC'_ : l'info du composer se situe dans la table tracks qu'il faut join avec album. Il faut utiliser _DISTINCT_ pour qu'une seule valeur d'album soit sortie

- **Récupérer tous les titres des albums de AC/DC**
  _SELECT Title FROM albums NATURAL JOIN artists WHERE Name = 'AC/DC'_

- **Récupérer la liste des titres de l'album "Let There Be Rock"**
  _SELECT Name FROM tracks NATURAL JOIN albums WHERE Title = 'Let There Be Rock'_

- **Afficher le prix de cet album ainsi que sa durée totale**
  _SELECT SUM(UnitPrice) AS 'Album cost' FROM albums NATURAL JOIN tracks WHERE Title = 'Let There Be Rock'_ : utilisation de la fonction SUM() pour calculer le cout de l'album en additionnant le cout unitaire des morceaux.

- **Afficher le coût de l'intégralité de la discographie de "Deep Purple"**
  _SELECT SUM(t.UnitPrice) AS 'Discography cost' FROM albums al, tracks t, artists ar WHERE t.AlbumId = al.AlbumId AND al.ArtistId = ar.ArtistId AND ar.Name = 'Deep Purple'_

On utilise une table d'association et 2 jointuures pour parvenir au résultat souhaité. Sur plusieurs lignescela donne :

SELECT SUM(t.UnitPrice) AS 'Discography cost'
FROM albums al,
tracks t,
artists ar
WHERE t.AlbumId = al.AlbumId
AND al.ArtistId = ar.ArtistId
AND ar.Name = 'Deep Purple'

- **Créer l'album de ton artiste favori en base, en renseignant correctement les trois tables albums, artists et tracks**
  INSERT INTO artists (Name) VALUES('Kasabian');
  INSERT INTO albums (Title, ArtistId) VALUES ('Kasabian', '276');
  INSERT INTO tracks (Name, AlbumId, MediaTypeId, GenreId, Composer, Milliseconds, Bytes, UnitPrice)
  VALUES ('Club Foot', '348', '1', '4', 'Kasabian', '214000', '500', '0.99'),
  ('Processed Beats', '348', '1', '4', 'Kasabian', '187000', '500', '0.99'),
  ('Reason Is Treason', '348', '1', '4', 'Kasabian', '275000', '500', '0.99');

etc pour les 9 autres titres 