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

------

### Procés fill

És un procés creat a partir d'un fork(); des d'un altre procés. Aquest es crea en total semblança al procés pare. Quan un procés crea un altre, l'esquema de processos s'organitza en forma d'arbre. Cada procés té el seu PID propi, amés del PPID (PID del procés pare), els quals **són únics per a cada procés**.

Aquest procés fill hereda les següents característiques del pare:

- Codi, dades i pila
- Programació dels signals
- ID d'usuari i ID de grup
- Variables d'entorn
- Màscara de signals (sigmask)
- PC del pare (continuarà al mateix lloc que el pare).

I no herederà:

- Contadors interns
- Alarmes pendents
- Signals pendents

------

### Thread (Fils d'execució)

Com hem dit, un procés és la unitat d'assignaicó de recursos d'un programa en execució. Entre aquests recursos trobem els **fils d'execució** (threads) d'un procés. Un **thread** és un mecanisme que permet a una aplicacio realitzar varies tasques a la vegada de manera concurrent. És la mateixa filosofia que utilitza el SO per a executar diferents processos a la vegada enfocat a executar subprocessos dins un mateix procés; però és una mica diferent ja que per definició **els processos no comparteixen espai de memòria entre ells**, en canvi els threads si.

Imaginem tres processos, **A, B i C** executant-se a la mateixa vegada. Què passaria si el procés A, necessités mostrar una interfaç gràfica i a la vegada estar escribint un arxiu? Sense els threads això no seria possible. La idea del thread és permetre que el procés pugui executar una o més tasques a la vegada (o al menys que així ho vegi l'usuari ;) ), de tal manera que cada vegada que a un procés li correspon un **quantum** d'execució del sistema, aquest alterni entre una de les seves tasques o una altre.

Això ens dona la necessitat de reestructurar el concepte de procés, ja que un procés ara no és **la unitat mínima d'execució**, ara un procés és un conjunt de threads. Quan un procés, aparentment, no utilitza threads; està executant **un únic thread**.

Hem de tenir en compte dues coses:

- La memòria de treball del procés, segueix siguent assignada **per procés**, però els threads dins el procés comparteixen tots la mateixa regió de memòria, el mateix espai de direccións.
- El SO assigna Quantums a cada thread creat, i ja no calendaritza processos sinó cada un dels threads en execució al sistema.
- Cada thread té el seu propi context (estat d'execució), així que cada vegada que es suspen un thread per a permetre l'execució d'un altre, el seu contexte es guarda i es reestableix novament només quan és el seu torn [veure Round Robin més avall].

El procés segueix sent una part important ja que és qui se li assigna la prioritat d'execució, la memòria i els recursos, privilegis i altres dades importants. El thread és només qui s'executa.

Imaginem un sistema amb **2 CPU** i una aplicació que executa més de dos threads:
El què passarà és que només dos threads s'estaràn executant en paralel en un moment donat, i el sistema operatiu alternarà l'execució d'aquests threads de tal manera que tots tinguin Quantums assingats, però només hi haurà **2 threads en paralel**.

------

### Estats d'un procés i Propietats

------

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

![statediagram](img/statediagram.jpg)

Imaginem que **P1** entra a la CPU amb un temps de 53, es carrega el seu PCB i el rellotge del sistema comença  contar, quan arriba a 20 es llança una interrupció de rellotge. Es treu **P1** de la CPU, s'actualitza el seu PCB i el procés torna a la cua de READY. Ara entrarà **P2** a la CPU, que era el següent en la llista de ready, un cop passat el seu temps d'execució ja que TE(P2) < Q acabarà el procés, **P2** haurà acabat i farà exit, per tant anirà a estat ZOMBI o TERMINATED. Ara entrarà el procés **P3** que necessita un imput per a continuar, un cop passats 10q demana l'entrada, s'actualitzarà el seu PCB i passa a la cua de processos BLOCKED o WAITING, i llavors entra **P4** a la CPU i passa el mateix que amb **P1**. Es va seguint l'algoritme fins que tots els processos han acabat.

------

### Fi d'execució d'un procés (fill)

> _void exit(int status);_
> Causa un acabament normal del procés i es retorna el valor status al procés pare.

Un procés pot acabar la seva execució de manera **voluntària** (exit) o **involuntària** (signals).

Quan un procés vol acabar la seva execució (voluntàriament), alliberar els seus recursos i alliberar les estructures de kernel que té reservades per ell, s'executa la crida de sistema **exit**.

Si volem sincronitzar el pare amb la finalització del seu/seus fill/s, podem utilitzar la crida a sistema **waitpid**  (veure apartat _COMANDOS -> waitpid_).

El fill pot enviar informació de finalització (_exit code_) al pare a través de la crida al sistema **exit** i el pare la recull a través del _waitpid_.

- El SO fa d'intermediari, guarda aquesta informació fins que el pare la consulta.
- Mentre el pare no consulta aquesta informació, el PCB del fill no serà alliberat i el procés es queda en estat ZOMBIE.
  - Convé fer waitpid dels processos que creem per tal d'alliberar els recursos ocupats del kernel.

> Si un procés mor sense alliberar els PCBs dels fills, el procés init() del sistema ho farà.

------

### Signals

Podem interpretar un signal com una notificació de que ha passat un event, i cada event té un signal associat.

Cada procés té un tractament associat a cada signal el qual pot ser modificat [veure _sigaction_ a comandos].

Per exemple el signal **SIGCONT** fa que un procés continuï si aquest estava parat; **SIGSTOP** fa parar el procés el qual el rep; **SIGCHLD** té un tractament predefinit de _IGNORE_ i notifica que el procés fill ha mort o ha estat parat; **SIGSEGV** indica una referència invàlida a memòria, coneguda també com Segmentation Fault; **SIGALRM** indica que el temps d'alarma ha acabat [veure _alarm_ a comandos] i el tractament és acabar el procés; **SIGKILL** fa acabar el procés també; **SIGINT** és un signal que té un tractament predefinit d'acabar i és donat a través de la combinació de tecles Ctrl+C en el shell.

------

### Màscares

Són estructures de dades que permeten designar quins signals pot rebre un procés en un moment determinat de l'execució.

## COMANDOS

#### fork();

> Crea un procés fill

El pare i el fill s'executaràn concurrent-ment ("a la vegada"), es duplicarà el codi del pare juntament amb la pila i les seves dades i s'assignaràn al fill.

**El fill inicia la execució en el punt on estava el pare en el moment de la creació**, per tant el PC del fill és el mateix del pare.

[Per saber més sobre processos fills veure apartat **Processos fills** a **CONCEPTES BÀSICS**]

**Mutació**
_int execlp(const char *file, const char *arg, ...);_

Al fer un fork, l'espai de direccións és el mateix. Si volem executar un altre codi, aquest procés fill ha de **mutar** (canviar el binari del procés).

_execlp_: Fa canviar (mutar) l'executable d'un procés per un altre executable (però el procés segueix sent el mateix):

- **Tot el contingut** de l'espai de direccións canvia, codis, dades, pila...
  - Es reinicia el PC a la primera instrucció (main).
- Es manté tot el que està relacionat amb **l'identitat del procés**.
  - Contadors d'ús intern, signals pendents, etc...
- Es modifiquen aspectes relacionats amb l'executable o l'espai de direccións:
  - Es defineix per defecte la taula de signals (mask).

**Valors de retorn**:

- Si el fork ha tingut èxit, es retorna el PID del fill al pare.
- Si el fork ha tingut èxit, es retorna 0 en el fill.
- Si el fork ha fallat es retorna -1 al pare, i el fill no serà creat; _errno_ contindrà informació de l'error.

------

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

L'struct _sigaction_ conté les variables següents:

- **sa_handler**:
  - *SIG_IGN* - Ignorarà el signal.
  - *SIG_DFL* - Farà el tractament per defecte.
  - *foo_whatever* - Funció que haurem creat per a tractar-lo.
- **sa_mask**
  - Buida, ja que només s'afegirà el signal que estem capturant.
  - Es restaurarà l'anterior al acabar.
- **sa_flags**
  - **0** -> Configuració per defecte
  - **SA_RESETHAND** -> després de tractar el signal es restaurarà el tractament per defecte.
  - **SA_RESTART** -> Si estàs fent una crida al sistema i reps un signal, es reiniciarà la crida.

```c
void foo(int x){
    //codi que executarà la funció.
}

struct sigaction s;
/* Inicialitzem l'estructura de dades s amb:
	handler -> Ha de contenir el nom de la funció que tractarà el signal.
	mask -> una màscara que crearem
	flags -> SA_RESTART
*/

sigaction(SIGUSR1, &sa, NULL);
```



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

Aquesta crida a sistema s'utilitza per a esperar a un canvi d'estat d'un procés fill des del procés el qual fa la crida i obtenir la informació del fill el qual l'estat ha canviat. Un _canvi d'estat_ es considera: El fill ha acabat; el fill ha sigut parat per un signal; o el fill ha sigut reprès a través d'un signal. En el cas d'un fill que ha acabat, utilitzar el waitpid permet al sistema alliberar els recursos (PCB) associats amb aquest fill; si no es fa un wait/waitpid, el fill es queda en estat _zombie_ i no s'alliberarà l'espai del PCB del fill mort (per això es diu _zombie_).

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

EXEMPLES MÉS COMUNS:

- waitpid(-1, NULL, 0) --> Esperar (amb bloqueig si és necessari) a un fill qualsevol.
- waitpid(pid_fill, NULL, 0) --> Esperar (amb bloqueig si és necessari) a un fill amb pid = pid_fill.

# SISTEMES OPERATIUS - PARCIAL 2

## Accessos a Disc

### Inode

Un inode conté tota l'infomació bàsica d'un fitxer (tamany, propietari, permisos, etc.). Per simplificar-ho, a SO només utilitzem els camps següents:

- Nombre de Referències
- Tipus (d, l, -) [D = directori, l = softlink, - = "normal"]
- Llista de blocs de dades de l'inode

| INODE                                                        |
| :----------------------------------------------------------- |
| Nombre de Referències (míinim 1)                             |
| d, l, -                                                      |
| Nº de Bloc (Si ocupa més d'un, es van posant consecutivament; simulem que ocupa 2 o més). |
| Nº de bloc                                                   |
| etc....                                                      |

### EXEMPLE D'ACCES A DISC

Primer, veurem una mica de detalls del **sistema de fitxers**, farem tots els passos, donat un sistema de fitxers: Generar els inodes i blocs de dades equivalents i calcularem els accessos a disc per a dos codis diferents. Suposarem el següent sistema d'arxius basat en inodes: 

![filesist](img/sistfitx.png)

Podem deduïr d'aquest sistema d'arxius (i perquè ens ho diuen en l'enunciat normalment):

- Els quadrats són directoris.
- Els rodons són altres tipus d'arxius.
  - /appl/usr1 és un softlink a /home/usr1
  - /home/usr1/F1.txt i /home/usr1/F2.txt són hardlinks.
- Un inode ocupa 1 bloc.
- F1.txt ocupa 5 blocs de dades.
- 1 bloc de dades són 256 bytes.
- Els fitxers "." i ".." no estàn dibuixats per a simplificar.

### Hard links i softlinks

Hardlinks i softlinks són dos mètodes d'enllaçar noms de fitxers amb inode, dos tipus de vincles.

- **Hardlink**: Enllaça el nom amb un inode de tipus "normal" (fitxer de dades), en les dades d'aquest inode hi ha les dades a les quals volem accedir. Apunten a un arxiu en el sistema d'arxius; associen dos o més fitxer compartint el mateix inode, això fa que cada hardlink sigui una còpia exacta de la resta de fitxers associats, tant de dades com de permisos, propietari... Encara que els anomenem de diferentes formes, tant els hardlinks com els arxius originals ofereixen la mateixa funcionalitat. Al modificar les dades apuntades per a qualsevol d'ells, es canvien les dades reals emmagatzemades al disc, quedant modificats els dos per igual.
  - Quan tenim N noms de fitxers que són hardlinks (entenem que al mateix inode), siginifca que tenen el mateix nombre de inode; són directament el mateix fitxer. F1.txt i F2.txt han de tindre el mateix nombre de inode.

#### Exemple de Hardlink

En l'imatge de sota (extreta de Wikipedia) hi ha dos HardLinks: "Enlace1.txt" i "Enlace2.txt". Els dos apunten a les mateixes dades físiques. Si el nom de fitxer "Enlace1.txt" s'obra en un editor de text, es modifica i es guarda, aquests canvis seràn també visibles en el fitxer "Enlace2.txt" i viceversa. Això es degut a que apunten a les mateixes dades.

![hardlink](img/Enlace_duro.svg)

Tornant al esquema del sistema d'arxius que hem vist, anem a definir com queden la taula d'inodes (en el disc) i els blocs de dades:

- Primer hem d'assignar nombres d'inode als fitxers seqüencialment de 0 ... a N.
- F1.txt i F2.txt recordem que son hardlinks a un mateix arxiu, i per tant han de tenir el mateix inode.
- Els enumerem seguint l'ordre de creació (ens ho inventem), però podriem seguir altres mètodes d'enumeració, tot i que es recomana utilitzar una enumeració llògica i clara.

**IMATGE ESQUEMA ENUMERAT^**

### INFORMACIÓ EN EL DISC

- Metadades (taula de inodes).
  - Dades com tipus de fitxer, nombre d'enllaços (incloent . i ..) i llista de blocs de dades.
- Dades (blocs de dades).
  - Els blocs que són directoris, softlinks i blocs de fitxers de dades.

### BLOCS DE DADES

| Directori:                                                 | Softlink:                                                    | Dades "normals"                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| És una taula (nom, inode). <br />Exemple: /home/usr1<br /> | Posem el path (camí) del fitxer al qual apunta:<br />Exemple: /appl/usr1 | Si tenim l'informació la podem posar, sinó indicada:<br />Exemple: F1.txt conté a's. |

#### Com quedaria? [INODES]

| Nº Inode                | 0       | 1    | 2    | 3    | 4    | 5    | 6       |
| ----------------------- | ------- | ---- | ---- | ---- | ---- | ---- | ------- |
| Nº Refs                 | *\*(1)* |      |      |      |      |      |         |
| Tipus (d, l, -) *\*(2)* | d       | d    | d    | d    | d    | l    | -       |
| Bloc 1                  | 0       | 1    | 2    | 3    | 4    | 5    | 6 \*(3) |
| Bloc 2                  |         |      |      |      |      |      | 7       |
| Bloc 3                  |         |      |      |      |      |      | 8       |
| Bloc 4                  |         |      |      |      |      |      | 9       |
| Bloc 5                  |         |      |      |      |      |      | 10      |

\*(1) El nombre de referències l'emplenem al final.

\*(2) Del bloc 0 al 4, són directoris. El 5 és un softlink. I el 6 és un fitxer de dades.

\*(3) Els directoris ocupen 1 bloc, el soft-link també, el fitxer F1.txt i F2.txt comparteixen inode i ocupen 5 blocs.

### Blocs de dades (En necessitem 11: 0...10)

[Imatge bloc de dades]

- Els directoris tenen dues columnes: Nom del fitxer i el nombre d'inode.
- Han de tindre semre els fitxers "." i ".."
- El soft-link ha de contenir el nom del fitxer al que apunta, ja que és un inode diferent.
- Els noms F1.txt i F2.txt són simplement el mateix inode (ja que són hard-links)
- Se suposa que hi ha 256 bytes en cada bloc de dades

Resultat final (amb les referències).

| Nº Inode        | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
| --------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Nº Refs         | 4    | 4    | 2    | 2    | 2    | 1    | 2    |
| Tipus (d, l, -) | d    | d    | d    | d    | d    | l    | -    |
| Bloc 1          | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
| Bloc 2          |      |      |      |      |      |      | 7    |
| Bloc 3          |      |      |      |      |      |      | 8    |
| Bloc 4          |      |      |      |      |      |      | 9    |
| Bloc 5          |      |      |      |      |      |      | 10   |

### EXERCICI D'EXEMPLE

```c
fd = open("/Home/Usr1/F1.txt", O_RDONLY);
ret = read(fd, &c, sizeof(char));
while(ret > 0){
    write(1, &c, sizeof(char));
    ret = read(fd, &c, sizeof(char));
}
close(fd);
```

- Com es tradueix?
  - Hem d'accedir a l'inode de F1.txt i copiarlo a la taula d'inodes a memòria: accés al disc.
  - Llegir tot el contingut de F1.txt: Accessos a disc (byte a byte).
  - Hem d'escriure el que hem llegit per la sortida std.

1. fd = open("/Home/Usr1/F1.txt", O_RDONLY);

   - Amb buffer caché: Tot el que llegim es guarda a la buffer cache (els inodes ocupen 1 bloc de disc):
     1. Llegim l'inode arrel (0).
     2. Llegim les dades de l'inode arrel (bloc de dades 0); veiem que "home" és l'inode 1.
     3. Llegim l'inode 1 (Home)
     4. Llegim les dades de l'inode 1 (bloc de dades 1); veiem que Usr1  es l'inode 3.
     5. Llegim l'inode 3.
     6. Llegim les dades de l'inode 3 (bloc de dades 3); Veiem que F1.txt es l'inode 6.
     7. Llegim l'inode 6; inode destí; ho copiem en la taula d'inodes a memòria.

   2 i 5. ret = read(fd, &c, sizeof(char));

   - S'executen 256*5 crides a sistema (hi ha 5 blocs de dades) per a llegir les dades.
     - Si hi ha buffer cache farem 5 accessos al disc.
     - D'altre manera farem 256*5 accessos a disc.
   - Farem un últim read per a detectar que hem arribat al final (el read que retorna 0).

3.write(1, &c, sizeof(char));

- Si el canal 1 és la consola : no genera accessos a disc.

7. close(fd);
   - Com no hem modificat l'inode no fa falta escriure'l a disc.
