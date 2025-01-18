# BBDD

## [SELECTS](https://moodle.elpuig.xeill.net/mod/page/view.php?id=15351)

1. Mostra totes les dades de totes les persones.
   ```postgresql
   SELECT * FROM persona;
   ```
   | passaport | nom       | cognom   | ciutat        |
   |:----------|:----------|:---------|:--------------|
   | 27673812M | Carme     | Peralta  | Cadaqués      |
   | X3478937A | Carles    | Sanàbria | Badalona      |
   | 47548338K | Anna      | Sanàbria | Badalona      |
   | 45493393Z | Jesús     | Hortesa  | Castelldefels |
   | C00001549 | Mick      | Brown    | San Francisco |
   | 294394950 | Klauss    | Stallman | Berlín        |
   | 39238229E | Sònia     | Aragall  | Barcelona     |
   | 187448338 | Rita      | Derbeken | Berlín        |
   | 46372382N | Roser     | Puente   | Tiana         |
   | C01X01TN  | Roberto   | Rietto   | Roma          |
   | 38433548L | Anna      | Margalef | Tiana         |
   | 32234958K | José      | Sanlúcar | Castelldefels |
   | 38474483Z | Miquel    | Vila     | Tredòs        |
   | 42065765F | Camila    | Noriega  | Castelldefels |
   | 59119283Z | Pere      | Camprubí | Barcelona     |
   | 27961020N | Pere      | Garcia   | Barcelona     |
   | 19891898A | Maria     | Martín   | Barcelona     |
   | Y4394950D | Helena    | Pérez    | Tona          |
   | 37228901C | Sebastià  | Fonollà  | Sitges        |
   | 29874567M | Leonardo  | Soler    | Lleida        |
   | 05CK02337 | JeanMarie | Godard   | París         |
   | C00021549 | Mick      | Brown    | New York      |
   | 36940559Y | Magdalena | Pinós    | Sitges        |
   | 38223890Y | Jordi     | Parmalat | Badalona      |
   | 51234329N | Mireia    | Matas    | Barcelona     |
   | 45847558W | José      | González | Montgat       |
   | 48377283A | Daniel    | González | Montgat       |
   | 37866969E | Carme     | Ferrer   | Amposta       |
   | 27827228B | Sonia     | Colmena  | Roses         |
   | X4534332C | Gabriel   | Cobos    | Roses         |
   | Y3439185D | Boris     | Santos   | Viladecans    |

2. Fer la llista de les ciutats de les persones de la base de dades, SENSE REPETITS.
   ```postgresql
   SELECT DISTINCT ciutat FROM persona;
   ```
   | ciutat        |
   |:--------------|
   | Roses         |
   | Tredòs        |
   | New York      |
   | Tiana         |
   | Amposta       |
   | San Francisco |
   | Sitges        |
   | Viladecans    |
   | Tona          |
   | Berlín        |
   | Barcelona     |
   | Badalona      |
   | Montgat       |
   | Lleida        |
   | Cadaqués      |
   | Castelldefels |
   | Roma          |
   | París         |

3. Experiment amb FROM. Una mica de 'teoria':
    1. Crear una taula t, amb un sol atribut que es digui n i sigui un enter: CREATE TABLE t(n INTEGER);
    2. Inserir un nombre qualsevol a la taula t: INSERT INTO t VALUES(314);
    3. Mostrar el nom de totes les comarques afegint innecessàriament la taula t en la clàusula FROM: SELECT comarca
       FROM comarca, t;
    4. Quants resultats treus?
    5. Inserir un nou nombre a la taula t, com en el pas 3.b)
    6. Mostrar de nou el nom de totes les comarques afegint innecessàriament la taula t en la clàusula FROM
    7. Explica el resultat que obtens.
    8. Eliminar la taula t de la teva base de dades (DROP)

