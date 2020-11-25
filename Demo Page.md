<!DOCTYPE html>
<html>
<head>
<title>Costruire un network per PAThs</title>
</head>
<style>    
    h1  {
        color: blue; 
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        margin: 50px;
        line-height: 1.3em;
        letter-spacing: 0.1em;
        font-size: 300%;
    }
    h2  {
        color: black; 
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        margin: 70px;
        line-height: 1.3em;
        letter-spacing: 0.1em;
        font-size: 230%;
    }
    h3  {
        color: rgb(56, 56, 56); 
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        margin: 70px;
        line-height: 1.3em;
        letter-spacing: 0.1em;
        font-size: 200%;
    }
    h4   {
        color: rgb(56, 56, 56); 
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        margin: 10px;
        line-height: 1.3em;
        letter-spacing: 0.1em;
        font-size: 100%;
        text-align: center;
    }
    p   {
        color: black;
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        margin: 30px;
        line-height: 1.3em;
        letter-spacing: 0.1em;
        text-align: justify;
        font-size: 130%;
    }
    img {width:1000px;height:600px;
        margin: 30px;
    }
    .codebox {
        padding: 20px;
        font-family: Courier, sans-serif;
        font-size: 1em;
	    line-height: 1.3;
        color: #fff;
        background-color: #2c3e50;
        width: 600px;
        height: 200px
        overflow: auto;
        margin-left: 400px;
    }
    footer {
        font-family: Courier, sans-serif;
        color: black;
        font-size: 1em;
        line-height: 1.3;
        background-color: rgb(255, 255, 255);
    }

</style>
</head>

<body> 

<figure>
    <center>
        <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image0.jpg" alt="Copertina">
    </center>
</figure>

    <h1>Costruire un network per PAThs</h1>
    <h3>Paolo Rosati, paolo.rosati@uniroma1.it</h3>
<br>
<p>La costruzione di un network viario per PAThs ha richiesto diversi punti e passaggi tecnici al fine di giungere al risultato finale.
L'operazione di digitalizzazione è stata compiuta tenendo a mente due cose: 
la prima riguarda la necessità del gruppo di ricerca di individuare rami storici del Nilo e delle sue derivazioni secondarie: 
principalmente <i>Bhar Yussuf</i>, <i>Damietta</i> e <i>Rosetta Branches</i>.<br><br>
La seconda legata alla possibilità di un mantenimento stabile del layer idrografico al fine di fornire una traccia storica del corso di questi
corsi d'acqua di buon dettaglio, utile metodo di confronto per il futuro e con la cartografia georiferita e
edita dal PAThs Team e disponibile al seguente sito <a href="https://docs.paths-erc.eu/data/"><i><b>PAThs data repository & documentation</i></b></a>.<br>
</p>
    <a name="primo"><h2>0. Prima di iniziare</h2></a>
    <h3>0.1 Software e metodi per la Demo</h3>
    <p>Per la presente demo è previsto l'uso senza conoscenza approfondita di <a href="https://www.qgis.org/it/site/forusers/download.html">QGIS 3.10.12 'A Coruña'</a> 
disponibile per Windows, macOS, Linux e Android. 
Una volta scaricato QGIS si dovrà lanciare il programma e creare un nuovo progetto chiamato <i>my_road_network</i>. Per lavorare con la piattaforma di PyQGIS presente all'interno di QGIS. Il metodo per eseguire gli script 
presenti all'interno di questo esercizio è molto semplice. Aprire nel menù principale la sezione <i>Plugin</i> e cliccare su <i>Console Python</i>, altrienti shortcut <b>Ctrl+Alt+P</b>, per tastiera macOS <b>Cmd+Alt+P</b>.
Cliccare quindi sulla console l'inconcina <i>Mostra Editor</i>. Seguire quindi le istruzioni, copiare e incollare il codice proposto, un passaggio dopo l'altro nello spazio bianco, 
salvare lo script in una propria directory <i>/home/.../Didattica/my_script</i> lanciarlo con il tasto play verde dell'Editor. Tutti gli script che sono proposti sono stati
scritti appositamente per l'esercizio della presente demo e sono stati riadattati dall'autore utilizzando gli strumenti del potente <i>QGIS Processing</i>.
    </p>

    <h2>1. Generare la linea mediana di una rete fluviale</h2>
    <h3>1.0 Digitalizzare la rete fluviale in forma di poligono</h3>
