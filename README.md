# BitFaktura API


Popis jak integrovat vlastní aplikaci nebo službu ze systémem [BitFaktura.cz](https://bitfaktura.cz)

Díky API můžete vystavovat faktury/účty/účtenky z jiných systému a spravovat tyto dokumenty, stejně jako klienty nebo produkty.

Funkční příklady vyvolání API BitFaktura naleznete také v systému BitFaktura (po přihlášení) v menu <b>Nastavení > API</b> a na webu: https://app.bitfaktura.cz/api

## Obsah
+ [API Token](#token)
+ [Další parametry dostupné při stahování seznamu záznamů](#list_params)
+ [Faktury - příklady volání](#examples)
	+ [Stahování seznamu faktur za aktuální měsíc](#f1)
	+ [Stahování seznamu faktur s jejich položkami](#f2)
	+ [Faktury daného klienta](#f3)
	+ [Stažení faktury po ID](#f4)
	+ [Stažení PDF](#f5)
	+ [Zaslání faktury E-MAILEM zákazníkovi](#f6)
	+ [Přidání nové faktury](#f7)
	+ [Přidání nové faktury (podle zákazníka, produktu, ID prodejce)](#f8)
	+ [Přidání podobné faktury (ID jiné faktury, např. záloha z objednávky, konečná faktura ze záloh atd.)](#f8b)
	+ [Přidání nové opravné faktury](#f9)
	+ [Aktualizace faktury](#f10)
	+ [Aktualizace položky na faktuře](#f10b)
	+ [Smazání položky na faktuře](#f10c)
	+ [Přidání položky na faktuře](#f10d)
	+ [Změna stavu faktury](#f11)
	+ [Stahování seznamu definic pravidelných faktur](#f12)
	+ [Přidání definice pravidelné faktury](#f13)
	+ [Aktualizace definice pravidelné faktury](#f14)
	+ [Smazání faktury](#f15)
	+ [Spojení existující faktury a účtenky](#f16)
	+ [Stahování příloh v archivu ZIP](#f17)
	+ [Přidání přílohy](#f17b)
+ [Odkaz na náhled faktury a stažení do PDF](#view_url)
+ [Příklady použití - nákup školení](#use_case1)
+ [Faktury – specifikace, typy políček, kódy GTU](#invoices)
+ [Klienti](#clients)
	+ [Seznam klientů](#k1)
	+ [Vyhledávání zákazníků podle jména, e-mailu, krátkého jména nebo daňového identifikačního čísla](#k1b)
	+ [Stáhnutí vybraného zákazníka podle ID](#k2)
	+ [Stáhnutí vybraného zákazníka podle externího ID](#k2b)
	+ [Přidání klienta](#k3)
	+ [Aktualizace klienta](#k4)
	+ [Smazání klienta](#k5)
+ [Produkty](#products)
	+ [Seznam produktů](#p1)
	+ [Seznam produktů se skladem z daného skladu](#p2)
	+ [Stažení vybraného produktu podle ID](#p3)
	+ [Stažení vybraného produktu podle ID se skladem z daného skladu](#p4)
	+ [Přidání  produktu](#p5)
	+ [Aktualizace produktu](#p6)
+ [Ceníky](#price_lists)
	+ [Seznam ceníků](#pricel1)
	+ [Přidání ceníků](#pricel2)
	+ [Aktualizace ceníků](#pricel3)
	+ [Smazání ceníků](#pricel4)
+ [Skladové dokumenty](#warehouse_documents)
	+ [Všechny skladové dokumenty](#wd1)
	+ [Stažení vybraného dokumentu podle ID](#wd2)
	+ [Přidání skladového dokumentu MM](#wd3a)
	+ [Přidání skladového dokumentu PZ](#wd3)
	+ [Přidání skladového dokumentu WZ](#wd4)
	+ [Přidání skladového dokladu PZ pro existujícího zákazníka, oddělení a produktu](#wd5)
	+ [Aktualizace dokumentu](#wd6)
	+ [Smazání dokumentu](#wd7)
	+ [Spojení existujících faktur a skladového dokumentu](#wd8)
+ [Platby](#payments)
	+ [Všechny platby](#pl1)
	+ [Stažení vybrané platby po ID](#pl2)
	+ [Přidání nové platby](#pl3)
	+ [Stahování platby spolu s připnutými fakturačními údaji](#pl4)
	+ [Přidání nové platby propojené se stávajícími fakturami](#pl5)
	+ [Aktualizace plateb](#pl6)
	+ [Smazání plateb](#pl7)
+ [Kategorie](#categories)
	+ [Seznam kategorií](#cat1)
	+ [Stažení vybrané kategorie po ID](#cat2)
	+ [Přidání nové kategorie](#cat3)
	+ [Aktualizace kategorie](#cat4)
	+ [Odstranění kategorie po ID](#cat5)
+ [Sklady](#warehouses)
	+ [Seznam skladů](#wh1)
	+ [Stažení vybraného skladu podle ID](#wh2)
	+ [Přidání nového skladu](#wh3)
	+ [Aktualizace skladu](#cat4)
	+ [Odstranění skladu podle ID](#cat5)
+ [Oddělení](#departments)
	+ [Seznam oddělení](#dep1)
	+ [Stažení vybraného oddělení podle ID](#dep2)
	+ [Přidání nového oddělení](#dep3)
	+ [Aktualizace oddělení](#dep4)
  	+ [Odstranění oddělení po ID](#dep5)
+ [Přihlášení a stažení tokenu přes API](#get_token_by_api)
+ [Systémové účty](#accounts)
+ [Příklady v PHP a Ruby](#codes)


<a name="token"/>

## API token

`API_TOKEN` je nutné stáhnout z nastavení aplikace ("Nastavení -> Nastavení účtu -> Integrace -> Autorizační kód API")

<a name="list_params"/>

## Další parametry dostupné při načítání seznamu záznamů
Díky vyvolání lze předat další parametry – stejné parametry, jaké se používají v aplikaci, např. `page =`, `period =` atd.

Parametr `page =` Vám umožňuje iterovat stránkované záznamy.
Ve výchozím nastavení je nastavena na `1` a zobrazuje prvních N záznamů, kde N je limit počtu vrácených záznamů.
Chcete-li získat dalších N záznamů, proveďte akci s parametrem `page = 2` atd.

Parametr `period = ' umožňuje vybrat záznamy z daného období.
Může přijmout následující hodnoty:
- last_12_months
- this_month
- last_30_days
- last_month
- this_year
- last_year
- all
- more (zde musíte zadat další parametry date_from (např. "2018-12-16") a date_to (např. "2018-12-21"))

Parametr `include_positions =` s hodnotou `true` umožňuje získat seznam záznamů s jejich pozicemi

Parametr `income =` s hodnotou `no` umožňuje stahovat výdajové faktury

<a name="examples"/>

## Příklady vyvolání

<a name="f1"/>
Stažení seznamu faktur za aktuální měsíc

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?period=this_month&api_token=API_TOKEN&page=1
```

<a name="f2"/>
Stažení seznamu faktur s jejich položkami

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?include_positions=true&api_token=API_TOKEN&page=1
```

<a name="f3"/>
Faktury zákazníka

```shell
curl https://twojaDomena.fakturownia.pl/invoices.json?client_id=ID_KLIENTA&api_token=API_TOKEN
```

<a name="f4"/>
Stažení faktury po ID


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.json?api_token=API_TOKEN
```

<a name="f5"/>
Stažení PDF


```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.pdf?api_token=API_TOKEN
```

<a name="f6"/>
Zaslání faktury e-mailem zákazníkovi (na e-mail zákazníka uvedený při vytváření faktury, pole "email_kupujícího")


```shell
curl -X POST https://twojaDomena.fakturownia.pl/invoices/100/send_by_email.json?api_token=API_TOKEN
```

další možnosti PDF:
* print_option=original - Originál
* print_option=copy - Kopie
* print_option=original_and_copy - Originál a kopie
* print_option=duplicate Duplikát


<a name="f7"/>
Přidání nové faktury

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "kind":"vat",
            "number": null,
            "sell_date": "2013-01-16",
            "issue_date": "2013-01-16",
            "payment_to": "2013-01-23",
            "seller_name": "Wystawca Sp. z o.o.",
            "seller_tax_no": "5252445767",
            "buyer_name": "Klient1 Sp. z o.o.",
            "buyer_email": "buyer@testemail.pl",
            "buyer_tax_no": "5252445767",
            "positions":[
                {"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1},
                {"name":"Produkt A2", "tax":0, "total_price_gross":50, "quantity":3}
            ]
        }
    }'
```

<a name="f8"/>
Dodanie nowej faktury - minimalna wersja (tylko pola wymagane), gdy mamy Id produktu (product_id), nabywcy (client_id) i sprzedawcy (department_id) wtedy nie musimy podawać pełnych danych. Opcjonalnie można podać również id odbiorcy (recipient_id).
Zostanie wystawiona Faktura VAT z aktualnym dniem i z 5 dniowym terminem płatności.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"product_id": 1, "quantity":2}
            ]
        }}'
```

<a name="f8b"/>
Dodanie nowej faktury – dokumentu podobnego do faktury o podanym ID (copy_invoice_from).

Dodanie identycznej faktury o podanym rodzaju

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DOKUMENTU_ZRODLOWEGO,
            "kind": "RODZAJ_FAKTURY"
        }
    }'
```

Dodanie faktury zaliczkowej na podstawie zamówienia – % pełnej kwoty

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_ZAMOWIENIA,
            "kind": "advance",
            "advance_creation_mode": "percent",
            "advance_value": "10",
            "position_name": "Zaliczka na wykonanie zamówienia ZAM-NR"
        }
    }'
```

Dodanie faktury zaliczkowej na podstawie zamówienia – podana kwota brutto

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_ZAMOWIENIA,
            "kind": "advance",
            "advance_creation_mode": "amount",
            "advance_value": "150",
            "position_name": "Zaliczka na wykonanie zamówienia ZAM-NR"
        }
    }'
```

Dodanie faktury końcowej na podstawie zamówienia i faktur zaliczkowych

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_ZAMOWIENIA,
            "kind": "final",
            "invoice_ids": [ID_ZALICZKI_1, ID_ZALICZKI_2, ...]
        }
    }'
```

Dodanie faktury VAT na podstawie faktury Proforma

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_PROFORMY,
            "kind": "vat"
        }
    }'
```

<a name="f9"/>
Dodanie nowej faktury korygującej

```shell
curl http://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "kind": "correction",
            "correction_reason": "Zła ilość",
            "invoice_id": "2432393",
            "from_invoice_id": "2432393",
            "client_id": 1,
            "positions":[
                {"name": "Product A1",
                "quantity":-1,
                "total_price_gross":"-10",
                "tax":"23",
                "kind":"correction",
                "correction_before_attributes": {
                    "name":"Product A1",
                    "quantity":"2",
                    "total_price_gross":"20",
                    "tax":"23",
                    "kind":"correction_before"
                },
                "correction_after_attributes": {
                    "name":"Product A1",
                    "quantity":"1",
                    "total_price_gross":"10",
                    "tax":"23",
                    "kind":"correction_after"
                }
            }]
        }}'
```

<a name="f10"/>
Aktualizacja faktury

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "buyer_name": "Nowa nazwa klienta Sp. z o.o."
        }
    }'
```

<a name="f10b"/>
Aktualizacja pozycji na fakturze - aby edytować pozycję na fakturze, należy podać id pozycji.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "name":"test"}]
        }
    }'
```

<a name="f10c"/>
Usunięcie pozycji na fakturze - aby usunąć pozycję na fakturze, należy podać id pozycji wraz z parametrem _destroy=1.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "_destroy": 1}]
        }
    }'
```

<a name="f10d"/>
Dodanie pozycji na fakturze. Pozycja zostanie dopisana jako ostatnia.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1}]
        }
    }'
```

<a name="f11"/>
Zmiana statusu faktury

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/invoices/111/change_status.json?api_token=API_TOKEN&status=STATUS" -X POST
```

<a name="f12"/>
Pobranie listy definicji faktur cyklicznych

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings.json?api_token=API_TOKEN
```

<a name="f13"/>
Dodanie definicji faktury cyklicznej

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "recurring": {
            "name": "Nazwa cyklicznosci",
            "invoice_id": 1,
            "start_date": "2016-01-01",
            "every": "1m",
            "issue_working_day_only": false,
            "send_email": true,
            "buyer_email": "mail1@mail.pl, mail2@mail.pl",
            "end_date": "null"
        }}'
```

<a name="f14"/>
Aktualizacja definicji faktury cyklicznej (zmiana daty wystawienia następnej faktury)

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/recurrings/111.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "recurring": {
            "next_invoice_date": "2016-02-01"
        }
    }'
```

<a name="f15"/>
Usunięcie faktury

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/invoices/INVOICE_ID.json?api_token=API_TOKEN"
```

<a name="f16"/>
Połączenie istniejącej faktury i paragonu

```shell
curl https://YOUR_DOMAIN.fakturownia.test/invoices/ID_FAKTURY.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "from_invoice_id": ID_PARAGONU,
            "invoice_id": ID_PARAGONU,
            "exclude_from_stock_level": true
        }
    }'
```

<a name="f17"/>
Pobranie wszystkich załączników faktury w archiwum ZIP

```shell
curl -o attachments.zip https://YOUR_DOMAIN.fakturownia.pl/invoices/INVOICE_ID/attachments_zip.json?api_token=API_TOKEN
```

<a name="f17b"/>
Dodanie nowego załącznika do faktury

1. Pobranie danych niezbędnych do przesłania pliku:
    ```shell
    curl https://YOUR_DOMAIN.fakturownia.pl/invoices/INVOICE_ID/get_new_attachment_credentials.json?api_token=API_TOKEN
    ```

2. Przesłanie pliku:
    ```shell
    curl -F 'AWSAccessKeyId=received_AWSAccessKeyId' \
         -F 'key=received_key' \
         -F 'policy=received_policy' \
         -F 'signature=received_signature' \
         -F 'acl=received_acl' \
         -F 'success_action_status=received_success_action_status' \
         -F 'file=@/file_path/name.ext' \
         received_url
    ```

3. Dodanie załącznika (przesłanego pliku) do faktury:
    ```shell
    curl -X POST https://YOUR_DOMAIN.fakturownia.pl/invoices/INVOICE_ID/add_attachment.json?api_token=API_TOKEN&file_name=name.ext
    ```

<a name="view_url"/>

## Link do podglądu faktury i pobieranie do PDF

Po pobraniu danych faktury np. przez:

```shell
curl https://twojaDomena.fakturownia.pl/invoices/100.json?api_token=API_TOKEN
```

API zwraca nam m.in. pole `token` na podstawie którego możemy otrzymać linki do podglądu faktury oraz do pobrania PDF-a z wygenrowaną fakturą.
Linki takie umożliwiają odwołanie się do wybranej faktury  bez konieczności logowania - czyli możemy np. te linki przesłać klientowi, który otrzyma dostęp do faktury i PDF-a.

Lini te są postaci:

podgląd: `https://twojaDomena.fakturownia.pl/invoice/{{token}}`
pdf: `https://twojaDomena.fakturownia.pl/invoice/{{token}}.pdf`

Np dla tokenu równego: `HBO3Npx2OzSW79RQL7XV2` publiczny PDF będzie pod adresem `https://twojaDomena.fakturownia.pl/invoice/HBO3Npx2OzSW79RQL7XV2.pdf`

<a name="use_case1"/>

## Przykłady użycia w PHP - zakup szkolenia

`TODO`

Przykład flow Portalu, który generuje dla klienta fakturę Proformę, wysyła ją klientowi i po opłaceniu wysyła do klienta bilet na szkolenie

* Klient wypełnia dane w Portalu
* Portal wywołuje API z fakturownia.pl i tworzy fakturę
* Portal wysyła Klientowi fakturę Proforma w PDF wraz z linkiem do płatności
* Klient opłaca fakturę Proforma (np. na PayPal lub PayU.pl)
* Fakturownia.pl otrzymuje informację, że płatność została wykonana, tworzy Fakturę VAT i wysyła ją Klientowi oraz wywołuje API Portalu
* Po otrzymaniu informacji o płatności (przez API) Portal wysyła Klientowi bilet na Szkolenie


<a name="invoices"/>

## Faktury


* `GET /invoices/1.json` pobranie faktury
* `POST /invoices.json` dodanie nowej faktury
* `PUT /invoices/1.json` aktualizacja faktury
* `DELETE /invoices/1.json` skasowanie faktury


Przykład - dodanie nowej faktury (minimalna wersja, gdy mamy Id produktu, nabywcy i sprzedawcy wtedy nie musimy podawać pełnych danych). Zostanie wystawiona Faktura VAT z aktualnym dniem i z 5 dniowym terminem płatności. Pole department_id określa firmę (lub dział) który wystawia fakturę (można go uzyskać klikając na firmę w menu Ustawienia > Dane firmy)

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"product_id": 1, "quantity":2}
            ]
        }}'
```

Pola faktury

```shell
"number" : "13/2012", - numer faktury (jeśli nie będzie podany wygeneruje się automatycznie)
"kind" : "vat", - rodzaj faktury (vat, proforma, bill, receipt, advance, correction, vat_mp, invoice_other, vat_margin, kp, kw, final, estimate)
"income" : "1", - faktura przychodowa (1) lub kosztowa (0)
"issue_date" : "2013-01-16", - data wystawienia
"place" : "Warszawa", - miejsce wystawienia
"sell_date" : "2013-01-16", - data sprzedaży (może być data lub miesiąc postaci 2012-12)
"category_id" : "", - id kategorii
"department_id" : "1", - id działu firmy (w menu Ustawienia > Dane firmy należy kliknąć na firmę/dział i ID działu pojawi się w URL); Jeśli nie będzie tego pola oraz nie będzie pola 'seller_name' wtedy będą wstawione domyślne dane Twojej firmy
"accounting_kind": "", - rodzaj wydatku dla faktur kosztowych - (purchases, expenses, media, salary, incident, fuel0, fuel_expl50, fuel_expl75, fuel_expl100, fixed_assets, fixed_assets50, no_vat_deduction)
"seller_name" : "Radgost Sp. z o.o.", - sprzedawca
"seller_tax_no" : "525-244-57-67", - numer identyfikacji podatkowej sprzedawcy (domyślnie NIP)
"seller_tax_no_kind" : "", - rodzaj numeru identyfikacyjnego sprzedawcy; pole puste (domyślnie) jest interpretowane jako "NIP"; w innym wypadku traktowane jako dowolny wpis własny (np. PESEL, REGON)
"seller_bank_account" : "24 1140 1977 0000 5921 7200 1001", - konto bankowe sprzedawcy
"seller_bank" : "BRE Bank",
"seller_post_code" : "02-548",
"seller_city" : "Warszawa",
"seller_street" : "ul. Olesińska 21",
"seller_country" : "", - kraj sprzedawcy (ISO 3166)
"seller_email" : "platnosci@radgost123.com",
"seller_www" : "",
"seller_fax" : "",
"seller_phone" : "",
"client_id" : "-1" - id kupującego (jeśi -1 to klient zostanie utworzony w systemie)
"buyer_name" : "Nazwa klienta" - nabywca
"buyer_tax_no" : "525-244-57-67", - numer identyfikacji podatkowej nabywcy (domyślnie NIP)
"buyer_tax_no_kind" : "", - rodzaj numeru identyfikacyjnego nabywcy; pole puste (domyślnie) jest interpretowane jako "NIP"; w innym wypadku traktowane jako wpis własny (np. PESEL, REGON)
"disable_tax_no_validation" : "",
"buyer_post_code" : "30-314", - kod pocztowy nabywcy
"buyer_city" : "Warszawa", - miasto nabywcy
"buyer_street" : "Nowa 44", - ulica nabywcy
"buyer_country" : "PL", - kraj nabywcy (ISO 3166)
"buyer_note" : "", - dodatkowy opis nabywcy
"buyer_email" : "", - email nabywcy
"recipient_id" : "", - id odbiorcy (id klienta z systemu)
"recipient_name" : "", - nazwa odbiorcy
"recipient_street" : "", - ulica odbiorcy
"recipient_post_code" : "", - kod pocztowy odbiorcy
"recipient_city" : "", - miasto odbiorcy
"recipient_country" : "", - kraj odbiorcy (ISO 3166)
"recipient_email" : "", - e-mail odbiorcy
"recipient_phone" : "", - numer telefonu odbiorcy
"recipient_note" : "", - dodatkowy opis odbiorcy
"additional_info" : "0" - czy wyświetlać dodatkowe pole na pozycjach faktury
"additional_info_desc" : "PKWiU" - nazwa dodatkowej kolumny na pozycjach faktury
"show_discount" : "0" - czy rabat
"payment_type" : "transfer",
"payment_to_kind" : termin płatności. gdy jest tu "other_date", wtedy można określić konkretną datę w polu "payment_to", jeśli jest tu liczba np. 5 to wtedy mamy 5 dniowy okres płatności
"payment_to" : "2013-01-16",
"status" : "issued",
"paid" : "0,00",
"oid" : "zamowienie10021", - numer zamówienia (np z zewnętrznego systemu zamówień)
"oid_unique" : jeśli to pole będzie ustawione na 'yes' wtedy nie system nie pozwoli stworzyc 2 faktur o takim samym OID (może to być przydatne w synchronizacji ze sklepem internetowym)
"warehouse_id" : "1090",
"seller_person" : "Imie Nazwisko",
"buyer_first_name" : "Imie",
"buyer_last_name" : "Nazwisko",
"paid_date" : "",
"currency" : "PLN",
"lang" : "pl",
"use_moss" : "0" - czy faktura MOSS
"exchange_currency" : "", - przeliczona waluta (przeliczanie sumy i podatku na inną walutę), np. "PLN"
"exchange_kind" : "", - źródło kursu do przeliczenia waluty ("ecb", "nbp", "cbr", "nbu", "nbg", "own")
"exchange_currency_rate" : "", - własny kurs przeliczenia waluty (używany, gdy parametr exchange_kind ustawiony jest na "own")
"invoice_template_id" : "1",
"description" : "- opis faktury",
"description_footer" : "", - opis umieszczony w stopce faktury
"description_long" : "", - opis umieszczony na odwrocie faktury
"invoice_id" : "" - pole z id powiązanego dokumentu, np. id zamówienia przy zaliczce albo id wzorcowej faktury przy fakturze cyklicznej,
"from_invoice_id" : "" - id faktury na podstawie której faktura została wygenerowana (przydatne np. w przypadku generacji Faktura VAT z Faktury Proforma),
"delivery_date" : "" - data wpłynięcia dokumentu (tylko przy wydatkach),
"buyer_company" : "1" - czy klient jest firmą
"additional_invoice_field" : "" - wartość dodatkowego pola na fakturze, Ustawienia > Ustawienia Konta > Konfiguracja > Faktury i dokumenty > Dodatkowe pole na fakturze
"internal_note" : "" - treść notatki prywatnej na fakturze, niewidoczna na wydruku.
"exclude_from_stock_level" : "" - informacja, czy system powinien liczyć tę fakturę do stanów magazynowych (true np., gdy faktura wystawiona na podstawie paragonu)
"gtu_codes" : [] - wartości kodów GTU produktów zawartych na fakturze - UWAGA - podane wartości nadpiszą wartości kodów GTU pobranych z kart produktów podawanych w pozycjach faktury, wartości tych kodów są nadrzędne dla całej faktury
"procedure_designations" : [] - oznaczenia dotyczące procedur
"positions":
   		"product_id" : "1",
   		"name" : "Fakturownia Start",
   		"additional_info" : "", - dodatkowa informacja na pozycji faktury (np. PKWiU)
   		"discount_percent" : "", - zniżka procentowa (uwaga: aby rabat był wyliczany trzeba ustawić pole: 'show_discount' na '1' oraz przed wywołaniem należy sprawdzić czy w Ustawieniach Konta pole: "Jak obliczać rabat" ustawione jest na "procentowo")
   		"discount" : "", - zniżka kwotowa (uwaga: aby rabat był wyliczany trzeba ustawić pole: 'show_discount' na 1 oraz przed wywołaniem należy sprawdzić czy w Ustawieniach Konta pole: "Jak obliczać rabat" ustawione jest na "kwotowo")
   		"quantity" : "1",
   		"quantity_unit" : "szt",
   		"price_net" : "59,00", - jeśli nie jest podana to zostanie wyliczona
   		"tax" : "23", - może być stawka lub "np" - dla nie podlega, "zw" - dla zwolniona
   		"price_gross" : "72,57", - jeśli nie jest podana to zostanie wyliczona
   		"total_price_net" : "59,00", - jeśli nie jest podana to zostanie wyliczona
   		"total_price_gross" : "72,57",
   		"code" : "" - kod produktu,
                "gtu_code" : "" - kod GTU produktu
"calculating_strategy":
{
  "position": "default" lub "keep_gross" - metoda wyliczania kwot na pozycjach faktury
  "sum": "sum" lub "keep_gross" lub "keep_net" - metoda sumowania kwot z pozycji
  "invoice_form_price_kind": "net" lub "gross" - cena jednostkowa na formatce faktury
},
"split_payment" : "1" - 1 lub 0 w zależności, czy faktura podlega pod split payment, czy nie
"accounting_vat_tax_date": "2020-12-18", Data księgowania podatku VAT (jeśli włączono w ustawieniach)
"accounting_income_tax_date": "2020-12-18", Data księgowania podatku dochodowego (jeśli włączono w ustawieniach)
```

Wartości pól

Pole: `kind`
```shell
	"vat" - faktura VAT
	"proforma" - faktura Proforma
	"bill" - rachunek
	"receipt" - paragon
	"advance" - faktura zaliczkowa
	"final" - faktura końcowa
	"correction" - faktura korekta
	"vat_mp" - faktura MP
	"invoice_other" - inna faktura
	"vat_margin" - faktura marża
	"kp" - kasa przyjmie
	"kw" - kasa wyda
	"estimate" - zamówienie
	"vat_mp" - faktura MP
	"vat_rr" - faktura RR
	"correction_note" - nota korygująca
	"accounting_note" - nota księgowa
	"client_order" - własny dokument nieksięgowy
	"dw" - dowód wewnętrzny
	"wnt" - Wewnątrzwspólnotowe Nabycie Towarów
	"wdt" - Wewnątrzwspólnotowa Dostawa Towarów
	"import_service" - import usług
	"import_service_eu" - import usług z UE
	"import_products" - import towarów - procedura uproszczona
	"export_products" - eksport towarów
```

Pole: `lang`
```shell
	"pl" - faktura w języku polskim
	"en" - język angielski
	"en-GB" - język angielski UK
	"de" - język niemiecki
	"fr" - język francuski
	"cz" - język czeski
	"ru" - język rosyjski
	"es" - język hiszpański
	"it" - język włoski
	"nl" - język niderlandzki
	"hr" - język chorwacki
	"ar" - język arabski
	"sk" - język słowacki
	"sl" - język słoweński
	"el" - język grecki
	"et" - język estoński
	"cn" - język chiński
	"hu" - język węgierski
	"tr" - język turecki
	"fa" - język perski

	można tworzyć faktury dwujęzyczne łącząc symbole dwóch języków przy pomocy ukośnika, np:
	"pl/en" - język polski i angielski
```

Pole: `income`
```shell
	"1" - fakura przychodwa
	"0" - faktura kosztowa
```

Pole: `accounting_kind`
```shell
  "purchases" - Zakup towarów i materiałów
  "expenses" - Koszty prowadzenia działalności
  "media" - Media i usługi telekomunikacyjne
  "salary" - Wynagrodzenia
  "incident" - Koszty uboczne zakupu
  "fuel0" - Zakup paliwa do pojazdów 0%
  "fuel_expl50" - Zakup paliwa i eksploatacja pojazdu 50%
  "fuel_expl75" - Zakup paliwa i eksploatacja pojazdu 75%
  "fuel_expl100" - Zakup paliwa i eksploatacja pojazdu 100%
  "fixed_assets" - Środki trwałe
  "fixed_assets50" - Środki trwałe 50%
  "no_vat_deduction" - Bez możliwości odliczenia podatku VAT
```

Pole: `payment_type`
```shell
	"transfer" - przelew
	"card" - karta płatnicza
	"cash" -  gotówka
	"barter" - barter
	"cheque" - czek
	"bill_of_exchange" - weksel
	"cash_on_delivery" - opłata za pobraniem
	"compensation" - kompensata
	"letter_of_credit" - akredytywa
	"payu" - PayU
	"paypal" - PayPal
	"off" - "nie wyświetlaj"
	"dowolny_inny_wpis_tekstowy"
```

Pole: `status`
```shell
	"issued" - wystawiona
	"sent" - wysłana
	"paid" - opłacona
	"partial" - częściowo opłacona
	"rejected" - odrzucona
```

Pole: `discount_kind` - rodzaj rabatu
```shell
	"percent_unit" - liczony od ceny jednostkowej
	"percent_total" - liczony od ceny całkowitej
	"amount" - kwotowy
```

Pole: `np_tax_kind` - podstawa zastosowania stawki NP
> Uwaga! Dla każdej pozycji, która nie podlega opodatkowaniu VAT należy podać jako stawkę podatku jeden z wariantów stawki "np" (przykład: "tax": "na").

> Warianty stawki "np" rozpoznawane przez system: "np", "n/a", "nie podlega", "not applicable", "na" - lub ich wersje pisane z wielkich liter.

```shell
    "export_service" - dostawa towarów oraz świadczenie usług poza terytorium kraju
    "export_service_eu" - w tym świadczenie usług, o których mowa w art.100 ust.1 pkt 4 ustawy
    "not_specified" - nie określono
```

Pole: `procedure_designations` - oznaczenia dotyczące procedur
```shell
  "SW" - Dostawa w ramach sprzedaży wysyłkowej z terytorium kraju, o której mowa w art. 23 ustawy,
  "EE" - Świadczenie usług telekomunikacyjnych, nadawczych i elektronicznych, o których mowa w art. 28k ustawy,
  "TP" - Istniejące powiązania między nabywcą a dokonującym dostawy towarów lub usługodawcą, o których mowa w art. 32 ust. 2 pkt 1 ustawy,
  "TT_WNT" - Wewnątrzwspólnotowe nabycie towarów dokonane przez drugiego w kolejności podatnika VAT w ramach transakcji trójstronnej w procedurze uproszczonej, o której mowa w dziale XII rozdziale 8 ustawy,
  "TT_D" - Dostawa towarów poza terytorium kraju dokonana przez drugiego w kolejności podatnika VAT w ramach transakcji trójstronnej w procedurze uproszczonej, o której mowa w dziale XII rozdziale 8 ustawy,
  "MR_T" - Świadczenie usług turystyki opodatkowane na zasadach marży zgodnie z art. 119 ustawy,
  "MR_UZ" - Dostawa towarów używanych, dzieł sztuki, przedmiotów kolekcjonerskich i antyków, opodatkowana na zasadach marży zgodnie z art. 120 ustawy,
  "I_42" - Wewnątrzwspólnotowa dostawa towarów następująca po imporcie tych towarów w ramach procedury celnej 42 (import),
  "I_63" - Wewnątrzwspólnotowa dostawa towarów następująca po imporcie tych towarów w ramach procedury celnej 63 (import),
  "B_SPV" - Transfer bonu jednego przeznaczenia dokonany przez podatnika działającego we własnym imieniu, opodatkowany zgodnie z art. 8a ust. 1 ustawy,
  "B_SPV_DOSTAWA" - Dostawa towarów oraz świadczenie usług, których dotyczy bon jednego przeznaczenia na rzecz podatnika, który wyemitował bon zgodnie z art. 8a ust. 4 ustawy,
  "B_MPV_PROWIZJA" - Świadczenie usług pośrednictwa oraz innych usług dotyczących transferu bonu różnego przeznaczenia, opodatkowane zgodnie z art. 8b ust. 2 ustawy,
  "MPP" - Transakcja objęta obowiązkiem stosowania mechanizmu podzielonej płatności
```

## Faktury - kody GTU
Więcej informacji o kodach GTU można znaleźć na naszej stronie pomocy: https://pomoc.fakturownia.pl/78131182-Kody-GTU-grupowanie-towarow-i-uslug  
Jeśli podczas tworzenia nowej faktury chcemy umieścic na niej kody GTU, możemy to zrobić na kilka sposobów.

* `Automatyczne pobieranie kodów z produktów istniejących` - jeśli dodajemy pozycję na fakturze korzystając z ID produktu, które już ma zapisany kod GTU w systemie, wtedy nowa faktura automatycznie pobierze kody GTU wszystkich produktów i wyświetli je na wydruku (jeśli tylko jest włączona opcja zamieszczenia kodów na wydruku faktury)  

* `Podanie kodów wraz z pozycjami na fakturze` - możemy podać kod GTU bezpośrednio dla każdej pozycji na fakturze korzystając z pola `gtu_code`

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"name":"Produkt A1", "tax":23, "total_price_gross":10.23, "quantity":1, "gtu_code": "GTU_01"},
                {"name":"Produkt A2", "tax":0, "total_price_gross":50, "quantity":3,  "gtu_code": "GTU_04"}
            ]
        }}'
```
* `Podanie kodów łącznie dla całej faktury` - możemy również podać zestaw kodów GTU łącznie dla całej faktury korzystając z pola `gtu_codes`. UWAGA - kody te są nadrzędne dla całej faktury - tylko wartości z `gtu_codes` zostaną wyświetlone na fakturze. Według przykładu poniżej na fakturze zostaną wyświetlone tylko kody ` "GTU_03", "GTU_04"`, niezależnie czy na pozycjach zostały podane inne kody GTU, bądź czy produkty z przykładu o id 1 lub 5 mają inne kody zdefiniowane w systemie.

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "kind":"vat",
            "number": null,
            "sell_date": "2013-01-16",
            "issue_date": "2013-01-16",
            "payment_to": "2013-01-23",
            "seller_name": "Wystawca Sp. z o.o.",
            "seller_tax_no": "5252445767",
            "buyer_name": "Klient1 Sp. z o.o.",
            "buyer_email": "buyer@testemail.pl",
            "buyer_tax_no": "5252445767",
            "gtu_codes": ["GTU_03", "GTU_04"],
            "positions":[
                {"product_id": 1, "quantity":2},
                {"product_id": 5, "quantity":4},
                {"name":"Produkt A2", "tax":0, "total_price_gross":50, "quantity":3,  "gtu_code": "GTU_07"} #Kod GTU_07 nie pojawi się na fakturze
            ]
        }
    }'
```


Pole: `gtu_codes` - typy kodów GTU
```shell
Dla produktów:
"GTU_01" - Dostawa napojów alkoholowych - alkoholu etylowego, piwa, wina, napojów fermentowanych i wyrobów pośrednich, w rozumieniu przepisów o podatku akcyzowym
"GTU_02" - Dostawa towarów, o których mowa w art. 103 ust. 5aa ustawy
"GTU_03" - Dostawa oleju opałowego w rozumieniu przepisów o podatku akcyzowym oraz olejów smarowych, pozostałych olejów o kodach CN od 2710 19 71 do 2710 19 99, z wyłączeniem wyrobów o kodzie CN 2710 19 85 (oleje białe, parafina ciekła) oraz smarów plastycznych zaliczanych do kodu CN 2710 19 99, olejów smarowych o kodzie CN 2710 20 90, preparatów smarowych objętych pozycją CN 3403, z wyłączeniem smarów plastycznych objętych tą pozycją
"GTU_04" - Dostawa wyrobów tytoniowych, suszu tytoniowego, płynu do papierosów elektronicznych i wyrobów nowatorskich, w rozumieniu przepisów o podatku akcyzowym
"GTU_05" - Dostawa odpadów - wyłącznie określonych w poz. 79-91 załącznika nr 15 do ustawy
"GTU_06" - Dostawa urządzeń elektronicznych oraz części i materiałów do nich, wyłącznie określonych w poz. 7-9, 59-63, 65, 66, 69 i 94-96 załącznika nr 15 do ustawy
"GTU_07" - Dostawa pojazdów oraz części samochodowych o kodach wyłącznie CN 8701 - 8708 oraz CN 8708 10
"GTU_08" - Dostawa metali szlachetnych oraz nieszlachetnych - wyłącznie określonych w poz. 1-3 załącznika nr 12 do ustawy oraz w poz. 12-25, 33-40, 45, 46, 56 i 78 załącznika nr 15 do ustawy
"GTU_09" - Dostawa leków oraz wyrobów medycznych - produktów leczniczych, środków spożywczych specjalnego przeznaczenia żywieniowego oraz wyrobów medycznych, objętych obowiązkiem zgłoszenia, o którym mowa w art. 37av ust. 1 ustawy z dnia 6 września 2001 r. - Prawo farmaceutyczne (Dz. U. z 2019 r. poz. 499, z późn. zm.)
"GTU_10" - Dostawa budynków, budowli i gruntów

Dla usług:
"GTU_11" - Świadczenie usług w zakresie przenoszenia uprawnień do emisji gazów cieplarnianych, o których mowa w ustawie z dnia 12 czerwca 2015 r. o systemie handlu uprawnieniami do emisji gazów cieplarnianych (Dz. U. z 2018 r. poz. 1201 i 2538 oraz z 2019 r. poz. 730, 1501 i 1532)
"GTU_12" - Świadczenie usług o charakterze niematerialnym - wyłącznie: doradczych, księgowych, prawnych, zarządczych, szkoleniowych, marketingowych, firm centralnych (head offices), reklamowych, badania rynku i opinii publicznej, w zakresie badań naukowych i prac rozwojowych
"GTU_13" - Świadczenie usług transportowych i gospodarki magazynowej - Sekcja H PKWiU 2015 symbol ex 49.4, ex 52.1
```

<a name="clients"/>

## Klienci

<a name="k1"/>
Lista klientów

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&page=1"
```

<a name="k1b"/>
Wyszukiwanie klientów po nazwie, mailu, nazwie skróconej lub numerze NIP

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&name=CLIENT_NAME"
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&email=EMAIL_ADDRESS"
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&shortcut=SHORT_NAME"
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?api_token=API_TOKEN&tax_no=TAX_NO"
```

<a name="k2"/>
Pobranie wybranego klienta po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients/100.json?api_token=API_TOKEN"
```

<a name="k2b"/>
Pobranie wybranego klienta po zewnętrznym ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/clients.json?external_id=100&api_token=API_TOKEN"
```

<a name="k3"/>
Dodanie klienta

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/clients.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Klient1",
            "tax_no": "5252445767",
            "bank" : "bank1",
            "bank_account" : "bank_account1",
            "city" : "city1",
            "country" : "",
            "email" : "email@gmail.com",
            "person" : "person1",
            "post_code" : "post-code1",
            "phone" : "phone1",
            "street" : "street1"
        }}'
```

<a name="k4"/>
Aktualizacja klienta

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/clients/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Klient2",
            "tax_no": "52524457672",
            "bank" : "bank2",
            "bank_account" : "bank_account2",
            "city" : "city2",
            "country" : "PL",
            "email" : "email@gmail.com",
            "person" : "person2",
            "post_code" : "post-code2",
            "phone" : "phone2",
            "street" : "street2"
        }}'
```
<a name="k5"/>
Usunięcie klienta

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/clients/CLIENT_ID.json?api_token=API_TOKEN"
```

<a name="products"/>

## Produkty

<a name="p1"/>
Lista produktów

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products.json?api_token=API_TOKEN&page=1"
```

<a name="p2"/>
Lista produktów ze stanem magazynowym podanego magazynu

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID&page=1"
```

<a name="p3"/>
Pobranie wybranego produktu po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products/100.json?api_token=API_TOKEN"
```

<a name="p4"/>
Pobranie wybranego produktu po ID ze stanem magazynowym podanego magazynu

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/products/100.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID"
```

<a name="p5"/>
Dodanie produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/products.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA",
            "code": "A001",
            "price_net": "100",
            "tax": "23"
        }}'
```

<a name="p6"/>
Aktualizacja produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/products/333.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA2",
            "code": "A0012",
            "price_gross": "102",
	    "tax": "23"
        }}'
```
**Uwaga:** Cenna netto jest wyliczana na podstawie wartości ceny brutto oraz podatku, nie można jej edytować wprost przez API.


<a name="price_lists"/>

## Cenniki

<a name="warehouse_documents"/>

<a name="pricel1"/>
Lista cenników

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/price_lists.json?api_token=API_TOKEN"
```
można przekazywać takie same parametry jakie są przekazywane w aplikacji (na stronie listy faktur)

<a name="pricel2"/>
Dodanie cennika

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/price_lists.json
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "price_list": {
                    "name": "Nazwa cennika",
		    "description": "Opis",
		    "currency": "PLN",
                    "price_list_positions_attributes": {
		    	"0": {
				"priceable_id": "ID produktu",
				"priceable_name": "Nazwa produktu",
				"priceable_type": "Product",
				"use_percentage": "0",
				"percentage": "",
				"price_net": "111.0",
				"price_gross": "136.53",
				"use_tax": "1",
				"tax": "23"
			}
		    }
                }}'
```

<a name="pricel3"/>
Aktualizacja cennika

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/price_lists/100.json
		-X PUT
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "price_list": {
                    "name": "Nazwa cennika",
		    "description": "Opis",
		    "currency": "PLN",
                }}'
```

<a name="pricel4"/>
Usunięcie cennika

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/price_lists/100.json?api_token=API_TOKEN"
```

## Dokumenty magazynowe

<a name="wd1"/>
Wszystkie dokumenty magazynowe

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json?api_token=API_TOKEN"
```
można przekazywać takie same parametry jakie są przekazywane w aplikacji (na stronie listy faktur)

<a name="wd2"/>
Pobranie wybranego dokumentu po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/555.json?api_token=API_TOKEN"
```

<a name="wd3a"/>
Dodanie dokumentu magazynowego MM

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "warehouse_document": {
                    "kind":"mm",
                    "number": null,
                    "warehouse_id": "1",
                    "issue_date": "2017-10-23",
                    "department_name": "Department1 SA",
                    "client_name": "Client1 SA",
                    "warehouse_actions":[
                        {"product_name":"Produkt A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1, "warehouse2_id":13}
                    ]
                }}'
```

<a name="wd3"/>
Dodanie dokumentu magazynowego PZ

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_name":"Produkt A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1},
						{"product_name":"Produkt A2", "purchase_tax":0, "purchase_price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd4"/>
Dodanie dokumentu magazynowego WZ

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"wz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_id":"333", "tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd5"/>
Dodanie dokumentu magazynowego PZ dla istniejącego klienta, działu i produktu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_id": "222",
					"client_id": "111",
					"warehouse_actions":[
						{"product_id":"333", "purchase_tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "purchase_tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd6"/>
Aktualizacja dokumentu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/555.json
			 	-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"warehouse_document": {
						"client_name": "New client name SA"
				    }}'
```

<a name="wd7"/>
Usunięcie dokumentu

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/100.json?api_token=API_TOKEN"
```

<a name="wd8"/>
Połączenie istniejących faktur i dokumentu magazynowego

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouse_documents/555.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"warehouse_document": {
						"invoice_ids": [
							100,
							111
						]
					}}'
```

<a name="payments"/>

## Płatności

<a name="pl1"/>
Wszystkie płatności

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/banking/payments.json?api_token=API_TOKEN"
```

można przekazywać takie same parametry jakie są przekazywane w aplikacji (na stronie listy płatności)


<a name="pl2"/>
Pobranie wybranej płatności po ID

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/banking/payment/100.json?api_token=API_TOKEN"
```

<a name="pl3"/>
Dodawanie nowej płatności

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/banking/payments.json \
				-H 'Accept: application/json' \
				-H 'Content-Type: application/json' \
				-d '{
					"api_token": "API_TOKEN",
					"banking_payment": {
						"name":"Payment 001",
						"price": 100.05,
						"invoice_id": null,
						"paid":true,
						"kind": "api"
				    }}'
```

<a name="pl4"/>
Pobranie płatności wraz z danymi przypiętych faktur

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/banking/payments.json?include=invoices&api_token=API_TOKEN"
```
<a name="pl5"/>
Dodanie nowej płatności powiązanej z istniejącymi fakturami

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/banking/payments.json \
				-H 'Accept: application/json' \
				-H 'Content-Type: application/json' \
				-d '{
					"api_token": "API_TOKEN",
					"banking_payment": {
						"name":"Payment 003",
						"price": 200,
						"invoice_ids": [555, 666],
						"paid":true,
						"kind": "api"
						}}'
```

Trzeba pamiętać, że faktury zostaną opłacone w takiej kolejności, w jakiej zostały podane w atrybucie `invoice_ids` - jeśli faktura o id = 555 została wystawiona na kwotę 100 zł, a faktura o id = 666 na 200 zł, to po dodaniu płatności pierwsza faktura zostanie opłacona w całości, natomiast druga zostanie opłacona w połowie.

<a name="pl6"/>
Aktualizacja płatności

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/banking/payments/555.json
				-X PATCH
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"banking_payment": {
						"name": "New payment name",
						"price": 100
						}}'
```

<a name="pl7"/>
Usuwanie płatności

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/banking/payments/555.json?api_token=API_TOKEN"
```

<a name="categories"/>

## Kategorie

<a name="cat1"/>
Lista wszystkich kategorii

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/categories.json?api_token=API_TOKEN"
```

<a name="cat2"/>
Pobranie pojedycznej kategorii po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/categories/100.json?api_token=API_TOKEN"
```

<a name="cat3"/>
Dodanie nowej kategorii

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/categories.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": null
				}}'
```

<a name="cat4"/>
Aktualizacja kategorii

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/categories/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": "new_description"
				}}'
```


<a name="cat5"/>
Usunięcie kategorii o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/categories/100.json?api_token=API_TOKEN"
```

<a name="warehouses"/>

## Magazyny

<a name="wh1"/>
Lista wszystkich magazynów

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/warehouses.json?api_token=API_TOKEN"
```

<a name="wh2"/>
Pobranie pojedycznego magazynu po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json?api_token=API_TOKEN"
```

<a name="wh3"/>
Dodanie nowego magazynu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouses.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_warehouse",
					"kind": null,
					"description": null
				}}'
```

<a name="wh4"/>
Aktualizacja magazynu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_category",
					"kind": null,
					"description": "new_description"
				}}'
```


<a name="wh5"/>
Usunięcie magazynu o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/warehouses/100.json?api_token=API_TOKEN"
```


<a name="departments"/>

## Działy

<a name="dep1"/>
Lista wszystkich działów

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/departments.json?api_token=API_TOKEN"
```

<a name="dep2"/>
Pobranie pojedycznego działu po ID

```shell
curl "http://YOUR_DOMAIN.fakturownia.pl/departments/100.json?api_token=API_TOKEN"
```

<a name="dep3"/>
Dodanie nowego działu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/departments.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"department": {
					"name":"my_warehouse",
					"shortcut": "short_name",
					"tax_no": "-"
				}}'
```

<a name="dep4"/>
Aktualizacja działu

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/departments/100.json
        -X PUT
        -H 'Accept: application/json'
        -H 'Content-Type: application/json'
        -d '{
        "api_token": "API_TOKEN",
        "department": {
          "name":"new_name",
          "shortcut": "new_short_name",
          "tax_no": "xxx-xxx-xx-xx"
        }}'
```

<a name="dep5"/>
Usunięcie działu o podanym ID

```shell
curl -X DELETE "https://YOUR_DOMAIN.fakturownia.pl/departments/100.json?api_token=API_TOKEN"
```


<a name="get_token_by_api"/>

## Logowanie i pobranie danych przez API

```shell
curl https://app.fakturownia.pl/login.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
            "login": "login_or_email",
            "password": "password",
	    "integration_token": ""
    }'
```

To zapytanie zwraca token i informacje o URL konta w Fakturowni (pola `prefix` i `url`):

```shell
{
	"login":"marcin",
	"email":"email@test.pl",
	"prefix":"YYYYYYY",
	"url":"https://YYYYYYY.fakturownia.pl",
	"first_name":"Jan",
	"last_name":"Kowalski",
	"api_token":"XXXXXXXXXXXXXX"
}
```

UWAGA: api_token jest zwracany tylko jeśli dany użytkownik ma wygenerowany API Token (użytkownik może go dodać w ustawieniach konta)


<a name="accounts"/>

## Konta Systemowe

Jest to opcja dla Partnerów, którzy chcą zakładać konta Fakturowni z poziomu swojej aplikacji. Np. mogą to być
dostawcy sklepów internetowych, systemów rezerwacji itp lub innych systemów którzy chcą udostępnić swoim użytkownikom funkcjonalność wystawiania faktur.

Klient w portalu Partnera jednym przyciskiem może założyć konto i od razu zacząć wystawiać faktury (nie musi samodzielnie zakładać konta w Fakturownia.pl)

Pola: user.login, user.from_partner, user, company nie są wymagane

```shell
curl https://YOUR_DOMAIN.fakturownia.pl/account.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
            "api_token": "API_TOKEN",
            "account": {
                "prefix": "prefix1",
		"lang": "pl"
            },
            "user": {
                "login": "login1",
                "email": "email1@email.pl",
                "password": "password1",
                "from_partner": "PARTNER_CODE"
            },
            "company": {
                "name": "Company1",
                "tax_no": "5252445700",
                "post_code": "00-112",
                "city": "Warsaw",
                "street": "Street 1/10",
                "person": "Jan Nowak",
                "bank": "Bank1",
                "bank_account": "111222333444555666111"
            },
	    "integration_token": ""
        }'

```

UWAGA: parametr ```integration_token``` jest wymagany, jeśli chcemy pobrać aktualny api_token użytkownika (aby otrzymać integration_token dla Twojej zintegrowanej aplikacji prosimy o kontakt z nami)


Po utworzeniu konta zwracane są:

```shell

{
	"prefix":"prefix126", - prefix utworzonego konta (moze byc innny niz podany, gdy podany już istniał)
	"api_token":"62YPJfIekoo111111", - kod dostepu do utworzonego konta
	"url":"https://prefix126.fakturownia.pl", - url utworzonego konta
	"login":"login1", - login użytkownika  (moze byc innny niz podany, gdy podany już istniał)
	"email":"email1@email.pl"
}
```

Inne pola dostępne przy tworzeniu nowego konta (pomocne przy integracji)

```shell
	"account": {
		"prefix": "prefix-konta",
		"lang": "pl",
		"integration_fast_login": true - umożliwia automatyczne logowanie Twoich użytkowników w Fakturowni
		"integration_logout_url": "http://twojastrona.pl/" - umożliwia powrót Twoich użytkowników na Twoją stronę po ich wylogowaniu się z Fakturowni
	}
```

Pobranie informacji o koncie:

```shell
curl "https://YOUR_DOMAIN.fakturownia.pl/account.json?api_token=API_TOKEN&integration_token="
```

<a name="codes"/>

## Przykłady w PHP i Ruby

<https://github.com/radgost/fakturownia-api/blob/master/example1.php/>

<https://github.com/radgost/fakturownia-api/blob/master/example1.rb/>

Ruby Gem do integracji z Fakturownia.pl: <https://github.com/kkempin/fakturownia/>
