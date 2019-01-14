---
title: swaggerdoc
description: Swaggerdoc
permalink: swaggerdoc.html

layout: page
sidebar: eformidling
foler: NextMove
---

<div id="swagger-ui"></div>

<script src="/swagger/dist/swagger-ui-bundle.js"> </script>
<script src="/swagger/dist/swagger-ui-standalone-preset.js"> </script>
<script>
window.onload = function() {
    // Build a system
    const ui = SwaggerUIBundle({
    url: "nextMoveSwagger.yaml",
    dom_id: '#swagger-ui',
    deepLinking: true,
    presets: [
        SwaggerUIBundle.presets.apis,
        SwaggerUIStandalonePreset
    ],
    plugins: [
        SwaggerUIBundle.plugins.DownloadUrl
    ],
    layout: "StandaloneLayout"
    })
    window.ui = ui
}
</script>