4. Obtenir totes les combinacions possibles de ciutats i comarques
   ```postgresql
   SELECT ciutat.ciutat, comarca.comarca FROM ciutat, comarca;
   ```
   | ciutat     | comarca        |
   |:-----------|:---------------|
   | Cadaqués   | Barcelonès     |
   | Cadaqués   | Alt Empordà    |
   | Cadaqués   | Baix Llobregat |
   | Cadaqués   | Val d'Aran     |
   | Cadaqués   | Osona          |
   | Cadaqués   | Garraf         |
   | Cadaqués   | Segrià         |
   | Cadaqués   | Montsià        |
   | Cadaqués   | Maresme        |
   | Cadaqués   | Tarragonès     |
   | Badalona   | Barcelonès     |
   | Badalona   | Alt Empordà    |
   | Badalona   | Baix Llobregat |
   | Badalona   | Val d'Aran     |
   | Badalona   | Osona          |
   | Badalona   | Garraf         |
   | Badalona   | Segrià         |
   | Badalona   | Montsià        |
   | Badalona   | Maresme        |
   | Viladecans | Alt Empordà    |
   | Viladecans | Baix Llobregat |
   | Viladecans | Val d'Aran     |
   | Viladecans | Osona          |
   | Viladecans | Garraf         |
   | Viladecans | Segrià         |
   | Viladecans | Montsià        |
   | Viladecans | Maresme        |
   | Viladecans | Tarragonès     |
   **Hay demasiadas lineas... esto hará un cross join (todas las combinaciones posibles) de todas las ciudades de
   ciutat.ciutat y comarca.comarca**

5. RENOMENAMENT: De vegades, un mateix nom a un atribut de dues taules diferents ens pot portar problemes:

    1. Prova de fer aquesta consulta: SELECT comarca from ciutat, comarca;
       ```postgresql
       SELECT ciutat, comarca FROM ciutat, comarca;
       ```

       ```postgresql
       -- [42702] ERROR: column reference "comarca" is ambiguous Position: 16
       ```
    2. Quin error et dona? Per què?

       Da este error ya que la columna comarca está en ambas tablas, por ende Postgres no sabe cual usar.

    3. Els sistemes gestors porten mecanismes per renombrar camps o taules, tant per la sortida de la consulta com per
       especificar la taula que ens proveeix les dades:

       --> per exemple ciutat.comarca faria referencia explícita a l'atribut 'comarca' de la taula 'ciutat'

       --> Si fem select ciutat.comarca AS comarques, estaríem renombrant la sortida del SELECT a 'comarques'.

       --> Ara sí, prova de fer la consulta 5.a amb renombrant i esquivant l'error ;)

6. RENOMBRAMENTS II:
   Executa aquesta consulta i explica què estàs fent:

   -> SELECT c.pepito FROM ciutat c(x,x,x,pepito), comarca;

   INTRODUÏM el WHERE!

7. Noms de les persones de Cadaqués.

   ```postgresql
   SELECT nom FROM persona WHERE ciutat='Cadaqués';
   ```

   | nom   |
   |:------|
   | Carme |

8. Nom de les persones de la comarca del Barcelonès (OJO! amb la clau forana!)
   ```postgresql
   SELECT nom FROM persona +N ciutat WHERE comarca='Barcelonès';
   ```
   | nom    |
   |:-------|
   | Carles |
   | Anna   |
   | Sònia  |
   | Pere   |
   | Pere   |
   | Maria  |
   | Jordi  |
   | Mireia |

9. A qui coneix la Carme?
   ```postgresql
   SELECT otro.*
     FROM persona AS carme, coneix, persona AS otro
   WHERE carme.nom = 'Carme'
     AND carme.cognom = 'Peralta'
     AND coneix.coneix = carme.passaport
     AND coneix.es_coneguda = otro.passaport;
   ```
   | passaport | nom    | cognom   | ciutat   |
   |:----------|:-------|:---------|:---------|
   | X3478937A | Carles | Sanàbria | Badalona |
   | 47548338K | Anna   | Sanàbria | Badalona |
   | 294394950 | Klauss | Stallman | Berlín   |

   ```postgresql
   SELECT (SELECT nom FROM persona WHERE coneix.es_coneguda = persona.passaport)
   FROM coneix
   WHERE coneix.coneix = (SELECT passaport FROM persona WHERE nom = 'Carme' AND cognom = 'Peralta');
   ```

   | nom    |
   |:-------|
   | Carles |
   | Anna   |
   | Klauss |

10. Quantes persones hi ha a la base de dades?

   ```postgresql
   SELECT COUNT(*) FROM persona;
   ```
   | count |
   |:------|
   | 31    |

11. De quantes comarques hi ha persones a la base de dades?

   ```postgresql
   SELECT COUNT(DISTINCT ciutat.comarca)
   FROM persona, ciutat
   WHERE persona.ciutat = ciutat.ciutat;
   ```
   | count |
   |:------|
   | 9     |

