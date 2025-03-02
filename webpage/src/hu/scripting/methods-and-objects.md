# Módszerek és objektumok, amelyeket a QOwnNotes biztosít

Külső program indítása a háttérben
----------------------------------------------


### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló egy leválasztott folyamat elindításához
  *
  * @param végrehajthatóPath a futtatható fájl elérési útja
  * @param paraméterek a paraméterláncok listája
  * @param callbackIdentifier az onDetachedProcessCallback () függvényben használandó azonosító (opcionális)
  * @param callbackParameter egy további paraméter a hurkokhoz vagy hasonlókhoz (opcionális)
  * @param processA visszahívás használata esetén a folyamatba írt adatok (opcionális)
  * @retret true a sikerre, hamis egyébként
  */
bool startDetachedProcess (QString végrehajtható útvonal, QStringList paraméterek,
                             QString callbackIdentifier, QVariant callbackParameter,
                             QByteArray processData);
```

### Példa

Egyszerű példa:

```js
script.startDetachedProcess("/path/to/my/program", ["my parameter"]);
```

Sok folyamat futtatása:

```js
for (var i = 0; i < 100; i++) {
    var dur = Math.floor(Math.random() * 10) + 1;
    script.startDetachedProcess("sleep", [`${dur}s`], "my-callback", i);
}

function onDetachedProcessCallback(callbackIdentifier, resultSet, cmd, thread) {
    if (callbackIdentifier == "my-callback") {
        script.log(`#${thread[1]} i[${thread[0]}] t${cmd[1]}`);
    }
}
```

Érdemes megnézni a példát [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml), [callback.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/callback.qml) or [execute-command-after-note-update.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/execute-command-after-note-update.qml).

Érdemes egy pillantást vetni az [onDetachedProcessCallback](hooks.html#ondetachedprocesscallback) kampóra is.

::: tip Helyi és globális parancsikonokat is rendelhet az egyéni műveletekhez a *Shortcuts settings* részben. :::

Indítson egy külső programot, és várja meg a kimenetet
----------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló a szinkron folyamat elindításához
  *
  * @param végrehajthatóPath a futtatható fájl elérési útja
  * @param paraméterek a paraméterláncok listája
  * @param adatok a folyamatba írandó adatok (nem kötelező)
  * @ visszaadja a folyamat által visszaadott szöveget
QByteArray startSynchronousProcess(QString executablePath, QStringList parameters, QByteArray data);
```

### Példa
```js
var result = script.startSynchronousProcess("/path/to/my/program", ["my parameter"], "data");
```

Érdemes megnézni a példát [encryption-keybase.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/encryption-keybase.qml).

Az aktuális jegyzet mappa elérési útjának lekérése
-------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló az aktuális jegyzetmappa elérési útjának megszerzéséhez
  *
  * @ visszaállítja az aktuális jegyzet mappa elérési útját
  */
QString currentNoteFolderPath();
```

### Példa
```js
var path = script.currentNoteFolderPath();
```

Érdemes megnézni a példát [abszolút-media-links.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/absolute-media-links.qml).

Az aktuális jegyzet megszerzése
------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló az aktuális jegyzet megszerzéséhez
  *
  * @returns {NoteApi} az aktuális jegyzetobjektumot
  */
NoteApi currentNote ();
```

### Példa
```js
var note = script.currentNote();
```

Érdemes megnéznie az [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml) példát.

Naplózás a napló widgetbe
-------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló a napló widgetbe való bejelentkezéshez
  *
  * @param szöveg
  */
void log(QString text);
```

### Példa
```js
script.log("my text");
```

URL letöltése egy karakterláncra
------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Wrapper QML pour télécharger une URL et la renvoyer sous forme de texte
  *
  * @param url
  * @return {QString} le contenu de l'url téléchargée
  */
QString downloadUrlToString (QUrl url);
```

### Példa
```js
var html = script.downloadUrlToString("https://www.qownnotes.org");
```

Érdemes megnézni a példát [insert-headline-with-link-from-github-url.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/insert-headline-with-link-from-github-url.qml).

URL letöltése a média mappába
--------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló egy URL letöltésére a média mappába és a média visszaküldése
  * URL vagy az adathordozó jelölési képszövege az aktuális jegyzethez képest
  *
  * @param {QString} URL
  * @param {bool} returnUrlCsak akkor, ha igaz, csak a média URL kerül visszaadásra (alapértelmezett hamis)
  * @return {QString} a média jelölése vagy URL-je
  */
QString downloadUrlToMedia(QUrl url, bool returnUrlOnly);
```

### Példa
```js
var markdown = script.downloadUrlToMedia("http://latex.codecogs.com/gif.latex?\frac{1}{1+sin(x)}");
```

Érdemes megnézni a példát [paste-latex-image.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/paste-latex-image.qml).

Inserting a media file into the media folder
--------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * QML-csomagoló médiafájl beszúrásához a média mappába és visszatérés
  * az adathordozó URL-je vagy a hordozó jelölési képszövege az aktuális jegyzethez képest
  *
  * @param {QString} mediaFilePath
  * @param {bool} returnUrlCsak akkor, ha igaz, csak a média URL kerül visszaadásra (alapértelmezett hamis)
  * @return {QString} a média jelölése vagy URL-je
  */
QString ScriptingService :: insertMediaFile (QString mediaFilePath,
                                         bool returnUrlOnly);
```

