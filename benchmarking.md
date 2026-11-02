# BiasRadar – Konkurrenz- und Kooperationsanalyse


## 1 Zweck, Methode und Grenzen

BiasRadar soll Textkorpora **vor dem Training** eines KI-Modells auf Verzerrungen entlang geschützter Merkmale prüfen und die Ergebnisse in einem verständlichen Bericht zusammenfassen. Dieses Dokument ordnet das Vorhaben in die bestehende Landschaft ein: Was gibt es schon, was davon ist nah an unserer Fragestellung, worauf können wir aufbauen und wo müssen wir uns klar abgrenzen?

**Methode.** Desk Research (Dokumentation, Repositories, Paper-Abstracts, Paketindizes, Gesetzestext). Wir haben die Werkzeuge **nicht selbst installiert oder getestet**. Aussagen über Funktionsumfang beruhen auf der jeweiligen Dokumentation. Was wir nicht prüfen konnten, ist im Text als *ungeprüft* markiert.

**Grenzen.** Die Suche ist nicht erschöpfend. Wir erheben keinen Anspruch darauf, alle existierenden Ansätze gefunden zu haben, und freuen uns über Hinweise auf Lücken.

## 2 Landkarte: Auf welcher Ebene setzen Werkzeuge an?

| Ebene | Frage | Beispiele |
|---|---|---|
| **Modell / Vorhersagen** | Behandelt ein trainiertes System Gruppen unterschiedlich? | AI Fairness 360 (Teilfunktion), Aequitas |
| **Strukturierte Daten** | Sind Gruppen in einer Tabelle ungleich verteilt oder verknüpft? | AI Fairness 360 (Dataset-Metriken), Aequitas, Unsupervised Bias Detection Tool (siehe 3.8) |
| **Dokumentation von Datensätzen** | Welche Eigenschaften, Risiken, Herkunft hat ein Datensatz? | Dataset Nutrition Label, Dataset Cards |
| **Embeddings** | Enthält ein Embedding-Modell stereotype Assoziationen? | WEFE |
| **Rohtext-Korpus** | Welche Gruppen kommen wie vor und mit welchen Wörtern? | GenBit, akademische Pipelines, BiasRadar |

BiasRadar bewegt sich in der letzten Zeile. Dort ist die Auswahl an gepflegten, auffindbaren Werkzeugen deutlich dünner als in den anderen Zeilen. Dünn heißt aber nicht leer (siehe 3.3).

## 3 Einzelanalysen

### 3.1 AI Fairness 360 (IBM)

**Was es ist.** Eine quelloffene Python-Bibliothek (Apache 2.0), um Bias in ML-Modellen über den Lebenszyklus hinweg zu erkennen und zu mindern. Sie umfasst Datensätze, Schätzer, Metriken und Detektoren, dazu Mitigationsalgorithmen, die nach Eingriffszeitpunkt gegliedert sind (vor, während, nach dem Training). Eine interaktive Weboberfläche führt in die Konzepte ein.

**Wie es arbeitet.** Gruppenfairness-Metriken vergleichen statistische Kennzahlen zwischen Untergruppen, die über geschützte Attribute definiert sind. Diese werden als Spalte oder Array übergeben. Die Beispieldatensätze sind tabellarisch (z. B. COMPAS).

**Verhältnis zu BiasRadar.** In den von uns gesichteten Quellen ist keine Analyse unstrukturierter Textkorpora beschrieben. Die Metriken setzen Labels und geschützte Attribute als strukturierte Daten voraus; ein Rohtext liefert beides nicht ohne Vorverarbeitung. AIF360 ist daher kein direkter Konkurrent, sondern eine Referenz für die Modell- und Tabellenebene. Als Vorbild nehmen wir die verständliche Erklärung der Metriken.

### 3.2 Data Nutrition Project / Dataset Nutrition Label

**Was es ist.** Eine gemeinnützige Organisation (USA, laut Verzeichnis aktiv), die ein „Nährwertlabel" für Datensätze als Standard entwickelt und ein kostenloses Tool anbietet. Die dritte Generation des Labels erfasst unter anderem Verwendungszweck, Aufbereitung und Kuratierung, ethische oder technische Prüfungen, enthaltene Subpopulationen sowie Risiken und Einschränkungen.

