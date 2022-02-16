# Libray importieren. Das ist vorgefertigter Code, der mir fast die ganze Arbeit abnimmt
from bs4 import BeautifulSoup
import csv

# Weil das Programm doch einige Zeit braucht soll es mir sagen, dass es wenigstens läuft
print("running...")

# Ich habe mir die Webseite heruntergeladen und lese sie hier ein
with open('uruk3-1.html', 'r') as fp:
    html_content = fp.read()
    site = BeautifulSoup(html_content, 'html.parser')

    # CSV-Datei anlegen und den Header einfügen
    with open('uruk3-1.csv', 'w') as f:
        writer = csv.writer(f)
        header = ['Name', 'Typ', 'Provinienz', 'Zeit', 'Text']
        writer.writerow(header)

        # Das ganze Seitenlayout ist eine riesige Tabelle. Zum Glück hat jemand dran gedacht, die einzelnen Objekte
        # mit dem Klassennamen full_object_table zu versehen. Die extrahiere ich mir hier und packe sie in eine Liste.
        tablets = site.findAll("table", class_="full_object_table")

    # Die folgende Routine mache ich für alle Objekte auf der Seite
        for tablet in tablets:
            # eine Reihe im CSV, wo ich nach und nach alle Infos anfüge
            row = []

            # Nur diejenigen Objekte, die Tablets oder Tags sind
            titles = tablet.findAll("b")
            if titles[1].string == "Tablet" or titles[1].string == "Tag" or titles[1].string is None:

                row.append(titles[0].string)

                # Bei einigen Einträgen fehlt die Produktkategorie, es soll unknown reinschreiben damits beim Import kein Problem gibt
                if titles[1].string is None:
                    row.append("unknown")
                else:
                    row.append(titles[1].string)

                # Ich suche mir den Teil vom Layout, wo die Beschreibungen drin sind und packe mir die Zeilen in eine Liste
                description = tablet.table.findAll("tr")

                # von den 25 Zeilen brauche ich nur Provenience und Period, also Zeile 8 und 10. Wir zählen in der Informatik aber ab 0.
                provenience = description[7].findAll("td")
                period = description[9].findAll("td")

                # Davon nicht die Zeilenbeschreibung, sondern der Text in der 2. Spalte
                row.append(provenience[1].string)
                row.append(period[1].string)

                # Jetzt brauchen wir noch die Transliterationen, ohne Übersetzungen. Ich packe alle Zeilen in eine Liste:
                spans = tablet.findAll("span", text=True)

                # Gib mir alle Übersetzungs-Zeilen und mache draus einen langen Text mit Zeilenumbruch. Vorsichtshalber mit Fehlerbehandlung
                text = ''
                for line in spans:
                    try:
                        text += line.string + '\n'
                    except:
                        print('error at ' + titles[0].string)

                row.append(text)

                # Jetzt die Zeile mit allen Infos ins CSV schreiben
                writer.writerow(row)

                # Zur Sicherheit überprüfe ich noch, was es neben Tags und Tablets gibt. Könnte ja ein beschriftetes Siegel dabei sein, das ich einprogrammieren muss.
            else:
                print(titles[1].string)

# Ich will, dass mir das Programm sagt, wenn es fertig ist.
print("done");

