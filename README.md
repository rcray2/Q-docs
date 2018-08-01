<?xml version="1.0" encoding="utf-8"?>
<html xmlns:MadCap="http://www.madcapsoftware.com/Schemas/MadCap.xsd" MadCap:lastBlockDepth="7" MadCap:lastHeight="1276" MadCap:lastWidth="1254" class="task">
    <head><title>Use Global Entry Points to Query a Model</title>
    </head>
    <body>
        <h2>Use Global Entry Points to Query a Model</h2>
        <p class="BodyText">To generically access a Kind, use global entry points. When you use global entry points, reference Kinds by ID. To get the Kind ID from the service ID, run a query against XXX.XXX.XXX.XXX:8003.</p>
        <h3>Example: Read a Kind Through a Global Entry Point</h3>
        <p class="BodyText">To get the Kind ID from the service ID, run a query against XXX.XXX.XXX.XXX:8003.</p><pre xml:space="preserve" class="Codeblock">query foo {
  serviceSource(id:"54769aae-2c63-4415-9b34-74e9ceb1d789") {
    name
    kinds {
      name
      id
    }
  }
}</pre>
        <p class="BodyText">The data returned is in this format:</p><pre xml:space="preserve" class="Codeblock">{
  "data": {
    "serviceSource": {
      "name": "TestService",
      "kinds": [
        {
          "name": "54769aae-2c63-4415-9b34-74e9ceb1d789_TestKind",
          "id": "a745e4d7-74e9-4023-8f39-bc63d6076326"
        }
      ]
    }
  }
}</pre>
    </body>
</html>