<p>L'intero corso del Nilo e le sue derivazioni sono state digitalizzate ad una scala di base <b>1:5000</b> e inferiori nei punti salienti. Il formato
file scelto è stato il geopackage per le sue proprietà di apertura, interscambio dati, usabilità, stabilità e durevolezza. Il layer qui descritto è chiamato
EGY_water.gpkg ed è disponibile per la presente demo all'interno della cartella "data_demo" presente su Github <a href="https://github.com/archeorosati/Didattica/blob/main/data_demo/EGY_water.gpkg"><b>Download EGY_water.gpkg</b></a>.<br>
Il layer è in EPSG:4326, si consiglia di utilizzare come Basemap il layer <i>Bing Satellite</i> presente nel plugin di QGIS <i>QuickMapService</i>. <br>
Per chi non avesse mai usato questo plugin, una volta scaricato dal repository, click su <i>Web</i>, <i>QuickMapService</i> ricordarsi di caricare i <i>Contribute Services</i> da <i>Settings</i>, <i>More Services</i> e infine <i>Save</i>.
A questo punto tornare in <i>Web</i>, <i>QuickMapService</i> e scegliere <i>Bing Satellite</i>, tra i servizi <i>Bing</i> offerti dal plugin.
    </p>
    <figure>
<center>
    <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image1.jpeg" alt="Digitalized PAThs Nile">
</center>
 <figurecaption>
    <h4>Immagine 1: Il corso del Nilo a sud del Cairo 1:50000</h4>
</figurecaption>
</figure>
    <h3>1.1 Eliminare i buchi e semplificare il poligono </h3>
    <p>Come detto sopra nel <a href="#primo">primo paragrafo</a> una volta copiato e incollato il codice salvare in una directory 
<i>home/.../Didattica/my_script</i> e premere il tastino verde <i>Play</i>.<br><br>
Al fine di questa demo è più semplice aver il minor numero possibile di vertici nel perimetro, si procede quindi 
con due operazioni la prima è eliminare i buchi, le isole del nostro poligono attraverso il seguente codice:
    </p>    
<div class="codebox">
    <code>
        project = QgsProject.instance()<br>
        poligon = project.mapLayersByName('EGY_water')[0]<br>
        parameters = {<br>
            <pre>
            'INPUT':poligon,<br></pre>
            <pre>
            'MIN_AREA':0,<br></pre>
            <pre>
            'OUTPUT':'memory:new_nile'<br></pre>
        }<br>
        result = processing.run("native:deleteholes", parameters)<br>
        project.addMapLayer(result['OUTPUT'])
    </code>
</div>
    <h4>Code 1: il codice va a eliminare le "isole" del Nilo</h4>
<br>
    <p>Questa operazione e le seguenti, porteranno alla costruzione di "layer temporanei" che durante l'esercizio, o alla fine di esso devono essere 
salvati in qualsiasi formato <i>es. ESRI Shapefile</i> per poterli mantenere in memoria e nella memoria del progetto. <br><br>
Il secondo passo è quello di semplificare le geometrie e quindi lanciare un algoritmo che elimina in modo netto un numero di vertici sul perimetro del poligono 
al fine di creare la linea mediana più semplice possibile, senza compromettere il tracciato realistico dei fiumi.
    </p>
<div class="codebox">
    <code>
        project = QgsProject.instance()<br>
        buffer = project.mapLayersByName('new_nile')[0]<br>
        parameters = {<br>
            <pre>
            'INPUT': buffer,<br></pre>
            <pre>
            'DISTANCE':0.005,<br></pre>
            <pre>
            'SEGMENTS':5,<br></pre>
            <pre>
            'END_CAP_STYLE':0,<br></pre>
            <pre>
            'JOIN_STYLE':0,<br></pre>
            <pre>
            'MITER_LIMIT':2,<br></pre>
            <pre>
            'DISSOLVE':True,<br></pre>
            <pre>
            'OUTPUT':'memory:new_simple_nile',<br></pre>
        },<br>
        
        result = processing.run("native:buffer", parameters),<br>
        project.addMapLayer(result['OUTPUT']),<br>
    </code>
