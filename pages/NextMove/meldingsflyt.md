---
title: Meldingsflyt
description: Overordnet beskrivelse av meldingsflyt
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

    
    fs->>ip: GET /capabilities/{receiverid}
    ip->>sr: GET /identifier/{receiverid}
    sr-->>ip: capabilities
    ip-->>fs: capabilities
    fs->>fs: select capability
    opt
        fs->>ip: GET /prototype/{capabilietyid}
        ip-->>fs: prototype
    end
    fs->>fs: Create message   
    fs->>ip: POST /out/messages/{conversationId}
    ip-->>fs: conversationresponse
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


    