**Wie es arbeitet.** Das Label ist primär ein **Dokumentationsrahmen**. Das Gründungspapier (2018) beschreibt es als modular aufgebaut: qualitative und quantitative Module, die über verschiedene statistische Backends erzeugt, aber einheitlich dargestellt werden. Dort wird auch als Grenze genannt, dass „Ground-Truth"-Vergleichsdaten selbst verzerrt sein können. Laut Consumer Reports war ab 2023 ein Label-Maker-Tool (Beta) geplant; ob und wie es heute vorliegt, haben wir *nicht geprüft*.

**Verhältnis zu BiasRadar.** Kein Wettbewerber im engeren Sinn: Das Label dokumentiert, BiasRadar misst. Es ist ein möglicher **Anschlusspunkt** (siehe 6). Ob das Label ein maschinenlesbares Schema oder einen Export bietet, haben wir *nicht geprüft*.

### 3.3 GenBit (Microsoft) – das nächstverwandte Werkzeug

**Was es ist.** Eine Python-Bibliothek zur Messung von Gender-Bias in NLP-Datensätzen (Lizenz laut PyPI: MIT). Sie stützt sich auf Ideen von Bordia und Bowman (2019) und analysiert Korpora über Ko-Okkurrenz-Statistiken: Wie stark ist ein beliebiges Wort mit vordefinierten weiblichen, männlichen und weiteren Geschlechts-Definitionswörtern assoziiert?

**Funktionsumfang laut README.**
- Zentrale Kennzahl ist der `genbit_score` (durchschnittlicher absoluter bedingter Log-Quotient pro Token), ergänzt um Anteile der Definitionswörter und weitere Statistiken.
- Wortlisten für Englisch inkl. nicht-binär, trans und cis; für die übrigen Sprachen, darunter Deutsch, nur binär.
- Referenzbereiche für Scores, abgeleitet aus bestimmten Datensatztypen, und eine empfohlene Mindestgröße der Korpora.
- Wortlisten werden als Definitionswörter geführt (z. B. Pronomen, Verwandtschaftsbezeichnungen). Eigennamen als Signal sind in der README nicht beschrieben.

**Status.** Letzte PyPI-Version 2.2.0.0 vom 19. Dezember 2022. Das GitHub-Repository wurde laut Seitenanzeige am **15. Juni 2026 archiviert** (nur lesbar).

**Verhältnis zu BiasRadar.** Hier liegt die größte inhaltliche Überschneidung: Unser geplanter Ko-Okkurrenz-/PMI-Baustein ist methodisch verwandt. Wir sehen das offen so:
- Das Prinzip „Assoziation mit Gruppenwörtern über Ko-Okkurrenz" ist **nicht neu** und bei GenBit dokumentiert und publiziert.
- Was GenBit laut Dokumentation nicht abdeckt: weitere Merkmale jenseits von Geschlecht (Herkunft, Religion), ein kombinierter Bericht über mehrere Prüfverfahren, aktive Pflege.
- Aufgrund der MIT-Lizenz wären Nachnutzung von Code und Konzepten prinzipiell möglich. Ob wir forken, einzelne Teile übernehmen oder eigenständig implementieren, ist eine begründungspflichtige Entscheidung (siehe 7).

### 3.4 WEFE – Word Embeddings Fairness Evaluation Framework

**Was es ist.** Ein quelloffenes Framework (IJCAI 2020), das Fairness-Metriken für Word Embeddings kapselt, vergleichbar macht und Rankings über Modelle und Kriterien erlaubt. Eingabe sind vortrainierte Embeddings sowie Mengen von Zielwörtern und Attributwörtern.

**Verhältnis zu BiasRadar.** WEFE misst Bias **in Embedding-Modellen**, nicht im Rohtext. Es ist deshalb **kein Konkurrent, sondern ein Baustein**: Für den Embedding-Test planen wir, auf dem Korpus ein Embedding zu trainieren (z. B. mit Gensim) und es mit WEFE zu prüfen. Wichtig: Dieser Test misst, was ein Embedding aus dem Korpus *gelernt* hat. Das ist eine andere Aussage als eine direkte Zählung im Text, deshalb ergänzen sich beide Verfahren.

### 3.5 Akademische Pipelines für Pretraining-Daten