### Példa
```js
var markdown = script.insertMediaFile("/path/to/your/image.png");
```

Érdemes megnézni a példát [scribble.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/scribble.qml).

A jegyzet előnézetének regenerálása
-----------------------------

Frissíti a jegyzet előnézetét.

### Módszerhívás és paraméterek
```cpp
/**
  * Regenerálja a jegyzet előnézetét
  */
QString ScriptingService::regenerateNotePreview();
```

### Példa
```js
script.regenerateNotePreview();
```

Érdemes megnézni a példát [scribble.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/scribble.qml).

Egyéni művelet regisztrálása
---------------------------

### Módszerhívás és paraméterek
```cpp
/**
 * Egyéni műveletet regisztrál
 *
 * @param azonosítója a művelet azonosítója
 * @param menü Szöveg a menüben látható szöveg
 * @param gombText a gombon látható szöveg
 * (üres gomb nem jelenik meg)
 * @param ikon az ikonfájl útvonala vagy a freeskesk téma téma neve
 * az ikonok listáját itt találja:
 * https://specifications.freedesktop.org/icon-naming-spec/icon-naming-spec-latest.html
 * @param useInNoteEditContextMenu, ha igaz, használja a műveletet a jegyzet szerkesztésében
 * helyi menü (alapértelmezett: hamis)
 * @param hideButtonInToolbar ha igaz, akkor a gomb nem jelenik meg a
 * egyéni műveleti eszköztár (alapértelmezett: hamis)
 * @param useInNoteListContextMenu, ha igaz, használja a jegyzetlista műveletét
 * helyi menü (alapértelmezett: hamis)
 */
void ScriptingService::registerCustomAction(QString identifier,
                                            QString menuText,
                                            QString buttonText,
                                            QString ikon,
                                            bool useInNoteEditContextMenu,
                                            bool hideButtonInToolbar,
                                            bool useInNoteListContextMenu);
```

### Példa
```js
// egyéni művelet hozzáadása gomb nélkül
script.registerCustomAction ("mycustomaction1", "Menüszöveg");

// egyéni művelet hozzáadása egy gombbal
script.registerCustomAction ("mycustomaction1", "Menüszöveg", "Gombszöveg");

// egyéni művelet hozzáadása egy gombbal és a freeskesktop téma ikonnal
script.registerCustomAction ("mycustomaction1", "Menüszöveg", "Gombszöveg", "task-new");

// hozzáad egy egyéni műveletet egy gombbal és egy ikonnal egy fájlból
script.registerCustomAction("mycustomaction1", "Menu text", "Button text", "/usr/share/icons/breeze/actions/24/view-calendar-tasks.svg");
```

Ezután érdemes használni az azonosítót a function funkcióval `customActionInvoked` egy hasonló szkriptben [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml).