12. Quina és la primera ciutat d'aquelles en què hi viu alguna persona de la base de dades, per ordre alfabètic?
   
   ```postgresql
   ```

13. Quants habitants hi ha entre totes les ciutats de la base de dades?
   ```postgresql
   SELECT SUM(ciutat.habitants) FROM ciutat;
   ```
   
   | sum      |
   |:---------|
   | 37469270 |

14. Quin és el sou mitjà dels treballadors del club esportiu el mes de desembre del 2014? (Pots fer servir BETWEEN)

    1. Afegir l'esport 'tennis-dobles' i incloure-hi a dos jugadors a la taula fa.

15. Quin guany mensual es té en el club esportiu? (Total d'ingressos de quotes)

16. Quin guany mensual es té en el club esportiu per cada esport?
    1. Mostra ara també, el número de persones que practica cada esport, amb el total de les quotes i el nom de l'esport

17. Quines comarques tenen més de 100000 habitants?
   ```postgresql
   SELECT comarca, SUM(ciutat.habitants) FROM ciutat GROUP BY comarca HAVING comarca IS NOT NULL AND SUM(habitants) > 100000;
   ```

| comarca        | sum     |
|:---------------|:--------|
| Barcelonès     | 1831530 |
| Segrià         | 139809  |
| Baix Llobregat | 175188  |

18. Entre totes les ciutats de la BD, hi ha més d'un milió d'habitants?
   ```postgresql
   SELECT SUM(habitants) > 1000000 AS mes1milio FROM ciutat;
   ```

| mes1milio |
   |:----------|
| true      |

19. Nom de les ciutats ordenades pel nombre d'habitants.
   ```postgresql
   SELECT ciutat, habitants FROM ciutat ORDER BY habitants DESC;
   ```

| ciutat         | habitants |
|:---------------|:----------|
| New York       | 18897109  |
| Rio de Janeiro | 6320446   |
| Berlín         | 3499879   |
| Roma           | 2796102   |
| París          | 2249975   |
| Barcelona      | 1611822   |
| San Francisco  | 805235    |
| Atenes         | 664046    |
| Badalona       | 219708    |
| Lleida         | 139809    |
| Viladecans     | 65444     |
| Castelldefels  | 63077     |
| Esplugues      | 46667     |
| Sitges         | 29140     |
| Amposta        | 21511     |
| Roses          | 19891     |
| Tiana          | 8221      |
| Tona           | 8085      |
| Cadaqués       | 2938      |
| Tredòs         | 154       |
| Montgat        | 11        |

20. Nom de les 5 primeres ciutats ordenades pel nombre d'habitants (fes servir la clàusula LIMIT).

Consulta amb totes les clàusules: COUNT, GROUP BY, HAVING COUNT i ORDER BY.

21. Nom dels esports i la quantitat de socis que els practiquen, per als esports que practiquin més de 3 socis i que es
    juguin en solitari, ordenats per nombre de socis.

-------------------------------------

Fent servir SELECTS per dates i hores:

22. Quant sumen 2+3 i quant multipliquen?

23. Lo mateix que 22) però renombrant la primera columna com 'suma' i la segona com 'producte'

24. Quina hora es?

25. Quants dies has viscut?

----------------------------------------

CONSULTES D'ACTUALITZACIÓ:

26. Inserir un nou esport:'Atletisme', amb un jugador per equip.

-------------------------------- VISTES ---------------------------------------------

Encara que veurem les vistes a la UF3, les introduirem ara una mica, per tal de poder

fer l'exercici 27, ok?

Què es una vista? El suport físic d'una consulta.

Postgresql no permet operacions de UPDATE, INSERT o DELETE amb les vistes, altres SGBD de bases de dades sí, podem
implementar aquesta funcionalitat amb les regles (RULES) dels usuaris que veurem a la UF3.

SINTAXI general:

Creació de la vista:

CREATE VIEW nom_vista AS <fica_la_teva_consulta_SELECT_aqui>

Esborrat de la vista:

DROP VIEW nom_vista

Exemple:

Anem a crear una vista que ens facilitarà el que paga mensualment cada soci:

CREATE VIEW pagaments_soci AS

SELECT p.passaport, p.nom,p.cognom, SUM(f.quota) AS pagament