</div>
<br>
    <h4>Code 2: questo script serve per creare un buffer intorno al layer poligonale<br>il codice è utilizzato per semplificare il poligono</h4>
<br>
    <p>Dei parametri inseriti certamente il più ostico è <i>'DISTANANCE':0,005</i>. Questo valore viene spiegato grazie al fatto che stiamo operando su layer proiettati
tramite l'EPSG gerografico WGS84 che lavora in gradi, primi, secondi e frazioni di secondo. Il significato è di eliminare un vertice in un intorno di 0,005 gradi ovvero 5 primi.
    </p>
<br>
<figure>
    <center>
       <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image2.jpeg" alt="Simplified line for PAThs Nile">
   </center>
        <figurecaption>
    <h4>Immagine 2: Il confronto tra il layer non semplificato (senape) e quello semplificato (grigio)</h4>
       </figurecaption>
</figure>
<br>
    <h3>1.2 Estrarre i vertici</h3>
<p> Una volta scaricato e aggiunti i due layer <b>EGY_water</b> e <b>Bing Satellite</b> su QGIS. Aprire l'editor di Python e inserire il seguente script:
</p>
<div class="codebox">
    <a name="Code3">
        <code>   
        project = QgsProject.instance()<br>
        poly = project.mapLayersByName('new_simple_nile')[0]<br>
        vertices = {<br>
            <pre>
            'INPUT':'poly,<br></pre>
            <pre>
            'OUTPUT':'memory:nile_vertex'</pre>
        }<br>
        result = processing.run("native:extractvertices", vertices)<br>
        project.addMapLayer(result['OUTPUT'])<br>
        </code>
    </a>
</div>
<br>
<h4>Code 3: il codice Python per l'estrazione dei vertici</h4></a>
<br>
    <p>In questa maniera verrà creato un punto per ogni vertice lungo l'intero perimetro del poligono della rete fluviale <i>EGY_river</i> come da Immagine 3.
    </p>
<figure>
    <center>
       <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image3.jpeg" alt="Memory layer vertex in PAThs Nile">
   </center>
        <figurecaption>
           <h4>Immagine 3: Risultato dell'estrazione dei vertici</h4>
       </figurecaption>
</figure>
<h3>1.3 Il Diagramma di Voronoi</h3>
    <p>Ci avviciniamo quindi alla costruzione della linea mediana del Nilo e delle sue derivazioni un passo fondamentale passa per la costruzione dei poligoni del 
<a href="https://it.wikipedia.org/wiki/Diagramma_di_Voronoi"><i>Diagramma di Voronoi</i></a>, che in ambito archeologico sono comunemente conosciuti con il nome di <i>Poligoni di Thiessen</i>.
L'algoritmo che sarà quindi ora processato consentirà di utilizzare lo strumento dedicato riuscendo in breve ad ottenere il risultato. In questo frangente si andrà a lavorare con
gli algoritmi di una libreria proveniente da un altro software GIS <i>GRASS</i>, decisamente potente e adatto per il tipo di operazioni massive che sono richieste.
    </p>
<div class="codebox">
    <code>   
    project = QgsProject.instance()<br>
    poly = project.mapLayersByName('temporary_vertex')[0]<br>
        parameters = {<br>
            <pre>
            'INPUT': poly,<br></pre>
            <pre>
            'BUFFER':0,<br></pre>
            <pre>
	        'OUTPUT':'memory:nile_voronoi'<br></pre>
    }<br>
    result = processing.run("qgis:voronoipolygons", parameters)<br>
    project.addMapLayer(result['OUTPUT'])<br>
    </code>
</div>
        <h4>Code 4: una semplice stringa per estrarre con qualche secondo di attesa il Diagramma di Voronoi</h4>
    <br>
    <figure>
        <center>
           <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image4.jpeg" alt="Voronoi near Cairo Area">
       </center>
            <figurecaption>
               <h4>Immagine 4: Il diagramma di Voronoi presso il Cairo e 'new_simple_line' in trasparenza</h4>
           </figurecaption>
    </figure>
