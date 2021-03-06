---
marp: true
theme: default
footer: © 2020 BIT ADDICT
style: | 
  section {
      @extend .markdown-body;
      display: block;
      flex-direction: column;
      font-size: 32px
  }

  @import '../css/lecture_slides.css'

---

<div class="title-page">

# Programmering i team
</div>

---

<div class="title-page">

## Versionshantering med Git
</div>

---

## Gemensamt kodbas

- Ett team av utvecklare bidrar till en gemensam kodbas i ett källdkodsarkiv
- Inom versionhantering kallas källkodsarkiv för ***repository*** eller kortare  ***repo***
- Varje utvecklare arbetar lokalt i en klon av det gemensamma repot
- I klonen är det gemensamma repot en *remote* som heter  ``origin`` 

<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%; margin-top: 0.75em">

![repos](fig/repos.dot.svg)

</div>

</div>

---

## Kloning av repo

- En lokal klon av ett repo kan skapas med Git-kommandot ``clone``

```sh
> git clone <url>
```

---

## Commit = ögonblicksbild av källkodsarkiv

- En *commit* är en ögonblicksbild av katalogerna och filerna i repot
- Med varje commit lagras:
  - Ett meddelande som dokumenterar:
    - Innehåll/syfte med förändringen i kodbasen
    - Författaren till förändringen
  - En unik identifierare för commiten
- En commit kan också ha en (eller flera) föregående commits

---

## Identifierare för commit
- I Git är identifierare för en commit en SHA-1 kontrollsumma
- T.ex. ``70460b4b4aece5915caf5c68d12f560a9fe3e4``
- Kan refereras i kortform: ``70460b4``

--- 

## Historik

Ett repo lagrar alla historiska commits och deras relation. 

<br/>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 110%">

![history](fig/history.dot.svg)

</div>

<br/>

--- 

## Återställning av tidigare commit

- Ett lokalt repo kan återställas till en tidigare commit med ``git checkout``:

    ```sh
    > git checkout 0428486
    ```

- Den aktuellt utcheckade commiten har alias ``HEAD``
- Typiskt checkas commits ut via ett branchnamn, t.ex. 

  ```sh
  > git checkout master
  ```
  
- Historiken för utcheckad commit kan visas med kommandot ``git log``
---

## Status på filer i lokalt repo

- Filer i ett repo kan ha fyra olika tillstånd.
- Filer och kataloger som inte har status unmodified listas med: ``git status``


<div style="display: flex; flex-direction: column; align-items: center; zoom: 140%">

![file status](fig/file-status.dot.svg)

</div>

---

## Filen ``.gitignore``

- Det finns som regel lokala filer i repot som inte skall versionshanteras, t.ex.
  - Filer kompilerade från källkoden som ``.exe`` och ``.dll``.
  - Thumbnail-filer som operativsystemet skapar. 
  - Lokala inställningsfiler.
- Filen ``.gitignore`` i repot används för att utesluta ointressanta filer t.ex. i utskriften från ``git status`` 
  - Innehåller sökvägsmönster för filer som skall ignoreras av Git
  - Skall ligga på topnivå i repot

---

## Arbetsflöde

- Checka ut önskad branch
- Redigera filerna
- Arrangera ny commit med ``git add``
- Verkställ commit 

---

## Skapa ny commit 

- En commit görs med ``git commit``
- Utcheckad branch flyttas automatiskt fram till den nya comitten. 

<br/>
<center>
Före commit
</center>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 110%">

![history](fig/history.dot.svg)

</div>
<center>
Efter commit
</center>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 110%">

![history](fig/commit.dot.svg)

</div>

---

## Branch

- Från en given commit kan utvecklingen dela upp sig i flera spår
- I Git kallas ett utvecklingsspår för ***branch***
- En branch är ett alias för sista commiten i ett utvecklingsspår

<br/>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![branch](fig/branch.dot.svg)

</div>

---

## Skapa en ny branch 

- För att skapa en ny branch, checka först ut utgångscommiten för branchen
- För att t.ex. skapa en ny branch ``bug-fix`` ge sedan kommandot:

  ```sh
  > git checkout -b bug-fix
  ```
- För att lägga till den nya branchen i en remote, t.ex. ``origin`` används:

  ```sh
  > git push -u origin bug-fix
  ```

---

## Merge

- För att återföra arbetet t.ex. på branchen ``bug-fix`` till ``master`` används:

  ```sh
  > git merge bug-fix
  ```

  i repot med branchen ``master`` utcheckad.
- Skapar en ny commit med alla ändringar från ``bug-fix`` och ``master``

<br/>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![merge](fig/merge.dot.svg)

</div>

--- 

## Konflikter

- Om samma filrader har ändrats i två mergade branscher uppstår en **konflikt**
- Mergen pausas med konfliktmarkörer placeras i berörda filer

```text
<<<<<<< HEAD:file.txt
Hello world
=======
Goodbye
>>>>>>> 77976da:file.txt
```

---

## Konfliktlösning

- Utvecklaren måste manuellt lösa alla konflikter:
  - Ersätta texten inom konfliktmarkörerna med önskat innehåll
  - Avlägsna konfliktmarkörerna
  - Köra ``git add`` på filen för att markera att konflikten är löst
- När alla konflikter är lösta kör ``git commit`` för att fullfölja mergning
- Branchen ``bug-fix`` har efter merge fyllt sitt syfte och kan tas bort med: 

  ```sh 
  git branch -d bug-fix
  ```

---

## Resultat efter merge

<br/>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![after merge](fig/after-merge.dot.svg)

</div>

---

## Lokalt vs. remote

- Git lagrar alla commits, även unika för remotes och i egna repot
- Hämta nya commits och uppdaterade brancher på remote med: ``git fetch``
- Samma branch kan peka till olika commits lokalt och på remote

<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![remote branch](fig/remote-branch.dot.svg)

</div>

---

## Merge med remote

Ändringar på ``origin/master`` sammanförs med ändringar på ``master`` genom:

```sh
> git merge origin/master
```

med den lokala ``master``-branschen utcheckad. 

<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![remote merge](fig/remote-merge.dot.svg)

</div>

---

## Uppdatera remote med lokala ändringar

- Nu är finns en vägen från ``origin/master`` till ``master`` beskriven lokalt
- Kommandot ``git push`` med ``master`` utcheckad:
  - För över nya commits till remote
  - Flyttar fram ``origin/master`` till samma commit som ``master``

---

## Efter push

Efter ``git push`` är historiken för branchen identisk lokalt och på remote.

<br/>
<div style="display: flex; flex-direction: column; align-items: center; zoom: 150%">

![end result](fig/end-result.dot.svg)

</div>

 

