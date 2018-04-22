---

---

# Sistemes Operatius - Primer Parcial

Un sistema operatiu és un programa que gestiona el hardware d'un ordinador. També proveeix una base per als programes com a intermediari entre l'usuari i el hardware. Els sistemes operatius estàn dissenyats per a ser _adequats_ a algunes tasques particulars, altres per a ser _eficients_ i altres una combinació de les dues.

### Estructura dels Sistemes Operatius

El **sistema operatiu** bàsicament proveeix un entorn on els programes poden ser executats. Tot i les petites diferències que pot haver-hi entre sistemes operatius, tots tenen uns trets bàsics comuns, els quals tractarem.

Un dels aspectes més importants dels sistemes operatius és la capacitat de treballar amb diferents programes a la vegada, coneguda com **multiprograma**. D'aquesta manera s'incrementa l'ús de la CPU amb el fet de gestionar l'organització de les tasques de manera que la CPU sempre tingui alguna cosa que executar.

La idea funciona de la següent manera: El sistema operatiu manté diferents tasques en memòria siultàniament. Com que, en general, la memòria és massa petita per a emmagatzemar totes les tasques, aquestes són guardades inicialment al disc en l'anomenada _**job pool**_. Esquema de la memòria:

| Sistema operatiu |
| :--------------: |
|      tasca1      |
|      tasca2      |
|      tasca3      |
|      tasca4      |

El conjunt de les tasques en memòria pot ser un subconjunt de les tasques de la _job pool_. El sistema operatiu escull i comença a executar una de les tasques de memòria. Eventualment, una tasca pot haver d'esperar alguna acció, com una operació d'Entrada/Sortida (E/S), per a completar-se. En un sistema no-multiprograma, la CPU es quedaria esperant aquesta acció; en canvi, en un sistema multiprograma, el SO simplement canvia a una **nova tasca**, i així de manera successiva. Quan la primera tasca aconsegueix acabar l'espera, la CPU torna a "donar-li atenció". Sempre que hi hagi alguna tasca per executar, la CPU no para mai.

Els sistemes multiprograma proveeixen un entorn on els diferents recursos del sistema són utilitzats de manera efectiva. El **_time sharing_** també conegut com a _multitasking_ és una extensió llògica del multiprograma. Els sistemes _multitask_ o de **temps compartit**, la CPU executa multiples tasques canviant entre elles, de manera tant ràpida que l'usuari ho pot utilitzar tot a la vegada.

El temps compartit necessita un sistema **interactiu**, en el qual l'usuari dóna instruccións al sistema operatiu o un programa directament, utilitzant dispositius d'entrada com pot ser un teclat, un ratolí... I espera els resultats immediats al dispositiu de sortida, demanera que el **temps de resposta** hauria de ser curt. Cada _usuari_ (usuaris, programes) té la sensació que TOTA la CPU està sent utilitzada per ell, però en realitat és compartida amb molts _usuaris_.

Un programa carregat a memòria el qual s'està executant és anomenat un **procés**. Quan s'executa un procés, és durant un curt període de tempe normalment, ja que o bé ha acabat o necessita esperar a una acció E/S, la qual pot ser _interactiva_ com una sortida a una pantalla, una entrada des de teclat; les quals van a la "velocitat de les persones", ja que han d'esperar a l'usuari humà. Com que aquesta velocitat és molt lenta per un ordinador, i deixar la CPU en _standby_ seria una pèrdua d'eficiència, el sistema canviarà ràpidament a una altra tasca d'un altre _usuari_.

Com que fer _multitasking_ en un sistema multiprograma significa carregar diferents tasques a memòria simultàniament, si múltiples tasques estàn preparades per a ser portades a memòria però no hi ha prou espai per totes, lavors el sistema ha d'escollir entre elles; fer aquesta decisió implica la **distribució de tasques**. Al tenir la possibilitat de tenir diferents programes en la mateixa memòria requereix una forma de **gestió de memòria**; i amés si diferents tasques estàn preparades per a ser portades a terme, el sistema ha d'escollir quina anirà primer, aquesta gestió l'anomenem **distribució de la CPU**. Per últim, el fet de executar diferents programes concurrent-ment, requereix que no es "molestin" entre ells, en tots els aspectes (gestió de memòria, de CPU, etc).

En un sistema amb _multitasking_ ha d'assegurar un temps de resposta ràpid. Això s'aconsegueix gràcies a un procés d'**intercanvi**, on els processos són intercanviats dins i fora de la memòria principal al disc. Un mètode més comú és la **memòria virtual**, una tècnica que permet l'execució d'un procés que no està complet en memòria; això permet a l'usuari executar programes més grans que la pròpia **memòria física**.