Ein Paper auf arXiv (2504.14212) beschreibt eine skalierbare Annotations-Pipeline aus Schutzmerkmals-Erkennung und „Regard"-Klassifikation, angewendet auf eine Teilmenge von Common Crawl. Die frequenzbasierte Wortassoziationsanalyse nach Bordia und Bowman wird dort um Regard-Informationen verfeinert. Das ist konzeptionell **nah an unserem Vorhaben** (Erkennung von Gruppenbezügen plus Assoziationsanalyse auf Korpusebene). Ob zugehöriger Code veröffentlicht ist und unter welcher Lizenz, haben wir *nicht geprüft*. Es dient uns vorerst als Literaturreferenz und als Hinweis darauf, dass der Ansatz wissenschaftlich anschlussfähig ist.

### 3.6 Toxic Commons / Celadon

Ein Klassifikator für schädliche Inhalte in Pretraining-Daten (laut Beschreibung neun Sprachen), der u. a. nach Dimensionen wie Geschlecht, Religion und Behinderung kategorisiert. Auffällige Texte werden mit Inhaltswarnungen versehen oder synthetisch umgeschrieben. Das Ziel ist **Moderation und Aufbereitung** von Trainingsdaten, nicht ein erklärender Prüfbericht über Verzerrungen. Der Ansatz (Klassifikator, ggf. LLM-gestützt) unterscheidet sich von unserem (Zählung, Assoziation, Embedding-Test).

### 3.7 Weitere Einordnung

- **Dataset Cards / Data Cards für NLP** (Hugging Face, GEM) sind Dokumentationsvorlagen für Datensätze. Sie liefern keine Messwerte, sind aber ein naheliegendes Zielformat für unseren Bericht.
- **BiasAware** (Hugging Face Space der Organisation AVID) beschreibt sich als Werkzeug zur Erkennung und Quantifizierung von Bias in Textdaten und bindet u. a. die GenBit-Methodik ein. Wir haben nur den Quellcode gesehen; Status, Lizenz und Umfang sind *ungeprüft*.

### 3.8 Tabellarische Datensätze: etablierte Verfahren

Für strukturierte Daten ist die Bias-Analyse deutlich ausgereifter als für Rohtext. Der Grund: Geschützte Merkmale liegen dort als Spalten vor oder lassen sich direkt ableiten.

**Typische Prüfungen** (Standardpraxis, nicht auf ein Werkzeug beschränkt):
- Repräsentation: Wie groß sind die Gruppen im Vergleich zur Zielpopulation?
- Ergebnisraten je Gruppe: Wie häufig tritt das positive Label pro Gruppe auf? Daraus entstehen Kennzahlen wie Statistical Parity Difference oder Disparate Impact (Verhältnis der Raten).
- Proxy-Prüfung: Korrelieren scheinbar neutrale Spalten stark mit einem geschützten Merkmal?
- Datenqualität je Gruppe, etwa ungleich verteilte fehlende Werte.

**Werkzeuge:**
- **AI Fairness 360** (siehe 3.1): Gruppenmetriken auf Datensätzen und Modellausgaben, dazu Mitigationsverfahren, die schon vor dem Training an den Daten ansetzen.
- **Aequitas**: quelloffenes Toolkit für Bias-Audits und Fair ML. Laut Quickstart werden Zusammenfassungen und Disparitäts-Plots je Gruppe und Metrik erzeugt; der Schwerpunkt liegt auf Modellvorhersagen.
- **Unsupervised Bias Detection Tool**: für den Fall, dass keine Daten zu demografischen Gruppen vorliegen. Es sucht Cluster, die potenziell ungleich behandelte Gruppen darstellen, und ist als Python-Paket und Web-Anwendung verfügbar (Paper: arXiv 2502.01713, Kontext: Prüfung eines Risikoprofilierungs-Algorithmus einer niederländischen Behörde).
- **Dataset Nutrition Label** (siehe 3.2): dokumentiert u. a. demografische Repräsentation, Verteilungen und fehlende Daten.

**Verhältnis zu BiasRadar.**
- Diese Verfahren sind für unsere Fragestellung **nicht direkt anwendbar**, weil sie geschützte Merkmale als Spalten voraussetzen. Bei Rohtext ist die Erkennung dieser Merkmale (Namen, Pronomen, Herkunftsbegriffe) gerade der Kern des Problems.
- Sie sind aber ein **methodisches Vorbild**: Die Kennzahlen (Raten je Gruppe, Verhältnisse, Repräsentation) lassen sich als Berichtsstruktur denken, die Fachleute aus der Tabellenwelt bereits kennen.
- Das Unsupervised Bias Detection Tool zeigt, dass Bias-Suche auch ohne vorab definierte Gruppen publiziert und als Open Source verfügbar ist. Ob sich der Gedanke auf Text übertragen lässt, haben wir nicht geprüft.

