## Diagrama:

@startuml

[*] --> StandBy
state "StandBy" as StandBy
state "ValidarTarjeta" as ValidarTarjeta
state "SolicitarPIN" as SolicitarPIN
state "ValidarPIN" as ValidarPIN
state "TarjetaRetenida" as TarjetaRetenida
state "MenuTransacciones" as MenuTransacciones
state "EjecutarTransaccion" as EjecutarTransaccion
state "FinalizarSesion" as FinalizarSesion

StandBy --> ValidarTarjeta : Tarjeta introducida
ValidarTarjeta --> StandBy : Tarjeta inválida
ValidarTarjeta --> SolicitarPIN : Tarjeta válida

SolicitarPIN --> ValidarPIN : PIN introducido
ValidarPIN --> SolicitarPIN : PIN incorrecto (intentos < 3)
ValidarPIN --> TarjetaRetenida : PIN incorrecto (intentos = 3)
ValidarPIN --> MenuTransacciones : PIN correcto
SolicitarPIN --> StandBy : Cancelar

MenuTransacciones --> EjecutarTransaccion : Transacción seleccionada
EjecutarTransaccion --> MenuTransacciones : Transacción completada
MenuTransacciones --> FinalizarSesion : Cancelar

FinalizarSesion --> StandBy : Sesión finalizada
TarjetaRetenida --> StandBy : Tarjeta retenida
@enduml

-------------
## Explicacion de la interpretacion del diagrama
- StandBy: Es el estado inicial del cajero, donde el sistema está en esperando a que le introduzcan una tarjeta. Cuando se introduce, pasa al estado VAlidarTarjeta
- ValidarTarjeta: El sistema valida la tarjeta introducida. Si es invalida se expulsa la tarjeta y vuelve a Standby, si no pasa a SolicitarPin. 
- SolicitarPIN: El sistema solicita al usuario que introduzca su PIN. Dependiendo si se introduce el pin o se cancela, el sistema pasa al estado ValidarPin o al de Stanby.
- ValidarPIN: El sistema valida el PIN introducido. Si el PIN es incorrecto y el número de intentos es menor a 3, el sistema vuelve al estado SolicitarPin si no pasa a TarjetaRetenida. Si es correcto pasa al Menutransacciones.
- TarjetaRetenida: El sistema retiene la tarjeta por tener muchos intentos fallidos de PIN. Luego de esto vuelve a Standby.
- MenuTransacciones: El sistema muestra el menú de transacciones disponibles. Cuando el usuario selecciona una opción, el sistema pasa a EjectuarTransaccion si no pasa a FinalizarSesion.
- EjecutarTransaccion: El sistema ejecuta la transacción seleccionada. Una vez hecha, el sistema vuelve al MenúTransacciones por si el usuario quiere hacer otra cosa.
- FinalizarSesion: El sistema finaliza la sesión del usuario. Despues de esto el sistema vuelve a Standby. 