FROM persona p, fa f

WHERE p.passaport = f.passaport

GROUP BY p.passaport,p.nom,p.cognom

ORDER BY p.cognom;

Si tot ha anat bé, al executar la comand postgresql ens informarà amb 'CREATE VIEW'.

Ara si fem un SELECT * FROM pagaments_soci; haurà de sortir tota l'informació dels socis i quotes agrupades ok.

26.1) Com podem esbrinar si una BD conté vistes?  [Cerca a la documentació de Postgresql]

------------------------------------------- FI VISTES --------------------------------------------------

27) Explica què estàs fent amb aquesta consulta:

INSERT INTO fa ( SELECT passaport,'atletisme',0.0 FROM pagaments_soci

ORDER BY pagaments_soci DESC

LIMIT 5

);

28) Augmentar un 10% la quota del tennis als socis (x1.10)

------- Explicació de SUBCONSULTES!!! VEURE TRANSPARENCIES

29) Nomenar nova cap d'entrenadors a Helena Pérez

30) Esborreu les comarques que comencin amb la lletra O

31) Donar de baixa la ciutat d'en Klauss Stallman.

31.1) Nom, cognom i passaport de les persones que viuen a les 5 ciutats més poblades (amb més nombre d'habitants).

--------------------- Operacions amb conjunts -------------------------

Unions, diferències i interseccions de conjunts. Potser és un bon

moment per recuperar els apunts de fa un mes ;)

Sintaxi UNION:

SELECT A1,A2 ...

FROM r

WHERE p

UNION

SELECT B1,B2..

FROM s

WHERE q;

Sent els esquems A1, A2... i B1,B2.. COMPATIBLES

Anem a practicar el UNION!

32) Quins ingressos o despeses (soci - treballador) ha suposat pel club cada persona el novembre del 2014?

Exemple de sortida:

passaport |nom | cognom | pagaments | vincle

----------------------------------------------------------------------------------------

2904093840|Sonia| Aragall|33.75|soci

C928748927|Michael|Bros|-915.35| treballador

...etc

33) Multiconjunts vs conjunts

Anem a veure un exemple pas a pas per tal d'aclarir aquesta part, veurem l'ús de UNION i UNION ALL:

33.1) Selecciona nom i cognom de les persones que viuen a 'Amposta' [Carme Ferrer]

33.2) Selecciona nom i cognom de les persones que viuen a 'Cadaqués' [ Carme Peralta]

33.3) Nom i cognom de les persones que viuen a Amposta i de les persones que viuen a
Cadaqués [Carme Ferrer i Carme Peralta]

33.4) Nom de les persones que viuen a Amposta i de les persones que viuen a Cadaqués [Carme] -->només 1 resultat¿?¿?

33.5) Fent servir UNION ALL, nom de totes les persones d'Amposta i de Cadaqués [Carme, Carme] --> 2 resultats, ok!

------------------------------------

34) Clàusula EXCEPT

EXCEPT = Diferència de relacions, sintaxi igual a UNION però posant EXCEPT. ojo! RETORNEM ELS ELEMENTS DEL PRIMER
OPERAND (DEL PRIMER SELECT, PER ENTENDREN'S).

Exercici: Obtenir els noms i cognoms dels treballadors que no són caps de ningú, ordenats per cognom.

35) Clàusula INTERSECT: Sintaxi igual a UNION i a EXCEPT. Resultat: Registres de la primera consulta que també hi són a
    la segona!

Exercici: Quins socis practiquen futbol i bàsquet?

Aclariment: OJO! No estem preguntant quins socis practiquen futbol i quins bàsquet, el que demanem

és qui practica fútbol + bàsquet, els dos alhora, ok?

------------------------------------------------------------------------

Operadors de REUNIÓ (JOINS): NATURAL JOIN, NATURAL LEFT OUTER JOIN, NATURAL RIGHT OUTER JOIN, NATURAL FULL OUTER JOIN

JOIN USING, INNER JOIN ON

El Natural JOIN és la reunió interna (ara es veu l'utilitat que es diguin igual les claus primàries i les foranes entre
les taules)

SELECT X1,X2,....,Xn

FROM r NATURAL JOIN s

WHERE p;

I la resta de sintaxis són semblants (consulta la documentació de Postgres per + info)

Exercici: Resol les següents consultes fent servir JOINS del tipus anteriors:

