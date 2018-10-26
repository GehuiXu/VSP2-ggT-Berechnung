# VSP2-ggT-Berechnung


--------------------
Compilieren der Dateien:
--------------------
Zu dem Paket gehören die Dateien
ggt_process.erl; ggt_starter.erl; koordinator.erl; nameservice.erl; start_starter.erl;
util.erl; vsutil.erl; ko_send.erl; ns_send.erl;

sowie:
Readme.txt; ggt.cfg; koordinator.cfg

1> make:all().
% oder
1> c(<Dateiname>).

--------------------
Starten der Nodes:
--------------------
(w)erl -(s)name <KoordinatorName> -setcookie zummsel
(w)erl -(s)name <NameserviceName> -setcookie zummsel
(w)erl -(s)name <ggTPName> -setcookie zummsel

Starten des Namensdienstes:
--------------------------
1>nameservice:start( ).

ns_send:msg(State,NameServiceNode)
- State: einen der Nachrichten help, reset, list oder kill
- NameServiceNode: Name der Node, auf dem der Namensdienst gestartet wurde
% global:registered_names() zum nachschauen

Starten des Koordinators:
--------------------------
2>koordinator:start( ).

% liest die koordinator.cfg ein:
% {arbeitszeit, 3}:			simulierte Arbeitszeit für die ggT-Berechnung
% {termzeit, 3}:			Wenn termzeit lang keine Berechnung dann wird Terminierungsabstimmung initiiert
% {ggtprozessnummer, 42}:		Anzahl ggT Prozesse je Starter (und default ggT)
% {nameservicenode, 'ns@KI-VS'}:	node des Namensdienstes
% {koordinatorname, euklid}:		Name des Koordinators
% {quote, 80}:				Abstimmungsquote fuer Terminierung
% {korrigieren, 0}:			Korigiert falsche Terminierungsmeldungen (== 1 korrigieren, == 0 nicht korrigieren)

ko_send:msg(State)
- State: einen der Nachrichten help, vals (Steuerungswerte), ggt (Zufalls ggT), {ggt,Wggt}, step (Beendet Anmeldephase), nudge (eine Art ping), prompt (aktuelle Mi's), reset oder kill
  prompt: Frage nach Mi;  nudge (schubsen): Lebenszustand
  vals: Steuerwerte; ggt: Zufallswunschggt
- und liest koordinator.cfg ein

Starten der ggT-Prozesse:
--------------------------
3>start_starter:go(Anzahl,Start).

- Anzahl: Anzahl der Starter auf einer Node, nummeriert durchgehend ab Start
	Beispielaufruf: start_starter:go(2,2). startet zwei Starterprozesse, nummeriert mit 2 und 3
- aufruf von ggt_starter(Starternummer)
- aufruf von ggt_process:start(ArbeitsZeit,TermZeit,Praktikumsgruppe,Teamnummer,GGTProzessnummer,Starternummer,NameS,Koordinator)

% ggt_starter liest die ggt.cfg ein:
% {praktikumsgruppe, 4}:		Nummer der Praktikumsgruppe
% {teamnummer, 88}:			Nummer des Teams
% {nameservicenode, ns@KI-VS}:		node des Namensdienstes
% {koordinatorname, euklid}:		Name des Koordinators

Runterfahren:
-------------
2> Ctrl/Strg Shift G
-->q

Informationen zu Prozessen bzw. Modulen:
-------------
2> pman:start().
2> process_info(PID).
2> <Module>:module_info(). 
