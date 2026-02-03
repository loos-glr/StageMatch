# Overzicht
De app is een "Tinder" style app waarbij studenten die op zoek zijn naar een stageplek en bedrijven die stagaires zoeken kunnen "swipen" om met elkaar te matchen. Dit zou een laagdrempelige manier moeten zijn om stagaires en bedrijven te verbinden en tot een betere match te kunnen komen.

# Studenten Flow
```mermaid
graph TD

A(["Start: Open 'Swipe' modus"]) --> B["Stel voorkeuren in<br/>(Bv. 'Marketing' in 'Rotterdam')"]

B --> C["Toon eerste Stagekaart<br/>(Foto + Functie + Bedrijf)"]

C --> D{"Wat wil je weten?"}

D -- "Alleen basisinfo" --> F

D -- "Meer details" --> E["Tik op kaart: Toon omschrijving"]

E --> F{"Beslissing nemen"}

F -- "Swipe Links (Niks)" --> G["Kaart verdwijnt<br/>(Wordt niet opgeslagen)"]

F -- "Swipe Rechts (Leuk!)" --> H["Kaart gaat naar 'Mijn Matches'"]

H --> I["Toon feedback: Hartje/Animatie"]

G --> J{"Zijn er nog kaarten?"}

I --> J

J -- "Ja: Volgende" --> C

J -- "Nee: Klaar" --> K["Toon tekst: 'Je bent helemaal bij!'"]

K --> L["Ga naar overzicht 'Mijn Matches'"]

L --> M["Selecteer een match"]

M --> N(["Actie: Stuur bericht / Solliciteer"])
```