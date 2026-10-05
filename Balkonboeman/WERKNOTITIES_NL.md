# Werknotities — geen inleverklare manual

Dit is een feitelijke ordening van het gesprek en het beschikbare bewijs. Schrijf de Engelstalige uitleg zelf. Vul ontbrekende details aan en controleer claims tegen je eigen uitvoering.

## 1. Arduino-check
- Uno aangesloten, board/poort gekozen en Blink geüpload.
- Aanvankelijk leek het lampje continu te branden.
- Joep bevestigde daarna: ON blijft aan; het oranje ingebouwde lampje knippert. Niet bewezen dat een codewijziging nodig was: dit was vooral onderscheid tussen voedingsled en testled.
- Bewijs: boardselectie, Blink-menu en video.

## 2. Camera en voorbeeldmodel
- Grove Vision AI V1 op Uno met Base Shield gebruikt; Grove-kabel naar I2C.
- object_detection-voorbeeld uit Seeed Arduino GroveAI. Serial Monitor 115200.
- Bestaand persoonsmodel gaf aantallen en confidence-waarden. Joep testte van vriendin naar tafel en bevestigde dat het werkte.
- Dit is geen duifherkenning. Een labeltekst veranderen verandert het getrainde model niet.
- Exacte pinlabels en schakelaarstand nog door Joep bevestigen voor een herhaalbare aansluitinstructie.

## 3. Viewerproblemen
- Symptomen: lege modelkeuze; Model Invalid Or Not Existent; USB transferIn AbortError; Invoke Failed in serial.
- Pogingen zonder aangetoond herstel: Uno-reset ingedrukt houden tijdens reconnect; juiste USB-device controleren; Serial Monitor sluiten.
- Herstel: viewer sluiten/disconnect, beide USB's los, Uno eerst aansluiten, camera daarna, Uno reset. Serial-resultaten keerden terug. Viewer daarna opnieuw geopend; livebeeld werkte.
- Oorzaak niet bewezen omdat meerdere handelingen samen zijn veranderd. Geen willekeurige draadwissel als bewezen oplossing opvoeren.

## 4. Colab en dependencies
- Officiële oudere Seeed-notebook op modern Colab gaf problemen.
- Eerste runtime Python 3.13: TensorFlow 2.9 niet beschikbaar. Runtime 2025.07 gaf Python 3.11, maar TF2.9 bleef niet beschikbaar.
- Trainingrequirements apart gemaakt zonder vastgepinde tensorflow== en keras==; NumPy<2 en OpenCV<4.12 toegevoegd. Export is daarmee NIET opgelost.
- Roboflow-installatie upgrade vervolgens NumPy naar 2.2.6. Opgelost met installatieconstraints: roboflow + numpy==1.26.4 + opencv-python<4.12 + opencv-python-headless<4.12.
- Meerdere Colab-sessies: oude sessies via Manage sessions beëindigd.
- Herhaald klonen gaf geneste repositorymappen. Absolute /content/yolov5-swift en alleen clonen als map ontbreekt toegepast.
- Dataset-download bevestigd door data.yaml: Crow, Pigeon; twee klassen; train/valid-paden.

## 5. Trainingsfouten
- PyTorch 2.6 weights_only-fout blokkeerde legacycheckpoint. Voor het trainingsproces TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=1 gebruikt met de rechtstreeks van Seeed gedownloade weights. Niet toepassen op onbekende checkpoints; laden kan code uitvoeren.
- np.int ontbrak in NumPy. Eerst alleen datasets.py aangepast. Volgende run stopte in general.py.
- Daarna exact np.int vervangen door int in repository-Pythonbestanden met regexwoordgrenzen; np.int64 niet veranderd. Joep meldde: Aangepast general.py; Reparatie klaar.
- Training liep vervolgens 30 epochs. FutureWarning bij autocast verhinderde dit niet.
- Pillow FreeTypeFont.getsize-fout in plot_images-thread: voorbeeldfiguren mislukt; eindresultaten en checkpoints waren wel aanwezig. Nog GEEN geteste reparatie van deze Pillowfout.
- W&B-login timeout schakelde optionele tracking uit; geen geregistreerde train-stop daardoor.

## 6. Resultaten
- best.pt, last.pt en results.csv door Joep als aanwezig gerapporteerd; bestanden zelf niet in dit pakket.
- 30 rondes. Laatste CSV mAP0.5 0.069359. Recente rondes verbeteren, maar prestaties blijven laag.
- Finaleval all: P0.626 R0.146 mAP0.5 0.0702 mAP0.5:0.95 0.0231.
- Crow: P0.252 R0.292 mAP0.5 0.14.
- Pigeon: P1 R0 mAP0.5 0.000179. Geen succesvolle duifdetectie aangetoond.
- Displaycode had aanvankelijk een eigen fout: variabele map overschaduwde Pythonfunctie map(). Aangepast naar trainmap en listcomprehension. Drie voorbeeldbeelden verschenen.
- Witte vogel links in derde afbeelding lijkt duif maar label Crow. Verdacht label; geen volledige dataset-audit gedaan.
- Samenvoegen naar vogel voorgesteld, maar keuze/uitvoering nog niet bevestigd. Niet als afgeronde verbetering opvoeren.

## Zelf aanvullen per stap
1. Wat is het doel voor de lezer?
2. Welke exacte klik/handeling/code heb je gebruikt?
3. Wat moet de lezer zien wanneer de stap lukt?
4. Welke fout trad op? Welk beeld bewijst dat?
5. Welke alternatieven heb je echt getest? Welke hielp?
6. Wat is nog onzeker of niet uitgevoerd?

## Privacy en publicatie
- Alleen technische beelden geselecteerd; geen herkenbare persoonsfoto opgenomen.
- Voor publicatie nogmaals alle beelden/logs controleren op sleutels, wachtwoorden en persoonlijke gegevens.
- Geen Telegramtokens uit eerdere chat gebruiken of publiceren.
- README is AI-georganiseerde invulstructuur, niet de definitieve eigen manual.
