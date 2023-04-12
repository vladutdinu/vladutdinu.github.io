# Limbaje Scriptice


## Ubuntu VM
-   Instalare masina virtuala cu [Ubuntu Desktop](https://ubuntu.com/download/desktop) (22.04/20.04)
-   Configurat masina virtuala dupa preferine (user, password, etc.)
- Tool-uri de instalat:
    - [Docker](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
        -   De rulat comenzile de la <b>Set up the repository</b> pana la <b>Install Docker Engine</b> inclusiv
        -   De rulat `sudo usermod -aG docker $USER` ca sa poti rula docker fara <b>sudo</b>
        -   Relog ca sa se aplice configurarile
        -   De testat comanda `docker ps`
    -  [Docker Compose](https://docs.docker.com/compose/install/other/#on-linux)
        -   De rulat cele 2 comenzi de la punctul 1. si 3.
        -   De testat comanda `docker-compose` / `docker compose`
    - [Sqlite Browser](https://sqlitebrowser.org/dl/)
        -   De downloadat din sectiunea <b>Linux</b>, pachetul AppImage si pus pe desktop
        -   De rulat comanda `chmod +x <fisier>.AppImage` si apoi `./<fisier>.AppImage`
    - [VSCode](https://code.visualstudio.com/docs/setup/linux)
        -   De rulat comenzile pana la sectiunea <b>RHEL, Fedora and CentOS</b>

## Python
-   Masina virtuala vine cu Python3 deja instalat
-   De rulat comanda `pip install -r requirements.txt` pentru a instala toate pachetele de Python necesare
    -   Acestea contin un framework de creare a API-urilor, de conectare la o baza de date si serverul care ruleaza API-ul

-   De downloadat datasetul [Iris](https://gist.github.com/netj/8836201) si procesat astfel incat campul `Variety` sa devina din string in numeric (folosind <b>pandas</b>)
    -   Pasi:
        -   Importare <b>pandas</b> `import pandas as pd`
        -   Citire dataset `dataset=pd.read_csv("path_to_dataset.csv", index_col=False)`
        -   Transformare `dataset['class] = dataset['variety].replace({"Setosa": 0, "Versicolor": 1, <to_be_completed>})`
        -   Salvare dataset `dataset.to_csv("path_to_dataset.csv")`

-   Framework-ul pentru API este numit [FastAPI](https://fastapi.tiangolo.com/) 
-   De urmarit si implementat tutorialul de pe site-ul lor -> [Tutorial](https://fastapi.tiangolo.com/tutorial/first-steps/)
-   De creat [CRUD](https://fastapi.tiangolo.com/tutorial/sql-databases/#crud-utils) Endpoints + [Model](https://fastapi.tiangolo.com/tutorial/extra-models/) + DB [Schema](https://github.com/marshmallow-code/marshmallow-sqlalchemy) pentru flori (sepal_length, sepal_width, petal_length, petal_width, class)
    -   Pentru mai multe detalii e un tutorial [aici](https://fastapi.tiangolo.com/tutorial/sql-databases) in care sunt expuse mai multe notiuni si mai detaliat
    -   Endpoints:
        -   GetAllFlowers
        -   GetFlowerById
        -   GetAllFlowersByClass
        -   CreateFlower
        -   DeleteFlower(by id)
        -   UpdateFlower(by id)

-   De modificat fisierul `./db/init_db.py` pentru a incarca datele in baza de date (folosind functiile de CRUD)
    -   <b>Tip:</b> Pentru a genera baza de date trebuie rulat scriptul `init_db.py`
    -   Optional de verificat cu <b>Sqlite Browser</b> daca s-au incarcat datele
    -   Optional de incarcat date si cu request-uri

-   De testat endpoint-urile folosind [Swagger](http://localhost:8000/docs)

## Docker
-   De creat imagine Docker
    -   Se creaza fisierul `Dockerfile`
        -   Trebuie sa contina versiunea de Python folosita pe masina virtuala
        -   Trebuie sa contina fisierul de requirements
        -   Trebuie sa expui portul pe care ruleaza 
        -   Trebuie rulata comanda care porneste serverul de API cu 2 parametrii `host` si `port` astfel incat sa fie accesibil dinafara containerului
    -   Se [construieste](https://docs.docker.com/engine/reference/commandline/build/) imaginea cu `docker build` (de vazut care sunt ceilalti parametrii)
-   Se [ruleaza](https://docs.docker.com/engine/reference/commandline/run/) containerul cu `docker run` (de vazut care sunt ceilalti parametrii pentru nume, port, sa porneasca in background, etc.)
-   Se verifica starea containerului cu `docker ps`
-   Se verifica log-urile containerului cu `docker logs <nume_container>`
-   Se verifica daca se poate accesa [Swagger](http://localhost:8000/docs)
-   De identificat problemele existente si cum le putem rezolva

## Docker Compose
-   De creat [fisierul](https://github.com/vladutdinu/AstroSwipe/blob/main/dev-ops/docker-compose.yaml) `docker-compose.yaml`
    -   Se creaza configuratia pentru containerul de API
    -   Se creaza network
    -   Se creaza volum local
    -   Se expuna portul
    -   Se rezolva problemele legate de rularea simpla a containerului (de discutat)
-   Se porneste configuratia din `docker-compose` cu `docker-compose up` (de gasit si flag-uri care pot fi folosite)


## Machine Learning
### Descriere

-   In fisierul de requirements exista un pachet de pip PyOD care are mai multe implementari de algoritmi si modele de ML. In cazul de fata o sa folosim [KNN](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.knn.KNN) (putem evalua si incerca si altele) pentru a antrena un model, a salva si a face predictii.
-   Pe VSCode o sa instalam cateva extensii pentru Python, Prettify si Jupyter Notebook ca sa te familiarizezi cu functionalitatea bibliotecii PyOD
-   Antrenam modele, testam, facem predictii si integram in API (facem endpoint-uri de predictie, de antrenare, etc.)

### Implementare
#### ML
-   Facem un fisier `train.ipynb`, importam tot ce e necesar pentru a lucra cu KNN din PyOD
-   Citim datasetul Iris prelucrat anterior 
-   Antrenam modelul
-   Salvam modelul
-   Incarcam modelul
-   Facem predictio in functie de anumite valori date de noi si cu modelul incarcat
-   Evaluam pasii facuti
#### Integrare API
-   Creare model pentru datele care vor veni pentru predictie
-   Creare endpoint pentru predictie
-   Reconstruirea imaginii de docker
-   Testarea containerului si a functionalitatilor adaugate

## MQTT
#### Descriere
-   [MQTT](https://mqtt.org/) e un protocol de transmisie de date folosit mult in IoT
-   Va trebui creat un `client` si un `server` care vor transmite si vor procesa date (predictie), vor fi 2 [fisiere](https://pypi.org/project/paho-mqtt/) de python separate
-   Vor trebui construite containere docker si integrat fisierul de server in `docker-compose` pentru a putea fi folosit 
-   Va trebui integrat si un client [MQTT](https://hub.docker.com/_/eclipse-mosquitto) in `docker-compose` ca sa poate comunica modulele intre ele
-   Va trebui testat serverul si comunicarea cu clientul

#### Implementare
-   Se fac 2 fisiere `client.py` si `server.py` folosind exemplele de pe pagina pachetului de [MQTT](https://pypi.org/project/paho-mqtt/) pentru python, dar si de inteles implementarea abordarii din exemple
-   Se configureaza adecvat problemei, serverul de mqtt se lasa asa cum e
-   Se testeaza comunicarea pe un topic cu un mesaj oarecare (sa se afiseze in consola ceea ce trimitem pe topicul `/knn/prediction/`)
-   Se rescriu fisierele astfel incat:
    -   `client.py` sa transmita date pentru server pentru a putea fi interpretate corect
    -   `server.py` sa primeasca datele cum trebuie, sa le decodeze daca este necesar si sa aplice predictie pe acestea si sa returneze clasa in care se incadreaza datele despre floarea trimisa
-   Dupa ce tot ceea ce am testat si integrat functioenaza, se construieste o imagine Docker pentru `server.py`
-   Se integreaza cele 2 containere de MQTT si de Server in `docker-compose`, se configureaza adecvat si se pornesc containerele
-   Se testeaza functionalitatile adaugate


## CI/CD
#### Descriere
-   Continuous Integration/Continuous Development, pe scurt [CI/CD](https://www.redhat.com/en/topics/devops/what-is-ci-cd), este o modalitate de a integra ori de cate ori se fac update-uri sau se adauga features in plus unei aplicatii intr-un mod automatizat
-   Gitlab are un astfel de mecanism folosindu-se de [Gitlab Runner](https://docs.gitlab.com/runner/)
-   O sa fie nevoie de un repository pe `code.siemens.com`, instalat Gitlab Runner pe VM si atasat repository-ului si de facut fisierul de configurare pe care Gitlab Runner sa il citeasca si sa il execute

#### Implementare
-   Creare repo pe `code.siemens.com`, stabilirea conexiunii pe masina virtuala
-   Instalare [Gitlab Runner](https://docs.gitlab.com/runner/install/linux-repository.html) si  configurare
-   Creare fisier `.gitlab-ci.yaml` ([model](https://gitlab.com/VladutDinu/fiiincentru-monitorizare_consum_energie/-/blob/pipeline/.gitlab-ci.yml))
    -   Creare stage-uri de Build si Deploy
    -   Creare task-uri de Build si Deploy
    -   Adaugare permisiuni pentru Gitlab Runner pentru a putea rula comenzi de Docker
    -   Commit&Push pe `code.siemens.com`
-   Testarea si vizualizarea functionalitatii de Gitlab Runner si a procesului de CI/CD