---
title: Meldinsflyt
description: Overorndet beskrivelse av meldingsflyt
permalink: meldingsflyt.html
layout: page
sidebar: eformidling
foler: NextMove
---

## Sende melding

<div class="mermaid">

sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler

    
    fs->>ip: GET /capabilities/{orgnr}
    ip->>sr: GET /identifier/{orgnr}
    sr-->>ip: capabilities
    ip-->>fs: capabilities
    fs->>fs: select capability
    fs->>ip: POST /out/messages
    ip-->>fs: conversationId
    fs->>ip: POST /out/messages/{conversationId}
    ip->>mf: Upload
    loop 
        ip->>mf: GetStatus
        mf-->>ip: status
    end
    fs->>ip: GET /statuses/{id}
    ip-->>fs: statuses


</div>

## Motta melding

<div class="mermaid">

sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant mf  as Meldingsformidler

    loop
        ip->>mf: GetMessages
        mf-->>ip: messages
    end
    fs->>ip: GET /in/messages/peek 
    ip-->>fs: messageMetaData
    fs->>ip: POST /in/messages/pop
    ip-->>fs: ASiC
    fs->>ip: DELETE /in/messages/pop

</div>

## Sende melding med Arkivmelding 

Denne måten å sende melding på er ikke implementert i integrasjonspunktet, men er en tanke for hvordan man kan få en enklere sendeflyt når man bruker arkivmelding. Arkivmeldingen inneholder informasasjon som også kan brukes til adrssering.

<!-- <div class="mermaid"> -->
```mermaid
sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler
    
    fs->>ip: POST /out/messages    
    Note over fs, ip: Arkivmelding
    ip->>sr: GET /identifier/{orgnr/pnr}
    sr-->>ip: capabilities
    ip-->>fs: prototypes + conversationId
    opt multiple- or edit selected prototype
        fs->>ip: PUT /out/messages/{conversationId}
    end
    fs->>ip: POST /out/messages/{conversationId}
    ip->>mf: Upload
    loop 
        ip->>mf: GetStatus
        mf-->>ip: status
    end
    fs->>ip: GET /statuses/{id}
    ip-->>fs: statuses
```

<!-- </div>     -->