<h3>1.4 Estrarre il perimetro di new_simple_nile</h3>
<p>Il layer più utile di questo workflow è <b>new_simple_nile</b> con il quale, come si è letto sono state compiute la maggiorparte delle operazioni.<br>
Di questo, non si getterà via nulla ne abbiamo utilizzato la forma poligonale, i vertici e ora estraiamo il suo perimetro. La linea che ne deriva andrà tagliata,
scegliendo quale sponda mantenere del sistema fluviale, l'orientale o l'occidentale, non fa differenza. Questa che è l'unica operazione manuale di questo workflow,
ci consentirà di ottenere delle linee tramite le queli selezionare tutti i poligoni che sono intersecati dalla linea di costa scelta del sistema.
</p>
<div class="codebox">
    <a name="Code5">
        <code>  
project = QgsProject.instance()<br>
poly = project.mapLayersByName('new_simple_nile')[0]<br>
parameters ={<br>
        <pre>
        'INPUT': poly,<br></pre>
        'OUTPUT':'memory:riversides_line'<br></pre>
}<br>
result = processing.run("native:polygonstolines", parameters)<br>
project.addMapLayer(result['OUTPUT'])
         </code>
    </a>
</div>
<br>
    <h4>Code 5: estrazione del perimetro</h4>
<br>
    <figure>
        <center>
           <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image5.jpeg" alt="Extracted riversides_line">
       </center>
            <figurecaption>
               <h4>Immagine 5: In rosso il riverside_lines, sullo sfondo il Diagramma di Voronoi presso il Cairo e 'new_simple_line' in trasparenza</h4>
           </figurecaption>
    </figure>
<p>Giunti a questo punto <i>Dividere l'elemento</i> linea ottenuta ed eliminare la restante porzione. Per semplicità di esecuzione è stato quindi scelto 
il lato occidentale del <i>Bahr Yussuf</i> e quello orientale del Nilo, del <i>Rosetta</i> e <i>Damietta Branches</i>, facendo attenzione a far lo stesso anche per i Wadi del <i>Nasser Lake</i>.
<br><br> A questo punto devono essere effettuate alcune operazioni semplici tramite processing si <i>selezionano tramite posizione</i> tutti i Poligoni di Voronoi attraversati e contenuti dalle linee scelte.
Si deve poi selezionare il loro inverso ed eliminare la porzione del Diagramma appena selezionato, si puliscono eventuali poligoni refusi. Una volta fatto questo si passa al punto successivo.
</p>
<br>
    <figure>
        <center>
           <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image6.jpeg" alt="Select by position">
       </center>
            <figurecaption>
               <h4>Immagine 6: il risultato della selezione tramite posizione dei poligoni del Diagramma di Voronoi</h4>
           </figurecaption>
    </figure>
<h3>1.4 Estrarre la linea mediana</h3>
<p>Eccoci quindi giunti all'estrazione della linea mediana, prima di fare ciò però dobbiamo unire in un unica semplice operazione, unire tutti i restanti Poligoni di Voronoi con il seguente codice.
</p>
<div class="codebox">
    <a name="Code6">
        <code>  
project = QgsProject.instance()<br>
poly = project.mapLayersByName('nile_voronoy')[0]<br>
parameters = {<br>
    <pre>
    'INPUT': poly,<br></pre>
    <pre>
    'FIELD':NULL,<br></pre>
    <pre>
    'OUTPUT':'memory:unified_nile_voronoi'<br></pre>
}<br>
result = processing.run("native:dissolve", parameters)<br>
project.addMapLayer(result['OUTPUT'])
        </code>
    </a>
</div>
<br>
    <h4>Code 6: il presente codice dissolve i poligoni del Diagramma di Voronoi</h4>
<br>
    <figure>
        <center>
           <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image7.jpeg" alt="Dissolve voronoi poly">
       </center>
            <figurecaption>
               <h4>Immagine 7: la dissoluzione del restante Diagramma di Voronoi in un unico poligono</h4>
           </figurecaption>
    </figure>
<p>Non ci restano che tre semplici operazioni di cui una automatica e riguarda l'estrazione della linea del presente poligono e una manuale,
dividere in parti il perimetro ottenuto ed eliminare la parte esterna del poligono in maniera tale da ottenere unicamente le linee mediane
della nostra rete idrica primaria e secondaria. Il codice del primo passaggio è semplice ed è decisamente simile a quello precedente 
<a href=#Code5>Code 5</a>, cambiano unicamente alcuni parametri:
</p>
<div class="codebox">
    <a name="Code7">
        <code>  
