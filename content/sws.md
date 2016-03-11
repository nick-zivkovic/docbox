# SWS

The SWS offers a REST API to CWC content. Clients can request a list of documents and retrieve those documents.
A document can consist of multiple files. A document always has a document key (see the [Document Key Specification](http://confluence.sdu.nl/confluence/display/BETTY/Document+Key+Specification)) by which it can be retrieved.

## Documents

SWS serves a list of documents at its /documents URL. This list is offered in [Atom Syndication Format](https://en.wikipedia.org/wiki/Atom_%28standard%29) and is sorted by the updated timestamp of the documents.

### Get documents

Returns list of documents in Atom Syndication Format

```endpoint
GET /documents
```

Query parameter | Description
--- | ---
`updatedFrom` | ISO-8601 timestamp
`updatedTo` | ISO-8601 timestamp
`maxResults` | limit for number of results


The list of documents can be limited to only include documents that have been updated during a certain time frame by using the updatedFrom and updatedTo query string parameter.The value of the parameter should be a timestamp formatted according to the [ISO-8601](https://www.w3.org/TR/NOTE-datetime) standard (yyyy-MM-dd'T'HH:mm:ss.SSSZZ)

When large amount of documents is planned to be retrieved, it is very useful to have a paginated response. The response format is still list in Atom Syndication Format (AST), except that paginated response may contain an additional link element (rel="next", according to the AST format) in the response header, which represents the link to the next portion/page of data. If there is no such link, that means there is no more data to be retrieved.

Parameter maxResults must be used, because it represents the size of the page (date filtering parameters are optional).

#### Example request

```curl
curl http://localhost:9040/documents?updatedFrom=2016-03-06T14:04:32.355Z&updatedTo=2016-03-08T15:04:32.355&maxResults=1
```

#### Example response

```html
<feed xmlns="http://www.w3.org/2005/Atom">
   <title>CWC IWS Documents</title>
   <link rel="self" href="http://localhost:9040/documents?updatedFrom=2016-03-07T14:59:09.926Z&amp;updatedTo=2016-03-08T15:04:32.355" />
   <updated>2016-03-07T14:59:09.927Z</updated>
   <author>
      <name>Sdu Uitgevers</name>
   </author>
   <generator uri="http://localhost:9040/" version="1.4.4-SNAPSHOT local build">CWC IWS</generator>
   <id></id>
   <entry>
      <title>m-SDUNDSZ-be48138d-ac60-49a5-9408-7996ab73fc5e</title>
      <link href="http://localhost:9040/documents/m-SDUNDSZ-be48138d-ac60-49a5-9408-7996ab73fc5e" />
      <id>m-SDUNDSZ-be48138d-ac60-49a5-9408-7996ab73fc5e</id>
      <updated>2016-03-07T14:59:09.927Z</updated>
      <category term="sdu-misc" scheme="http://schema.sdu.nl/2014/07/cwc-vocabulary/documentFormat" />
   </entry>
</feed>
```

### Get document

Returns XML TOC for a given document key

Path parameter | Description
--- | ---
`documentKey` | document key

```endpoint
GET /documents/{documentKey}
```

#### Example request

```curl
curl http://localhost:9040/documents/c-MR-W6269-3.116
```

#### Example response

```html
<document>
    <key>c-MR-W6269-3.116</key>
    <lastUpdated>2016-01-22T14:27:45.935Z</lastUpdated>
    <documentFormat>sdu-commentaar</documentFormat>
    <metadata>http://localhost:9040/documents/c-MR-W6269-3.116/metadata</metadata>
    <files>
    <file href="http://localhost:9040/documents/c-MR-W6269-3.116/files/c-MR-W6269-3.116.xml">c-MR-W6269-3.116.xml</file>
  </files>
</document>
```
### Get metadata

Returns metadata XML of the document

```endpoint
GET /documents/{documentKey}/metadata
```

Path parameter | Description
--- | ---
`documentKey` | document key

#### Example request

```curl
curl http://localhost:9040/documents/c-MR-W6269-3.116/metadata
```

#### Example response

```html
<metadata>
   <citationTitles>
      <citationTitle>c-MR-W6269-3.116</citationTitle>
   </citationTitles>
   <authors>
      <author>
         <fullName>mr. J.H. van der Winden</fullName>
         <published>mr. J.H. van der Winden</published>
      </author>
   </authors>
   <cwcSummary>De artikelen 3.109 tot en met 3.118a zijn laatstelijk gewijzigd door het Besluit van 23 juni 2010 tot wijziging van het Vreemdelingenbesluit 2000 in verband met het aanpassen van de asielprocedure per 1 juli 2010. Het hier besproken artikel regelt – kort gezegd – de zogeheten verlengde procedure in gevallen waarin de procedure niet in het aanmeldcentrum wordt voltooid.</cwcSummary>
   <publisher>http://www.sdu.nl/</publisher>
   <accessRights>private</accessRights>
   <legalAreas>
      <legalArea>
         <uri>http://sdu.nl/legalArea/migr.vrmd</uri>
      </legalArea>
   </legalAreas>	
   <aspects />
   <publication>
      <website>
         <documentKey>c-MR-W6269-3.116</documentKey>
         <title>OpMaat</title>
         <url>http://opmaatnieuw.sdu.nl/opmaat/show.do?type=cmt&amp;key=MR-W6269-3.116</url>
      </website>
   </publication>
   <otherPublications />
</metadata>
```

### Get files 

Streams a file that is part of the document with the given key

```endpoint
GET /documents/{documentKey}/files/{filename}
```

Path parameter | Description
--- | ---
`documentKey` | document key
`filename` | file name

#### Example request

```curl
curl http://localhost:9040/documents/c-MR-W6269-3.116/files/c-MR-W6269-3.116.xml
```

```html
<cmt:commentaar-samenstelling xmlns:cmt="http://www.sdu.nl/commentaar" xmlns:sdu="http://www.sdu.nl/wetgeving">
   <cmt:object-commentaar>
      <cmt:artikel>
         <citeertitel>Vreemdelingenbesluit 2000</citeertitel>
         <alias-titels>
            <afkorting>VB</afkorting>
            <afkorting>Vb 2000</afkorting>
         </alias-titels>
         <artikel versie-id="17484362" status="goed" stam-id="2623863" label-id="2568564" id="26060691" code="b1-3.116">
            <kop>
               <label>Artikel</label>
               <nr>3.116</nr>
            </kop>
            <sdu:kantnoot>Voornemen, verlengde asielprocedure</sdu:kantnoot>
            <lid>
               <lidnr status="officieel">1</lidnr>
               <al>Het
            schriftelijke voornemen
          om:</al>
               <lijst type="expliciet" start="1" nr-sluiting="." level="single">
                  <li>
                     <li.nr>a.</li.nr>
                     <al>
                        de
                aanvraag tot het verlenen van de verblijfsvergunning, bedoeld in
                        <extref status="actief" reeks="Wet" doc="1384504">artikel 28 van de Wet</extref>
                        , af te wijzen indien de termijnen, bedoeld in de
                        <extref status="actief" doc="74839" anker="2568524">artikelen 3.112, eerste en derde lid</extref>
                        ,
                        <extref status="actief" doc="74839" anker="2568534">3.113, tweede en derde lid</extref>
                        , of
                        <extref status="actief" doc="74839" anker="2568544">3.114, eerste en zesde lid</extref>
                        , dan wel de op grond van
                        <extref status="actief" doc="74839" anker="2568554">artikel 3.115,
        eerste lid</extref>
                        , verlengde termijn, zijn
      overschreden;
                     </al>
                  </li>
                  <li>
                     <li.nr>b.</li.nr>
                     <al>
                        de
        aanvraag tot het verlengen van de geldigheidsduur van de
      verblijfsvergunning, bedoeld in
                        <extref status="actief" reeks="Wet" doc="1384504">artikel 28 van de Wet</extref>
                        , af te
    wijzen;
                     </al>
                  </li>
                  <li>
                     <li.nr>c.</li.nr>
                     <al>
                        de aanvraag
      tot het verlenen van de verblijfsvergunning, bedoeld in
                        <extref status="actief" reeks="Wet" doc="1384564">artikel 33 van
      de Wet</extref>
                        , af te wijzen,
    of
                     </al>
                  </li>
                  <li>
                     <li.nr>d.</li.nr>
                     <al>
                        de
    verblijfsvergunning, bedoeld in de
                        <extref status="actief" reeks="Wet" doc="1384504">artikelen 28</extref>
                        en
                        <extref status="actief" reeks="Wet" doc="1384564">33 van de Wet</extref>
                        , in te
    trekken, wordt aan de vreemdeling meegedeeld door uitreiking of
  toezending ervan.
                     </al>
                  </li>
               </lijst>
            </lid>
            <lid>
               <lidnr status="officieel">2</lidnr>
               <al>De
termijn waarbinnen de vreemdeling zijn zienswijze schriftelijk naar
voren brengt bedraagt, tenzij een met redenen omkleed verzoek om
verlenging van deze termijn wordt
  ingewilligd:</al>
               <lijst type="expliciet" start="1" nr-sluiting="." level="single">
                  <li>
                     <li.nr>a.</li.nr>
                     <al>in
    het geval, bedoeld in het eerste lid, onder a: vier weken,
  en</al>
                  </li>
                  <li>
                     <li.nr>b.</li.nr>
                     <al>in de gevallen,
    bedoeld in het eerste lid, onder b, c en d: zes
  weken.</al>
                  </li>
               </lijst>
            </lid>
            <lid>
               <lidnr status="officieel">3</lidnr>
               <al>De
termijn, bedoeld in het tweede lid, vangt aan met ingang van de dag na
die waarop het voornemen is uitgereikt of
  toegezonden.</al>
            </lid>
            <lid>
               <lidnr status="officieel">4</lidnr>
               <al>De
schriftelijke zienswijze is tijdig bij Onze Minister ingediend, indien
deze voor het einde van de termijn is ontvangen. Bij verzending per
post is de zienswijze tijdig ingediend, indien deze voor het einde van
de termijn ter post is bezorgd, mits deze niet later dan een week na
afloop van de termijn is
  ontvangen.</al>
            </lid>
            <lid>
               <lidnr status="officieel">5</lidnr>
               <al>De
ontvangst van de schriftelijke zienswijze wordt door Onze Minister
  bevestigd.</al>
            </lid>
            <lid>
               <lidnr status="officieel">6</lidnr>
               <al>Onze
Minister houdt rekening met een na afloop van de termijn ontvangen
schriftelijke zienswijze, indien de beschikking nog niet bekend is
gemaakt. Met een na afloop van de termijn ontvangen aanvulling op een
eerder ingediende schriftelijke zienswijze wordt rekening gehouden,
indien de beschikking nog niet bekend is gemaakt en de afdoening van de
zaak daardoor niet ontoelaatbaar wordt vertraagd. Het ontbreken van de
schriftelijke zienswijze, na het verstrijken van de termijn waarbinnen
de vreemdeling zijn zienswijze schriftelijk naar voren kan brengen,
staat aan het geven van de beschikking niet in de
  weg.</al>
            </lid>
            <meta-data>
               <brondata>
                  <oorspronkelijk>
                     <publicatie soort="Stb" pskey="STB13133" effect="wijziging">
                        <publicatiejaar>2010</publicatiejaar>
                        <publicatienr>244</publicatienr>
                        <uitgiftedatum isodatum="2010-06-29">29-06-2010</uitgiftedatum>
                        <ondertekeningsdatum isodatum="2010-06-23">23-06-2010</ondertekeningsdatum>
                     </publicatie>
                  </oorspronkelijk>
                  <inwerkingtreding>
                     <publicatie soort="Stb" pskey="STB13133">
                        <publicatiejaar>2010</publicatiejaar>
                        <publicatienr>244</publicatienr>
                        <uitgiftedatum isodatum="2010-06-29">29-06-2010</uitgiftedatum>
                        <ondertekeningsdatum isodatum="2010-06-23">23-06-2010</ondertekeningsdatum>
                     </publicatie>
                     <inwerkingtreding.datum isodatum="2010-07-01">01-07-2010</inwerkingtreding.datum>
                  </inwerkingtreding>
                  <opmerkingen-inhoud>
                     <al>Artikel II van Stb. 2010/244 bevat overgangsrecht m.b.t. deze wijziging.</al>
                  </opmerkingen-inhoud>
               </brondata>
               <historische-brondata>
                  <brondata>
                     <oorspronkelijk>
                        <publicatie soort="Stb" pskey="STB16077" effect="wijziging">
                           <publicatiejaar>2014</publicatiejaar>
                           <publicatienr>111</publicatienr>
                           <uitgiftedatum isodatum="2014-03-28">28-03-2014</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2014-03-04">04-03-2014</ondertekeningsdatum>
                        </publicatie>
                     </oorspronkelijk>
                     <inwerkingtreding>
                        <publicatie soort="Stb" pskey="STB15924">
                           <publicatiejaar>2014</publicatiejaar>
                           <publicatienr>110</publicatienr>
                           <uitgiftedatum isodatum="2014-03-28">28-03-2014</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2013-12-18">18-12-2013</ondertekeningsdatum>
                           <dossierref dossier="33581">33581</dossierref>
                        </publicatie>
                        <inwerkingtreding.datum isodatum="2014-03-29">29-03-2014</inwerkingtreding.datum>
                        <beschrijving-inwerking>
                           <al>Treedt in werking op het tijdstip waarop de Wijzigingswet
            Vreemdelingenwet 2000 (uitbreiding werkingssfeer tot personen die
          internationale bescherming genieten) in werking treedt.</al>
                        </beschrijving-inwerking>
                     </inwerkingtreding>
                  </brondata>
                  <brondata>
                     <oorspronkelijk>
                        <publicatie soort="Stb" pskey="STB13133" effect="wijziging">
                           <publicatiejaar>2010</publicatiejaar>
                           <publicatienr>244</publicatienr>
                           <uitgiftedatum isodatum="2010-06-29">29-06-2010</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2010-06-23">23-06-2010</ondertekeningsdatum>
                        </publicatie>
                     </oorspronkelijk>
                     <inwerkingtreding>
                        <publicatie soort="Stb" pskey="STB13133">
                           <publicatiejaar>2010</publicatiejaar>
                           <publicatienr>244</publicatienr>
                           <uitgiftedatum isodatum="2010-06-29">29-06-2010</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2010-06-23">23-06-2010</ondertekeningsdatum>
                        </publicatie>
                        <inwerkingtreding.datum isodatum="2010-07-01">01-07-2010</inwerkingtreding.datum>
                     </inwerkingtreding>
                     <opmerkingen-inhoud>
                        <al>Artikel II van Stb. 2010/244 bevat overgangsrecht m.b.t. deze wijziging.</al>
                     </opmerkingen-inhoud>
                  </brondata>
                  <brondata>
                     <oorspronkelijk>
                        <publicatie soort="Stb" pskey="STB5986" effect="nieuwe-regeling">
                           <publicatiejaar>2000</publicatiejaar>
                           <publicatienr>497</publicatienr>
                           <uitgiftedatum isodatum="2000-12-07">07-12-2000</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2000-11-23">23-11-2000</ondertekeningsdatum>
                        </publicatie>
                     </oorspronkelijk>
                     <inwerkingtreding>
                        <publicatie soort="Stb" pskey="STB6295">
                           <publicatiejaar>2001</publicatiejaar>
                           <publicatienr>144</publicatienr>
                           <uitgiftedatum isodatum="2001-03-29">29-03-2001</uitgiftedatum>
                           <ondertekeningsdatum isodatum="2001-03-20">20-03-2001</ondertekeningsdatum>
                        </publicatie>
                        <inwerkingtreding.datum isodatum="2001-04-01">01-04-2001</inwerkingtreding.datum>
                        <beschrijving-inwerking>
                           <al>Treedt in werking als de Vreemdelingenwet 2000 in werking treedt.</al>
                        </beschrijving-inwerking>
                     </inwerkingtreding>
                  </brondata>
               </historische-brondata>
            </meta-data>
         </artikel>
      </cmt:artikel>
   </cmt:object-commentaar>
   <cmt:commentaar>
      <cmt:commentaar-meta xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:pcv="http://prismstandard.org/namespaces/1.2/pcv/" xmlns:prism="http://prismstandard.org/namespaces/1.2/basic/" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
         <dc:identifier>MR-W6269-3.116-20100701</dc:identifier>
         <dc:creator sdu:afkorting="WINDE">mr. J.H. van der Winden</dc:creator>
         <dc:rights>
            <prism:copyRight>&amp;amp;#169; Sdu Uitgevers, Den Haag, 2005</prism:copyRight>
         </dc:rights>
         <dc:date>
            <prism:creationDate>2011-11-17T14:58:48</prism:creationDate>
            <prism:receptionDate>2011-07-15T00:00:00</prism:receptionDate>
            <prism:modificationDate>2012-05-16T10:46:40</prism:modificationDate>
         </dc:date>
         <dc:subject>
            <pcv:Descriptor rdf:about="http://commentaar.sdu/wetsartikelen">
               <pcv:vocabulary>wetsartikelen</pcv:vocabulary>
               <pcv:label struct="" reeks="Wet" pcv:code="W6269_ART 3.116" doc="W6269" anker="ART 3.116">Artikel 3.116 Vreemdelingenbesluit 2000</pcv:label>
            </pcv:Descriptor>
            <pcv:Descriptor rdf:about="http://commentaar.sdu/commentaardomeinen">
               <pcv:vocabulary>commentaardomeinen</pcv:vocabulary>
               <pcv:label pcv:code="MR">Migratierecht</pcv:label>
            </pcv:Descriptor>
            <pcv:Descriptor rdf:about="http://commentaar.sdu/rechtsthemas">
               <pcv:vocabulary>rechtsthemas</pcv:vocabulary>
               <pcv:label pcv:code="MR04">Vb 2000</pcv:label>
            </pcv:Descriptor>
            <pcv:Descriptor rdf:about="rechtsgebieden.xml">
               <pcv:vocabulary>bwb-rechtsgebieden</pcv:vocabulary>
               <pcv:label pcv:code="18.2">18.2</pcv:label>
            </pcv:Descriptor>
         </dc:subject>
         <dc:type rdf:resource="#commentaar" />
         <dc:format>text/xml</dc:format>
      </cmt:commentaar-meta>
      <cmt:commentaar-sectie type="inleiding">
         <cmt:kop>
            <cmt:nr>A</cmt:nr>
            <cmt:titel>Inleiding</cmt:titel>
         </cmt:kop>
         <cmt:al>
            De artikelen 3.109 tot en met 3.118a zijn laatstelijk gewijzigd door het Besluit van 23 juni 2010 tot wijziging van het Vreemdelingenbesluit 2000 in verband met het aanpassen van de asielprocedure per 1 juli 2010.
            <cmt:noot type="voet" id="d6e37">
               <cmt:noot.nr>1</cmt:noot.nr>
               <cmt:noot.al>
                  Besluit van 23 juni 2010 tot wijziging van het Vreemdelingenbesluit 2000 in verband met het aanpassen van de asielprocedure en vaststelling van het tijdstip van inwerkingtreding van de Wet tot wijziging van de Vreemdelingenwet 2000 in verband met het aanpassen van de asielprocedure,
                  <cmt:nadruk type="cur">Stb</cmt:nadruk>
                  . 2010, 244.
               </cmt:noot.al>
            </cmt:noot>
            Het hier besproken artikel regelt – kort gezegd – de zogeheten verlengde procedure in gevallen waarin de procedure niet in het aanmeldcentrum wordt voltooid.
         </cmt:al>
      </cmt:commentaar-sectie>
      <cmt:commentaar-sectie type="wetstechnische-informatie">
         <cmt:kop>
            <cmt:nr>B</cmt:nr>
            <cmt:titel>Wetstechnische informatie</cmt:titel>
         </cmt:kop>
         <cmt:al>Voor de wetstechnische informatie verwijzen wij u naar de wetstechnische informatie van de regeling.</cmt:al>
      </cmt:commentaar-sectie>
      <cmt:commentaar-sectie type="behandeling-kern">
         <cmt:kop>
            <cmt:nr>C</cmt:nr>
            <cmt:titel>Kernproblematiek</cmt:titel>
         </cmt:kop>
         <cmt:divisie>
            <cmt:kop>
               <cmt:nr status="officieel">C.1</cmt:nr>
               <cmt:titel>Verlengde procedure</cmt:titel>
            </cmt:kop>
            <cmt:al>De hier besproken bepaling bevat de voornemenprocedure indien de aanvraag om verlening van een verblijfsvergunning asiel voor bepaalde tijd verder wordt behandeld in de verlengde asielprocedure. Deze procedure wordt gevolgd indien de minister de termijnen inzake het eerste gehoor, het nader gehoor, het voornemen en de beschikking, al dan niet na verlenging ervan, heeft overschreden. Hiermee wordt tot uitdrukking gebracht dat de (verlengde) termijnen voor de minister bindend zijn. Tevens biedt het voor de IND houvast in het bepalen van welke zaken in de verlengde procedure dienen te worden behandeld, namelijk de zaken waarin de asielaanvraag binnen de gegeven termijnen niet zorgvuldig in het aanmeldcentrum beoordeeld kan worden. De asielzoeker wordt dan doorgezonden naar de verlengde asielprocedure en naar een opvangvoorziening, zo veel mogelijk na afronding van het nader gehoor en indiening van de correcties en aanvullingen in het aanmeldcentrum. De voorheen bestaande verplichting om asielzoekers die al in het aanmeldcentrum over hun asielmotieven zijn gehoord, maar op wiens aanvraag niet is beslist in de procedure in het aanmeldcentrum, na de algemene asielprocedure opnieuw te horen over hun asielmotieven, is met ingang van 1 juli 2010 vervallen. Deze verplichting vloeide voort uit het oude artikel 3.111 van het Vb 2000 op grond waarvan de asielzoeker van wie de aanvraag niet in de procedure in het aanmeldcentrum wordt afgewezen, niet eerder dan zes dagen na de indiening van de aanvraag nader gehoord mag worden. In de verlengde procedure kan zo nodig een aanvullend gehoor worden afgenomen, kunnen verdere aanvullingen en correcties worden ingediend of nadere documenten ter ondersteuning van het asielrelaas worden overgelegd.</cmt:al>
            <sdu:kantnoot bron="redactie">Afwijzing verlenging, intrekking en verlening verblijfsvergunning voor onbepaalde tijd</sdu:kantnoot>
            <cmt:al>De hier besproken bepaling bevat verder de voornemenprocedure in gevallen van afwijzing van de aanvraag tot verlenging van de geldigheidsduur van de verblijfsvergunning asiel voor bepaalde tijd, intrekking van de verblijfsvergunning asiel voor bepaalde tijd en afwijzing van de aanvraag tot verlening van een verblijfsvergunning asiel voor onbepaalde tijd.</cmt:al>
            <sdu:kantnoot bron="redactie">Artikel 64</sdu:kantnoot>
            <cmt:al>
               Waar mogelijk wordt de toepassing van
               <cmt:extref status="actief" reeks="Wet" doc="W6267" anker="ART 64">artikel 64 van de Vw 2000</cmt:extref>
               tijdens de asielprocedure ambtshalve beoordeeld. Daartoe is in de Vw 2000 geregeld dat een voornemen, naast het voornemen om niet ambtshalve een verblijfsvergunning voor bepaalde tijd als bedoeld in
               <cmt:extref status="actief" reeks="Wet" doc="W6267" anker="ART 14">artikel 14 Vw</cmt:extref>
               te verlenen, eveneens betrekking kan hebben op het voornemen om de uitzetting niet op grond van artikel 64 van de Vw 2000 achterwege te laten. Indien de toepassing van artikel 64 niet wordt meegenomen in de asielprocedure, wordt niet een voornemenprocedure gevolgd, maar staat in geval van een afwijzende beslissing de mogelijkheid van bezwaar en beroep open. Van een zelfstandige voornemenprocedure inzake de toepassing van artikel 64 van de Vw 2000 is geen sprake (NvT, p. 10, 11-27).
            </cmt:al>
         </cmt:divisie>
      </cmt:commentaar-sectie>
      <cmt:commentaar-sectie type="jurisprudentie">
         <cmt:kop>
            <cmt:nr>D</cmt:nr>
            <cmt:titel>Jurisprudentie uitgebreid</cmt:titel>
         </cmt:kop>
         <cmt:ad-juris>
            <cmt:juris-ref>
               Zie ook de bij de bespreking van
               <cmt:extref status="actief" reeks="Wet" doc="W6269" anker="ART 3.114">artikel 3.114 Vb 2000</cmt:extref>
               vermelde jurisprudentie.
            </cmt:juris-ref>
         </cmt:ad-juris>
      </cmt:commentaar-sectie>
      <cmt:commentaar-sectie type="jurisprudentie-nieuw">
         <cmt:kop>
            <cmt:nr>E</cmt:nr>
            <cmt:titel>Jurisprudentie nieuw</cmt:titel>
         </cmt:kop>
         <cmt:al>Bij dit artikel is nog geen nieuwe jurisprudentie geselecteerd.</cmt:al>
      </cmt:commentaar-sectie>
      <cmt:commentaar-sectie type="bibliografie">
         <cmt:kop>
            <cmt:nr>F</cmt:nr>
            <cmt:titel>Literatuurverwijzing</cmt:titel>
         </cmt:kop>
         <cmt:bib>
            W.J. van Bennekom en J.H. van der Winden,
            <cmt:nadruk type="cur">Asielrecht</cmt:nadruk>
            , Den Haag: Boom Juridische uitgevers 2011, hoofdstuk 2.
         </cmt:bib>
         <cmt:bib>H. Booij, ‘De herziene asielprocedure sinds 1 juli 2010: korte beschrijving en enkele eerste ervaringen vanuit het perspectief van de rechtsbijstand’, «JNVR» 2011, nr. 1, p. 34-41.</cmt:bib>
         <cmt:bib>
            T. Spijkerboer,
            <cmt:nadruk type="cur">De nieuwe asielprocedure</cmt:nadruk>
            , Nijmegen: Ars Aequi Libri 2010 (&lt;www.arsaequi.nl/pdf/nieuwe%20asielprocedure.pdf&gt;).
         </cmt:bib>
         <cmt:bib>R.A. Visser, ‘Ervaringen met de Algemene Asielprocedure’, «JNVR» 2011, nr. 1, p. 30-33.</cmt:bib>
         <cmt:bib>
            J. Wedemeijer, ‘De nieuwe asielprocedure’,
            <cmt:nadruk type="cur">A&amp;MR</cmt:nadruk>
            2010, nr. 8, p. 398-407.
         </cmt:bib>
         <cmt:bib>J.H. van der Winden, ‘Sneller en beter. Wetsvoorstel tot wijziging van de Vreemdelingenwet 2000 in verband met het aanpassen van de asielprocedure aangenomen door Tweede en Eerste kamer’, «JNVR» 2010, nr. 2, p. 135-150</cmt:bib>
         <cmt:bib>J.H. van der Winden, ‘Van uren naar dagen. Spoorboekje aangepaste asielprocedure</cmt:bib>
         <cmt:bib>vooral te vinden in het gewijzigde Vreemdelingenbesluit 2000’, «JNVR» 2010, nr. 3, p. 238-250.</cmt:bib>
      </cmt:commentaar-sectie>
   </cmt:commentaar>
</cmt:commentaar-samenstelling>
```

