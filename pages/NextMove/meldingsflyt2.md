---
title: Meldinsflyt
description: Overorndet beskrivelse av meldingsflyt
permalink: meldingsflyt.html
layout: page
sidebar: eformidling
foler: NextMove
---



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
