---
title: Meldingsflyt
description: Overordnet beskrivelse av meldingsflyt
permalink: meldingsflyt.html
layout: page
sidebar: eformidling
foler: NextMove
---

## Sende melding
NextMove APIet lar deg sende filer på to måter. Hvilke av de du velger avhenger av størrelsen på meldingen du skal sende. 

Dersom meldingen er opp til 5 MB brukes multipart sending, der en kan sende forettningsmelding og payload i samme kall. 
Ved meldinger større enn 5 MB må en for å oppnå god ytelse bruke metoden for store meldinger

Ved behov kan en få generert prototype på en melding ved GET mot endepunktet api/prototype/{capability}. Denne vil da returnere en preutfylt SBD bestående av SBDH og forettningsmelding





### Små meldinger

<div class="mermaid">

sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler

    
    fs->>ip: GET api/capabilities/{receiverid}
    ip->>sr: GET /identifier/{receiverid}
    sr-->>ip: capabilities
    ip-->>fs: capabilities
    fs->>fs: select capability  

    fs->>fs: Create message   
    fs->>ip: POST api/messages/out/
    ip-->>fs: conversationresponse
    
    ip->>mf: Upload
    loop 
        ip->>mf: GetStatus
        mf-->>ip: status
    end
    fs->>ip: GET api/statuses/{conversationId}
    ip-->>fs: statuses

</div>

### Store Meldinger

<div class="mermaid">

sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler

    
    fs->>ip: GET api/capabilities/{receiverid}

    ip->>sr: GET /identifier/{receiverid}
    sr-->>ip: capabilities

    ip-->>fs: capabilities
    fs->>fs: select capability   
    fs->>fs: Create message      
    fs->>ip: POST api/messages/
    ip-->>fs: conversationresponse
    fs->>ip: PUT api/messages/out/{conversationId}
    fs->>ip: POST api/messages/out/{conversationId}
    
    ip->>mf: Upload
    loop 
        ip->>mf: GetStatus
        mf-->>ip: status
    end

    fs->>ip: GET api/statuses/{conversationId}
    ip-->>fs: statuses

</div>


## Motta melding

Når en skal laste ned meldinger fra integrasjonspunktet må dette initieres med en peek, som låser førte meldingen i køen. Dersom en er ute etter meldinger av en bestemt type kan dette gjøres ved å sende med filter for denne 
Etter man har låst meldingen kan denne deretter lastes ned via endepunktet
/messages/in/pop/{conversationId}.

Etter meldingen er lastet ned kan denne slettes via å kalle DELETE mot 
/in/messages/{conversationId} eller den kan låses opp igjen (hva med unlock batch)


<div class="mermaid">

sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant mf  as Meldingsformidler

    loop
        ip->>mf: GetMessages
        mf-->>ip: messages
    end
    
    fs->>ip: GET /messages/in/peek?serviceIdentifier=converationType
    ip-->>fs: messageMetaData
    fs->>ip: GET /messages/in/pop/{conversationId}
    ip-->>fs: ASiC
    fs->>ip: DELETE /in/messages/{conversationId}

</div>

## Status 

En kan finne status på sendte meldinger med GET mot /messages/status/{conversationID}
Returen vil da være en liste med statser en melding har vært gjennom.
Statusene vil avhenge av de underliggende meldingsformidlingstjenetene.

Generelt
-mottatt lokalt
-sendt 
-levert

*DPO*

*DPF*

*DPI*

*DPV*

## WebHooks (Under utvikling)

Integrasjonspunktet vil på sikt ha støtte for webhooks slik at man kan abontere på eventer i integrasjonspunktet
Eventer som vil bli støttet i første omgang er:
- Innkommende melding
- Statusending

    