project = QgsProject.instance()<br>
poly = project.mapLayersByName('unified_nile_voronoi')[0]<br>
parameters ={<br>
        <pre>
        'INPUT': poly,<br></pre>
        <pre>
        'OUTPUT':'memory:median_line'<br></pre>
}<br>
result = processing.run("native:polygonstolines", parameters)<br>
project.addMapLayer(result['OUTPUT'])
         </code>
    </a>
</div>
<br>
    <h4>Code 7: estrazione del perimetro del unified_nile_voronoi</h4>
<br>
<p> Infine si dividono gli elementi manualmente, mantenendo le linee mediane dei nostri corsi d'acqua e si eliminano le parti restanti.
Si riconnettono le linee come nel caso del <i>Damietta Branch</i>.
</p>
<figure>
    <center>
       <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/image8.jpeg" alt="Estrazione della linea mediana">
   </center>
        <figurecaption>
           <h4>Immagine 8: il risultato dell'estrazione della linea mediana </h4>
       </figurecaption>
</figure>
<h2>2. Generare il network</h2>
<h3>2.0 Inserire i <i>PAThs Places</i> e costruire il primo ramo del network</h3>
<p> Il primo passo è quindi quello di inserire nel nostro progetto i <i>PAThs Places</i> che si trovano nella cartella <i>data_demo</i>.
I punti utilizzati in questa demo non sono totalmente aggiornati all'ultimo giorno corrente in quanto la mappatura dei <i>PAThs Places</i> continua
in maniera molto veloce soprattutto nella parte del Delta. Per praticità inoltre questa demo non si applica a tutti <i>PAThs Places</i> ma unicamente
ad un settore particolare quelli più vicini al Nilo <i>firts_places</i>, la prima fascia. Infatti in uno schema che valorizza la posizione dei siti in base al Nilo,
i PAThs Places sono stati catalogati in base al grado di vicinanza. <br><br>
</p>
<div class="codebox">
    <a name="Code8">
        <code>  
select p.id, ST_ShortestLine (l.geometry, p.geometry) as geometry, st_length(ST_ShortestLine(l.geometry, p.geometry)) as lungh_m<br>
from first_places as p, median_line as l<br>
group by p.id<br>
having min(st_length(shortestline(p.geometry,l.geometry)))
         </code>
    </a>
</div>
<br>
    <h4>Code 8: Creazione della linea più corta</h4>
<br>
<p>
Questo codice diversamente dai precedenti non va inserito nell'editor di Python (si tratta di alcune righe di SQL "dialetto" Spatialite), ma va inserito all'interno del box di inserimento codice
presente nell'inserimento di un vettore temporaneo. Per raggiungere questo spazio si deve quindi cliccare su <i>Layer</i>, poi su <i>Crea Layer</i>, <i>Aggiungi Layer Virtuale</i>
e infine incollare la riga di codice nel box bianco chiamato <i>Interrogazione</i> non far nulla oltre se si è rispettato il nome dei layer basterà cliccare su <i>Aggiungi</i>.
</p>
<p>Così a questo punto si connettono con una linea i punti della prima fascia si selezionano a mano e si estraggono i punti di seconda fascia e si applica il medesimo Code 8,
camniando ovviamente i nomi dei nuovi layer, in questo modo si ricava un network con schema "ad albero" così come nell'Immagine 9: 
</p>
<center>
    <img src="https://raw.githubusercontent.com/archeorosati/Didattica/main/Images/immage9.jpeg" alt="Risultato del Network">
</center>
     <figurecaption>
        <h4>Immagine 9: risultato del network nella zona di Tebe notare lo schema ad albero </h4>
    </figurecaption>
</figure>
<p>
<footer>
<center>
    <small>&copy; Paolo Rosati 2020 for PAThs ERC Project (P.I. Paola Buzi), Sapienza Università di Roma.<br>
        Use, sharing and remixing permitted under terms of the<br>
    <a rel="license" href="https://creativecommons.org/licenses/by/4.0/">
            Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License
    </a>
    <br>
        <a href ="https://atlas.paths-erc.eu/">Visit PAThs the Archaeological Atlas of the Coptic Literature 
    </a>
    <br>
</center>
    </small>
</footer>  
</p>
</body>
</html>
