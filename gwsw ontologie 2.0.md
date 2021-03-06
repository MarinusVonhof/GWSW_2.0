GWSW Ontologie 2.0
==================

**Een beschrijving van de GWSW Ontologie op basis van de [NTA 8035](https://www.nen.nl/NEN-Shop/Norm/NTA-80352020-nl.htm)**

Versie historie
<div style="font-size: 0.95em">

20201005: Hst Modelleerprincipes bijgewerkt
20200817: Voorwaarden bij concept-annotaties uitgeschreven, hst 3.2.1  
20200814: Annotaties vanuit Gellish toegevoegd (editor, fact-collectie)  
20200514: Figuren met metamodellen en bestandsvormen bijgewerkt  
20200509: URI-strategie toegevoegd, ook voorstel voor individuen  
20200507: Commentaar van Michel (overleg dd 20200506) verwerkt  
20200505: Diverse kleine tekstaanpassingen  
20200430: Volledig bijgewerkt op basis laatste NTA 8035 en SHACL proefnemingen  
20200415: Proefneming in voorbeelden uitgewerkt  
20200408: SHACL toepassing voor data-verificatie toegevoegd  
20200325: Bijgewerkt op basis (teams)overleg met TNO (Michel) (20200324)  
20200214: Bijgewerkt op basis laatste versie van NTA 8035 (20200211)  
20200103: Opmerking toegevoegd: is Topologisch element wel een bs:Property?  
20200102: Voorstel integratie van NTA 8035
</div> 

Inleiding
=========

Het W3C definieert standaarden voor het Semantisch Web met als basis de triple-vorm: de Subject-Predicate-Object constructie. Het basisprotocol dat hieraan ten grondslag ligt is de linked data taal RDF.

Het GegevensWoordenboek Stedelijk Water (GWSW), een ontwikkeling van de stichting RIONED, is oorspronkelijk ontwikkeld in de Gellish taal. Ook Gellish is een semantische modelleringstaal in het zogenaamde ORO (Object-Relatie-Object) formaat.

In het najaar van 2015 is het GWSW omgezet naar RDF, daarvoor is het GWSW-OroX protocol geïntroduceerd. Die versie van het GWSW is doorontwikkeld tot medio 2020, toen is versie 1.5.1 uitgebracht.

Begin 2020 is gestart met de ontwikkeling van GWSW 2.0, gebaseerd op de in die tijd uitgebrachte NEN NTA 8035 (NTA 8035). <span class="mark">Het generieke uitwisselformaat GWSW-OroX wordt hiermee ook herzien, functioneel zal de uitwisseling van de GWSW linked data gegevens niet veel gaan wijzigen</span> GWSW versie 2.0 beschrijft de RDF-implementatie op de NTA 8035 voor de discipline Stedelijk Water.

Bij de uitwerking van dit document is er van uitgegaan dat de lezer bekend is met de principes van RDF/RDFS/OWL 2/SHACL en het uitwisselformaat Turtle.

In de voorbeelden en in de praktijk (bij uitwisseling van GWSW-gegevens) gebruiken we het Turtle-formaat.

In dit hoofdstuk vind u de begrippen en uitgangspunten bij de modellering van het GWSW 2.0. In het volgende hoofdstuk wordt de globale inrichting van het model beschreven. In het laatste hoofdstuk vind u de gedetaillleerde uitwerking met voorbeelden.

Gebruikte begrippen
-------------------

**Linked data: RDF, RDFS, OWL 2, SHACL**  
RDF staat voor Resource Description Format, de basisdefinitie van modellen op basis van subject-predicate-object. In de tekst verstaan we onder "linked data" de combinatie van RDF en de daarop gebaseerde schema's RDFS (RDF Schema), OWL 2 (Web Ontology Language) en SHACL (Shapes Constraint Language). Met de term OWL wordt OWL 2 aangeduid.

**CE, OPE, DPE**  
Deze termen hanteren we in de OWL definities. De afkorting CE wordt gebruikt voor Class Expressions (in Description Logics “complex concepts”). CE’s worden ondermeer gevormd door Classes te binden aan Object Property Expressions (OPE). Data Property Expressions (DPE) beschrijven restricties op de waardetypes.

**Concept**  
De mentale voorstelling van iets uit de werkelijkheid (NTA 8035).

**Individu**  
Een instantie van een concept, iets (potentieel) aanwijsbaars uit de werkelijkheid (NTA 8035). Zoals individu “0980” in werkelijkheid de betonnen constructie van de klasse/concept “rioolput” is.

**Klasse, subtype, supertype**  
Concepten die hiërarchisch zijn onderverdeeld in groepen noemen we klassen, het zijn de bouwstenen van de soortenboom. Zo'n soortenboom wordt ook wel taxonomie genoemd. In de GWSW-hiërarchie gebruiken we de termen Supertype - Concept - Subtype. Een subtype is de "specialisatie" van de klasse, het supertype is de "generalisatie" van de klasse.

**Property, predicate**  
Voor de relatie (tussen subject en object) zijn meerdere namen gebruikelijk (“predicate”, “property name”). Conform de NTA 8035 hanteren we de term "property" voor attributen (inclusief aspecten) en relaties.

**Ontologie**  
Een samenhangende gegevensstructuur bestaande uit concepten, hun attributen en onderlinge relaties, instanties van die concepten en waardetypen (verkort vanuit de NTA 8035).

**Model, Dataset**  
Binnen de GWSW ontologie beschrijft het datamodel (model) de concepten en hun relaties, de dataset bevat de “individuen”, bijvoorbeeld een fysiek stedelijk water systeem. Voor het model wordt ook wel de term TBox gebruikt: “terminological components”. Voor de dataset wordt ook de term ABox gebruikt: “assertion components”.

Uitgangspunten
--------------

### Modellering

Een belangrijk principe is de object-georiënteerde modellering: het model hanteert overerving-principes en maakt zo expliciet mogelijk onderscheid in subtypen van de genoemde supertypen. Dat is een heel andere benadering dan bijvoorbeeld het ontwerp van een relationeel model. Daarbij ligt de nadruk ligt op het interpreteren van informatie met een hoofdrol voor de normalisatie-techniek om opslagruimte te beperken en redundantie te voorkomen. RDF is vanwege zijn binaire relaties van zichzelf optimaal genormaliseerd, wijzigingen beperken zich tot toevoegen of verwijderen van triples.

Uitgangspunten bij de bouw van de GWSW ontologie:
*  Bij opbouw van het GWSW in linked data vorm wordt het oorspronkelijke Gellish-model zonder informatieverlies getransformeerd naar het linked data model.
*  Een mooie erfenis vanuit Gellish: Objectypen (klassen) blijven zoveel mogelijk expliciet gedefinieerd. Voor de indeling in soorten, de vaststelling van de taxonomie volgen we het principe dat op basis van de objecteigenschappen het objecttype wordt gedefinieerd ("onderscheidende kenmerken", "data-afleiding"). Om dat uit te drukken gebruiken we (de CE's en OPE's) in OWL. Er is uitgegaan van het OWL RL (Rule Language) profiel. Dit profiel gebruikt nagenoeg alle OWL semantiek en is toereikend voor de GWSW ontologie.
*  Validaties en specificaties voor data-verificatie beschrijven we in SHACL.
*  De SHACL shapes kunnen in meerdere vormen voorkomen en staan naast de GWSW ontologie. De shapes worden gebaseerd op de vereiste datakwaliteit per proces. De zogenaamde conformiteitsklassen.
*  De ontologie is volledig gebaseerd op de NTA 8035, uitgave voorjaar 2020. Het GWSW gebruikt ook de NTA-methode voor het beschrijven van attributen. Die is geïnspireerd op de Ontology for Property Management (OPM), zie https://w3c-lbd-cg.github.io/opm .
*  De modellering wordt getest met Protégé 5.5.0, in combinatie met de HermiT- en Pellet-reasoners en SPARQL- en SHACL-plugins.

Het laatste hoofdstuk bevat voorbeelden waarbij deze uitgangspunten worden toegepast.

### Taalbinding naar RDF gebaseerde standaarden

De volgende standaardformaten of -talen hanteren we in de GWSW ontologie:

<table class="simp">
<tr><td>RDF </td><td>Het Resource Description Format, de basisdefinitie van modellen op basis van subject-predicate-object.</td></tr>
<tr><td>RDFS </td><td>Een schema op RDF voor het structureren van ontologieën, onderscheidt concepten en individuen</td></tr>
<tr><td>OWL </td><td>Web Ontology Language (versie 2), voegt mogelijkheden toe om klassen, properties en datatypes door middel van restricties te definiëren</td></tr>
<tr><td>SHACL </td><td>Shapes Constraint Language, een taal waarmee restricties op RDF graphs beschreven worden</td></tr>
<tr><td>SKOS </td><td>Simple Knowledge Organization System. Gericht op het uitdrukken van kennisorganisatie systemen (KOS) zoals vocabulaires, woordenboeken en thesauri.
<tr><td>NTA 8035 </td><td>De door NEN gepubliceerde Nederlands Technische Afspraak voor Semantische gegevensmodellering in de gebouwde omgeving. Gebaseerd op RDF, RDFS, SKOS, OWL en SHACL. Het Conceptuele Meta Model van de NTA 8035 is in OWL geïmplementeerd onder de naam "basissemantics-owl".</td></tr>
</table>

### Gebruikte namespaces 

<table class="simp">
<tr><td>RDF</td><td>rdf:</td><td>&lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#&gt;</td></tr>
<tr><td>RDFS</td><td>rdfs:</td><td>&lt;http://www.w3.org/2000/01/rdf-schema#&gt;</td></tr>
<tr><td>OWL</td><td>owl:</td><td>&lt;http://www.w3.org/2002/07/owl#&gt;</td></tr>
<tr><td>SHACL</td><td>sh:</td><td>&lt;http://www.w3.org/ns/shacl#&gt;</td></tr>
<tr><td>SKOS</td><td>skos:</td><td>&lt;http://www.w3.org/2004/02/skos/core#&gt;</td></tr>
<tr><td>NTA8035</td><td>bs:</td><td>&lt;https://w3id.org/def/basicsemantics-owl#&gt;</td></tr>
<tr><td></td><td>smls:</td><td>&lt;https://w3id.org/def/basicsemantics-shacl#&gt;</td></tr>
<tr><td>Datatypes</td><td>xsd:</td><td>&lt;http://www.w3.org/2001/XMLSchema#&gt;</td></tr>
<tr><td>Geo-definities</td><td>geo:</td><td>&lt;http://www.opengis.net/ont/geosparql#&gt;</td></tr>
<tr><td>Grootheden</td><td>quantitykind:</td><td>&lt;http://qudt.org/vocab/quantitykind/&gt;</td></tr>
<tr><td>Eenheden</td><td>unit:</td><td>&lt;http://qudt.org/vocab/unit/&gt;</td></tr>
</table>

Voor de concepten en relaties uit de GWSW-Ontologie hanteren we in de voorbeelden de prefix “gwsw:”. Voor individuen in een dataset wordt de prefix “ex:” gebruikt.

<table class="simp">
<tr><td>GWSW-Model</td><td>gwsw:</td><td>&lt;https://data.gwsw.nl/2.0/totaal/&gt;</td></tr>
<tr><td>GWSW-Dataset</td><td>ex:</td><td>&lt;https://w3id.org/def/example#&gt;</td></tr>
</table>

### URI-strategie, naamgeving, identificatie

<span style="font-size: 1.5em">De URI-strategie wordt herzien, zie https://geonovum.github.io/uri-bp . De inhoud van dit hoofdstuk is daarop nog niet aangepast.</span>

De details van de URI-opbouw zijn beschreven in het document "GWSW URI Strategie". Daarin is op basis van de richtlijnen van het Platform Linked Data Nederland (PLDN) voor het GWSW-model de URI-opbouw beschreven.

**<span class="smallcaps">Identificatie van concepten</span>**

Om te verwijzen naar documentlocaties van GWSW-concepten gebruiken we:

<pre class="simp">https://{domain}/{type}/{version}/{filter}/{reference}</pre>

**{domain}** is het web-domein: voor de GWSW-Ontologie is {locatie}.gwsw.nl. Het subdomein {locatie} voor de ontologie is <span class="blue">data</span>.

**{type}** is het soort resource: voor concepten / definities van een term is dat type <span class="blue">def</span>. In de URI hoeft het type niet te worden opgenomen, het default-type (bij ontbreken) is <span class="blue">def</span>.

**{version}** is de versie: voor deze GWSW ontologie is dat <span class="blue">2.0</span>.

**{filter}** is het geldende filter ("view") op de GWSW ontologie: om alle concepten op te kunnen vragen geldt filter <span class="blue">Totaal</span>.

**{reference}** is de verwijzing naar het specifieke concept:

Het hanteren van begrijpbare namen voor concepten is een gangbare RDF praktijk en ook voor het GWSW heel bruikbaar. We gaan uit van camelCase en CamelCase notatie van de namen voor respectievelijk de properties (starten met lowercase) en de klassen (starten met uppercase).

Een <span class="blue">externe overstortput</span> is een GWSW concept (klasse) en heeft in GWSW versie 2.0 de URI 
<pre class="simp">https://data.gwsw.nl/2.0/Totaal/ExterneOverstortput</pre>

De <span class="blue">breedte</span> van een <span class="blue">put</span> is een attribuut en heeft de URI 
<pre class="simp">https://data.gwsw.nl/2.0/Totaal/breedtePut></pre>

Een URI van een GWSW-klasse bestaat dus uit een ontologie-locatie, aangevuld met de conceptnaam (in CamelCase). In de voorbeelden wordt de ontologie-locatie aangeduid met de prefix gwsw: .

**<span class="smallcaps">Voorkeursnaam, synoniem, vertaling</span>**

We volgen de NTA 8035, de voorkeursterm van een GWSW concept wordt aangeduid met de property skos:prefLabel. Voor vertalingen en synoniemen van de voorkeursterm gebruiken we de property skos:altLabel. (Over de te gebruiken property voor vertalingen doet de NTA 8035 overigens geen uitspraak, wel geeft de NTA aan dat skos:prefLabel precies één keer wordt gebruikt)

<div class="example"><div class="example-title marker">Model: Voorbeeld URI en namen</div><pre>
    @prefix gwsw:                         &lt;https://data.gwsw.nl/2.0/totaal/&gt; . 
    gwsw:ExterneOverstortput              skos:prefLabel             "Externe overstortput"@nl ; 
                                          skos:altLabel              "External weir"@en ; 
                                          skos:altLabel              "Externe overstort"@nl . 
    gwsw:breedtePut                       skos:prefLabel             "Breedte put"@nl . 
</pre></div>

**<span class="smallcaps">Identificatie individuen</span>**

Het verwijzen naar individuen met URI’s is essentieel in het linked-data principe, zeker nu er meer linked-data platforms verschijnen en de GWSW datasets steeds breder worden toegepast.

Een uniforme URI-strategie voor individuen in de "bebouwde omgeving" ontbreekt nog. In zo'n URI kan dan bijvoorbeeld de eigenaar van de gegevens (gemeente, samenwerkingsregio, waterschap, provincie) worden onderscheiden. Op korte termijn zou deze strategie uitgewerkt, vastgesteld en gebruikt moeten worden.

Een voorbeeld van de mogelijkheden met gebruik van de eerder genoemde opbouw:
<pre class="simp">https://{domain}/{type}/{organisatie}#{reference} </pre>

**{domain}**: Identiek aan het GWSW-model

**{type}**: Het betreft nu een individual, dus is het type een identifier <span class="blue">id</span>.

**{organisatie}**: De organisatie/eigenaar/beheerder van het individu. Voor het organisatienummer (het identificeren van een lokaal pad) wordt conform de URI-strategie van het Digitaal Stelsel Omgevingswet de CBS-systematiek gehanteerd. Dit is de code van de overheidslaag (01 rijk, 02 uitvoeringsorgaan, 03 provincie, 04 waterschap, 05 gemeenschappelijke regeling, 06 gemeente) gevolgd door de viercijferige CBS-code van de overheidsinstelling. Voor "Roosendaal" betekent dit de code "0601674".

**{reference}**: Als URL-fragment, de identificatie van het object (bijvoorbeeld een GUID).

De URI naar een specifieke rioolput in gemeente Roosendaal wordt daarmee:
<pre class="simp">https://data.gwsw.nl/id/061674#b2ad189a-8c46-49f2-557ba07c49a2</pre>

Vanwege het ontbreken van een uniforme identificatie gebruiken we in dit document de neutrale aanduiding van individuen.

<div class="example-dataset"><div class="example-title marker">Dataset: Voorbeeld identificatie</div><pre>
    @prefix gwsw: &lt;https://data.gwsw.nl/2.0/totaal/&gt; .
    @prefix ex: &lt;https://w3id.org/def/example#&gt; .
    ex:Put_1    rdf:type gwsw:ExterneOverstortput .
</pre></div>


Modelleerprincipes
------------------

<span style="font-size: 1.5em">Hou synchroon met "GWSW Ontologie in RDF.docx" (versie 1.n), wordt daar geactualiseerd.</span>

<!--* Best Practices for Publishing Linked Data [[ld-bp]]-->

### Terminologie - Het vakgebied is leidend

(uitwerking: zie hst 3.1)

1.  Volg de gebruikelijke termen binnen het vakgebied, bedenk geen nieuwe conceptnamen die misschien de lading beter dekken of neutraler zijn. Dat geldt ook - waar mogelijk - voor abstracte concepten.
2.  Geef alle gebruikelijke vakgebied-termen die gelden voor het te modelleren systeem of proces een plek, als apart concept of als synoniem van een concept. De zoekfunctie moet volledig zijn.
3.  Laat algemene termen die niet specifiek bij de discipline horen zoveel mogelijk buiten beschouwing. Modelleer bijvoorbeeld het concept "calamiteit" alleen als het als supertype nodig is.
4.  Verwijs voor algemene termen waar mogelijk naar andere databronnen (rdfs:seeAlso).

### Concepten en annotaties

1.  Elk GWSW-concept is van het generieke type owl:Class.
2.  Een concept wordt geïdentificeerd door de URI (prefix + naam) conform hst 3.1.1
3.  Een concept wordt altijd voorzien van de annotaties zoals opgenomen in hst 3.1.2.

### Specialiseren - Onderscheidende kenmerken

(het opbouwen van de soortenboom, uitwerking: zie hst 3.6)

1.  Voor het classificeren van een concept uitgaan van zoeken op onderscheidende kenmerken in de (abstracte) soortenboom. Denk aan determineren van planten volgens Linnaeus: na het maken van een aantal keuzes wordt de soort gevonden
2.  Streef ernaar om met de onderscheidende kenmerken de uitgeschreven definitie af te dekken
3.  Gebruik de volgende onderscheidende kenmerken bij fysieke objecten:
    -   Doel (waarvoor)
    -   Toepassing (waarin)
    -   Functie (wat doet het)
    -   Uitvoering (hoe)
    -   Structuur (waaruit)

4.  Activiteiten worden altijd gekenmerkt door de relaties
    -   Invoer / hasInput (het lijdend voorwerp)
    -   Uitvoer / hasOutput (het gewijzigde voorwerp, een rapport)

5.  Gebruik daarnaast de volgende onderscheidende kenmerken bij activiteiten:
    -   Doel (waarvoor)
    -   Toepassing (waarin)
    -   Resultaat (wat doet het)
    -   Technologie (werkwijze, eisen)
    -   Mechanisme (waarmee)
6.  Kwalificeer het onderscheidende kenmerk impliciet (gwsw:uitvoering "groot"). Expliciete kwalificaties (in de vorm van subtypes van generieke kenmerken) worden dus niet gebruikt (zijn - nog - onnodig).

### Specialiseren - Abstracte concepten (en collecties)

1.  Hou ze beperkt, de soortenboom zo veel mogelijk concreet houden. Supertypes zijn vaak alleen verdichtingen, geen soort.
2.  Concepten zijn herkenbaar als abstract wanneer ze bijvoorbeeld niet in de deel-geheel relaties (bijvoorbeeld als deel van een rioleringsgebied) voorkomen.
3.  Abstracte concepten bij voorkeur als groep/collectie (en niet als supertype) definiëren. Bijvoorbeeld groepering naar thema's, denk aan infiltratievoorzieningen.
4.  Subtypes op hetzelfde niveau dienen in grote lijn hetzelfde samenstellingsniveau te hebben.

### Specialiseren - Bladerobjecten

1.  Specialiseer de concepten zoveel als mogelijk: definieer de subtypes, de "bladerobjecten".
2.  Introduceer geen subtype als het geen onderscheidend kenmerk heeft. Bijvoorbeeld geen extra subtype "standaard hemelwaterstelsel" naast "verbeterd hemelwaterstelsel".
3.  Hou er rekening mee dat de individuen zo specifiek mogelijk geclassificeerd dienen te worden. Classificatie met een supertype gebeurt alleen als het subtype niet van toepassing is (denk aan het eerdere voorbeeld "hemelwaterstelsel") of als het onbekend is en wel toegepast kan worden. Bijvoorbeeld bij gebruik van de inspectienorm voor "vrijverval rioolleidingen" (met subtypes gemengd, hemelwater, vuilwater).

### Samenstellen - Definiërende onderdelen en verbindingen

1.  Maak waar mogelijk de deel-geheel relatie definiërend. Een fysiek object heeft dan per definitie andere fysieke objecten als onderdeel.
2.  Maak waar mogelijk ook de cardinaliteit definiërend, het aantal onderdelen is dan bepalend.

### Samenstellen - Erven van samenstellingen

1.  Voorkom redundantie van deel-geheel relaties, die relatie mag niet dubbel voorkomen voor een subtype en het bijbehorende supertype.
2.  Definieer de samenstelling dus niet op een te hoog niveau (blijft verleidelijk)

### Kenmerken - Horen exclusief bij een concept

1.  Kenmerken zijn altijd eigendom van een entiteit/geheel en kunnen niet bestaan zonder de eigenaar.
2.  Vermeng dus geen kenmerken van entiteiten. Bijvoorbeeld Dekselmateriaal, Beheerdernaam kunnen geen kenmerken van een Put zijn, die horen bij het concept Deksel en Beheerder. Die concepten zijn vervolgens gerelateerd aan de put.
3.  Voorkom het opnemen van optionele kenmerken bij een supertype (kenmerken die niet voor alle subtypes gelden), definieer de kenmerken dus niet op een te hoog niveau.
4.  Gebruik bijvoorbeeld het multi-parent principe. Als alleen kunststof leidingen het gebruikte kenmerk "kleur" hebben, introduceer dan het concept "kunststof leiding" met het kenmerk "kleur". Een indivual is dan zowel een "vrijverval rioolleiding" als een "kunststof leiding" zijn en heeft daarmee dat extra kenmerk.

### Kenmerken - Geen typelijsten

1.  Kenmerken die verwijzen naar een typelijst (bijvoorbeeld het kenmerk Soort Deksel van het concept Deksel) mogen niet voorkomen. Een typelijst moet altijd uitgedrukt worden in de taxonomie, bijvoorbeeld subtypes van Deksel.

### Kenmerken - Specialiseren, maak ze intrinsiek

1.  Specialiseer waar nodig de kenmerken, zodat restricties op de kenmerk-waarde zo specifiek mogelijk zijn. Bijvoorbeeld:
    -   Definieer het generieke kenmerk "materiaal" met het subtype "materiaal put" met bijbehorende domeinwaarden
    -   Definieer het generieke kenmerk "diameter" met het subtype "diameter leiding" met specifieke restricties op de afmetingen.
2.  Intrinsieke kenmerken zijn geen noodzaak maar de combinatie met bepaalde restricties maakt ze waardevol en daarnaast wordt het model robuuster: als een individual het kenmerk heeft, dan hoort het van een bepaald type te zijn.

### Contexten onderscheiden

TOP, BAS, hoe in te delen?
Opsplitsen in model-bestanden?
Combineren van model-bestanden in editor mogelijk? Onderscheid houden, ook na aanpassingen in de editor.
Enkelvoudige contexten altijd in apart model-bestand?

Inrichting GWSW ontologie
=========================

De ontologie is gebaseerd op de volgende hoofdstructuren:
-   Soortenboom (de taxonomie of specialisatie-indeling)
-   Samenstelling (de meronomie of deel-geheel indeling)
-   Proces (de activiteiten met in- en uitvoer)
-   Collecties (groeperingen van concepten of individuen)

De belangrijkste "top level" concepten (supertypes) zijn:
-   Fysiek object
-   Activiteit

Bij het ontwerp van de datastructuur spelen deze elementen de hoofdrol. Met de NTA 8035 vormen ze het ontwerpkader, de ruggengraat van het GWSW. De GWSW ontologie onderscheidt zich door diepgang in semantiek en reikwijdte in de toepassing (van systeem tot proces). De soortenboom van het GWSW bevat op dit moment (versie 1.5) circa 1500 klassen.

Drie bestandsvormen
-------------------

De ontologie omvat zowel het model (de concepten en relaties, zie data.gwsw.nl) als de daarop gebaseerde datasets (de individuen, zie apps.gwsw.nl). Vanaf GWSW 2.0 wordt naast het <span class="blue">Model</span> en <span class="blue">Dataset</span> een derde datastructuur gebruikt: de <span class="blue">Shapes</span> met daarin SHACL graphs voor data-verificatie (de conformiteitsklasse - CFK). (De Shapes vervangen de filters op vorige GWSW-versies met daarin aangescherpte OWL-restricties).

Het model is zo ingericht dat hiermee vergaande afleiding van de data in de Dataset mogelijk is ("inferencing"). Daarnaast dienen de Shapes om - afhankelijk van het toepassingsproces - kwaliteitseisen te formuleren en via de SHACL processor te verifiëren.  

*De rol van de drie bestandsvormen Model, Dataset en Shapes bij de toepassing van het GWSW:*

<img src="media/image1.png" style="width:100%;height:40%" />

Voorbeelden van de drie bestandsvormen zijn in dit document als volgt gemarkeerd:

<div class="example"><div class="example-title marker">Model:</div><pre>
    Voorbeeld model
</pre></div>
<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    Voorbeeld dataset
</pre></div>
<div class="example-shapes"><div class="example-title marker">Shapes:</div><pre>
    Voorbeeld SHACL graphs
</pre></div>

Concepten, specialisatie
------------------------

De "top level" concepten in de GWSW ontologie zijn de concepten die boven in de soortenboom staan. Deze concepten zijn geen subtype van andere concepten, ze zijn van het generieke type owl:Class.

Tabel: Top Level-concepten

<table class="default">
<thead>
<tr class="header">
<th>Naam</th>
<th>Oude naam (1.5)</th>
<th>Toelichting / definitie</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>bs:Activity</td>
<td>gwsw:Activiteit</td>
<td>NTA 8035: Iets dat plaatsvindt of zou kunnen plaatsvinden in een concrete dan wel virtuele ruimte resp. tijd. Een activiteit transformeert fysieke objecten en/of informatie objecten en wordt uitgevoerd door een fysiek object. Informatie objecten kunnen als input resp. sturing dienen voor het uitvoeren van een activiteit.</td>
</tr>
<tr class="even">
<td>bs:PhysicalObject</td>
<td><p>gwsw:FysiekObject</p>
<p>gwsw:Levensvorm</p>
<p>gwsw:Ruimte</p></td>
<td><p>NTA 8035: Iets dat bestaat of zou kunnen bestaan in ruimte en tijd, een manifestatie en een afbakening van materie en/of energie vormt en waarneembaar is door de zintuigen. Een fysiek object voert activiteiten uit, en wordt ook getransformeerd door activiteiten. (…) Ook een (levend) organisme is een FysiekObject, waarmee ook een mens een FysiekObject is maar ook een organisatie van mensen is een FysiekObject.</p>
<p>Ook gsws:Ruimte is een FysiekObject. bs:SpatialRegion is een topologisch / representatie-element en daardoor geen logisch supertype voor gwsw:Ruimte.</p></td>
</tr>
<tr class="odd">
<td>bs:InformationObject</td>
<td>gwsw:Informatiedrager</td>
<td>NTA 8035: Een op zichzelf staand geheel van gegevens met een eigen identiteit.</td>
</tr>
<tr class="even">
<td>gwsw:TopologischElement</td>
<td>gwsw:TopologischElement</td>
<td>Netwerkbeschrijving van knooppunt (vertex) of verbinding (edge en vertices). <span class="mark">(link met bs:SpatialRegion?)</span></td>
</tr>
<tr class="odd">
<td>bs:QuantityValue</td>
<td>gwsw:Kenmerk</td>
<td>[Gellish] Is an individual object that is a phenomenon that is possessed by a totality and cannot exist without the existence of its possessor. It is an intrinsic, non-separable facet of its possessor</td>
</tr>
<tr class="even">
<td>bs:EnumerationType</td>
<td>gwsw:VerzamelingSoorten</td>
<td>Enumeratie, verzameling individuen</td>
</tr>
<tr class="odd">
<td>rdfs:Container</td>
<td>gwsw:VerzamelingSoorten</td>
<td>Collectie, verzameling classes</td>
</tr>
</tbody>
</table>

Voor de hiërachische indeling van soorten, de bepaling van de taxonomie, wordt de onderscheidende definitie zo expliciet mogelijk beschreven. Determinerend kan daarmee (de naam van) een soort (klasse) worden bepaald. Verschillende elementen in de ontologie spelen hierbij een rol, die zijn beschreven in de volgende paragrafen.

De onderscheidende kenmerken definiëren dus de klassen, de GWSW ontologie hanteert de volgende:
-   Doel (waarvoor)
-   Toepassing (waarin)
-   Functie (wat doet het)
-   Uitvoering (hoe)
-   Structuur (waaruit)

Meer specifiek voor activiteiten:
-   Technologie (werkwijze, eisen)
-   Mechanisme (waarmee)

Daarnaast kunnen ook relaties tussen GWSW concepten definiërend voor de classificering zijn. Een gwsw:Inspectieput moet bijvoorbeeld een gwsw:Deksel hebben. De compositie (de deel-geheel relatie) is dan bepalend, er geldt een beperking voor de property <span class="blue">bs:hasPart</span> .

Properties in de GWSW ontologie
-------------------------------

Conform de NTA 8035 maken we voor de properties een onderscheid in attributen en relaties.

### Attributen (en waardetypen) in het model

Het oorspronkelijke Gellish-model bevat (meta)gegevens die zoveel als mogelijk in de linked data versie zijn meegenomen.

De **Definitietekst, Opmerkingtekst** worden als annotaties bij de concepten opgenomen.

De **Datum aanmaak/wijziging, Auteur aanmaak** wordt in Gellish op triple-niveau bijgehouden. In linked data vorm zou dit met reïficatie worden opgelost. Praktisch gezien lijkt alleen registratie als concept-annotatie haalbaar.

**Auteur aanmaak** wordt meer algemeen **Editor**. Kan meervoudig voorkomen, zowel voor aanmaak als voor meerdere wijzigingen.

De **Taalgemeenschap** wordt als extra naam bij de concepten (met property skos:altLabel) vermeld.

De **Collection Of Facts** speelt een belangrijke rol in het GWSW-model, hiermee kan het model gefilterd worden voor een specifieke discipline (GWSW-Basis, GWSW-RIB, enz.). Belangrijk voor presentatie-doeleinden. Mogelijk via named graphs de triples per discipline groeperen.

Het **Identificatie-nummer**, in het oorspronkelijke Gellish-model is een unieke nummer-identificatie (naast het unieke combinatie discipline+label) belangrijk. Dit nummer wordt voor naslag-doeleinden vermeld in het attribuut rdfs:comment, maar heeft geen waarde meer voor het actuele GWSW. De URI van een concept is de enige identificatie.

**<span class="smallcaps">Overzicht attributen</span>**

*Schema meta-model:*

<img src="media/image2.png" style="width:100%; height:85%" />

De in het GWSW voorkomende attributen:

<table class="default">
<thead>
<tr class="header">
<th>Predicate</th>
<th>Oude naam (1.5)</th>
<th>Omschrijving</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="3">&nbsp</td>
</tr>
<tr class="even">
<td colspan="3"><strong>Annotaties</strong></td>
</tr>
<tr class="odd">
<td>owl:versionInfo</td>
<td>owl:versionInfo</td>
<td><em>Subject (ontologie)</em> <span class="blue">heeft versieomschrijving</span> <em>Literal</em></td>
</tr>
<tr class="even">
<td>skos:prefLabel</td>
<td>rdfs:label</td>
<td><em>Subject</em> <span class="blue">heeft als voorkeursnaam</span> <em>Literal</em> (één per concept)</td>
</tr>
<tr class="odd">
<td>skos:altLabel</td>
<td>skos:altLabel</td>
<td><em>Subject</em> <span class="blue">heeft als synoniem</span> <em>Literal</em> (ook vertalingen, dan meerdere rdfs:altLabel properties)</td>
</tr>
<tr class="even">
<td>skos:editorialNote</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als notitie</span> <em>Literal (notitie over de aanpassingen van de editor)</em></td>
</tr>
<tr class="odd">
<td>skos:notation</td>
<td>skos:notation</td>
<td><em>Subject</em> <span class="blue">heeft als code</span> <em>Literal</em></td>
</tr>
<tr class="even">
<td>skos:definition</td>
<td>skos:definition</td>
<td><em>Subject</em> <span class="blue">heeft als definitie</span> <em>Literal</em> (definitie <em>zonder</em> bron-referentie)</td>
</tr>
<tr class="odd">
<td>rdfs:isDefinedBy</td>
<td>rdfs:isDefinedBy</td>
<td><em>Subject</em> <span class="blue">is gedefinieerd door</span> <em>Literal</em> (definitie <em>met</em> bron-referentie)</td>
</tr>
<tr class="even">
<td>rdfs:seeAlso</td>
<td>rdfs:seeAlso</td>
<td><em>Subject</em> <span class="blue">heeft aanvullende infomatie op</span> <em>Literal</em> (URL)</td>
</tr>
<tr class="odd">
<td>rdfs:comment</td>
<td><p>rdfs:comment</p>
<p>skos:hiddenLabel</p></td>
<td><em>Subject</em> <span class="blue">heeft als commentaar</span> <em>Literal</em></td>
</tr>
<tr class="even">
<td>gwsw:hasDateStart</td>
<td>gwsw:hasDateStart</td>
<td><em>Subject</em> <span class="blue">heeft als begindatum</span> <em>Literal</em></td>
</tr>
<tr class="odd">
<td>gwsw:hasDateChange</td>
<td>gwsw:hasDateChange</td>
<td><em>Subject</em> <span class="blue">heeft als wijzigingsdatum</span> <em>Literal (kan meervoudig voorkomen)</em></td>
</tr>
<tr class="even">
<td>gwsw:hasEditor</td>
<td>gwsw:hasEditor</td>
<td><em>Subject</em> <span class="blue">heeft als editor</span> <em>Literal</em> (naam persoon die het concept - op de startdatum of wijzigingsdatum - heeft gewijzigd)</td>
</tr>
<tr class="odd">
<td>gwsw:hasFactColl</td>
<td>Gwsw:hasFactColl</td>
<td><em>Subject</em> <span class="blue">heeft als feitencollectie</span> <em>Literal</em> (string met codes van één of meer collecties)</td>
</tr>
<tr class="even">
<td colspan="3"><strong>Kwalitatieve aspecten</strong></td>
</tr>
<tr class="odd">
<td>gwsw:doel</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als doel</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("waarvoor")</td>
</tr>
<tr class="even">
<td>gwsw:toepassing</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als toepassing</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("waarin")</td>
</tr>
<tr class="odd">
<td>gwsw:functie</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als functie</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("wat doet het")</td>
</tr>
<tr class="even">
<td>gwsw:uitvoering</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als uitvoering</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("hoe")</td>
</tr>
<tr class="odd">
<td>gwsw:structuur</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als structuur</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("waaruit")</td>
</tr>
<tr class="even">
<td>gwsw:technologie</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als technologie</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("werkwijze")</td>
</tr>
<tr class="odd">
<td>gwsw:mechanisme</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als mechanisme</span> <em>Kwalitatief aspect.</em> Het object is een onderscheidend kenmerk ("waarmee")</td>
</tr>
<tr class="even">
<td>gwsw:"kwaliteit"</td>
<td>gwsw:hasReference</td>
<td><em>Subject</em> <span class="blue">heeft als "kwaliteit"</span> <em>Kwalitatief aspect.</em> Het object is een kenmerk, een element uit een enumeratie.</td>
</tr>
<tr class="odd">
<td>bs:quantityKind</td>
<td></td>
<td><em>Subject</em> <span class="blue">heeft als grootheid</span> <em>qudt:Quantitykind</em></td>
</tr>
<tr class="even">
<td>bs:unit</td>
<td>gwsw:hasUnit</td>
<td><em>Subject</em> <span class="blue">heeft als eenheid</span> <em>qudt:Unit</em></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td colspan="3"><strong>Kwantitatieve aspecten</strong></td>
</tr>
<tr class="odd">
<td>gwsw:"kwantiteit"</td>
<td><p>gwsw:hasAspect</p>
<p>(isAspectOf)</p></td>
<td><em>Subject</em> <span class="blue">heeft als "kwantiteit"</span> <em>Kwantitatief aspect.</em> Het object is een kenmerk, een individu van het type bs:QuantityValue</td>
</tr>
</tbody>
</table>

**<span class="smallcaps">Intrinsieke kenmerken, pocessed aspects</span>**

Intrinsieke kenmerken horen exclusief bij een klasse, ze hebben maar één domein. We gebruiken voor bijvoorbeeld de lengte van de klasse gwsw:Leiding niet de algemene property gwsw:lengte maar de gespecialiseerde property gwsw:lengteLeiding.

**<span class="smallcaps">Waarden, grootheden en eenheden</span>**

Voor de specificatie van waarden bij de kwantitatieve aspecten (in het GWSW alleen rdf:value) hanteert de NTA 8035 de QUDT-ontologie versie 2.1 voor de definitie van grootheden en eenheden. QUDT is volledig afgestemd met ISO/IEC 80000 (systematiek, namen, definities, symbolen, enz.).

Grootheden: http://qudt.org/vocab/quantitykind , wordt naar verwezen door de property bs:quantityKind.

Eenheden: http://qudt.org/vocab/unit , wordt naar verwezen door de property bs:unit.

Het GWSW definieert nog weinig grootheden, de eenheden zijn wel volledig gemodelleerd. We hanteren de volgende:

<table class="default">
<thead>
<tr class="header">
<th><p>QUDT Units</p>
<p>http://qudt.org/2.1/vocab/unit/</p></th>
<th>Unit (Gellish)</th>
<th>Datatype</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>PERCENT</td>
<td>%</td>
<td>xsd:integer</td>
</tr>
<tr class="even">
<td>PER-HR</td>
<td>1/h</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>PER-MIN</td>
<td>1/min</td>
<td>xsd:decimal</td>
</tr>
<tr class="even">
<td>BAR</td>
<td>bar</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>DEG_C</td>
<td>degC</td>
<td>xsd:integer</td>
</tr>
<tr class="even">
<td>DeciM3</td>
<td>dm3</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>DeciM3-PER-SEC</td>
<td>dm3/s</td>
<td>xsd:decimal</td>
</tr>
<tr class="even">
<td>HR</td>
<td>h</td>
<td>xsd:integer</td>
</tr>
<tr class="odd">
<td>M</td>
<td>m</td>
<td>xsd:decimal</td>
</tr>
<tr class="even">
<td><p><span class="mark">M-PER-HR of gwsw:M-PER-DAY</span></p>
<p>(instantie van qudt:unit, of wel bekend in qudt-versie 2.1?)</p></td>
<td>m/dag</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>M2</td>
<td>m2</td>
<td>xsd:decimal</td>
</tr>
<tr class="even">
<td>M3-PER-HR</td>
<td>m3/h</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>MiLiM</td>
<td>mm</td>
<td>xsd:integer</td>
</tr>
<tr class="even">
<td>MiLiM-PER-HR</td>
<td>mm/h</td>
<td>xsd:decimal</td>
</tr>
<tr class="odd">
<td>MiLiM-PER-MIN</td>
<td>mm/min</td>
<td>xsd:decimal</td>
</tr>
<tr class="even">
<td>Literal "yyyy-mm-dd"</td>
<td>yyyymmdd</td>
<td>xsd:date</td>
</tr>
<tr class="odd">
<td>YR</td>
<td>yyyy</td>
<td>xsd:gYear</td>
</tr>
<tr class="even">
<td>NUM</td>
<td>pcs</td>
<td>xsd:integer</td>
</tr>
<tr class="odd">
<td>PPM</td>
<td>ppm</td>
<td>xsd:integer</td>
</tr>
<tr class="even">
<td>Literal "hh:mm:ss"</td>
<td>hhmmss</td>
<td>xsd:time</td>
</tr>
<tr class="odd">
<td></td>
<td>- (factor)</td>
<td>xsd:decimal</td>
</tr>
</tbody>
</table>

Uit praktische overwegingen worden de eenheden op model-niveau voorgeschreven, in de dataset wordt de eenheid niet meegegeven. Dat maakt de toepassingen op GWSW-datasets veel efficienter.

### Relaties in het model

*Schema meta-model:*

<img src="media/image3.png" style="width:100%;height:50%" />

De in het GWSW voorkomende relaties:

<table class="default">
<thead>
<tr class="header">
<th>Predicate</th>
<th>Oude naam (1.5)</th>
<th>Omschrijving</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="3">&nbsp;</td>
</tr>
<tr class="even">
<td colspan="3"><strong>Classificatie</strong></td>
</tr>
<tr class="odd">
<td>rdf:type</td>
<td>rdf:type</td>
<td><em>Subject</em> <span class="blue">is van het type</span> <em>Object</em></td>
</tr>
<tr class="even">
<td>owl:inverseOf</td>
<td>owl:inverseOf</td>
<td><em>Subject-property</em> <span class="blue">is de inverse van</span> <em>Object-property</em></td>
</tr>
<tr class="odd">
<td colspan="3"><strong>Specialisatie</strong></td>
</tr>
<tr class="even">
<td>rdfs:subClassOf</td>
<td>rdfs:subClassOf</td>
<td><em>Subject</em> is van het subtype <em>Object</em></td>
</tr>
<tr class="odd">
<td colspan="3"><strong>Compositie</strong></td>
</tr>
<tr class="even">
<td><p>bs:hasPart</p>
<p>(isPartOf)</p></td>
<td><p>gwsw:hasPart</p>
<p>(isPartOf)</p></td>
<td><p><span class="blue">CE</span> beschrijft restrictie op cardinaliteit: Bij subject mag property hasPart 0-n maal of min 0-n en max 1-n maal voorkomen. De NTA heeft niet de inverse property</p>
<p>Opmerking: de NTA 8035 hanteert bs:hasPart alleen voor relaties tussen FysiekObject, InformatieObject of Activiteit onderling. Ruimte is ook een FysiekObject, daarmee blijft bs:hasPart voor het GWSW algemeen toepasbaar.</p></td>
</tr>
<tr class="odd">
<td colspan="3"><strong>Associatie</strong></td>
</tr>
<tr class="even">
<td><p>gwsw:hasInput</p>
<p>(isInputOf)</p></td>
<td><p>gwsw:hasInput</p>
<p>(isInputOf)</p></td>
<td><span class="blue">CE</span> beschrijft restrictie op cardinaliteit: Bij subject mag property hasInput 0-n maal of min 0-n en max 1-n maal voorkomen</td>
</tr>
<tr class="odd">
<td>gwsw:hasOutput (isOutputOf)</td>
<td>gwsw:hasOutput (isOutputOf)</td>
<td><span class="blue">CE</span> beschrijft restrictie op cardinaliteit: Bij subject mag property hasOutput 0-n maal of min 0-n en max 1-n maal voorkomen</td>
</tr>
<tr class="even">
<td>gwsw:hasConnection</td>
<td>gwsw:hasConnection</td>
<td><span class="blue">CE</span> beschrijft restrictie op cardinaliteit: Bij subject mag property hasConnection 0-n maal of min 0-n en max 1-n maal voorkomen</td>
</tr>
<tr class="odd">
<td>gwsw:hasRepresentation (isRepresentationOf)</td>
<td>gwsw:hasRepresentation (isRepresentationOf)</td>
<td>Verwijst naar (range is) InformationObject</td>
</tr>
</tbody>
</table>

Inverse properties zijn voor data-afleiding nodig om verschillen in cardinaliteit bij omgekeerde relaties te kunnen definiëren. Ze worden alleen gebruikt bij object-properties waarvan het type niet symmetrisch (<span class="blue">gwsw:hasConnection</span>) of functioneel is.

**<span class="smallcaps">Netwerkbeschrijving</span>**

Het GWSW definieert alle concepten voor een netwerkbeschrijving. Daarvoor worden de onderlinge verbindingen beschreven via de elementen "oriëntatie" die onderling gerelateerd zijn via <span class="blue">gwsw:hasConnection</span>.

<img src="media/image4.png" style="width:100%;height:50%" />

Een oriëntatie kan een vertex zijn of kan bestaan uit een egde met begin- en eindpunt (vertices). In een netwerk voor hydraulische modellering worden die twee vormen "knooppunt" en "verbinding" genoemd. Een verbinding kan dan zowel een leiding (de meest voorkomende) als een pomp, doorlaat of wand zijn.

### Properties in de dataset

In datasets conform het GWSW komen de volgende properties voor:

<table class="default">
<thead>
<tr class="header">
<th>Property</th>
<th>Oude naam (1.5)</th>
<th>Toelichting</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="3">&nbsp</td>
</tr>
<tr class="even">
<td colspan="3"><strong>Attributen</strong></td>
</tr>
<tr class="odd">
<td>skos:prefLabel</td>
<td>rdfs:label</td>
<td><em>Subject</em> heeft als naam <em>Literal</em></td>
</tr>
<tr class="even">
<td>gwsw:"kwaliteit"</td>
<td>gwsw:hasReference</td>
<td><em>Subject</em> heeft als "kwaliteit" <em>Kwalitatief aspect.</em> Het object is een kenmerk, een element uit een enumeratie.</td>
</tr>
<tr class="odd">
<td>gwsw:"kwantiteit"</td>
<td>gwsw:hasAspect</td>
<td><em>Subject</em> heeft als "kwantiteit" <em>Kwantitatief aspect.</em> Het object is een kenmerk, een individu van het type bs:QuantityValue</td>
</tr>
<tr class="even">
<td colspan="3"><strong>Relaties</strong></td>
</tr>
<tr class="odd">
<td>rdf:type</td>
<td>rdf:type</td>
<td><em>Subject</em> is van het type <em>Object</em> (klasse-naam)</td>
</tr>
<tr class="even">
<td>rdf:value</td>
<td>gwsw:hasValue</td>
<td><em>Subject</em> heeft als waarde <em>Literal</em> (subject is attribuut)</td>
</tr>
<tr class="odd">
<td>gwsw:hasInput</td>
<td>gwsw:hasInput</td>
<td><em>Subject</em> heeft als invoer <em>Object</em></td>
</tr>
<tr class="even">
<td>gwsw:hasOutput</td>
<td>gwsw:hasOutput</td>
<td><em>Subject</em> heeft als uitvoer <em>Object</em></td>
</tr>
<tr class="odd">
<td>bs:hasPart</td>
<td>gwsw:hasPart</td>
<td><em>Subject</em> heeft als deel <em>Object</em></td>
</tr>
</tbody>
</table>

Reikwijdte data-afleiding en -verificatie
-----------------------------------------

Hier volgt een opsomming van de mogelijke afleidingen en verificaties. Definiërende specificaties van concepten en relaties (data-afleiding) worden beschreven in OWL. Validaties en specificaties die niet onderscheidend zijn voor de classificatie (data-verificatie) beschrijven we met de SHACL taal.

Data verificatie met OWL is beperkt vanwege het niet-UNA (Unique Name Assumption) principe en het OWA (Open World Assumption) principe. Dat laatste principe beperkt vooral de controle-mogelijkheden op cardinaliteit.

Mogelijke data-verificatie en -afleiding op datasets conform het GWSW:

*  Controle op aspect-waarden binnen domein van collecties (enumeraties) (via SHACL) (niet-UNA principe beperkt de toepassing OWL)
*  Inferencing: Individu-klasse wordt afgeleid uit intrinsiek kenmerk (via OWL)
    *  <span class="blue">attribuut</span> breedteLeiding + domein = Leiding =&gt; individu = Leiding
*  In vervolg daarop: controle op intrinsiek kenmerk bij objecttype (via SHACL)
*  Inferencing: Individu-klasse wordt afgeleid uit onderscheidend kenmerk (via OWL)
    *  <span class="blue">attribuut</span> uitvoering + bereik = Klein =&gt; individu = KleinObject
*  In vervolg daarop: controle op onderscheidend kenmerk bij objecttype (via SHACL)
*  Controle op correct gebruik datatype bij <span class="blue">rdf:value</span>: decimal, string, integer, double, date, time, year (via OWL, via SHACL)
*  Controle op numerieke waarden binnen minimum maximum grenzen (via SHACL, indien definiërend ook via OWL)
*  Inferencing: Individu-klasse wordt afgeleid van aantal voorkomende relaties (OWL)
*  Controle op afwijkende cardinaliteit bij individu-klassen (via SHACL) (OWA principe beperkt de toepassing van OWL)
    *  Deze data-afleiding en -verificatie geldt ook voor cardinaliteit op inverse-relaties
*  Controle op mogelijke en verplichte properties (aspecten en relaties) bij individu-klasse. De mogelijke aspecten en relaties zijn eindig gespecificeerd per klasse. (via SHACL: sh:closed true)

Voor het uitdrukken van CE/OPE voorziet OWL in een groot aantal (restrictie) properties. Daarmee kunnen we klassen expliciet onderscheiden, de GWSW Ontologie bevat de volgende:

<table class="default">
<thead>
<tr class="header">
<th>Predicate</th>
<th>Wijze van toepassing in GWSW</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>owl:onClass</td>
<td>Uitdrukken van cardinaliteit en onderscheidende kenmerken</td>
</tr>
<tr class="even">
<td>owl:onProperty</td>
<td>Uitdrukken van cardinaliteit en onderscheidende kenmerken</td>
</tr>
<tr class="odd">
<td>owl:hasValue</td>
<td>Uitdrukken van onderscheidende kenmerken</td>
</tr>
<tr class="even">
<td>owl:allValuesFrom</td>
<td>Uitdrukken van range bij waarden</td>
</tr>
<tr class="odd">
<td>owl:qualifiedCardinality</td>
<td>Uitdrukken van verplichte properties met een specifieke range</td>
</tr>
<tr class="even">
<td>owl:maxQualifiedCardinality</td>
<td>Uitdrukken van maximum aantal properties met een specifieke range</td>
</tr>
<tr class="odd">
<td>owl:minQualifiedCardinality</td>
<td>Uitdrukken van minimum aantal properties met een specifieke range</td>
</tr>
</tbody>
</table>

Details GWSW ontologie
======================

Concepten, specialisatie
------------------------

Voor de definitie van de soortenboom gebruiken we basiselementen uit RDF, RDFS en OWL. Ter illustratie de hiërachische indeling van de familie "put":

<div class="example"><div class="example-title marker">Model: de put-familie</div><pre>
    gwsw:Put        rdf:type                   owl:Class ;                   
                    rdfs:subClassOf            bs:PhysicalObject ; # het supertype uit NTA 8035
                    skos:prefLabel             "Put"@nl .
    gwsw:Rioolput   rdf:type                   owl:Class ;                   
                    rdfs:subClassOf            gwsw:Put ;
                    skos:prefLabel             "Rioolput"@nl .               
    gwsw:Stuwput    rdf:type                   owl:Class ;                   
                    rdfs:subClassOf            gwsw:Rioolput ;               
                    skos:prefLabel             "Stuwput"@nl ,                
                    skos:altLabel              “Weir”@en .         # vertaling als synoniem opnemen
    gwsw:BlindePut  rdf:type                   owl:Class ;                   
                    rdfs:subClassOf            gwsw:Rioolput ;               
                    skos:prefLabel             "Blinde put"@nl .             
</pre></div>

In een dataset wordt altijd zoveel mogelijk verwezen naar de zogenaamde "bladerobjecten". Dat zijn de soorten waarvan geen subtypes bestaan, ze staan onderaan in de boomstructuur. In het GWSW model worden uit principe ook zo min mogelijk supertypes gedefinieerd. Die worden alleen opgenomen indien noodzakelijk voor de indeling (ervingsprincipe) of als de klasse voorkomt als gebruikelijke term.

<div class="example"><div class="example-title marker">Dataset: individuen typeren als GWSW concept</div><pre>
    ex:Put_1        rdf:type      gwsw:Put .          # te generiek voor veel toepassingen
    ex:Put_2        rdf:type      gwsw:Stuwput ,      # specifiek genoeg voor veel toepassingen
                                  gwsw:BlindePut .    # tweede typering
</pre></div>

Het individu ex:Put_2 is dus zowel een stuwput (een put met een stuwconstructie) als een blinde put (een put zonder deksel).

Attributen
----------

### Annotaties

De volgende annotaties worden bij een GWSW-concept opgenomen:

<table class="default">
<thead>
<tr class="header">
<th>Annotatie</th>
<th>Omschrijving</th>
<th>Voorwaarden</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>skos:prefLabel</td>
<td>De voorkeursnaam van het concept</td>
<td>Exact 1</td>
</tr>
<tr class="even">
<td>skos:altLabel</td>
<td>Synoniemen en vertalingen van het concept</td>
<td>Onbeperkt (min=0)</td>
</tr>
<tr class="odd">
<td>skos:editorialNote</td>
<td>Notitie van de editor bij de aanpassingen</td>
<td>Onbeperkt (min=0)</td>
</tr>
<tr class="even">
<td>skos:notation</td>
<td>De code van het concept, eventueel per context (zie verderop in dit hst)</td>
<td>Maximaal 1 per context (min=0)</td>
</tr>
<tr class="odd">
<td>skos:definition</td>
<td>Een "interne" omschrijving, vastgesteld binnen het GWSW-project</td>
<td>Onbeperkt (min=0)</td>
</tr>
<tr class="even">
<td>rdfs:isDefinedBy</td>
<td>Een "externe" omschrijving</td>
<td><p>Onbeperkt (min=0)</p>
<p>Opbouw: [externe bron] Omschrijving</p></td>
</tr>
<tr class="odd">
<td>rdfs:seeAlso</td>
<td>Een verwijzing naar een externe bron</td>
<td><p>Onbeperkt (min=0)</p>
<p>Opbouw: URI (webadres)</p></td>
</tr>
<tr class="even">
<td>rdfs:comment</td>
<td>Aanvullend commentaar en extra verwijzingen</td>
<td><p>Onbeperkt (min=0)</p>
<p>Algemene opbouw: Commentaar-tekst</p>
<p>Verwijzing naar figuur: [Bijlage nnn.jpg]</p>
<p>- als "nnn" identiek is aan de URI-naam: [Bijlage *.jpg]</p>
<p>Verwijzing naar het oude Gellish-ID: [GellishID nnn] ("nnn" is vervallen Gellish-ID, heeft geen functie meer, is vervangen door de URI</p></td>
</tr>
<tr class="odd">
<td>gwsw:hasDateStart</td>
<td>Datum waarop het concept is gemaakt</td>
<td>Exact 1</td>
</tr>
<tr class="even">
<td>gwsw:hasDateChange</td>
<td>Datum waarop het concept is gewijzigd</td>
<td>Onbeperkt (min=0), invullen als de waarde van één van de attributen wijzigt of als het concept andere properties (attributen/relaties) krijgt.</td>
</tr>
<tr class="odd">
<td>gwsw:hasEditor</td>
<td>Naam van de persoon die het concept heeft gemaakt of gewijzigd</td>
<td>Onbeperkt (min=1)</td>
</tr>
<tr class="even">
<td>gwsw:hasFactColl</td>
<td>Een literal met de verzameling codes van de factcollecties</td>
<td>Exact 1</td>
</tr>
</tbody>
</table>

Een voorbeeld van gebruikte annotaties:

<div class="example"><div class="example-title marker">Model:</div><pre>
  gwsw:Put  rdf:type             owl:Class ;           
            skos:prefLabel       "Put"@nl ;            
            rdfs:subClassOf      bs:PhysicalObject ;   
            skos:definition      "Verticale waterdichte ….”@nl ; # interne (eigen) definitie  
            rdfs:isDefinedBy     "[IMGeo:1.0/2007] Gegraven of … "@nl ;
            rdfs:seeAlso         "http://www.w3.org" ; 
            rdfs:comment         "Toelichting bij put" ;         
            skos:notation        "AAA" .               
</pre></div>

Coderingen komen veel voor in het GWSW, bijvoorbeeld als taalonafhankelijke aanduidigen van toestandsaspecten in de EN13508-2. Codes van concepten (voorbeeld "AAA") zijn literals bij de property skos:notation.

Meerdere codes kunnen voorkomen voor verschillende contexten. Bijvoorbeeld bij het reinigen van een leiding worden andere codes gebruikt dan bij het inspecteren van een leiding. Voor dat onderscheid is in de GWSW-Ontologie een datatype aan de code toegevoegd. Dat datatype representeert het geldende notatie-schema.

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:StartNodeReference skos:notation  “AAB"^^gwsw:Dt_Notation_RIB .  # inspecteren leiding
    gwsw:StartNodeReference skos:notation  "GAB"^^gwsw:Dt_Notation_RRB .  # reinigen leiding
    gwsw:Dt_Notation_RRB    skos:prefLabel "Codering reinigen put/leiding"@nl ;
                            rdf:type       rdfs:Datatype .            
</pre></div>

### Aspecten

De NTA 8035 hanteert voor alle aspecten predicates van het type owl:ObjectProperty. Aan bijvoorbeeld kwantitatieve attributen worden metagegevens zoals de eenheid gekoppeld. De NTA 8035 geeft de voorkeur aan impliciete typering van de attribuut-waarde-klasse bs:QuantityValue.

Het GWSW hanteerde in voorgaande versies ook het principe van "geobjectiviceerde attributen", de attributen werden echter via de generieke relatie "hasAspect" aan het subject toegewezen. Vanaf versie 2.0 volgt het GWSW de NTA 8035.

In de GWSW ontologie heeft elk aspect minimaal één domein (bij welke klasse hoort het: rdfs:domain) en één bereik (welke kwantiteit of kwaliteit heeft het: rdfs:range). Alle mogelijke domeinen en de range van een aspect zijn in de ontologie opgenomen.

Voorbeeld met de aspecten gwsw:begindatum en gwsw:leidingMateriaal:

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:Leiding             rdfs:subClassOf  bs:PhysicalObject .    
    gwsw:begindatum          rdf:type         owl:ObjectProperty ;     
                             rdfs:domain      bs:PhysicalObject ;     # minimaal 1 domein          
                             rdfs:range       bs:QuantityValue .      # 1 range (OWA: tenminste 1) 
    gwsw:leidingMateriaal    rdf:type         owl:ObjectProperty ;     
                             rdfs:domain      gwsw:Leiding ;          # minimaal 1 domein          
                             rdfs:range       gwsw:LeidingMateriaal .
    gwsw:Beton               rdf:type         gwsw:LeidingMateriaal . # wordt individu             
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Leiding_1  rdf:type              gwsw:Leiding ;
                  gwsw:begindatum                          # via blank node            
                  [                                   
                    rdf:type            bs:QuantityValue ; # niet nodig, wordt impliciet getypeerd
                    rdf:value           "2012-05-01"^^xsd:date; 
                  ] ;     
                  gwsw:leidingMateriaal gwsw:Beton .  
</pre></div>

Individuals van kenmerken zoals vorm en materiaal worden direct gekoppeld via de property gwsw:leidingMateriaal. Vanaf GWSW 2.0 is voor elke koppeling met een individu een exclusief predicate gemodelleerd.

**<span class="smallcaps">Eenheden op model-niveau</span>**

De eenheden bij waarden definiëren we vooraf - op model-niveau - dat kan eenvoudig door ze als annotaties bij de properties op te nemen.

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:diameterLeiding     rdf:type                   owl:ObjectProperty ;
                             rdfs:domain                bs:PhysicalObject ; 
                             rdfs:range                 bs:QuantityValue ;  
                             bs:unit                    unit:MiLiM .        
</pre></div>

Zoals genoemd nemen we in de datasets geen eenheden of alleen de voorgeschreven eenheden op. Dat maakt toepassingen op de data veel efficienter.

<div class="example-dataset"><div class="example-title marker">Dataset: niet correct</div><pre>
    ex:Leiding_1             gwsw:diameterLeiding                           
                             [        
                               rdf:value                0.160 ;             
                               bs:unit                  unit:M ; # kwalitatief aspect        
                             ] .      
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: wel correct</div><pre>
    ex:Leiding_1             gwsw:diameterLeiding         
                             [         
                                rdf:value               160.0 ;
                                bs:unit                 unit:MiLiM ; # optioneel
                             ] .         
</pre></div>

Verificatie van de gebruikte eenheid in de dataset wordt uitgedrukt in SHACL:

<div class="example-shapes"><div class="example-title marker">Shapes: Controleer de gebruikte eenheid</div><pre>
    gwsw:diameterLeidingShape  rdf:type             sh:NodeShape ;      
                               sh:targetObjectsOf   gwsw:diameterLeiding ;        
                               sh:property                                    
                               [        
                                 sh:path            bs:unit ;           
                                 sh:hasValue        unit:MiLiM ;        
                                 sh:message         "Use unit Milimeter for value of gwsw:diameterLeiding " ; 
                                 sh:severity        sh:Violation ;      
                               ] .      
</pre></div>

**<span class="smallcaps">Metagegevens bij aspecten (aspecten van aspecten)</span>**

Naast de eenheid kan het aspect ook metagegevens als aspecten hebben. Bijvoorbeeld gegevens over de inwinning van aspect, waarmee de kwaliteit van het gegeven wordt beschreven. In het GWSW komen deze constructies veel voor.

<div class="example"><div class="example-title marker">Model: het aspect/metagegeven gwsw:inwinning</div><pre>
    gwsw:inwinning           rdf:type                   owl:ObjectProperty ;   
                             rdfs:range                 gwsw:Inwinning .       
    gwsw:Inwinning           rdf:type                   owl:Class ;            
                             rdfs:subClassOf            bs:InformationObject . 
    gwsw:wijzeVanInwinning   rdf:type                   owl:ObjectProperty ;   
                             rdfs:range                 gwsw:WijzeVanInwinning .       
    gwsw:WijzeVanInwinning   rdf:type                   owl:Class ;            
                             rdfs:subClassOf            bs:EnumerationType ;   
                             owl:equivalentClass                               
                             [           
                               rdf:type                 owl:Class ;            
                               owl:oneOf                (gwsw:Inmeting gwsw:Schatting) ; 
                             ]           
    gwsw:Inmeting            rdf:type                   gwsw:WijzeVanInwinning . # waarde is ingemeten
    gwsw:Schatting           rdf:type                   gwsw:WijzeVanInwinning . # waarde is geschat
</pre></div>

Bij ex:Put_1 registreren dat de waarde van gwsw:hoogtePut is geschat

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Put_1                 gwsw:hoogtePut                                 
                             [        
                               rdf:value                1000^^xsd:integer ; 
                               gwsw:inwinning                               
                               [      
                                 gwsw:wijzeVanInwinning gwsw:Schatting ;    
                                 gwsw:datumInwinning    "2020-05-03"^^xsd:date ;      
                               ] ;    
                             ] .      
</pre></div>

### Onderscheidende kenmerken

Een onderscheidend kenmerk speelt een belangrijke rol bij de specialisatie. Het wordt gemodelleerd als enumeratiewaarde van het type onderscheidend kenmerk.

<div class="example"><div class="example-title marker">Model: Definieer de onderscheidende kenmerken</div><pre>
    gwsw:onderscheidendKenmerk rdf:type                 owl:ObjectProperty ;
                               rdfs:range               gwsw:OnderscheidendKenmerk .
    gwsw:OnderscheidendKenmerk rdf:type                 owl:Class ;         
                               rdfs:subClassOf          bs:Enumerationtype ;
                               rdfs:range               gwsw:OnderscheidendKenmerk .
</pre></div>
<div class="example"><div class="example-title marker">Model: Definieer type uitvoering "Klein" als onderscheidend Kenmerk</div><pre>
    gwsw:uitvoering          rdf:type                   owl:ObjectProperty ;
                             rdfs:subPropertyOf         gwsw:onderscheidendKenmerk ;  
                             rdfs:range                 gwsw:Uitvoering ;   
                             skos:prefLabel             "Wijze van uitvoering"@nl . 
    gwsw:Uitvoering          rdf:type                   owl:Class ;         
                             rdfs:subClassOf            gwsw:OnderscheidendKenmerk .
    gwsw:Klein               rdf:type                   gwsw:Uitvoering ;   
                             skos:definition            “dat is klein" .    
</pre></div>
<div class="example"><div class="example-title marker">Model: Combineer het kenmerk en de waarde ervan in een CE</div><pre>
    gwsw:Putje               rdfs:subClassOf            bs:PhysicalObject ; 
                             rdfs:subClassOf                                
                             [        
                               rdf:type                 owl:Restriction ;   
                               owl:onProperty           gwsw:uitvoering ;   
                               owl:hasValue             gwsw:Klein ;      # individu                  
                             ] .      
</pre></div>

Afgeleid wordt dat ex:Put_1 (ook) van het type gwsw:Putje is

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Put_1                 gwsw:uitvoering            gwsw:Klein .        
</pre></div>
 

### Intrinsieke kenmerken

Een intrinsiek kenmerk behoort specifiek (per definitie) bij een klasse, voor zo'n kenmerk is exact één domein gespecificeerd.

<div class="example"><div class="example-title marker">Model: Definiërend</div><pre>
    gwsw:hoogtePut           rdfs:comment               “Intrinsiek kenmerk” ;        
                             rdf:type                   owl:ObjectProperty ;
                             rdf:type                   owl:FunctionalProperty ;      # max 1 per object          
                             rdfs:domain                gwsw:Put ;          
                             rdfs:range                 bs:QuantityValue .  
</pre></div>

Afgeleid wordt dat ex:Object_1 mogelijk van de klasse Put is

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Object_1              gwsw:hoogtePut ;   
                             [        
                               rdf:value                "2000"^^xsd:integer ;        
                             ] .      
</pre></div>

### Kwalificaties met standaardwaarde

Met de komst van de NTA 8035 definiëren we in het model kwalificerende aspecten als properties met een specifieke kwaliteit (als bereik).

### Beperking en afleiding

Conform de NTA 8035 maakt het GWSW onderscheid in definiërende en specificerende beperkingen op attributen en datatypes.

**Definiërende beperking, Afleiding**

Ter illustratie: de klasse Drukleiding is een subtype van Mechanische Transportleiding. Een drukleiding onderscheidt zich vanwege de kleine diameter, leidingen met grotere diameter worden anders geclassificeerd (als voorbeeld, de werkelijkheid ligt iets anders). De kleine diameter is in dit geval een defiërende beperking waarmee een afleiding (iets met een kleine diameter is een Drukleiding) wordt gemaakt.

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:diameterLeiding     rdf:type                   owl:ObjectProperty ;
                             rdfs:domain                gwsw:Leiding ;      
                             rdfs:range                 bs:QuantityValue .  
    gwsw:Drukleiding         rdf:type                   owl:Class ;         
                             rdfs:subClassOf            gwsw:MechanischeTransportleiding ;
                             owl:equivalentClass                            
                             [        
                               rdf:type                 owl:Restriction ;   
                               owl:qualifiedCardinality "1"^^xsd:nonNegativeInteger ; 
                               owl:onProperty           gwsw:diameterLeiding ;        
                               owl:onClass              gwsw:DiameterDrukleiding ;    
                             ] .      
    gwsw:DiameterDrukleiding rdf:type                   owl:Class ;         
                             rdfs:subClassOf            bs:QuantityValue ;  
                             owl:equivalentClass                            
                             [        
                               rdf:type                 owl:Restriction ;   
                               owl:onProperty           rdf:value ;         
                               owl:allValuesFrom                            
                               [      
                                 skos:prefLabel         “Diameter drukleiding - datatype” ;
                                 owl:equivalentClass                        
                                 [    
                                    rdf:type            rdfs:Datatype ;     
                                    owl:onDatatype      xsd:integer ;       
                                    owl:withRestrictions                    
                                    ( 
                                      [xsd:minInclusive "50"^^xsd:integer] 
                                      [xsd:maxExclusive "160"^^xsd:integer] 
                                    ) ;         
                                 ] ;  
                               ] ;    
                             ] .      
</pre></div>

Afgeleid wordt dat ex:Leiding_1 van de klasse Drukleiding is

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Leiding_1             gwsw:diameterLeiding         
                             [         
                                rdf:value                        63 ;
                             ] .         
</pre></div>

Gesignaleerd wordt dat de typering van ex:Leiding_2 niet consistent is

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Leiding_2             rdf:type              gwsw:Drukleiding ;
                             gwsw:diameterLeiding         
                             [          
                                rdf:value          163 ;
                             ] .         
</pre></div>

Een reasoner leidt op basis van het model af of de QuantityValye van het type gwsw:DiameterDrukleiding is.
Vervolgens zal de validator rapporteren dat de typering van ex:Leiding_2 in de vorige dataset niet consistent is

<div class="example-shapes"><div class="example-title marker">Shapes:</div><pre>
    gwsw:DrukleidingShape    rdf:type               sh:NodeShape ;
                             sh:targetClass         gwsw:Drukleiding ;
                             sh:property         
                             [         
                                sh:path             (gwsw:diameterLeiding rdf:value) ;
                                sh:class            gwsw:DiameterDrukleiding ;
                                sh:message          "diameterLeiding: QuantityValue incorrect" ;
                                sh:severity         sh:Violation ;
                             ] .         
</pre></div>

Of, validatie zonder gebruik te maken van de gespecificeerde gwsw:DiameterDrukleiding:

<div class="example-shapes"><div class="example-title marker">Shapes:</div><pre>
    gwsw:DrukleidingShape    rdf:type               sh:NodeShape ;
                             sh:targetClass         gwsw:Drukleiding ;
                             sh:property         
                             [         
                                sh:path             (gwsw:diameterLeiding rdf:value) ;
                                sh:minInclusive     50 ;
                                sh:maxInclusive     160 ;
                                sh:message          "diameterLeiding: value is out of bounds" ;
                                sh:severity         sh:Violation ;
                             ] .         
</pre></div>

**Specificerende beperking**

Dit type beperkingen gebruiken we voor data-verificatie. In de conformiteitsklassen is altijd volledig beschreven welke aspecten per klasse kunnen voorkomen, binnen die reeks kunnen ook aspecten verplicht zijn.

Bijvoorbeeld de klasse Leiding kent alleen de aspecten gwsw:begindatum en gwsw:diameterLeiding, waarvan het aspect gwsw:diameterLeiding moet voorkomen bij een leiding.

<div class="example-shapes"><div class="example-title marker">Shapes: De mogelijke aspecten per leiding</div><pre>
    gwsw:LeidingClass        rdf:type                 sh:NodeShape ;
                             sh:targetClass           gwsw:Leiding ;
                             sh:closed                true ;  # de reeks properties is eindig
                             sh:property         
                             [         
                               sh:path                gwsw:begindatum ;
                             ] ;         
                             sh:ignoredProperties     (rdf:type gwsw:hasPart) ;   # Deze properties
                             sh:property                                          # mogen ook voorkomen
                             [         
                               sh:path                gwsw:diameterLeiding ;
                               sh:minCount            1 ;
                               sh:maxCount            1 ;
                               sh:message             "diameterLeiding: property is missing" ;
                               sh:severity            sh:Violation ;
                             ] .         
</pre></div>

Individuele aspecten worden soms verder beperkt. Bijvoorbeeld dient de waarde van gwsw:diameterLeiding een geheel getal (integer) te zijn tussen 50 en 4000.

Specificeer de beperkingen voor gwsw:leidingDiameter:

<div class="example-shapes"><div class="example-title marker">Shapes:</div><pre>
    gwsw:diameterLeidingShape  rdf:type                sh:NodeShape ;
                             sh:targetObjectsOf        gwsw:diameterLeiding ;
                             sh:property         
                             [         
                               sh:path                 rdf:value ;
                               sh:minCount             1 ;
                               sh:maxCount             1 ;
                               sh:message              "diameterLeiding: has no value" ;
                               sh:severity             sh:Violation ;
                             ] ;         
                             sh:property         
                             [         
                               sh:path                 rdf:value ;
                               sh:datatype             xsd:nonNegativeInteger ;
                               sh:message              "diameterLeiding: not an integer" ;
                               sh:severity             sh:Violation ;
                             ] ;         
                             sh:property         
                             [         
                               sh:path                 rdf:value ;
                               sh:minExclusive         50 ;
                               sh:maxExclusive         4000 ;
                               sh:message              "diameterLeiding: out of bounds" ;
                               sh:severity             sh:Warning ;
                             ] .         
</pre></div>

Relaties
--------

### Relaties proces (activiteiten)

De relaties <span class="blue">gwsw:hasInput</span> en <span class="blue">gwsw:hasOutput</span> worden gebruikt voor de beschrijving van activiteiten en processen, een voorbeeld:

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:hasInput            rdf:type                   owl:ObjectProperty ;
                             skos:prefLabel             "has as input" .
    gwsw:InspecterenPut      rdf:type                   owl:Class ;
                             rdfs:subClassOf            bs:Activity ;
                             skos:prefLabel             "Inspecteren van een put"@nl .
</pre></div>

Het individu ex:Rioolput_1 is onderwerp van inspectie

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Rioolput_1            rdf:type                   gwsw:Rioolput ;
    ex:InspecterenPut_1      rdf:type                   gwsw:InspecterenPut ;  # de activiteit
                             gwsw:hasInput              ex:Rioolput_1 .
</pre></div>

### Relaties topologie (verbindingen)

<span class="blue">gwsw:hasConnection</span> is van het type owl:SymmetricProperty (heeft geen inverse). Voor de netwerk-beschrijving is deze relatie essentieel, het is dan de relatie tussen topologische elementen van fysieke objecten. De relatie wordt echter ook voor de algemene beschrijving gebruikt, bijvoorbeeld om te beschrijven dat een gemaal vaak verbonden is met een persleiding.

De topologie wordt beschreven via de elementen "oriëntatie". Een oriëntatie kan een vertex zijn of kan bestaan uit een egde met begin- en eindpunt (vertices).

<div class="example"><div class="example-title marker">Model: Topologie-elementen bij leiding en put</div><pre>
    gwsw:leidingorientatie   rdf:type                   owl:ObjectProperty ;
                             rdfs:domain                gwsw:Leiding ;
                             rdfs:range                 gwsw:Leidingorientatie .
    gwsw:Leidingorientatie   rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:TopologischElement ;
                             skos:prefLabel             "Leidingoriëntatie"@nl .
    gwsw:BeginpuntLeiding    rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Vertex ;
                             skos:prefLabel             "Beginpunt leiding"@nl .
    gwsw:EindpuntLeiding     rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Vertex ;
                             skos:prefLabel             "Eindpunt leiding"@nl .
    gwsw:putorientatie       rdf:type                   owl:ObjectProperty ;
                             rdfs:domain                gwsw:Put ;
                             rdfs:range                 gwsw:Putorientatie .
    gwsw:Putorientatie       rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Vertex ;
                             skos:prefLabel             "Putoriëntatie"@nl .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: In een netwerk is een put verbonden met een leiding</div><pre>
    ex:Leiding_1             gwsw:leidingorientatie         
                             [         
                                bs:hasPart              ex:Eindpunt_1 ;
                             ] .         
    ex:Put_1                 gwsw:putorientatie         
                             [         
                                gwsw:hasConnection      ex:Eindpunt_1 ;
                             ] .         

</pre></div>

### Relaties compositie (deel-geheel)

De NTA 8035 definieert <span class="blue">bs:hasPart</span> van het type owl:ObjectProperty, de relatie geldt tussen Fysiek Objecten (inclusief gwsw:Ruimte) onderling en Activiteiten onderling.

<div class="example"><div class="example-title marker">Model: Stuwput en zijn onderdelen</div><pre>
    gwsw:Stuwput             rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Rioolput .
    gwsw:Stuwmuur            rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Wand .
    gwsw:Compartiment        rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Ruimte .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: Compositie van een stuwput, heeft als deel een stuwmuur en een compartiment</div><pre>
    ex:Stuwmuur_1            rdf:type                   gwsw:Stuwmuur ;
                             skos:prefLabel             "0987/w1" .
    ex:Comp_1                rdf:type                   gwsw:Compartiment;
                             skos:prefLabel             "0987/c1" .
    ex:Stuwput_1             rdf:type                   gwsw:Stuwput ;
                             skos:prefLabel             "0987" ;
                             bs:hasPart                 ex:Stuwmuur_1 ;
                             bs:hasPart                 ex:Comp_1 .
</pre></div>

<div class="example"><div class="example-title marker">Model: Gebied en stelsel</div><pre>
    gwsw:Kern                rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Ruimte .
    gwsw:Rioolstelsel        rdf:type                   owl:Class ;
                             rdfs:subClassOf            gwsw:Stelsel .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: Het gebied bevat het stelsel</div><pre>
    ex:Stelsel_1             rdf:type                   gwsw:Rioolstelsel;
                             skos:prefLabel             "Stelsel 1" .
    ex:Kern_1                rdf:type                   gwsw:Kern ;
                             bs:hasPart                 ex:Stelsel_1 ;
                             skos:prefLabel             "Kern 1" .
</pre></div>

### Beperking en afleiding

Ook voor relaties maken we conform de NTA 8035 onderscheid in definiërende en specificerende beperkingen. Relaties die een deel-geheel, een proces of een topologie beschrijven zijn vaak definiërend voor de classificatie. Zo geldt bijvoorbeeld:

*   de activiteit Putinspectie heeft als invoer minimaal één Rioolput (proces)
*   het concept Inpectieput moet minimaal één Deksel hebben (deel-geheel)
*   een rioolput is verbonden aan een rioolleiding (topologie)

Deze relaties beschrijven we definiërend in OWL. Voor de genoemde relaties geldt een gelijke graph-structuur.

<div class="example"><div class="example-title marker">Model: Een inspectieput moet een deksel hebben om een echte inspectieput te zijn</div><pre>
    gwsw:Inspectieput        rdfs:subClassOf                    # restrictie in 1 richting, 
                             [                                  # andere puttypes hebben ook deksels
                               rdf:type                         owl:Restriction ;
                               owl:minQualifiedCardinality      "1"^^xsd:nonNegativeInteger ;
                               owl:onProperty                   gwsw:hasPart ;
                               owl:onClass                      gwsw:Deksel .
                             ] .         
</pre></div>

Cardinaliteit kan tweezijdig worden beschreven, daarvoor zijn er omgekeerde relaties nodig: gwsw:isPartOf is gedefinieerd als inverse van bs:hasPart.

<div class="example"><div class="example-title marker">Model: Een deksel hoort bijvoorbeeld bij één inspectieput</div><pre>
    gwsw:Deksel              owl:equivalentClass         
                             [         
                                rdf:type                        owl:Restriction ;
                                owl:maxQualifiedCardinality     "1"^^xsd:nonNegativeInteger ;
                                owl:onProperty                  gwsw:isPartOf ;
                                owl:onClass                     gwsw:Inspectieput ;
                             ] .         
</pre></div>

Afgeleid wordt dat ex:Put_1 mogelijk een gwsw:Inspectieput is:

<div class="example-dataset"><div class="example-title marker">Dataset:</div><pre>
    ex:Put_1                 bs:hasPart         
                             [         
                                rdf:type                        gwsw:Deksel ;
                             ] .         
</pre></div>

En we definiëren in SHACL de data-verificatie:

<div class="example-shapes"><div class="example-title marker">Shapes: SHACL processor rapporteert dat de putdeksel ontbreekt</div><pre>
    gwsw:InspectieputShape   rdf:type                          sh:NodeShape ;
                             sh:targetClass                    gwsw:Inspectieput ;
                             sh:property         
                             [         
                               sh:path                         bs:hasPart ;
                               sh:class                        gwsw:Deksel ;
                               sh:minCount                     1 ;
                               sh:message                      "Inspectieput: deksel ontbreekt" ;
                               sh:severity                     sh:Warning ;
                             ] .         
</pre></div>

Groepen, collecties, enumeratie
-------------------------------

### Enumeratie

Voor de modellering van waarde-collecties gebruiken we een enumeratie van individuen. Alle collectiemembers (elementen) zijn dus in de GWSW-topologie opgenomen als individuen (met eigen annotatieproperties.


<div class="example"><div class="example-title marker">Model: De putmaterialen</div><pre>
    gwsw:Materiaal           rdf:type             owl:Class ;
                             rdfs:subClassOf      bs:EnumerationType .
    gwsw:MateriaalPut        rdf:type             owl:Class ;
                             rdfs:subClassOf      gwsw:Materiaal ;
                             skos:prefLabel       "Materiaal put"@nl ;
                             owl:equivalentClass         
                             [         
                               rdf:type           owl:Class ;
                               owl:oneOf          (gwsw:Beton gwsw:PVC) ;   # individuen
                             ] .        
    gwsw:Beton               rdf:type             gwsw:MateriaalPut ;       # wordt hiermee individu
                             skos:prefLabel       "beton" ;                 # annotatie: naam
                             skos:notation        "A" .                     # annotatie: code
    gwsw:materiaalPut        rdfs:subClassOf      owl:ObjectProperty ;
                             rdfs:domain          gwsw:Put ;
                             rdfs:range           gwsw:MateriaalPut .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: In de dataset verwijzen naar het materiaal-individu</div><pre>
    ex:Put_1                 rdf:type                           gwsw:Put ;
                             gwsw:materiaalPut                  gwsw:Beton .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: In de dataset ontbreekt het materiaal-individu</div><pre>
    ex:Put_2                 rdf:type                           gwsw:Put .
</pre></div>

<div class="example-dataset"><div class="example-title marker">Dataset: In de dataset verwijzen naar een niet getypeerd individu</div><pre>
    ex:Put_3                 rdf:type                           gwsw:Put ;
                             gwsw:materiaalPut                  gwsw:Betonn .
</pre></div>

Validator rapporteert dat het materiaal van ex:Put_2 ontbreekt en van ex:Put3 onbekend is

<div class="example-shapes"><div class="example-title marker">Shapes:</div><pre>
    gwsw:MateriaalPutShape   rdf:type                          sh:NodeShape ;
                             sh:targetClass                    gwsw:Put ;
                             sh:property         
                             [         
                                sh:path                        gwsw:materiaalPut ;
                                sh:minCount                    1 ;      # Put heeft 1 materiaal
                                sh:maxCount                    1 ;
                                sh:message                     "Materiaal put onbreekt" ;
                                sh:severity                    sh:Warning ;
                             ] ;         
                             sh:property         
                             [         
                                sh:path                        gwsw:materiaalPut ;
                                sh:in                          ( gwsw:Beton gwsw:PVC ) ;
                                sh:message                     "Materiaal put onbekend" ;
                                sh:severity                    sh:Violation ;
                             ] .         
</pre></div>


### Groepering

In het GWSW komen verzamelingen van concepten en individuen voor die niet gestructureerd zijn via een taxonomie, meronomie of enumeratie. Voorbeeld is de verzameling infiltratievoorzieningen, deze concepten zijn in de taxonomie als subtype van reservoir, leiding, put gedefinieerd. Het begrip infiltratievoorziening is echter als verzamelnaam in gebruik:

<div class="example"><div class="example-title marker">Model:</div><pre>
    gwsw:Infiltratiebassin         rdfs:subClassOf              gwsw:Infiltratiereservoir .
    gwsw:Infiltratiekolk           rdfs:subClassOf              gwsw:Kolk .
    gwsw:Infiltratieriool          rdfs:subClassOf              gwsw:VrijvervalRioolleiding .
    gwsw:Infiltratievoorziening    rdf:type                     rdfs:Container ;  # is individual
                                   rdfs:member                  gwsw:Infiltratiebassin ;
                                   rdfs:member                  gwsw:Infiltratiekolk ;
                                   rdfs:member                  gwsw:Infiltratieleiding .
</pre></div>

Voorbeelden data-afleiding en data-verificatie
=======================================================

Zie ook hoofdstuk 1.2 met de uitgangspunten voor het modelleren van de GWSW Ontologie:

**Toepassing OWL (data-afleiding)**  
Voor de indeling in soorten, de vaststelling van de taxonomie volgen we het principe dat op basis van objecteigenschappen het objecttype wordt gedefinieerd. Om dat uit te drukken gebruiken we de class expressions in OWL.

**Toepassing SHACL (data-verificatie)**  
Validaties en specificaties die niet onderscheidend zijn voor de classificatie van GWSW concepten beschrijven we met de SHACL taal.

Ter illustratie, beschrijf de situaties met de volgende voorwaarden:

> 1a De klasse "brug" heeft per definitie als deel een "brugdek"  
> 1b Toets of de klasse "brug" als deel een "brugdek" heeft
>
> 2a Een "metalen brug" heeft per definitie als materiaal "staal" of "ijzer"  
> 2b Toets of de klasse "metalen brug" van het materiaal "staal" of "ijzer" is
>
> 3a Een "korte brug" heeft per definitie een lengte tussen 0 en 100  
> 3b Toets of de klasse "korte brug" een lengte tussen 0 en 100 heeft
>
> 4 Toets of bij de eigenschap "hoogte" minimaal 1 maal is geregistreerd hoe de waarde is ingewonnen. Deze metagegevens staan in de klasse "inwinning" met de eigenschappen "wijze van inwinning" en "datum inwinning"

Hierna volgt de inhoud van drie proefbestanden:

Bestand <span class="blue">Proef GWSW 2.0.ttl</span> bevat het model en de dataset  
Bestand <span class="blue">Proef GWSW 2.0 query.rq</span> bevat de SPARQL query om de inferencing te testen  
Bestand <span class="blue">Proef GWSW 2.0 SHACL.txt</span> bevat de SHACL shapes graphs

Bestand "Proef GWSW 2.0.ttl"
----------------------------
<pre class="file">
@prefix bs: &lt;https://w3id.org/def/basicsemantics-owl#&gt; .
@prefix ex: &lt;https://w3id.org/def/example#&gt; .
@prefix owl: &lt;http://www.w3.org/2002/07/owl#&gt; .
@prefix rdf: &lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#&gt; .
@prefix xml: &lt;http://www.w3.org/XML/1998/namespace&gt; .
@prefix xsd: &lt;http://www.w3.org/2001/XMLSchema#&gt; .
@prefix qudt: &lt;http://qudt.org/schema/qudt/&gt; .
@prefix rdfs: &lt;http://www.w3.org/2000/01/rdf-schema#&gt; .
@prefix sh: &lt;http://www.w3.org/ns/shacl#&gt; .
@prefix skos: &lt;http://www.w3.org/2004/02/skos/core#&gt; .
@prefix unit: &lt;http://qudt.org/vocab/unit/&gt; .
@prefix quantitykind: &lt;http://qudt.org/vocab/quantitykind/&gt; .

# basic semantics -------------------------------------------------------------------------
bs:QuantityValue rdf:type owl:Class .
bs:EnumerationType rdf:type owl:Class .
bs:InformationObject rdf:type owl:Class .
bs:PhysicalObject rdf:type owl:Class .
bs:hasPart rdf:type owl:ObjectProperty .

# concepten -------------------------------------------------------------------------------
ex:Bridge rdf:type owl:Class ;
rdfs:subClassOf bs:PhysicalObject .
ex:MetalBridge rdf:type owl:Class ;
rdfs:subClassOf ex:Bridge .
ex:ShortBridge rdf:type owl:Class ;
rdfs:subClassOf ex:Bridge .
ex:BridgeDeck rdf:type owl:Class ;
rdfs:subClassOf bs:PhysicalObject .
ex:Obtainment rdf:type owl:Class ;
rdfs:subClassOf bs:InformationObject .
ex:Material rdf:type owl:Class ;
rdfs:subClassOf bs:EnumerationType ;
owl:equivalentClass
[
  rdf:type owl:Class ;
  owl:oneOf (ex:Steel ex:Iron ex:Concrete) ;
] .
ex:Steel rdf:type ex:Material . # individu
ex:Iron rdf:type ex:Material . # individu
ex:Concrete rdf:type ex:Material .
ex:ObtainedBy rdf:type owl:Class ; # wijze van inwinning
rdfs:subClassOf bs:EnumerationType ;
owl:equivalentClass
[
  rdf:type owl:Class ;
  owl:oneOf (ex:Design ex:Revision) ;
] .
ex:Design rdf:type ex:ObtainedBy . # vanuit ontwerp
ex:Revision rdf:type ex:ObtainedBy . # vanuit revisie

# attributen en properties -------------------------------------------------------------------
ex:length rdf:type owl:ObjectProperty ;
rdfs:range bs:QuantityValue .
ex:bridgeHeight rdf:type owl:ObjectProperty ;
rdfs:domain ex:Bridge ;
rdfs:range bs:QuantityValue .
ex:material rdf:type owl:ObjectProperty ;
rdfs:range ex:Material . # geen rdfs:range - via SHACL
ex:obtainment rdf:type owl:ObjectProperty ;
rdfs:range ex:Obtainment .
ex:obtainedBy rdf:type owl:ObjectProperty ;
rdfs:range ex:ObtainedBy . # geen rdfs:range - via SHACL
ex:obtainedDate rdf:type owl:ObjectProperty ;
rdfs:range bs:QuantityValue .

# Voorwaarde 1a -------------------------------------------------------------------------------
# Iets is een brug als het minimaal één brugdek heeft
# Beschrijf de onderscheidende kenmerken in OWL class expressions:

ex:Bridge owl:equivalentClass
[
  rdf:type owl:Restriction ;
  owl:minQualifiedCardinality "1"^^xsd:nonNegativeInteger ;
  owl:onProperty bs:hasPart ;
  owl:onClass ex:BridgeDeck ;
] .

# Een OWL-reasoner kan afleiden dat Bridge_1 van het type ex:Bridge is:
ex:Bridge_1 bs:hasPart
[
  rdf:type ex:BridgeDeck ;
] .

# Voorwaarde 2a -------------------------------------------------------------------------------
# Iets is een metalen brug als het materiaal van staal of ijzer is
# Beschrijf de onderscheidende kenmerken in OWL class expressions:

ex:MetalBridge owl:equivalentClass
[
  rdf:type owl:Restriction ;

  owl:qualifiedCardinality "1"^^xsd:nonNegativeInteger ;
  # gebruik deze voor inference-test zonder OWA
  # owl:minQualifiedCardinality "1"^^xsd:nonNegativeInteger ;
  
  owl:onProperty ex:material ;
  owl:onClass
  [
     rdf:type owl:Class ;
     owl:oneOf (ex:Iron ex:Steel) ;

  ] ;
] .

# Een OWL-reasoner kan afleiden dat Bridge_1 van het type ex:MetalBridge is:
ex:Bridge_1 ex:material ex:Steel .

# Voorwaarde 3a ---------------------------------------------------------------------------
# Iets is een korte brug als de lengte tussen 0 en 100 ligt
# Beschrijf de onderscheidende kenmerken in OWL class expressions:

ex:ShortBridge rdf:type owl:Class ;
owl:equivalentClass
[
  rdf:type owl:Restriction ;
  
  owl:qualifiedCardinality "1"^^xsd:nonNegativeInteger ;
  # gebruik deze voor inference-test zonder OWA
  # owl:minQualifiedCardinality "1"^^xsd:nonNegativeInteger ; 
  
  owl:onProperty ex:length ;
  owl:onClass ex:Max100m ;
] .
ex:Max100m rdf:type owl:Class ;
rdfs:subClassOf bs:QuantityValue ;
owl:equivalentClass
[
  rdf:type owl:Restriction ;
  owl:onProperty rdf:value ;

  owl:allValuesFrom
  # gebruik deze voor inference-test zonder OWA
  # owl:someValuesFrom 
  [
     rdf:type rdfs:Datatype ;
     owl:onDatatype xsd:decimal ;
     owl:withRestrictions
     (
       [ xsd:minExclusive "0"^^xsd:decimal ; ]
       [ xsd:maxExclusive "100"^^xsd:decimal ; ]
     );
  ] ;
] .

# Een OWL-reasoner kan afleiden dat ex:length verwijst naar een ex:BridgeLength 
# en vervolgens dat Bridge_1 van het type ex:ShortBridge is:
ex:Bridge_1 ex:length
[
  rdf:value 50.0 ;
] .

#--- Data bij SHACL proef -----------------------------------------------------------------
ex:Bridge_1b rdf:type ex:Bridge .
ex:Bridge_2b rdf:type ex:MetalBridge ;
bs:hasPart
[
  rdf:type ex:BridgeDeck ;
] ;
ex:material ex:Concrete .
ex:Bridge_3b rdf:type ex:ShortBridge ;
bs:hasPart
[
  rdf:type ex:BridgeDeck ;
] ;
ex:length
[
  rdf:value 100.0 ;
] .
ex:Bridge_4
bs:hasPart
[
  rdf:type ex:BridgeDeck ;
] ;
ex:bridgeHeight
[
  rdf:value 12.0 ;
  ex:obtainment
  [
    ex:obtainedBy ex:Deesign ;
    ex:obtainedDate "13-4-2020"^^xsd:date ;
  ];
] ;
ex:bridgeHeight
[
  rdf:value 12.1 ;
  ex:obtainment
  [
     ex:obtainedDate "2020-04-14"^^xsd:date ;
  ];
] ;
ex:bridgeHeight
[
  rdf:value 12.2 ;
] .
</pre>

Bestand "Proef GWSW 2.0 query.rq"
---------------------------------
<pre class="file">
PREFIX rdf: &lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#&gt;
PREFIX owl: &lt;http://www.w3.org/2002/07/owl#&gt;
PREFIX rdfs: &lt;http://www.w3.org/2000/01/rdf-schema#&gt;
PREFIX skos: &lt;http://www.w3.org/2004/02/skos/core#&gt;
PREFIX xsd: &lt;http://www.w3.org/2001/XMLSchema#&gt;
PREFIX bs: &lt;https://w3id.org/def/basicsemantics-owl#&gt;
PREFIX ex: &lt;https://w3id.org/def/example#&gt;

SELECT *
WHERE
{
  ex:Bridge_1 rdf:type ?typeBridge .
  optional {
     ex:Bridge_1 ex:length ?length .
     ?length rdf:type ?typeValue .
  }
}
</pre>

Bestand "Proef GWSW 2.0 SHACL.txt"
----------------------------------
<pre class="file">
@prefix bs: &lt;https://w3id.org/def/basicsemantics-owl#&gt; .
@prefix ex: &lt;https://w3id.org/def/example#&gt; .
@prefix owl: &lt;http://www.w3.org/2002/07/owl#&gt; .
@prefix rdf: &lt;http://www.w3.org/1999/02/22-rdf-syntax-ns#&gt; .
@prefix xml: &lt;http://www.w3.org/XML/1998/namespace&gt; .
@prefix xsd: &lt;http://www.w3.org/2001/XMLSchema#&gt; .
@prefix qudt: &lt;http://qudt.org/schema/qudt/&gt; .
@prefix rdfs: &lt;http://www.w3.org/2000/01/rdf-schema#&gt; .
@prefix sh: &lt;http://www.w3.org/ns/shacl#&gt; .
@prefix skos: &lt;http://www.w3.org/2004/02/skos/core#&gt; .
@prefix unit: &lt;http://qudt.org/vocab/unit/&gt; .
@prefix quantitykind: &lt;http://qudt.org/vocab/quantitykind/&gt; .

# Voorwaarde 1b -------------------------------------------------------------------------------
# De klasse "brug" moet als deel een "brugdek" hebben
# Beschrijf de restrictie in SHACL:

ex:BridgeShape rdf:type sh:NodeShape ;
sh:targetClass ex:Bridge ;
sh:property
[ # impliciete typering sh:PropertyShape
  sh:path bs:hasPart ;
  sh:class ex:BridgeDeck ;
  sh:minCount 1 ;
  sh:message "Bridge: BridgeDeck is missing as part" ;
  sh:severity sh:Violation ;
] .

# Voorwaarde 2b -------------------------------------------------------------------------------
# De klasse "metalen brug" moet als materiaal "ijzer" of "staal" hebben
# Beschrijf de restrictie in SHACL:

ex:MetalBridgeShape rdf:type sh:NodeShape ;
sh:targetClass ex:MetalBridge ;
sh:property
[ # impliciete typering sh:PropertyShape
  sh:path ex:material ;
  sh:or ( [sh:hasValue ex:Iron;][sh:hasValue ex:Steel;] ) ;
  sh:minCount 1 ;
  sh:message "MetalBridge: Material is missing or has wrong value" ;
  sh:severity sh:Warning ;
] .

# Voorwaarde 3b -------------------------------------------------------------------------------
# De lengte van een korte brug ligt tussen 0 en 100
# Beschrijf de restrictie in SHACL:

ex:ShortBridgeShape rdf:type sh:NodeShape ;
sh:targetClass ex:ShortBridge ;
sh:property
[
  sh:path (ex:length rdf:value) ; # predicate path
  sh:datatype xsd:decimal ;
  sh:minExclusive 0 ;
  sh:maxExclusive 100 ;
  sh:message "ShortBridge-length is not a number or out of bounds" ;
  sh:severity sh:Warning ;
] .

# Voorwaarde 4 -------------------------------------------------------------------------------
# Als een brug de eigenschap "hoogte" heeft, dan ook de wijze van inwinning registreren.
# De hoogte hoeft er niet te zijn, maar als ie er is moet de metadata er ook zijn.
# Beschrijf het in SHACL:
# Controleer de samenstelling van meta-gegeven inwinning (moet de wijze + datum bevatten)

ex:ObtainedShape rdf:type sh:NodeShape ;
sh:targetClass ex:Obtainment ;
sh:property
[
  sh:path ex:obtainedBy ;
  sh:minCount 1 ;
  sh:maxCount 1 ;
  sh:message "The way-obtained is missing" ;
] ;
sh:property
[
  sh:path ex:obtainedDate ;
  sh:minCount 1 ;
  sh:maxCount 1 ;
  sh:message "The date-obtained is missing" ;
] ;
sh:property
[
  sh:path ex:obtainedBy ;
  sh:in ( ex:Design ex:Revision ) ;
  sh:message "This way-obtained is unknown" ;
] ;
sh:property
[
  sh:path ex:obtainedDate ;
  sh:datatype xsd:date ;
  sh:message "This date-obtained has wrong value" ;
] .
# Controleer of de brughoogte een inwinning heeft
ex:BridgeHeightInwShape rdf:type sh:NodeShape ;
sh:targetObjectsOf ex:bridgeHeight ;
sh:property
[
  sh:path ex:obtainment ;
  sh:minCount 1 ;
  sh:maxCount 1 ;
  sh:message "Bridge-Heigth: Obtainment unknown" ;
  sh:severity sh:Violation ; # constraint violation
] .
</pre>

Oefening: Leid af dat het individu een brug is
----------------------------------------------

Iets is een brug als het minimaal één brugdek en een lengte tussen 0-200 heeft*
Beschrijf de onderscheidende kenmerken in OWL class expressions:

<pre class="file">
ex:Bridge 	owl:equivalentClass
	[ 
	   rdf:type		owl:Class ;
	   owl:intersectionOf		(_:x _:y) . # zowel 1 brugdek als hoogte 12-19
	] .

_:x	rdf:type                      			owl:Restriction ;
	owl:minQualifiedCardinality	"1"^^xsd:nonNegativeInteger ;
	owl:onProperty                		bs:hasPart ;
	owl:onClass      			ex:BridgeDeck . 

_:y	rdf:type                      		owl:Restriction ;
	owl:minQualifiedCardinality	"1"^^xsd:nonNegativeInteger ;
	owl:onProperty                	ex:length ;
	owl:allValuesFrom
	[
	     owl:intersectionOf		# zowel QuanVal als CE-restrictie op rdf:value
	     (  bs:QuantityValue
	        [
	          rdf:type		owl:Restriction ;
	          owl:onProperty		rdf:value ;
	          owl:allValuesFrom 		
	          [
	             rdf:type 			rdfs:Datatype ;
	             owl:onDatatype 		xsd:decimal ;  
	             owl:withRestrictions	
	             ( 
	                [ xsd:minExclusive  "0"^^xsd:decimal; ] 
	                [ xsd:maxInclusive  "200"^^xsd:decimal; ] 
                                                                     );
	          ] ;
	         # Of bijvoorbeeld, als de lengte exact 12 moet zijn, ipv owl:allValuesFrom:	
	         # owl:hasValue 		"150"^^xsd:decimal	
	        ]
	     )				
	] .
</pre>

Een OWL-reasoner leidt af dat ex:Bridge_1 van het type ex:Brigde is:

<pre class="file">
ex:Brigde_1	bs:hasPart			
	[
	  rdf:type		ex:BridgeDeck;
	] ; 
</pre>