### 2. Les operacións del Sistema Operatiu.

------

Els sistemes operatius moderns es basen en les **interrupcións**. Si no hi ha processos per a executar, ni accións E/S per atendre, i no hi ha usuaris als quals respondre, un sistema operatiu es quedarà esperant algun event. Els events venen normalment senyalats per alguna interrupció o **trap**. Una **trap** (o excepció) és una interrupció generada pel software causada o bé per un error (eg. una divisió per zero o un accés de memòria no vàlid) o per una sol·licitud específica d'un programa d'usuari. Per a cada tipus d'interrupció, diferents segments de codi en el SO determinen què s'hauria de fer, són les anomenades _rutines de servei d'interrupcións_.

Com que els usuaris i el sistema operatiu comparteixen el hardware i el software de l'ordinador, hem d'assegurar que un error en el programa només pugui causar problemes al programa en particular, ja que compartint un sol _bug_ en un programa podria causar problemes a molts processos . O fins i tot errors minoritaris com cambiar dades utilitzades per altres programes o el propi sistema operatiu.

Sense protecció cap a aquests tipus d'errors tenim dues opcións: O només executem un sol procés a la vegada, o totes les sortides són dubtoses de ser correctes. Un bon sistema operatiu ha d'assegurar que un programa incorrecte o maliciós no pugui causar problemes als altres programes.

#### 2.1 Operacións de mode dual i multimode

Per tal d'assegurar la correcte execució del sistema operatiu, hem de distingir entre les execucións del codi del propi sistema operatiu i les del codi de l'usuari. Per això trobem dos modes separats d'execució: **mode usuari** i **mode privilegiat** (també sistema, kernel, root...). Un bit anomenat **_mode bit_** s'afegeix al hardware per indicar el mode actual _kernel_ (0), _usuari_ (1). Amb el bit de mode podem distingir entre tasques executades a petició del sistema operatiu o a petició de l'usuari. Quan s'està executant codi d'una aplicació d'usuari, el sistema està en _mode usuari_. De tota manera, quan una aplicació d'usuari o un usuari demana serveis del sistema operatiu (a través de _crides al sistema_), el sistema ha de canviar de mode usuari a mode kernel per a completar la petició.

Quan es provoca una interrupció el sistema canvia a mode kernel, sempre que el sistema operatiu guanya control del sistema, està en mode kernel i sempre torna a mode usuari abans de donar el control a una aplicació d'usuari de nou.

Aquesta tècnica ens permet assegurar el sistema operatiu d'executar operacións perilloses. Aquestes operacións perilloses estàn designades com **instruccións privilegiades**. Només són permeses d'executar en mode kernel, si s'intenta fer en mode usuari el hardware no farà l'instrucció sinó donarà un _trap_ al sistema operatiu. La instrucció per a canviar de mode, és una instrucció privilegiada per exemple.



## CONCEPTES

### Que és un procés?

Un procés és un programa en execució. Un programa és un algoritme escrit en un llenguatge de programació.

Es pot afirmar que cada procés te la seva CPU virtual en el sentit que sempre es té una còpia d'ella. A aquesta CPU virtual, se li diu **Bloc de Control de Procés** (PCB), el qual es una estructura de dades que conté informació referent al procés, informació que necessita el Sistema Operatiu per a poder planificar els processos (assignar CPU i desallotjar-los [veure Round Robin]), això significa que cada procés té el seu propi PCB.

- Cada vegada que un procés entra en estat d'execució (RUNNING) aquesta còpia s'utilitzarpa per a carregar la CPU amb els valors que permeten continuar aquest procés exactament des d'on va acabar l'anterior execució.
- Cada vegada que un procés sigui parat per qualsevol cosa, s'han d'agafar els valors de la CPU per a actualitzar-los en el PCB.

Algunes informacións del PCB són:

- **Estat del procés**: Ready, Running o Blocked.
- **PC (Contador de Programa)**: Conté la direcció de la següent instrucció a executar pel procés.
- **Informació de planificació**: Aquesta informació conté: prioritat del procés, apuntadors a cues de planificació...
- **Informació del sistema d'arxius**: Proteccións, identificacións d'usuari, grup...
- **Informació de l'estat d'E/S**: Conté solicituds pendents d'E/S, dispositius d'E/S assignats al procés, etc...

Per a fer ús del PCB el Sistema Operatiu utilitza una Taula de Processos, de manera que cada entrada en aquesta taula fa referència a un PCB.

