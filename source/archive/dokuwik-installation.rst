Dokuwiki Installation
=====================

DokuWiki Basis-Installation
---------------------------

#. DokuWiki runterladen: https://download.dokuwiki.org/

   #. nur die beiden deutschen Sprachen
   #. alle plugins außer "Translation Plugin"

#. DokuWiki auf Webspace hochladen

   #. Achtung: im ersten Ordner befindet sich eine versteckte Datei
      ".htaccess.dist" (zumindest für Linux und Mac Systeme)

#. der Installationsanleitung folgen: https://www.dokuwiki.org/install

   #. "install.php" öffnen (besser gleich per https)
   #. nach erfolgreicher Installation: "install.php" löschen

Schöne URLs konfigurieren
-------------------------

#. siehe auch die Anleitung hier: https://www.dokuwiki.org/rewrite
#. ggf. vorher als Admin einloggen
#. unter dem Menüpunkt "Admin" (oben rechts) auf "Configuration
   Settings" gehen
#. im Abschnitt "Advanced" userewrite (Use nice URLs) auf ".htaccess"
   setzen
#. darunter "useslash" (Use slash as namespace separator in URLs)
   aktivieren
#. speichern (save)
#. die Datei ".htaccess.dist" im Wurzelverzeichnis bearbeiten

   #. die Kommentare unterhalb von "Uncomment these rules if you want to
      have nice URLs using" entfernen
   #. die Kommentare vor den optionalen Optionen nicht entfernen
   #. die Datei von ".htaccess.dist" in ".htaccess" umbenennen

Sprache auf Deutsch stellen
---------------------------

#. unter dem Menüpunkt "Admin" (oben rechts) auf "Configuration
   Settings" gehen
#. im Abschnitt "Basic" die Option "lang" (Interface language) auf
   "de-informal" stellen
#. speichern (save)

Umleitung um immer HTTPS zu nutzen
----------------------------------

in der ".htaccess" Dateil unterhalb von "RewriteEngine on" folgende
Codezeilen ergänzen:

::

   RewriteCond %{HTTPS} off
   RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

Unnötige Plugins löschen
------------------------

Folgende Plugins werden nicht benötigt und können gelöscht werden:

-  Active Directory Auth Plugin
-  LDAP Auth Plugin
-  [DEPRECATED] MYSQL Auth Plugin
-  [DEPRECATED] PostgreSQL Auth Plugin
-  authpdo plugin
-  Popularity Feedback Plugin

Texte des deutschen Sprachepaketes anpassen
-------------------------------------------

-  siehe auch hier:
   https://www.dokuwiki.org/localization#changing_some_localized_texts_and_strings_in_your_installation
-  folgende Datei anlegen: "conf/lang/de-informal/edit.txt"
-  mit folgendem Inhalt füllen:

::

   Nach dem Bearbeiten den **Speichern**-Knopf drücken (siehe auch [[wiki:syntax|Wiki-Syntax]]). Zum Testen bitte erst im [[playground:playground|Spielplatz]] üben.

Kommentar Plugin installieren und konfigurieren
-----------------------------------------------

-  "discussion plugin" installieren:
   https://www.dokuwiki.org/plugin:discussion
-  Haken setzen bei: "plugin»discussion»automatic" - Standardmäßig
   Diskussionen auf allen Seiten
-  bug beheben: action.php im Discussion plugin - Zeile 288 „test“
   entfernen
-  zur Vergrößerung des Abstandes oberhalb des Kommentarbereiches
   folgendes in der Datei "conf/userall.css" einfügen (siehe auch hier:
   https://www.dokuwiki.org/devel:css#user_styles)

.. code:: css

   div.dokuwiki div.comment_wrapper {
       margin-top: 5em;
   }

Weitere Plugins installieren
----------------------------

-  "MathJax plugin" installieren:
   https://www.dokuwiki.org/plugin:mathjax
-  "Dw2Pdf plugin" installieren: https://www.dokuwiki.org/plugin:dw2pdf
   -

   -  hier ist eine manuelle Installation notwendig, da die ZIP Datei zu
      groß ist
   -  ablegen in "lib/plugins/dw2pdf/"

-  "move" installieren: https://www.dokuwiki.org/plugin:move

Noch offen / Todo
-----------------

-  https://www.dokuwiki.org/tips:backup_script
-  umleitung, so dass "www." immer entfernt wird

Liste der Plugins
-----------------

-  Blog - http://www.dokuwiki.org/plugin:blog
-  discussion plugin - https://dokuwiki.org/plugin:discussion
-  Dw2Pdf plugin - https://www.dokuwiki.org/plugin:dw2pdf
-  MathJax plugin - https://www.dokuwiki.org/plugin:mathjax
-  Wrap Plugin - https://www.dokuwiki.org/plugin:wrap
-  Move plugin - http://www.dokuwiki.org/plugin:move
