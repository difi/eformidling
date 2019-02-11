---
title: Integrasjonspunktet
description: Beskrivelse av integrasjonspunktets funksjon
summary: "Informasjon om integrasjonspunktet"
permalink: integrasjonspunktet.html
layout: page
sidebar: eformidling
folder: eformidling
---



Integrasjonspunktet er en komponent utviklet av Difi for å være grenselinjen mellom det interne og eksterne miljøet.
Integrasjonspunktet blir brukt til å utveksle meldinger fra en offentlig virksomhet, gjennom en formidlingstjeneste, og til mottakeren. Mottakeren kan være en annen offentlig virksomhet, en privat virksomhet eller en kommune. 

Integrasjonspunktets oppgave er å bygge korrekte meldinger samt stå for sikkerhet i forhold til de underliggende meldingsformidlerenne ved sending, samt å motta fra de forskjellige meldingstjenestene og eksponere disse til virksomheten på en enhetlig måte.

## Grensesnitt 

Integrasjonpunktet støtter sending av meldinger via 2 API; BestEdu og NextMove.

### BestEdu
BEST/EDU er et SOAP basert grensesnitt. Gjennom å reimplementere tjenestegrensesnittet til BEST/EDU i integrasjonspunktet kan man utnytte det eksisterende ekspedering- og importgrensensesnittet som er en del av BEST/EDU løsningen. 
Virksomheter som bruker denne løsningen kan enkelt peke saks-/arkivsystemet til å ekspedere mot integrasjonspunktet i stenden for MSH (VismaLink/DIPS communicator). På denne måten vil en raskt kunne komme igang med meldingsuveksling mot andre offnetlige virksomheter, private virksomehter og virksomheter på FIKS plattformen. Virksomheter en allerede utveksler meldinger med over MSH, vil fremdeles få disse da integrasjonspuntket vil sende disse videre til lokal MSH intill mottaker er klar til å motta med integrasjonspunkt.

### NextMove
NextMove er neste generasjons grensesnitt mot integrasjonspunktet. 