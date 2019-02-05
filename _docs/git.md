---
permalink:      /docs/git/
title:          "Git"
author:         Diego
author_profile: true
comments:       true
---
{% include toc %}

Diese Dokumentation gibt einen Überblick über Git, insbesondere wie man es zur Verwaltung dieser CanSat-Webseite einsetzt.

## Geforktes Repository updaten

### Das Problem

Warum Forks updaten? Die CanSat-Webseite leitet sich vom Theme [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) ab. Ab dem Zeitpunkt des Forks gehen das Original-Repo und das PosiTUne-Repo getrennte Wege. Während wir unsere Seite hauptsächlich mit Inhalt füllen, werden im Ursprungs-Repo Bugs gefixt und neue Funktionalitäten eingeführt. Beide Repos driften also immer mehr auseinander.

<figure>
    <a href="/assets/images/docs/git/github_branch_ahead+behind.png"><img src="/assets/images/docs/git/github_branch_ahead+behind.png" alt="Screenshot eines GitHub-Repos mit dem Hinweis 'This branch is 1 commit ahead, 3 commits behind PosiTUne:master.'"></a>
    <figcaption>Beispiel für einen Fork, der nicht mehr mit dem Original-Repo übereinstimmt. <i>(<a href="https://creativecommons.org/licenses/by/4.0/">CC-BY 4.0</a>)</i>.</figcaption>
</figure>

Das muss kein Problem sein, denn in aller Regel wird unsere Webseite weiter funktionieren, nur kriegen wir nicht automatisch alle Commits und damit Verbesserungen des Ursprungs-Repos eingespielt.

Es gibt im Fall der CanSat-Webseite zwei Fork-Szenarios:

1. Remote: [Minimal-Mistakes-Theme](https://github.com/mmistakes/minimal-mistakes), Lokal: [PosiTUne-Repo](https://github.com/PosiTUne/positune.github.io)
2. Remote: [PosiTUne-Repo](https://github.com/PosiTUne/positune.github.io), Lokal: Nutzer-Fork, z. B. [das von jdieg0](https://github.com/jdieg0/positune.github.io)

Im ersten Fall wurde wie oben beschrieben das Original-Jekyll-Theme geforkt. Hin und wieder sollte die lokale PosiTUne-Instanz geupdatet werden. Das Remote-Repo eines aktiven Projektes wie des Minimal-Mistakes-Themes hat nach ein paar Monaten häufig mehrere hundert Commits hinter sich und es ist nicht unwahrscheinlich, dass beim Zusammenführen beider Repos Konflikte auftreten. Das kann richtig mühsam und zeitaufwendig sein. Auch weil man direkt an der Webseite herumdoktert, sollte man schon genau wissen, was man hier tut.

Weitaus häufiger und in der Regel weniger komplex ist das zweite Szenario. Ihr habt einen persönlichen Fork auf eurer Nutzerseite und arbeitet dort an der Webseite. Mit Pull-Requests schlagt ihr Änderungen an der Webseite vor. In dem Moment, in dem ein Pull-Request für die PosiTUne-Seite angenommen wird, das kann euer eigener oder eine Änderung einer anderen Person sein, driftet auch hier wieder der Stand zwischen dem Remote-Repo und eurem lokalen auseinander. Bevor ihr weiter an euren Pull-Requests arbeitet, solltet ihr mit einem sogenannten Rebase einen Abgleich zwischen beiden Repos durchführen.

### Workflow

Um einen Fork eines Repos auf der eigenen Nutzerseite auf GitHub zu updaten, kann ein *Rebase* durchgeführt werden. Der typische Workflow hierfür wird bspw. [hier auf Stackoverflow](https://stackoverflow.com/a/7244456/7192373) beschrieben. Man bringt durch ein Rebase das eigene, lokale, geforkte Repo auf den aktuellen Stand eines Remote-Repos.

Zuerst wird das Remote-Repo hinzugefügt und ihm der Name "upstream" gegeben.

	git remote add upstream https://github.com/PosiTUne/positune.github.io.git

Anschließend holt man sich alle Branches des Remote-Repos, vor allem ```upstream/master``` ist meist von Interesse.

	git fetch upstream

Jetzt noch kurz sicherstellen, dass wir im lokalen Repo gerade im ```master```-Branch sind.

	git checkout master

Und noch den Rebase durchführen, um den lokalen ```master```-Branch auf den Stand des Upstream-Branches zu kriegen.

	git rebase upstream/master

Zum Schluss muss noch die lokale Änderung zum entfernten Repository auf GitHub transferiert werden (das Flag ```-f``` muss nur das erste Mal nach dem Rebase gesetzt werden).

	git push -f origin master


[comment]: # (### Konflikte)

[comment]: # (## Weiterführende Links)


