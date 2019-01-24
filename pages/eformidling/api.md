---
title: Grensesnitt
description: Støttede grensenitt i integrasjonspunktet
summary: "Støttede grensenitt i integrasjonspunktet"
permalink: api.html
layout: page
sidebar: eformidling
folder: eformidling
---

# Grensesnitt

Integrasjonpunktet støtter meldingsformidling via 2 API; BestEdu og NextMove

## BestEdu
BEST/EDU er et SOAP basert grensesnitt. Gjennom å reimplementere tjenestegrensesnittet til BEST/EDU i integrasjonspunktet kan man utnytte det eksisterende ekspedering- og importgrensensesnittet som er en del av BEST/EDU løsningen. 
Virksomheter som bruker denne løsningen kan enkelt peke saks-/arkivsystemet til å ekspedere mot integrasjonspunktet i stenden for MSH (VismaLink/DIPS communicator). På denne måten vil en raskt kunne komme igang med meldingsuveksling mot andre offnetlige virksomheter, private virksomehter og virksomheter på FIKS plattformen. Virksomheter en allerede utveksler meldinger med over MSH, vil fremdeles få disse da integrasjonspuntket vil sende disse videre til lokal MSH intill mottaker er klar til å motta med integrasjonspunkt.

## NextMove
NextMove er neste generasjons grensesnitt mot integrasjonspunktet. 


Mer informasjon om grensesnittet og meldingsstandard finne [her](https://difi.github.io/eformidling/meldingsflyt.html) 