## 4 Vergleichsmatrix

| | AIF360 | Dataset Nutrition Label | GenBit | WEFE | BiasRadar (geplant) |
|---|---|---|---|---|---|
| Ebene | Modell, Tabellen | Dokumentation | Rohtext-Korpus | Embeddings | Rohtext-Korpus |
| Eingabe | strukturierte Daten mit Labels | Metadaten, Selbstauskunft | Textkorpus | vortrainiertes Embedding | Textkorpus (Text/CSV/JSON) |
| Merkmale | geschützte Attribute (Spalten) | frei dokumentierbar | Geschlecht | u. a. Geschlecht, Herkunft, Religion (über Wortlisten) | Zunächst Geschlecht, danach geplant: Herkunft, Hautfarbe, Religion |
| Methode | Gruppenmetriken, Mitigation | Label-Module | Ko-Okkurrenz | Assoziationstests (z. B. WEAT) | NER, Ko-Okkurrenz/PMI, Embedding-Test |
| Ausgabe | Metriken, Mitigation | Label | Scores, Wortstatistik | Rankings, Plots | verständlicher Bericht (HTML/PDF) |
| Lizenz | Apache 2.0 | nicht geprüft | MIT (laut PyPI) | nicht geprüft | FOSS (Lizenz noch festzulegen) |
| Status | aktiv (nicht im Detail geprüft) | laut Verzeichnis aktiv | Repo archiviert (06/2026) | nicht geprüft | Prototyp im Förderzeitraum |

## 5 Unsere Abgrenzung

**Was nicht neu ist, und wir auch nicht so verkaufen:**
- Ko-Okkurrenz-Analyse auf Korpusebene gegen Gruppenwörter (GenBit).
- Assoziationstests auf Embeddings (WEFE).
- Die Idee, Datensätze standardisiert zu dokumentieren (Dataset Nutrition Label, Dataset Cards).
- Akademische Pipelines für Schutzmerkmals-Erkennung in großen Textkorpora (arXiv 2504.14212).

**Was wir beitragen wollen (geplant, nicht bewiesen):**
1. **Mehrere Verfahren in einem Lauf**, mit einem gemeinsamen Bericht statt einzelner Kennzahlen.
2. **Schrittweise mehr Merkmale als Geschlecht** (geplant: Herkunft, Hautfarbe, Religion), unter anderem über Namens- und Herkunftsbegriff-Erkennung (NER).
3. **Verständlichkeit für kleine und mittlere Teams ohne Fairness-Expertise**: Bericht mit Beispielstellen, nicht nur Scores.
4. **Lokale Ausführung**: Korpora verlassen den Rechner der Nutzer:innen nicht (Datenschutz und Ressourcenschonung).
5. **Bezug zu den Dokumentationspflichten für Trainingsdaten** nach Art. 10 der EU-KI-Verordnung. Dort sind Bias-Prüfung (Abs. 2 f) und geeignete Maßnahmen (Abs. 2 g) für Hochrisiko-Systeme verankert. BiasRadar ersetzt keine Konformitätsprüfung, sondern liefert ein Dokumentationsartefakt für die Prüfung.

**Offen, bevor wir Punkt 1 bis 3 als Vorteil behaupten:** Wir müssen zeigen, dass die Verfahren auf realen Korpora brauchbare Ergebnisse liefern. **[OFFEN: Evaluationskonzept ergänzen – Referenzkorpora, Annotation, Kennzahlen für Zuverlässigkeit.]**

**Ideenskizze (Hypothese, noch nicht geprüft):** Tabellarische Datensätze mit bekanntem Bias könnten als Referenz dienen. Die Datensätze würden in Text überführt (z. B. über Vorlagensätze), sodass die Verzerrung bekannt ist, und anschließend prüfen wir, ob BiasRadar sie in der erwarteten Stärke wiederfindet. Offen ist, wie realistisch solche künstlich erzeugten Texte im Vergleich zu echten Korpora sind. Wir behandeln das als einen möglichen Baustein neben annotierten echten Korpora, nicht als Ersatz.

## 6 Kooperations- und Anschlussmöglichkeiten