### Algoritme Round Robin

És un algoritme de gestió i repartiment equitatiu i senzill de la CPU entre els processos que evita la monopolitzaició de la CPU molt utilitzat en entorns de **temps compartit** o **multitasking**.

Consisteix en definir una unitat de temps petita, anomenada **quantum** la qual s'assigna a cada process per a que estigui en mode **READY**. Si el procés esgota el seu quantum (Q) de temps, s'escull un nou procés per a ocupar la CPU. Si el procés es bloqueja o acaba abans d'esgotar el seu quantum també s'alterna l'ús de la CPU.

És per això que surgeix la necessitat d'un rellotge de sistema; un dispositiu que genera periòdicament interrupcións. El quantum d'un procés equival a un nombre fixe de cicles de rellotge. Al haver-hi una interrupció de rellotge, o (és el mateix) la fi d'un quantum es cedeix el control de la CPU al procés seleccionat per el planificador.

Si el **quantum** és molt gran, els processos acabaràn els seus temps de CPU abans d'acabar el quantum.
Si és molt petit, hi haurà molts canvis de processos i la CPU perdrà rendiment.

| Processos | Temps d'execució |
| :-------: | :--------------: |
|    P1     |        53        |
|    P2     |        17        |
|    P3     |        68        |
|    P4     |        24        |

Anem a fer un exemple amb la taula que veiem a sobre:

Per aquest exemple imaginarem 4 processos, amb els seus respectius temps d'execució i un Quantum (Q) de 20.

Hem de tenir en compte que quan hi ha un _canvi de context_ (canvi de procés a la CPU) s'ha de guardar l'estat del procés que s'estava executant al PCB i llavros cargar a la CPU l'estat del PCB del nou procés.

![statediagram](C:\Users\Alvaro\Desktop\so\img\statediagram.jpg)

Imaginem que **P1** entra a la CPU amb un temps de 53, es carrega el seu PCB i el rellotge del sistema comença  contar, quan arriba a 20 es llança una interrupció de rellotge. Es treu **P1** de la CPU, s'actualitza el seu PCB i el procés torna a la cua de READY. Ara entrarà **P2** a la CPU, que era el següent en la llista de ready, un cop passat el seu temps d'execució ja que TE(P2) < Q acabarà el procés, **P2** haurà acabat i farà exit, per tant anirà a estat ZOMBI o TERMINATED. Ara entrarà el procés **P3** que necessita un imput per a continuar, un cop passats 10q demana l'entrada, s'actualitzarà el seu PCB i passa a la cua de processos BLOCKED o WAITING, i llavors entra **P4** a la CPU i passa el mateix que amb **P1**. Es va seguint l'algoritme fins que tots els processos han acabat.

## COMANDOS



### <u>SIGNALS</u>

> Els signals poden:
>
> - Ignorar un event (Tractament designat com a IGNORE, o tractament per defecte és IGNORE).
> - Realitzar accións per defecte.
> - Realitzar accións definides amb _sigaction_.
>
> Quan el sistema rep un signal, aquest passa a executar un tractament per aquest. Si el tractament és una funció dissenyada (no tractament per defecte), ha de rebre com a paràmetre el número de signal (ja que una mateixa funció podria tractar diferents signals).



#### sigaction

> Reprograma l'acció associada a un signal concret, és a dir, permet canviar el tractament d'un signal.

_int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact)_;

- _signum_: especifica el signal, ha de ser un vàlid. (sigkill i sigstop no són vàlids).
- _act_: Si és diferent de NULL, la nova acció per el signal _signum_ és instalada des de act.
- _oldact_: Si és diferent de NULL, la acciò prèvia a la nova es guarda a _oldact_.

------

#### kill

> Envia un signal a un procés.

_kill [opcións] \<pid> [...]_

El signal per defecte de kill és TERM. Altres sinals poden ser especifcats amb -\<signal>; per exemple 
-SIGKILL, -9 o -KILL.

------

#### sigsuspend

> bloqueja el procés que l'executa fins que rebi un signal (amb tractament diferent de IGNORE).

_int sigsuspend(const sigset_t *mask);_

Sigsuspend reemplaça temporalment la mascara de signal del procés que fa la crida amb la màscara del paràmetre _mask_, llavors suspen el procés fins que rebi un signal amb l'acció de invocar una funció de tractament de signal (_signal handler_) o d'acabar el procés.

Quan arriba un signal, si el pid és positiu, el signal sig s'envia al procés amb l'ID especificat per pid.

