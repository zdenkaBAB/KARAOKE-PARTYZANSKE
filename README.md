# PARTYZANSKE KARAOKE HITS – HALLOWEEN 2026 (online prototyp)

## Čo táto verzia obsahuje
- Katalóg 67 položiek z dodaného zoznamu (duplicitné položky sú zámerne ponechané).
- Vyhľadávanie skladieb a pridanie vlastnej piesne.
- Spoločný živý front cez Firebase Realtime Database.
- QR kód a odkaz pre mobily.
- Vložený YouTube prehrávač.
- 10-sekundové odpočítavanie medzi skladbami, tlačidlá „Spustiť hneď“ a „Pridať 10 sekúnd“.
- Režim projektora.

**Dôležité:** aplikácia ešte nie je zverejnená na internete. QR kód bude použiteľný až po nasadení na verejnú HTTPS adresu a nastavení Firebase. Z tohto chatu nemám prístup k tvojmu hostingovému účtu, preto nemôžem vytvoriť skutočnú verejnú adresu priamo za teba.

## 1. Vytvor Firebase projekt (bezplatný plán Spark)
1. Otvor https://console.firebase.google.com/ a vytvor projekt, napr. `partyzanske-karaoke-hits`.
2. V projekte pridaj webovú aplikáciu (ikona `</>`).
3. V nastaveniach webovej aplikácie skopíruj jej `firebaseConfig`.
4. V **Authentication → Sign-in method** povoľ **Anonymous** prihlasovanie.
5. V **Realtime Database** vytvor databázu. Vyber európsku lokalitu, ak je ponúknutá.
6. V databáze otvor **Rules** a nastav tieto pravidlá pre prototyp párty:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

Tieto pravidlá vyžadujú prihlásenie (aplikácia používa anonymné prihlásenie), ale umožňujú každému používateľovi aplikácie čítať a meniť dáta v databáze. Použi ich len pre dočasnú súkromnú párty bez citlivých údajov. Po akcii databázu vypni alebo pravidlá sprísni; nezadávaj do nej citlivé údaje.

## 2. Vlož konfiguráciu do aplikácie
1. Otvor `index.html` v textovom editore.
2. Nájdeš riadok `const PUBLIC_FIREBASE_CONFIG = null;`
3. Nahraď `null` objektom konfigurácie, ktorý poskytol Firebase, napríklad:

```js
const PUBLIC_FIREBASE_CONFIG = {
  apiKey: "TVOJ_API_KEY",
  authDomain: "TVOJ_PROJEKT.firebaseapp.com",
  databaseURL: "https://TVOJ_PROJEKT-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "TVOJ_PROJEKT",
  appId: "TVOJE_APP_ID"
};
```

Použi presné hodnoty zo svojej Firebase konzoly. Môžu obsahovať aj ďalšie polia. Ak Firebase ukáže inú adresu databázy, použi presne tú.

## 3. Zverejni aplikáciu cez GitHub Pages
1. Prihlás sa na https://github.com/ a vytvor nový **Public repository**, napr. `partyzanske-karaoke-hits`.
2. Nahraj doň `index.html` a `halloween-courtyard.png` (obrázok musí byť v tom istom priečinku ako HTML).
3. V repozitári otvor **Settings → Pages**.
4. V časti **Build and deployment** zvoľ **Deploy from a branch**, vetvu `main` a priečinok `/ (root)`, potom ulož.
5. Po dokončení nasadenia GitHub zobrazí verejnú HTTPS adresu, napr. `https://TVOJE-MENO.github.io/partyzanske-karaoke-hits/`.

## 4. Spustenie na párty
1. Otvor verejnú HTTPS adresu na notebooku.
2. Aplikácia sa pripojí k Firebase a vytvorí QR kód s adresou pre mobil.
3. Premietaj notebook na projektor; použi tlačidlo **Režim projektora** alebo F11.
4. Kamaráti naskenujú QR a zadajú meno, názov skladby a interpreta. Front sa aktualizuje v reálnom čase.

## Limity prototypu
- Karaoke verzie pre všetkých 67 titulov nie sú individuálne overené. Pri každej skladbe treba vybrať konkrétne video. Uprednostni verzie s hudobným podkladom a sprievodnými vokálmi.
- Niektoré videá nepovoľujú vložené prehrávanie.
- Automatický prechod funguje po skončení videa, ak sa prehrávač úspešne inicializoval. Ak ďalšia pieseň nemá uložený odkaz na video, organizátor bude musieť vybrať jej video.
- Toto je prototyp pre malú párty, nie produkčne zabezpečená aplikácia.