| Akteur | Art | Möglicher Anknüpfungspunkt | Voraussetzung / Stand |
|---|---|---|---|
| Data Nutrition Project | Dokumentationsstandard | BiasRadar-Ergebnisse (Subpopulationen, Risiken) als Beitrag zu einem Label | Schema/Export des Labels *nicht geprüft*; Kontaktaufnahme nötig |
| WEFE (Universidad de Chile) | Bibliothek | Nutzung als Abhängigkeit; Rückmeldungen und Beiträge an das Projekt | Lizenz und Pflegestatus prüfen |
| GenBit (Microsoft) | Bibliothek, archiviert | Vergleichsmessungen als Plausibilitätscheck; Wiederverwendung von Konzepten und ggf. Wortlisten | MIT laut PyPI; Lizenz der Wortlisten und Fork-Option prüfen |
| Hugging Face Dataset Cards | Dokumentationsformat | Bericht-Export als Abschnitt einer Dataset Card | Format ist offen dokumentiert; konkrete Umsetzung offen |
| AI Fairness 360 | Bibliothek | Konzeptionelles Vorbild für Metrik-Erklärungen; keine direkte Anbindung an Text erkannt | – |

Kooperationen mit Praxispartnern aus der Zivilgesellschaft sind **nicht Gegenstand dieses Dokuments**.

## 7 Offene Fragen und Risiken

1. **Überschneidung mit GenBit.** Wenn unser Kern-Baustein methodisch GenBit ähnelt, muss die Entscheidung „Fork, Teilübernahme oder Eigenentwicklung" begründet sein. Archiviert heißt nicht automatisch ungeeignet; es heißt, dass niemand mehr Fehler behebt.
2. **Zuverlässigkeit.** Wortlistenbasierte und NER-basierte Verfahren können Gruppenbezüge übersehen oder falsch zuordnen (Mehrdeutigkeit, Eigennamen, Sprache). Das ist unsere Einschätzung; es braucht eine Messung im Projekt, nicht nur eine Behauptung.
3. **Sprachabdeckung.** GenBit bietet Deutsch nur binär. **[OFFEN: Welche Sprachen decken wir im Prototyp ab?]**
4. **Ungeprüfte Punkte:** Schema/Export des Dataset Nutrition Labels; Code und Lizenz der akademischen Pipeline (arXiv 2504.14212); Status und Umfang von BiasAware; Lizenzen von WEFE und der GenBit-Wortlisten; aktueller Pflegestand von AIF360.
5. **Regulatorischer Zeitplan.** Zu den Anwendungsterminen der Pflichten aus Art. 10 gibt es abweichende Angaben in Sekundärquellen. Vor jeder Aussage dazu im Antrag ist die amtliche Quelle zu prüfen.

## 8 Quellen (Abruf: Oktober 2026)

- AI Fairness 360, Getting Started: https://aif360.readthedocs.io/en/latest/Getting%20Started.html
- AI Fairness 360, IBM Research (Lizenz, Überblick): https://researcher.ibm.com/publications/ai-fairness-360-an-extensible-toolkit-for-detecting-and-mitigating-algorithmic-bias
- Data Nutrition Project (Verzeichniseintrag): https://civictech.guide/projects/the-data-nutrition-project
- Dataset Nutrition Label, Gründungspapier 2018: https://huggingface.co/papers/1805.03677
- Dataset Nutrition Label, 2nd Gen: https://arxiv.org/abs/2201.03954v2
- Data Nutrition Project bei Consumer Reports: https://innovation.consumerreports.org/initiatives/data-nutrition-project
- GenBit (Repository): https://github.com/microsoft/responsible-ai-toolbox-genbit
- GenBit (PyPI): https://pypi.org/project/genbit/
- Bordia & Bowman 2019: https://arxiv.org/abs/1904.03035
- WEFE (IJCAI 2020): https://www.ijcai.org/proceedings/2020/60
- Bias Analysis and Mitigation through Protected Attribute Detection and Regard Classification: https://arxiv.org/pdf/2504.14212
- Toxic Commons / Celadon: https://huggingface.co/blog/catherinearnett/toxic-commons
- Aequitas: https://github.com/dssg/aequitas
- Auditing a Dutch Public Sector Risk Profiling Algorithm Using an Unsupervised Bias Detection Tool: https://arxiv.org/pdf/2502.01713
- Hugging Face, Landscape of ML Documentation Tools: https://huggingface.co/docs/hub/model-card-landscape-analysis
- BiasAware (Quellcode des Space): https://huggingface.co/spaces/avid-ml/biasaware/blob/1dc0d8b744687508587281bec43664e5d8cfdde2/app.py
- EU-KI-Verordnung, Art. 10: https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-10