36.1) Donar totes les dades de les persones amb les ciutats on viuen.

[ciutat][passaport][nom][cognom][habitants][comarca]

36.2) Donar les dades de totes les persones i, si se sap, de les ciutats on viuen.

[Fixa't en la gent que no té ciutat assignada, haurien de sortit-te igualment]

36.3) Donar les dades de totes les ciutats i les persones que hi viuen si se sap

36.4) Obtenir totes les dades de les persones i les ciutats en una sola relació.

36.5) Què obté aquesta consulta?

SELECT * FROM persona JOIN ciutat USING(ciutat);

36.6) Què obté aquesta consulta?

SELECT * FROM persona JOIN ciutat ON persona.ciutat = ciutat.ciutat;

Fi de festa, treball amb valors nuls:

Reflexionem:

L'expressió 'A= NULL' SEMPRE dona fals! Independentment si A té un valor o no...

En canvi, A IS NULL sí que retorna el que desitgem tenint en compte els valors de A.

(Recorda que pots fer servir també el NOT, quedant l'expressió IS NOT NULL, depenent del

que t'interessi)

Fes una prova:

37) Anem a fer una consulta INCORRECTA sobre les persones que no se sap on viuen:

SELECT * FROM persona WHERE ciutat = NULL;

--> 0 rows... segur?

37.1 ) Transforma la consulta anterior correctament.

37.2) Si hem de fer servir valors no booleans, tenim el predicat IS UNKOWN. el seu ús és molt particular ....

Respón: Què fa aquesta consulta?

SELECT * FROM persona WHERE ciutat='Mart' IS UNKNOWN;

Si et ve de gust, fes un update per ficar a Sònia Aragalll a la ciutat 'Mart' i repeteix la consulta, què passa ara ?

--------------------------------------------------------------------------------

Clàusula IN:

Retorna cert si un valor pertany a la relació, la seva sintaxi general es:

SELECT A1,A2,..., Ak

FROM R

WHERE A IN (

SELECT B FROM S....

);

Fer una intersecció és relativament senzill fent servir IN, així resol aquesta qüestió:

38) Passaport, nom i cognom dels socis que practiquin fútbol i bàsquet.

Et sona veritat? És la mateixa consulta 35 del INTERSECT, però ara la farem no pensant en

conjunts, sino en files i columnes de taules... quina forma t'és més còmode? Totes dues

són vàlides!

39) Continuem amb IN... (difícil!).

Mostra el passaport , nom i cognoms de les persones conegudes pels treballadors de l'Alt Empordà amb sous superiors a
1000

Pista: Aquesta consulta te un IN i dins un altre IN, pensa que travessem gairebé completament la BD, taules: coneix,
persona, nomines, ciutat....)

-----------------------------------------------------------------------------------

Bé, ara ja ens podem considerar els/les cracks de SQL, les següents consultes

no estàn 'guiades' en el sentit que podeu fer servir el que vulgueu, sempre que el resultat

sigui correcte. Si teniu curiositat, podeu fer servir la comanda de psql \timing on per mesurar

el temps de la vostre consulta. En general, qualsevol consulta superior a mig segon (abans de ser a la memòria cau (
caché), sería moooolt dolent).

Sort!

40) Obtenir el cognom dels socis, amb el que paguen per cada esport.

41) Passaport, nom i cognom dels socis que practiquen futbol

42) Nom dels socis amb alguna quota més alta que el sou d'algún treballador/a

43) Nom de les ciutats on no hi ha cap persona (Mirar clàusula 'EXISTS' a veure què fa)

44) Donar el nom dels esports que practiquin socis de totes les ciutats de la comarca del Barcelonès.

------------------------------

Consultas de repaso antes del examen, enjoy!

- Cuántos socios tienen un email de gmail.com?

- Prepara una VISTA con las ciudades de personas de la BD y cuantas viven en cada ciudad ordenadas por nombre de ciudad.

- Comarca de los socios que practiquen tennis. [Baix Llobregat, Barcelonès i Val d'Aran]

- En las ciudades que vivan 3 personas, sumar 1000 habitantes.

EXTRA EXTRA!!!!

--------------------------

- Para los deportes que los practiquen mas de 2 personas, sube la cuota un 10%

- Deportes que no son practicados por ningún socio

- Sueldo medio de los trabajadores de la sección de 'comercial'
