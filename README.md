<?xml version="1.0" encoding="utf-8"?>
<html xmlns:MadCap="http://www.madcapsoftware.com/Schemas/MadCap.xsd" MadCap:lastBlockDepth="7" MadCap:lastHeight="1581" MadCap:lastWidth="1837" class="task">
    <head>
    </head>
    <body>
        <h2>Query Across Kinds</h2>
        <p class="BodyText">Use complex queries to efficiently query across Kinds. Many internal services use this end point to reduce the number of DB requests required to fulfill a request.</p><pre xml:space="preserve" class="Codeblock">query {
    kindDBQuery(kindQuery:{
      kindId:"fcd39416-337c-4566-b5bf-56cc8ff725d2"
      fieldFilters: [{
        op: "=="
        fieldName: "State"
        value: {
          STRING:"WA"
        }
      }]
      and: [{
        kindId:"fcd39416-337c-4566-b5bf-56cc8ff725d2"
        toFieldName:"City"
        fromFieldName: "City"
        fieldFilters:[{
          op: "=="
          fieldName: "Zipcode"
          value: {
            STRING: "98053"
          }
        }]
      }]
    }) {
      records {
        ID
        STRING
        INT
        FLOAT
      }
    }
  }</pre>
        <p class="BodyText">The outer most Kind is the type returned. Inner Kinds form a directed acyclic graph (DAG), each of which constrains its outer Kind instances. The DAG is a graph walk, where constraints are applied at each step in the walk and data is only returned if all constraints are true.</p>
        <p class="BodyText">Filters are a collection of &lt;op, field, value&gt; triples that constrain the Kind on which they are applied. Operators are:</p>
        <ul>
            <li>"==" "!=" "oneof" for Strings and numbers (Int and Float).</li>
            <li>"&lt;", "&lt;=", "&gt;", "&gt;=" for numbers.</li>
        </ul>
    </body>
</html>