Si pid és 0, llavors signal sig s'envia a tots els processosdel grup del procés que ha fet la crida.
Si pid és -1, sig s'envia a tots els processos pels quals el procés que ha fet sigsuspend té permís per a enviar signals, excepte el procés 1 (init).
Si pid és < -1 llavors sig s'envia a tots els processos del grup de processos els quals el seu pid és -pid.

**Valors de retorn**

- Si el signal acaba -> No hi ha retorn.
- Si s'ha fet restore de la signal mask a l'estat d'abans de la crida -> Sisgsuspend sempre retorna -1 (amb errno).

------

#### sigprocmask

> Permet modificar la màscara de signals bloquejats del procés.

_int sigprocmask(int how, const sigset_t *set, sigset_t *oldset)_;

La *màscara de signals* és el conjunt de signals els quals estàn bloquejats actualment per qui els crida.

el valor de _how_ pot ser: 

- **SIG_BLOCK** -> Els signals bloquejats són la unió dels actualment bloquejats i els del paràmetre _set_.
- **SIG_UNBLOCK** -> Els signals de _set_ es borren dels signals bloquejats actualment.
- **SIG_SETMASK** -> Els signals bloquejats i permesos passen a ser els de _set_.

Si _oldset_ no és null, s'hi guardarà el valor previ de la mascara de signals. Si és NULL, la signal mask no canviarà (_how_ serà ignorat), però el valor de la mascara de signals es retorna a _oldset_ (si aquest no és NULL).

**valor de retorn**
**0** si ha estat un èxit, **-1** si ha estat un error. Si hi ha un error, _errno_ indicarà la causa d'aquest error.
**!**No es poden bloquejar SIGKILL o SIGSTOP. Cada thread té la seva pròpia màscara de signals.

**exemple del que normalment es fa a l'assignatura**:
mask = vector de signals activats i desactivats.
int sigprocmask(SIG_BLOCK, mask, NULL);

------

#### alarm

> Programa l'enviament d'un SIGALARM després d'x segons.

_unsigned int alarm(unsigned int x)_;

Si _x_ és zero, totes les alarmes pendents són cancelades, en tots els events.
alarm() retorna el nombre de segons restants abans que qualsevol alarma anterior està prevista de disparar-se. Si no hi havia alarmes anteriors, retorna zero.

------

#### sleep

> Bloqueja un procés durant el temps que es passa per al paràmetre.

_sleep_ NUMBER[SUFFIX]...
_SUFFIX_: s (seconds), m (minutes), h (hours), d (days)

------

#### kill

Envia un event concret a un procé.

------

#### waitpid

_waitpid(pid_t pid, int *status, int options);_

Aquesta crida a sistema s'utilitza per a esperar a un canvi d'estat d'un procés fill des del procés el qual fa la crida i obtenir la informació del fill el qual l'estat ha canviat. Un _canvi d'estat_ es considera: El fill ha acabat; el fill ha sigut parat per un signal; o el fill ha sigut reprès a través d'un signal. En el cas d'un fill que ha acabat, utilitzar el waitpid permet al sistema alliberar els recursos (PCB) associats amb aquest fill; si no es fa un wait/waitpid, el fill es queda en estat _zombie_.

Executar aquesta comanda suspen l'execució del procés que fa la crida fins que un fill (especificat en el paràmetre _pid_) ha canviat d'estat. El paràmetre _pid_ pot ser:

- **< -1 **  Espera qualsevol procés fill el qual tingui un ID de grup de processos igual que el 				 	valor absolut de _pid_.
- **-1**      Espera qualsevol procés fill.
- **0**       Espera qualsevol procés fill que tingui un ID de grup de processos igual que al del procés que fa la crida _waitpid_.
- **>0**     Espera el fill amb el PID igual a _pid_.

En el paràmetre _options_ podem posar:

- WNOHANG: retorna immediatament si cap fill ha fet _exit_.
- WCONTINUED: retorna si un fill parat a reprès l'execució amb un SIGCONT.
- Alguns més que podem veure al man del bash.

Els valors de retorn del _waitpid_ són:

- <u>Èxit</u>: Retorna el **valor del PID** del fill que ha canviat d'estat. (Exemple: 3 fills en estat zombi; retorna el pid del primer fill en acabar i passar a ZOMBIE i en _status_ el codi de finalització del fill.)
  - Si tenim la opció WNOHANG i existeixen un o més fills que es corresponen amb el paràmetre _pid_ especificat, però no han canviat d'estat, es **retorna 0**.
    - Exemple: Un fill en estat BLOCKED i un altre fill en estat READY.
- <u>Error</u>: **Retorna -1**.

------