::: tip Egyéni műveletet is kiválthat az alkalmazás indítása után a(z) `--action customAction_<identifier>` paraméterrel. További információért tekintse meg a(z) [Indítás utáni menüműveletek](../getting-started/cli-parameters.md#trigger-menu-actions-after-startup)et. :::

Címke regisztrálása
-------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Regisztrál egy címkét, amelyhez írhat
  *
  * @param azonosítója a címke azonosítója
  * @param szöveg a címkén látható szöveg (nem kötelező)
  */
void ScriptingService :: registerLabel (QString azonosító, QString szöveg);
```

### Példa
```js
script.registerLabel("html-label", "<strong>Strong</strong> HTML text<br />with three lines<br />and a <a href='https://www.qownnotes.org'>link to a website</a>.");

script.registerLabel ("long-label", "egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú, egy másik nagyon hosszú szöveg , egy másik nagyon hosszú szöveg, egy másik nagyon hosszú szöveg, egy másik nagyon hosszú szöveg, amely be fog burkolni ");

script.registerLabel ("ellencímke");
```

A címkék láthatók lesznek a szkriptek dokkoló moduljában.

A címkékben használhat sima szöveget vagy HTML-t is. A szöveg választható lesz, és a linkekre kattintani lehet.

Ezután érdemes megnéznie a példa szkriptet [scripting-label-demo.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/scripting-label-demo.qml).

Bejegyzett címke szövegének beállítása
--------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Beállítja a bejegyzett címke szövegét
  *
  * @param azonosítója a címke azonosítója
  * @param szöveg a címkén látható szöveget
  */
void ScriptingService::setLabelText(QString identifier, QString text);
```

### Példa
```js
script.setLabelText("counter-label", "counter text");
```

A címkékben használhat sima szöveget vagy HTML-t is. A szöveg választható lesz, és a linkekre kattintani lehet.

Ezután érdemes megnéznie a példa szkriptet [scripting-label-demo.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/scripting-label-demo.qml).

Új jegyzet létrehozása
-------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Új jegyzetet hoz létre
  *
  * @param írja be a jegyzet szövegét
  */
void ScriptingService::createNote(QString text);
```

### Példa
```js
script.createNote("My note headline\n===\n\nMy text");
```

Érdemes megnéznie a [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml) példát.

::: tipp Ha kikapcsolta, hogy a jegyzet címsora határozza meg a jegyzet fájlnevét, akkor utána magának kell átneveznie a jegyzetfájlt, így:

```js
var note = script.currentNote();
note.renameNoteFile('your-filename');
```
:::

A vágólap elérése
-----------------------

### Módszerhívás és paraméterek
```cpp
/**
  * A vágólap tartalmát szövegként vagy HTML-ként adja vissza
  *
  * @param asHtml a vágólap tartalmát html-ként adja vissza szöveg helyett
  */
QString ScriptingService::clipboard(bool asHtml);
```

### Példa
```js
var clipboardText = script.clipboard();
var clipboardHtml = script.clipboard(true);
```

Érdemes megnéznie az [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml) példát.

Írjon szöveget a jegyzet szövegszerkesztésébe
--------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Szöveget ír a kurzor aktuális pozíciójába a jegyzet szövegszerkesztésében
  *
  * @param szöveg
  */
void ScriptingService::noteTextEditWrite(QString text);
```

### Példa
```js
// szöveget ír a jegyzet szövegszerkesztésébe
script.noteTextEditWrite ("Saját szöveg");
```

Érdemes megnéznie az `transformTextRot13` egyéni műveletet a [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml) példában.

Használhatja ezt a `noteTextEditSelectAll` vel együtt az aktuális jegyzet teljes szövegének felülírásához.

Olvassa el a kijelölt szöveget a jegyzet szövegszerkesztésében
--------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Elolvassa a kiválasztott szöveget a jegyzet szövegszerkesztésében
  *
  * @Visszatérés
  */
QString ScriptingService :: noteTextEditSelectedText ();
```

### Példa
```js
// elolvassa a kijelölt szöveget a jegyzet szövegszerkesztéséből
var text = script.noteTextEditSelectedText();
```

Érdemes megnéznie az `transformTextRot13` egyéni műveletet a [custom-actions.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-actions.qml) példában.

Jelölje ki az összes szöveget a jegyzet szövegszerkesztésében
-------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Kiválasztja az összes szöveget a jegyzet szövegszerkesztésében
  */
void ScriptingService::noteTextEditSelectAll();
```

### Példa
```js
script.noteTextEditSelectAll();
```

Használhatja ezt a `noteTextEditWrite` vel együtt az aktuális jegyzet teljes szövegének felülírásához.

Válassza ki az aktuális sort a jegyzet szövegszerkesztésében
---------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Kiválasztja az aktuális sort a jegyzet szövegszerkesztésében
  */
void ScriptingService::noteTextEditSelectCurrentLine();
```

### Példa
```js
script.noteTextEditSelectCurrentLine();
```

Válassza ki az aktuális szót a jegyzet szövegszerkesztésében
---------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Kiválasztja az aktuális sort a jegyzet szövegszerkesztésében
  */
void ScriptingService :: noteTextEditSelectCurrentWord();
```

### Példa
```js
script.noteTextEditSelectCurrentWord();
```

Állítsa be az aktuálisan kiválasztott szöveget a jegyzet szövegszerkesztésében
-----------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Beállítja az aktuálisan kiválasztott szöveget a jegyzet szövegszerkesztésében
  *
  * @param indítás
  * @param vége
  */
void ScriptingService :: noteTextEditSetSelection (int kezdet, int vég);
```

### Példa
```js
// egy karakterrel kibontja az aktuális választást
script.noteTextEditSetSelection(
    script.noteTextEditSelectionStart() - 1,
    script.noteTextEditSelectionEnd() + 1);
```

Az aktuális kijelölés kiindulási helyének megszerzése a jegyzetszöveg szerkesztésében
---------------------------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az aktuális kijelölés kezdő pozícióját a jegyzetszöveg szerkesztésében
  */
int ScriptingService::noteTextEditSelectionStart();
```

### Példa
```js
script.log(script.noteTextEditSelectionStart());
```

Az aktuális kijelölés végpozíciója a jegyzetszöveg szerkesztésében
-------------------------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az aktuális kijelölés végpozícióját a jegyzetszöveg szerkesztésében
  */
int ScriptingService :: noteTextEditSelectionEnd ();
```

### Példa
```js
script.log(script.noteTextEditSelectionEnd());
```

Állítsa a kurzort a jegyzet szövegszerkesztésében egy bizonyos pozícióra
---------------------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Beállítja a kurzort a jegyzet szövegszerkesztésében egy bizonyos pozícióba
  * 0 lenne a hang eleje
  * speciális eset: -1 lenne a hang vége
  *
  * @param pozíció
  */
void ScriptingService::noteTextEditSetCursorPosition(int position);
```

### Példa
```js
// ugrás a jegyzet 11. karakterére
script.noteTextEditSetCursorPosition(10);

// ugrás a hang végére
script.noteTextEditSetCursorPosition(-1);
```

Szerezze be a szövegmutató aktuális helyzetét a jegyzetszöveg szerkesztésében
-----------------------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja a kurzor aktuális helyzetét a jegyzet szövegszerkesztésében
  * 0 lenne a hang eleje
  */
int ScriptingService::noteTextEditCursorPosition();
```

### Példa
```js
script.log(script.noteTextEditCursorPosition());
```

Olvassa el az aktuális szót a jegyzet szövegszerkesztéséből
---------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Elolvassa az aktuális szót a jegyzet szövegszerkesztésében
  *
  * @param withPreviousCharacters is több karaktert kap az elején
  * olyan karakterek megszerzéséhez, mint a "@", amelyek nem
  * szó-karakterek
  * @return
  */
QString ScriptingService::noteTextEditCurrentWord(bool withPreviousCharacters);
```

### Példa
```js
// olvassa el az aktuális szót a jegyzet szövegszerkesztésében
var text = script.noteTextEditCurrentWord();
```

Érdemes megnéznie az [autocompletion.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/autocompletion.qml) példát.

Ellenőrizze, hogy a platform Linux, OS X vagy Windows
------------------------------------------------

### Módszerhívás és paraméterek
```cpp
bool ScriptingService::platformIsLinux();
bool ScriptingService::platformIsOSX();
bool ScriptingService::platformIsWindows();
```

### Példa
```js
if (script.platformIsLinux()) {
    // only will be executed if under Linux
}
```

Jelölje be az aktuális jegyzetet
--------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Az aktuális jegyzetet tagName nevű címkével látja el
  *
  * @param tagName
  */
void ScriptingService :: tagCurrentNote (QString tagName);
```

### Példa
```js
// adjon hozzá egy "kedvenc" címkét az aktuális jegyzethez
script.tagCurrentNote ("kedvenc");
```

Érdemes megnéznie a `kedvencNote` egyéni műveletet a [favorite-note.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/favorite-note.qml) példában.

Hozzon létre vagy hozzon létre egy címkét a nevének rákattintási listáján
-------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * A címkék nevének "rákattintási listáján" lekér vagy létrehoz egy címkét
  * Az Element nameList [0] legmagasabb lenne a fában (parentId: 0 értékkel)
  *
  * @param nameList
  * @param createMissing {bool}, ha true (alapértelmezett) az összes hiányzó címke létrejön
  * @return TagApi objektum a név legmélyebb címkéje
*/
TagApi *ScriptingService::getTagByNameBreadcrumbList(
    const QStringList &nameList, bool createMissing);
```

### Példa
```js
// az összes címkét létrehozza a 3. szintig, és visszaadja a címke objektumot
// tag "level3", amely így nézne ki a címkefában:
// szint1 > 2. szint > szint3
var tag = script.getTagByNameBreadcrumbList (["szint1", "szint2", "szint3"]);
```

Címkék keresése név szerint
-----------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Az összes címkét lekérdezi a névmezőben egy részlekereséssel
  *
  * @param név {QString} név a keresésre
  * @return {QStringList} címke nevek listája
 */
QStringList ScriptingService::searchTagsByName(QString name);
```

### Példa
```js
// megkeresi az összes címkét, benne a játék szóval
var tags = script.searchTagsByName("game");
```

Érdemes megnéznie az [autocompletion.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/autocompletion.qml) példát.

Jegyzetek keresése jegyzetszöveg alapján
-----------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az összes jegyzet jegyzetazonosítóinak listáját, egy bizonyos szöveggel a jegyzet szövegében
  *
  * Sajnos nincs egyszerű módszer a QList <NoteApi*>használatára a QML-ben, ezért mi
  * csak a jegyzetazonosítókat tudja átvinni
  *
  * @return {QList<int>} jegyzetazonosítók listája
*/
QList<int>ScriptingService::fetchNoteIdsByNoteTextPart(QString text);

```

### Példa
```js
var noteIds = script.fetchNoteIdsByNoteTextPart ("mytext");

noteIds.forEach function (noteId){
     var note = script.fetchNoteById(noteId);

     // tegyen valamit a jegyzettel
});
```

Érdemes megnéznie az [unique-note-id.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/unique-note-id.qml) példát.

Adjon hozzá egy egyedi stíluslapot
-----------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Hozzáad egy egyedi stíluslapot az alkalmazáshoz
  *
  * @param stíluslap
  */
void ScriptingService :: addStyleSheet (QString stíluslap);
```

### Példa
```js
// nagyítsa a jegyzetlista szövegét
script.addStyleSheet("QTreeWidget#noteTreeWidget {font-size: 30px;}");
```

Érdemes megnézni a példát [egyéni stíluslap.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/custom-stylesheet.qml).

Az objektumneveket például a `*.ui` fájlokból szerezheti be [mainwindow.ui](https://github.com/pbek/QOwnNotes/blob/develop/src/mainwindow.ui).

::: tip The [style.qss](https://github.com/pbek/QOwnNotes/blob/develop/src/libraries/qdarkstyle/style.qss) of [qdarkstyle](https://github.com/pbek/QOwnNotes/blob/develop/src/libraries/qdarkstyle) might also be a good reference for styles you can change. :::

Tekintse meg a [Stíluslap referencia](http://doc.qt.io/qt-5/stylesheet-reference.html) t, hogy megtudja, milyen stílusok állnak rendelkezésre.

Ha stílusokat szeretne beilleszteni a HTML-előnézetbe a jegyzetek előnézetének megváltoztatásához, kérjük, tekintse meg a [notetomarkdownhtmlhook](hooks.html#notetomarkdownhtmlhook) oldalt.

A parancsfájlmotor újratöltése
------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Újra feltölti a parancsfájl-motor
  */
void ScriptingService :: reloadScriptingEngine ();
```

### Példa
```js
// töltse be újra a szkript motort
script.reloadScriptingEngine();
```

Jegyzet letöltése fájlneve alapján
--------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Megjegyzést a fájlneve alapján kap
  *
  * @param fileName karakterlánc a megjegyzés fájlneve (kötelező)
  * @param noteSubFolderId a jegyzet almappájának egész azonosítója
  * @return NoteApi *
  */
NoteApi* ScriptingService::fetchNoteByFileName(QString fileName,
                                                int noteSubFolderId);
```

### Példa
```js
// jegyzet letöltése fájlnév szerint
script.fetchNoteByFileName ("jegyzetem.md");
```

Megjegyzés behozása az azonosítója alapján
-------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * A jegyzetet az azonosítója alapján szerzi be
  *
  * @param id a jegyzet azonosítójában
  * @return NoteApi *
  */
NoteApi* ScriptingService::fetchNoteById(int id);
```

### Példa
```js
// a jegyzet lehívása azonosító szerint
script.fetchNoteById (243);
```

Érdemes megnézni a példát [export-notes-as-one-html.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/export-notes-as-one-html.qml).

Annak ellenőrzése, hogy létezik-e jegyzet a fájlneve alapján
------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Ellenőrzi, hogy a jegyzetfájl létezik-e a fájlnév alapján
  *
  * @param fileName karakterlánc a megjegyzés fájlneve (kötelező)
  * @param ignoreNoteId az ellenőrzés során figyelmen kívül hagyandó jegyzet egész azonosítója
  * @param noteSubFolderId a jegyzet almappájának egész azonosítója
  * @return bool
  */
bool ScriptingService::noteExistsByFileName(QString fileName,
                                            int ignoreNoteId,
                                            int noteSubFolderId);
```

### Példa
```js
// ellenőrizze, hogy létezik-e jegyzet, de hagyja figyelmen kívül a "jegyzet" azonosítóját
script.noteExistsByFileName("my note.md", note.id);
```

Érdemes megnézni a példát [use-tag-names-in-fájlnév.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/use-tag-names-in-filename.qml).

Szöveg másolása a vágólapra
-------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Szöveget vagy HTML-mime adatot másol a vágólapra
  *
  * @param szöveges szöveg a vágólapra
  * @param asHtml bool, ha igaz, a szöveg html mime adatként lesz beállítva
  */
void ScriptingService::setClipboardText(QString text, bool asHtml);
```

### Példa
```js
// szöveg másolása a vágólapra
script.setClipboardText("text to copy");
```

Érdemes megnézni a példát [selected-markdown-to-bbcode.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/selected-markdown-to-bbcode.qml).

Ugrás egy jegyzethez
-----------------

### Módszerhívás és paraméterek
```cpp
/**
  * Beállítja az aktuális jegyzetet, ha a jegyzet látható a jegyzetlistában
  *
  * @param note NoteApi jegyzet, ahova ugorhat
  */
void ScriptingService::setCurrentNote(NoteApi *note);
```

### Példa
```js
// ugrás a hangra
script.setCurrentNote(note);
```

Érdemes megnézni a példát [journal-entry.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/journal-entry.qml).

Ugrás egy jegyzet almappájára
---------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Ugrás egy jegyzet almappába
  *
  * @param noteSubFolderPath {QString} az almappa elérési útja a jegyzetmappához képest
  * @param elválasztó {QString} elválasztó az útvonal részei között, alapértelmezett "/"
  * @return true, ha az ugrás sikeres volt
 */
bool ScriptingService::jumpToNoteSubFolder(const QString &noteSubFolderPath,
                                            QString separator);
```

### Példa
```js
// ugrás a "almappa" jegyzet almappájához
script.jumpToNoteSubFolder("a sub folder");

// ugrás az "almappa" belsejében található "al" mappához
script.jumpToNoteSubFolder("a sub folder/sub");
```

::: tip Új jegyzetmappát hozhat létre az aktuális almappában a [`mainWindow.createNewNoteSubFolder`](classes.html#example-2) hívásával. :::

Információs üzenetdoboz megjelenítése
----------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Információs üzenet mezőt mutat
  *
  * @param szöveg
  * @param cím (nem kötelező)
  */
```

### Példa
```js
// információs üzenet mező megjelenítése
script.informationMessageBox("The text I want to show", "Some optional title");
```

Kérdés üzenetmező megjelenítése
------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Megjeleníti a kérdésüzenet mezőt
  *
  * A gombokkal kapcsolatos információkért lásd:
  * https://doc.qt.io/qt-5/qmessagebox.html#StandardButton-enum
  *
  * @param szöveg
  * @param cím (nem kötelező)
  * Megjelenítendő @param gombok (opcionális)
  * @param defaultButton alapértelmezett gomb, amely kiválasztásra kerül (opcionális)
  * @return gombjának megnyomása
  */
int ScriptingService :: questionMessageBox(
        QString text, QString title, int buttons, int defaultButton);
```

### Példa
```js
// kérdéses üzenetdoboz megjelenítése egy alkalmazással és egy súgó gombbal
// lásd: https://doc.qt.io/qt-5/qmessagebox.html#StandardButton-enum
var result = script.questionMessageBox (
     "A megjeleníteni kívánt szöveg", "Néhány választható cím", 0x01000000 | 0x02000000, 0x02000000);
script.log(result);
```

A gombokkal kapcsolatos információkért lásd: [StandardButton](https://doc.qt.io/qt-5/qmessagebox.html#StandardButton-enum).

Érdemes egy pillantást vetni a példára is [input-dialogs.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/input-dialogs.qml).

Megnyitott fájl párbeszédpanel megjelenítése
---------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Megnyitott fájl párbeszédpanelt mutat
  *
  * @param felirat (opcionális)
  * @param dir (opcionális)
  * @param szűrő (opcionális)
  * @return QString
  */
QString ScriptingService::getOpenFileName(QString caption, QString dir,
                                            QString filter);
```

### Példa
```js
// show an open file dialog
var fileName = script.getOpenFileName("Please select an image", "/home/user/images", "Images (*.png *.xpm *.jpg)");
```

Fájl mentési párbeszédpanel megjelenítése
--------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Fájl mentési párbeszédpanelt mutat
  *
  * @param felirat (opcionális)
  * @param dir (opcionális)
  * @param szűrő (opcionális)
  * @return QString
  */
QString ScriptingService::getSaveFileName(QString caption, QString dir,
                                            QString filter);
```

### Példa
```js
// a fájl mentése párbeszédpanel megjelenítése
var fileName = script.getSaveFileName ("Please select HTML file to save", "output.html", "HTML (*.html)");
```

Érdemes megnézni a példát [export-notes-as-one-html.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/export-notes-as-one-html.qml).

Registering script settings variables
-------------------------------------

Meg kell határoznia a beállítási változókat tulajdonságként a szkriptben, és regisztrálnia kell őket egy `settingsVariables` nevű tulajdonságba.

Ezután a felhasználó beállíthatja ezeket a tulajdonságokat a szkript beállításaiban.

### Példa
```js
// meg kell határoznia a regisztrált változókat, hogy később hozzájuk férhessen
property string myString;
property bool myBoolean;
property string myText;
property int myInt;
property string myFile;
property string mySelection;

// regisztrálja a beállítási változókat, hogy a felhasználó beállítsa őket a szkript beállításaiban
// használja ezt a tulajdonságot, ha nincs rá szüksége
//
// sajnos a Qt-ben nincs QVariantHash, csak használni tudjuk
// QVariantMap (amelynek nincs önkényes sorrendje) vagy QVariantList (amely a
// a legkevesebb tetszőlegesen megrendelhető)
tulajdonságváltozat beállításaiVáltozók: [
    {
        "identifier": "myString",
        "name": "I am a line edit",
        "description": "Please enter a valid string:",
        "type": "string",
        "default": "My default value",
    },
    {
        "identifier": "myBoolean",
        "name": "I am a checkbox",
        "description": "Some description",
        "text": "Check this checkbox",
        "type": "boolean",
        "default": true,
    },
    {
        "identifier": "myText",
        "name": "I am textbox",
        "description": "Please enter your text:",
        "type": "text",
        "default": "This can be a really long text\nwith multiple lines.",
    },
    {
        "identifier": "myInt",
        "name": "I am a number selector",
        "description": "Please enter a number:",
        "type": "integer",
        "default": 42,
    },
    {
        "identifier": "myFile",
        "name": "I am a file selector",
        "description": "Please select the file:",
        "type": "file",
        "default": "pandoc",
    },
    {
        "identifier": "mySelection",
        "name": "I am an item selector",
        "description": "Please select an item:",
        "type": "selection",
        "default": "option2",
        "items": {"option1": "Text for option 1", "option2": "Text for option 2", "option3": "Text for option 3"},
    }
];

```

Ezenkívül felülírhatja a `settingsVariables` funkciót egy `registerSettingsVariables()` speciális funkcióval, mint ez:

### Példa
```js
/**
  * Újra regisztrálja a beállítási változókat
  *
  * Használja ezt a módszert, ha kódot szeretne használni a változók felülírásához, például a beállításhoz
  * az alapértelmezett értékek az operációs rendszertől függtek.
 */
függvény registerSettingsVariables () {
     if (script.platformIsWindows ()) {
         // felülírja a myFile alapértelmezett értékét
         settingsVariables [3].default = "pandoc.exe"
     }
}
```

Érdemes egy pillantást vetni erre a példára is: [variables.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/variables.qml).

Tartós változók tárolása és betöltése
----------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Tartós változót tárol
  * Ezek a változók globálisan elérhetők az összes szkripten
  * Kérjük, használjon értelmes előtagot a kulcsában, például "PersistentVariablesTest / myVar"
  *
  * @param key {QString}
  * @param defaultValue {QVariant}
  */
void ScriptingService::setPersistentVariable (const QString & amp; kulcs,
                                                 const QVariant &amp);

/**
  * Tartós változót tölt be
  * Ezek a változók globálisan elérhetők az összes szkripten
  *
  * @param key {QString}
  * @param defaultValue {QVariant} visszatérési érték, ha a beállítás nem létezik (nem kötelező)
  * @Return
  */
QVariant ScriptingService::getPersistentVariable (const QString & kulcs,
                                                     const QVariant & defaultValue);
```

### Példa
```js
// perzisztens változó tárolása
script.setPersistentVariable("PersistentVariablesTest/myVar", result);

// perzisztens változó betöltése és naplózása
script.log(script.getPersistentVariable("PersistentVariablesTest/myVar", "nothing here yet"));
```

Kérjük, ne felejtsen el értelmes előtagot használni a kulcsában, például `PersistentVariablesTest / myVar`, mert a változók minden szkriptből elérhetők.

Érdemes megnéznie a [persistent-variables.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/persistent-variables.qml) példát is.

Az alkalmazás beállításainak változóinak betöltése
--------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
 * Betölti az alkalmazás beállításainak változóját
 *
 * @param key {QString}
 * @param defaultValue {QVariant} return value if the setting doesn't exist (optional)
 * @return
 */
QVariant ScriptingService::getApplicationSettingsVariable(const QString &key,
                                                            const QVariant &defaultValue);
```

### Példa
```js
// betölti és naplózza az alkalmazás beállításainak változóját
script.log(script.getApplicationSettingsVariable("gitExecutablePath"));
```

Ne feledje, hogy a beállítások valóban üresek lehetnek, effelől magának kell gondoskodnia. Az `defaultValue` csak akkor használható, ha a beállítás egyáltalán nem létezik.

Gyorsítótár könyvtár létrehozása
--------------------------

A fájlokat a rendszer alapértelmezett gyorsítótár-helyén tárolhatja.

### Módszerhívás és paraméterek
```cpp
/**
  * Egy szkript gyorsítótár-könyvtárát adja eredményül
  *
  * @param {QString} alirányítsa az almappát létrehozásra és használatra
  * @return {QString} a gyorsítótár dir útvonalát
  */
QString ScriptingService::cacheDir(const QString &subDir) const;
```

### Példa
```js
// létrehozza a cache könyvtárat a my-script-id számára
var cacheDirForScript = script.cacheDir ("my-script-id");
```

Gyorsítótár könyvtár törlése
--------------------------

A szkript gyorsítótárfájljait úgy törölheti, hogy átadja a nevét a clearCacheDir() fájlnak.

### Módszerhívás és paraméterek
```cpp
/**
 * Törli a szkript gyorsítótár könyvtárát
 *
 * @param {QString} subDir the subfolder to clear
 * @return {bool} true on success
 */
bool ScriptingService::clearCacheDir(const QString &subDir) const;
```

### Példa
```js
// clear cache directory of my-script-id 
script.clearCacheDir("my-script-id");
```

A szkript könyvtárának elérési útjának elolvasása
------------------------------------------------

Ha meg kell kapnia annak a könyvtárnak az elérési útját, ahová a szkript tartozik, például más fájlok betöltéséhez, regisztrálnia kell egy `script stringDirPath;` tulajdonságot. Ez a tulajdonság a szkript könyvtárának elérési útjával lesz beállítva.

### Példa
```js
importálja a QtQml 2.0 fájlt
importálja a QOwnNotesTypes 1.0 fájlt

Script {
    property string scriptDirPath;

    function init() {
        script.log(scriptDirPath);
    }
}
```

Az útvonalelválasztók átalakítása natívakká
-----------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * A '/' elválasztókkal elválasztókká alakított útvonalat adja vissza
  * megfelel az alapul szolgáló operációs rendszernek.
 *
 * Windows rendszeren a toNativeDirSeparators ("c:/winnt/system32") visszatér
 * "c:\winnt\system32".
 *
  * @param útvonal
  * @Visszatérés
  */
QString ScriptingService::toNativeDirSeparators(QString path);
```

### Példa
```js
// a "c:\winnt\system32" szót adja vissza Windows rendszeren
script.log(script.toNativeDirSeparators("c:/winnt/system32"));
```

Az ösvényelválasztók konvertálása a natívakról
-------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az elérési utat a '/' fájlelválasztóként.
 * On Windows, for instance, fromNativeDirSeparators("c:\\winnt\\system32")
 * returns "c:/winnt/system32".
 *
 * @param path
 * @return
 */
QString ScriptingService::fromNativeDirSeparators(QString path);
```

### Példa
```js
// will return "c:/winnt/system32" on Windows
script.log(script.fromNativeDirSeparators("c:\\winnt\\system32"));
```

A natív könyvtár elválasztó megszerzése
--------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja a natív könyvtárelválasztót "/" vagy "\" Windows rendszeren
  *
  * @ return
  */
QString ScriptingService :: dirSeparator ();
```

### Példa
```js
// will return "\" on Windows
script.log(script.dirSeparator());
```

Az összes kijelölt jegyzet elérési útjának felsorolása
-------------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az összes kijelölt jegyzet elérési útját
  *
  * @return {QStringList} a kiválasztott jegyzet útvonalak listája
  */
QStringList ScriptingService :: selectedNotesPaths ();
```

### Példa
```js
// az összes kijelölt jegyzet elérési útjának listáját adja vissza
script.log (script.selectedNotesPaths());
```

Érdemes megnézni a példát [external-note-diff.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/external-note-diff.qml).

Az összes kijelölt jegyzet azonosítóinak listájának lekérése
-----------------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Visszaadja az összes kijelölt jegyzet azonosítóinak listáját
  *
  * @return {QList <int>} a kiválasztott jegyzetazonosítók listája
  */
QList <int> ScriptingService :: selectedNotesIds ();
```

### Példa
```js
// az összes kijelölt jegyzet azonosítóinak listáját adja vissza a script.log (script.selectedNotesIds());
```

Érdemes megnézni a példát [export-notes-as-one-html.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/export-notes-as-one-html.qml).

Menüművelet kiváltása
------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Kiválaszt egy menüműveletet
  *
  * @param objectName {QString} a kiváltandó művelet objektumneve
  * A @param bejelölt {QString} csak akkor aktiválja a műveletet, ha a check-state van
  * eltér ettől a paramétertől (opcionális, lehet 0 vagy 1)
  */
void ScriptingService::triggerMenuAction(QString objectName, QString checked);
```

### Példa
```js
// az írásvédett mód váltása
script.triggerMenuAction("actionAllow_note_editing");

// tiltsa le az írásvédett módot
script.triggerMenuAction("actionAllow_note_editing", 1);
```

Érdemes megnézni a példát [disable-readonly-mode.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/disable-readonly-mode.qml).

::: tip A menümű objektumneveit innen szerezheti be [mainwindow.ui](https://github.com/pbek/QOwnNotes/blob/develop/src/mainwindow.ui). Csak keresse meg az angol menü címét. Ne feledje, hogy ezek a szövegek idővel változhatnak. :::

Beviteli párbeszédpanel megnyitása egy kiválasztott mezővel
-----------------------------------------

### Módszerhívás és paraméterek
```cpp
Amy D. Lang (nyersfreeamy)
@unchase itt is (lásd a másik kommentemet)
/**
  * Megnyit egy beviteli párbeszédpanelt egy kiválasztó mezővel
  *
  * @param title {QString} párbeszédpanel címe
  * @param label {QString} címke szövege a párbeszédpanelen
  * @param items {QStringList} a kiválasztandó elemek listája
  * A kiválasztandó elem @param current {int} indexe (alapértelmezett: 0)
  * @param szerkeszthető {bool}, ha igaz, a párbeszédpanelen lévő szöveg szerkeszthető (alapértelmezett: hamis)
  * @return {QString} szöveg a kiválasztott elemről
  */
QString ScriptingService::inputDialogGetItem(
        const QString &title, const QString &label, const QStringList &items,
        int current, bool editable);
```

### Példa
```js
var result = script.inputDialogGetItem(
    "combo box", "Please select an item", ["Item 1", "Item 2", "Item 3"]);
script.log(result);
```

Érdemes megnézni a példát [input-dialogs.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/input-dialogs.qml).

Beviteli párbeszédpanel megnyitása sorszerkesztéssel
----------------------------------------

### Módszerhívás és paraméterek
```cpp
/**
  * Megnyit egy beviteli párbeszédpanelt egy sor szerkesztésével
  *
  * @param title {QString} párbeszédpanel címe
  * @param label {QString} címke szövege a párbeszédpanelen
  * @param text {QString} szöveg a párbeszédpanelen (opcionális)
  * @Visszatérés
  */
QString ScriptingService :: inputDialogGetText (
         const QString & cím, const QString & címke, const QString & szöveg);
```

### Példa
```js
var result = script.inputDialogGetText(
    "line edit", "Please enter a name", "current text");
script.log(result);
```

Ellenőrizze, hogy létezik-e fájl
-------------------------

### Módszerhívás és paraméterek
```cpp
/**
 * Check if a file exists
 * @param filePath
 * @return
 */
bool ScriptingService::fileExists(QString &filePath);
```

### Példa
```js
var result = script.fileExists(filePath);
script.log(result);
```

Szöveg olvasása egy fájlból
------------------------

### Módszerhívás és paraméterek
```cpp
/**
 * Read text from a file
 *
 * @param filePath {QString} path of the file to load
 * @param codec {QString} file encoding (default: UTF-8)
 * @return the file data or null if the file does not exist
 */
QString ScriptingService::readFromFile(const QString &filePath, const QString &codec)
```

### Példa
```js
if(script.fileExists(filePath)){
    var data = script.readFromFile(filePath);
    script.log(data);
}
```


Szöveg írása fájlba
----------------------

### Módszerhívás és paraméterek
```cpp
/**
 * Writes a text to a file
 *
 * @param filePath {QString}
 * @param data {QString}
 * @param createParentDirs {bool} optional (default: false)
 * @return
 */
bool ScriptingService::writeToFile(const QString &filePath, const QString &data, bool createParentDirs);
```

### Példa
```js
var result = script.writeToFile(filePath, html);
script.log(result);
```

Érdemes megnézni a példát [export-notes-as-one-html.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/export-notes-as-one-html.qml).

Webhálózatok használata
-----------------------

A QOwnNotes távirányítása a `WebSocketServer` használatával történik.

Kérjük, nézze meg a példát [websocket-server.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/websocket-server.qml). A socket kiszolgálót úgy tesztelheti, hogy csatlakozik hozzá a  [Websocket test](https://www.websocket.org/echo.html?location=ws://127.0.0.1:35345).

A foglalatokat a `WebSocket` használatával is meghallgathatja. Kérjük, nézze meg a példát [websocket-client.qml](https://github.com/pbek/QOwnNotes/blob/develop/docs/scripting/examples/websocket-client.qml).

Ne feledje, hogy ennek használatához telepítenie kell a Qt QML `websocket` könyvtárát. Például az Ubuntu Linux alatt telepíthet `qml-module-qtwebsockets`.
