# 

# **PRD APPCHULA**

## Software requirements specification 

Proveedor: **Metlabs**  
Cliente: **APPCHULA**

Versión 1

Enero 2025

# 

# 

# 

# 

# 

# 

# 

# 

# 

**1\. Introducción y Contexto del Producto**

APPCHULA es una plataforma privada de gestión y visualización de capital institucional. Está diseñada para operar en un entorno de **bajo volumen de usuarios y alto valor económico por cliente**, donde la prioridad no es la captación ni el autoservicio, sino el control, la claridad y la trazabilidad.

El sistema actúa como una **extensión tecnológica** de un proceso de inversión que sucede principalmente fuera de la plataforma. No pretende reemplazar procesos financieros, contractuales o de compliance, sino aportar una capa de visibilidad y control una vez dichos procesos han sido completados.

### **1.1 Perfil de cliente**

Los clientes de APPCHULA son principalmente:

* Family Offices  
* Venture Capital  
* Empresas patrimoniales  
* Inversores institucionales  
* De forma excepcional, personas físicas de alto patrimonio

Todos los clientes que acceden a la plataforma han sido previamente:

* Cualificados a nivel comercial  
* Validados por compliance  
* Aceptados internamente

La plataforma **no está diseñada para usuarios no cualificados ni para uso público**.

### **1.2 Objetivos funcionales de la plataforma**

Desde el punto de vista del producto, APPCHULA persigue tres objetivos claros:

* Proporcionar al cliente una visión clara y actualizada de su capital y balance  
* Reflejar de forma transparente la evolución de los rendimientos a lo largo del tiempo  
* Servir como herramienta interna de control, trazabilidad y gestión administrativa

Estos objetivos definen el alcance funcional del sistema y condicionan todas las decisiones de diseño.

**2\. Flujo de Negocio Externo y Momento de Entrada en Plataforma**

El ciclo de vida de un cliente comienza **fuera del sistema**. Antes de que exista cualquier usuario en la plataforma, se han completado una serie de pasos que forman parte del modelo operativo de APPCHULA.

### **2.1 Flujo externo previo**

De forma general, el proceso externo incluye:

* Contacto comercial y cualificación inicial  
* Recopilación de información relevante  
* Proceso de compliance y due diligence  
* Firma del contrato de inversión  
* Recepción y confirmación del capital

Solo cuando todas estas fases se han completado satisfactoriamente se procede a crear el usuario dentro de la plataforma.

### **2.2 Implicaciones en la experiencia de usuario**

Este enfoque tiene consecuencias directas en la experiencia de usuario:

* El acceso a la plataforma **no implica verificación adicional**  
* No existen formularios de onboarding  
* No existen bloqueos por KYC  
* El usuario entra cuando ya está validado

La experiencia debe ser directa, sin fricciones ni pasos innecesarios.

**3\. Principios de Diseño del Sistema**

El diseño del sistema se basa en una serie de principios explícitos que actúan como criterios rectores para el desarrollo y la toma de decisiones técnicas.

### **3.1 Control administrativo total**

* No existen flujos críticos de autoservicio  
* El cliente no ejecuta acciones irreversibles  
* Todas las operaciones relevantes se supervisan desde Backoffice  
* Los datos financieros siempre están bajo control administrativo

### **3.2 Simplicidad operativa consciente**

APPCHULA gestiona un número reducido de clientes, lo que permite optar deliberadamente por procesos manuales y auditables.

* Se prioriza claridad frente a automatización  
* Se evitan procesos opacos o difíciles de auditar  
* Las operaciones críticas requieren validación humana

Esta simplicidad es una **decisión de producto**, no una limitación técnica.

### **3.3 Consistencia financiera**

* Moneda única del sistema: USD  
* Capital, balance, rendimientos y comisiones se expresan siempre en USD  
* No existe conversión de moneda ni multimoneda

Este principio garantiza consistencia en todos los cálculos y visualizaciones.

#### **3.4 Accesibilidad y Dispositivos**

La plataforma APPCHULA debe ser accesible desde dispositivos de escritorio y móviles.

Requisitos de acceso:

* La interfaz debe ser responsive.  
* Debe permitir la consulta correcta de la información desde dispositivos móviles y tablets.  
* No se requiere una aplicación nativa.

**4\. Modelo de Usuario**

El modelo de usuario de APPCHULA refleja su operativa real: el usuario no es el origen del proceso, sino su último eslabón. El sistema no está diseñado para que los usuarios se registren ni introduzcan información crítica por sí mismos.

### **4.1 Tipos y Roles de Usuario**

El modelo de usuario de APPCHULA distingue claramente entre **tipo de usuario** y **rol operativo**.

#### **Tipos de Usuario**

El tipo de usuario define la relación comercial del usuario con la plataforma y su comportamiento funcional principal. Un usuario puede pertenecer a uno de los siguientes tipos:

* Cliente  
* Agente  
* Super Agente

Estos tipos determinan la información financiera visible y las vistas adicionales disponibles (por ejemplo, visibilidad de clientes asociados).

#### **Roles Operativos**

De forma independiente al tipo de usuario, un usuario puede tener asignado uno o varios roles operativos que definen sus permisos administrativos dentro del sistema:

* Administrador  
* Operador

Los roles operativos no constituyen tipos de usuario por sí mismos, sino **permisos adicionales** que habilitan acceso al Backoffice y a funciones internas. 

#### **Combinación de Tipos y Roles**

Un mismo usuario puede:

* Ser Cliente, Agente o Super Agente  
* Tener simultáneamente el rol de Administrador o de Operador

Por ejemplo, un usuario puede ser Super Agente y Administrador al mismo tiempo.

La asignación de tipos y roles se realiza manualmente desde el Backoffice por un Administrador y puede modificarse en cualquier momento.

### **4.2 Existencia del Usuario**

Un usuario existe en la plataforma únicamente cuando:

* Ha sido validado comercialmente  
* Ha pasado compliance fuera de la plataforma  
* Ha firmado contrato  
* Ha realizado el depósito inicial

No existe registro ni onboarding desde el frontend.

### **4.3 Estado Operativo del Usuario**

Desde el punto de vista operativo, el sistema contempla los siguientes estados de usuario, diferenciando claramente entre el funcionamiento actual y la preparación para escenarios futuros:

* **Activo (verificado)**: estado por defecto en el funcionamiento actual de la plataforma. Todos los usuarios creados hoy acceden a la plataforma una vez han sido validados externamente y pueden visualizar su información financiera y operar dentro de los límites definidos.  
* **No verificado (estado reservado para KYC/KYB futuro)**: estado previsto para una futura integración de procesos de verificación dentro de la plataforma. Este estado no se utiliza en la operativa actual, pero se mantiene a nivel de modelo para permitir:  
  * Integración de KYC/KYB sin rediseños estructurales.  
  * Restricción de acciones sensibles (por ejemplo, retiros o modificaciones financieras) mientras el usuario no complete la verificación.  
  * Configuración flexible del comportamiento una vez el módulo de KYC esté activo.  
* **Bloqueado**: estado administrativo excepcional aplicado manualmente por un Administrador.

Un usuario bloqueado puede iniciar sesión y visualizar información, pero no genera rendimientos ni comisiones ni puede solicitar retiros.

### **4.4 Preparación para KYC / KYB Futuro (Sumsub)**

Aunque la verificación ocurre actualmente fuera de la plataforma, el sistema debe quedar preparado para una futura integración de KYC/KYB.

Esto implica:

* Campos internos de estado KYC  
* Registro de proveedor de verificación  
* Fecha de última verificación  
* Posibilidad de integración vía enlace o iframe

Inicialmente, este módulo no es bloqueante ni funcional.

**5\. Proceso de Alta de Usuario desde Backoffice**

El proceso de creación de usuarios es completamente manual y se ejecuta desde el Backoffice:

1. El Administrador crea el usuario  
2. Asigna el rol correspondiente  
3. Carga la información del cliente  
4. Guarda el usuario como activo  
5. Envía la invitación por email

El usuario nunca introduce datos críticos por sí mismo.

**6\. Autenticación y Seguridad**

La autenticación y la seguridad de la plataforma APPCHULA están diseñadas para equilibrar dos objetivos clave: garantizar un alto nivel de protección acorde al perfil institucional de los clientes y, al mismo tiempo, ofrecer una experiencia de uso sencilla, sin fricciones innecesarias.

Dado que los usuarios acceden con baja frecuencia y manejan información financiera sensible, el sistema prioriza mecanismos de control claros, auditables y bien delimitados.

### **6.1 Principios de Seguridad**

La arquitectura de seguridad se basa en los siguientes principios:

* No existe registro público ni autoservicio.  
* Todas las cuentas son creadas manualmente desde Backoffice.  
* El acceso inicial se realiza mediante invitación controlada.  
* Las acciones sensibles requieren verificación adicional.  
* Toda acción relevante queda registrada en logs.

### **6.2 Invitación y Activación de Cuenta**

El acceso inicial del usuario a la plataforma se realiza exclusivamente mediante un proceso de invitación.

El flujo es el siguiente:

1. El Administrador crea el usuario desde Backoffice.  
2. El sistema genera un token de activación único.  
3. Se envía un email de invitación al usuario.  
4. El usuario accede al enlace y define su contraseña.

Características del token de activación:

* Es de un solo uso.  
* Tiene caducidad configurable (recomendado: 24 horas).  
* Puede ser regenerado manualmente por el Administrador.

El Administrador dispone de las siguientes acciones:

* Reenviar el email de invitación.  
* Copiar manualmente el enlace de activación.  
* Invalidar invitaciones anteriores al generar una nueva.

### **6.3 Creación y Gestión de Contraseña**

Durante el proceso de activación, el usuario define su contraseña.

Requisitos de contraseña:

* Longitud mínima configurable.  
* Inclusión de caracteres alfanuméricos.  
* Política orientada a buenas prácticas estándar.

El sistema permite:

* Cambio de contraseña desde el perfil.  
* Recuperación de contraseña mediante email.

No se permite:

* Definir contraseñas temporales visibles.  
* Acceso sin contraseña.

### **6.4 Login**

El acceso a la plataforma se realiza mediante:

* Email  
* Contraseña

El teléfono es un campo opcional y no se utiliza como método de login.

No existen:

* Logins sociales.  
* Accesos mediante terceros.

### **6.5 Segundo Factor de Autenticación (2FA)**

El segundo factor de autenticación se aplica únicamente en acciones consideradas sensibles.

Acciones protegidas por 2FA:

* Login desde un dispositivo o ubicación no reconocidos.  
* Solicitud de retiro.  
* Modificación de datos financieros críticos.  
* Cambios de credenciales.

Características del 2FA:

* Método inicial: código de un solo uso enviado por email.  
* Posibilidad de recordar dispositivo durante 30 días.  
* Configuración orientada a buenas prácticas.

El sistema queda preparado para:

* Integración futura con aplicaciones de autenticación (Authenticator App).

### **6.6 Gestión de Sesión**

La sesión del usuario se gestiona de forma controlada para reducir riesgos de acceso indebido.

Reglas de sesión:

* Cierre automático tras 20 minutos de inactividad.  
* Invalidación de sesión al cambiar contraseña.  
* Invalidación manual desde Backoffice en caso de necesidad.

### **6.7 Bloqueo de Usuario**

El Administrador puede bloquear una cuenta en cualquier momento.

Un usuario bloqueado:

* Puede iniciar sesión.  
* Puede visualizar su información.  
* No puede ejecutar acciones sensibles.

El bloqueo no elimina datos ni sesiones históricas y queda registrado en logs.

### **6.8 Auditoría y Seguridad**

Todas las acciones relacionadas con autenticación y seguridad generan registros auditables, incluyendo:

* Envío y uso de invitaciones.  
* Cambios de contraseña.  
* Intentos de login.  
* Uso de 2FA.  
* Bloqueos y desbloqueos.

Estos registros permiten trazabilidad completa y análisis posterior ante cualquier incidencia de seguridad.

#### **6.9 Notificaciones al Usuario**

La plataforma APPCHULA incorpora un sistema de notificaciones automáticas por email asociado a eventos operativos relevantes. Estas notificaciones tienen un carácter informativo y no habilitan acciones directas desde el mensaje.

Las notificaciones se envían únicamente a la dirección de email registrada del usuario y no pueden desactivarse.

Eventos que generan notificación automática:

* Ejecución de cierre mensual de rendimientos.  
* Aprobación de una solicitud de retiro.  
* Ejecución de un retiro.  
* Acreditación de comisiones (cuando aplique según el tipo de usuario).

Características generales:

* Las notificaciones son exclusivamente informativas.  
* No incluyen enlaces para modificar datos ni ejecutar acciones.  
* No sustituyen la información disponible en el dashboard.  
* Cada envío queda registrado en los logs del sistema para auditoría.

El contenido y el formato de las notificaciones podrán ajustarse en fases posteriores sin afectar al core funcional del sistema.

**7\. Dashboard del Cliente**

El Dashboard del Cliente es el punto central de interacción entre el inversor y la plataforma. Su objetivo no es permitir la operativa, sino ofrecer una **visión clara, comprensible y fiable** del estado de la inversión.

El diseño del dashboard debe priorizar la simplicidad y la lectura rápida, teniendo en cuenta que los usuarios son perfiles institucionales que acceden con baja frecuencia, pero esperan información precisa y consistente.

### **7.1 Propósito del Dashboard**

El dashboard tiene como propósito:

* Proporcionar una visión global del estado de la inversión.  
* Permitir al cliente consultar la evolución histórica de su capital.  
* Mostrar de forma transparente los rendimientos generados.  
* Facilitar el seguimiento de retiros solicitados y ejecutados.

El dashboard **no permite realizar depósitos ni inversiones**, ni ejecutar acciones financieras directas.

### **7.2 Estructura General**

El dashboard se compone de los siguientes bloques principales, presentados en un orden lógico y consistente:

1. Resumen financiero  
2. Evolución del balance  
3. Histórico de rendimientos  
4. Historial de retiros  
5. Información de contacto del agente asignado

Cada bloque debe ser claramente identificable y accesible desde una única vista principal.

### **7.3 Resumen Financiero**

El resumen financiero es el bloque principal del dashboard y debe mostrar, de forma destacada:

* Capital inicial invertido  
* Balance actual  
* Rendimiento acumulado (en valor absoluto y porcentaje)

Este bloque representa el estado actual de la inversión tras el último cierre mensual aprobado.

Los valores mostrados:

* Se expresan siempre en USD  
* Se muestran con dos decimales  
* No se actualizan en tiempo real

### **7.4 Evolución del Balance**

El dashboard debe incluir una visualización de la evolución del balance a lo largo del tiempo.

Esta visualización puede representarse mediante:

* Gráfico temporal (línea o barras)  
* Periodicidad mensual

La información reflejada corresponde exclusivamente a cierres mensuales aprobados.

### **7.5 Histórico de Rendimientos**

El cliente debe poder consultar un histórico detallado de los rendimientos generados.

Cada registro mensual incluye:

* Mes y año  
* Porcentaje de rendimiento aplicado  
* Importe del rendimiento generado  
* Balance resultante tras el cierre

Este histórico es de solo lectura y no permite modificaciones ni acciones asociadas.

### **7.6 Historial de Retiros**

El dashboard incluye una sección específica para el seguimiento de retiros.

Cada retiro muestra:

* Fecha de solicitud  
* Importe solicitado  
* Estado del retiro (Solicitado, Aprobado, Ejecutado)  
* Fecha de ejecución (si aplica)

El cliente no puede editar ni cancelar retiros desde el dashboard una vez solicitados.

### **7.7 Información del Agente Asignado**

En el dashboard debe mostrarse claramente la información de contacto del agente asignado al cliente:

* Nombre  
* Email  
* Teléfono

Este bloque tiene como objetivo facilitar la comunicación directa fuera de la plataforma para cualquier gestión adicional.

### **7.8 Exportación de Información**

El cliente puede exportar la información histórica disponible en el dashboard.

Las opciones de exportación incluyen:

* Histórico de rendimientos  
* Historial de retiros

Los formatos soportados son CSV y Excel.

La exportación es exclusivamente de solo lectura y no permite modificar información.

**8\. Depósitos**

No existen flujos de depósito dentro de la plataforma. Todos los depósitos se gestionan externamente y son cargados manualmente por el Administrador.

## **9\. Retiros**

El módulo de retiros permite a los clientes solicitar la devolución parcial o total de su capital invertido, siempre dentro de las condiciones operativas definidas por APPCHULA. Este módulo no ejecuta transferencias ni movimientos de fondos, sino que actúa como un **sistema de solicitud, control y trazabilidad**.

Los retiros constituyen una de las operaciones más sensibles del sistema, por lo que su diseño prioriza el control administrativo, la validación manual y la claridad en los estados.

### **9.1 Principios Generales**

Los retiros en APPCHULA se rigen por los siguientes principios:

* No son inmediatos ni en tiempo real.  
* Están sujetos a ventanas temporales predefinidas.  
* Requieren validación manual por parte del Administrador.  
* No desencadenan automáticamente transferencias bancarias o cripto.

La plataforma refleja y registra el proceso, pero la ejecución final ocurre fuera del sistema.

### **9.2 Ventanas de Retiro**

Las solicitudes de retiro solo pueden realizarse dentro de ventanas temporales específicas, definidas a nivel de negocio.

Las ventanas establecidas son:

* 31 de marzo  
* 30 de junio  
* 30 de septiembre  
* 31 de diciembre

Para que una solicitud sea válida, debe realizarse con un mínimo de **10 días naturales de antelación** respecto a la fecha de ventana correspondiente.

Fuera de estas ventanas, el sistema no permite crear solicitudes de retiro.

Adicionalmente, para poder solicitar un retiro, el cliente debe cumplir una **antigüedad mínima de 3 meses** desde el inicio efectivo de generación de rendimientos. El sistema debe validar esta condición antes de permitir la creación de la solicitud.

### **9.3 Tipos de Retiro**

El sistema permite dos tipos de retiro:

* **Retiro parcial**: el cliente solicita un importe específico.  
* **Retiro total**: el cliente solicita la totalidad del balance disponible.

En ambos casos, el importe solicitado queda sujeto a validación administrativa.

### **9.4 Flujo de Solicitud de Retiro**

El flujo de retiro se compone de tres estados claramente diferenciados:

El flujo de retiro se compone de tres estados claramente diferenciados:

1. **Solicitado**: el cliente crea la solicitud desde el dashboard.  
2. **Aprobado**: el Administrador valida la solicitud.  
3. **Ejecutado**: el Administrador marca la solicitud como ejecutada una vez realizado el pago fuera del sistema.

Cada cambio de estado queda registrado en los logs del sistema.

### **9.5 Comportamiento del Cliente**

Desde el dashboard, el cliente puede:

* Crear una solicitud de retiro dentro de una ventana válida.  
* Consultar el estado de sus solicitudes.  
* Ver el historial completo de retiros.

El cliente **no puede**:

* Modificar una solicitud una vez enviada.  
* Cancelar una solicitud desde la plataforma.  
* Cambiar los datos de la cuenta de destino.

Cualquier modificación requiere comunicación directa con el equipo de APPCHULA.

### **9.6 Cuenta de Destino del Retiro**

Por defecto, los retiros se realizan siempre a la **misma cuenta bancaria desde la que se realizó el depósito inicial**.

Esta restricción responde a criterios de compliance y control de riesgo.

Si el cliente desea retirar a una cuenta distinta:

* Debe gestionarlo fuera de la plataforma.  
* Requiere validación manual por parte del equipo de APPCHULA.

La plataforma no permite seleccionar cuentas arbitrarias en el flujo de retiro.

### **9.7 Impacto en el Balance**

Una vez que un retiro es marcado como **Ejecutado**:

* El importe correspondiente se descuenta del balance del cliente.  
* El ajuste queda reflejado en el histórico financiero.  
* El cambio es irreversible desde el sistema.

En caso de **retiro total del capital**:

* El último mes de inversión se calcula aplicando el **porcentaje mínimo garantizado** del cliente.  
* No se aplica el porcentaje mensual estándar, incluso si este fuese superior.

No se realizan descuentos de balance en estados previos a la ejecución.

Una vez que un retiro es marcado como **Ejecutado**:

* El importe correspondiente se descuenta del balance del cliente.  
* El ajuste queda reflejado en el histórico financiero.  
* El cambio es irreversible desde el sistema.

No se realizan descuentos de balance en estados previos a la ejecución.

### **9.8 Auditoría y Trazabilidad**

Cada solicitud de retiro incluye:

* Identificador único.  
* Fecha de solicitud.  
* Importe solicitado.  
* Estados y fechas asociadas.  
* Usuario administrador que aprueba y ejecuta.

Toda esta información queda disponible para auditoría interna.

## **10\. Capital, Balance y Rendimientos**

Este módulo constituye el **núcleo financiero** de la plataforma APPCHULA. Toda la información mostrada al cliente, así como los cálculos posteriores de comisiones y retiros, dependen de la correcta definición y gestión del capital, el balance y los rendimientos.

El sistema no calcula rendimientos de forma autónoma ni en tiempo real. Todos los valores financieros se gestionan a partir de datos introducidos y validados manualmente desde el Backoffice, siguiendo reglas de negocio bien definidas.

### **10.1 Definiciones Fundamentales**

Para evitar ambigüedades, el sistema distingue claramente entre los siguientes conceptos:

* **Capital inicial**: importe total depositado por el cliente al inicio de la relación o en incrementos posteriores. Este valor representa el capital base sobre el que se aplican determinadas condiciones comerciales.  
* **Balance**: valor total de la cuenta del cliente en un momento dado. Incluye el capital inicial más los rendimientos acumulados y las comisiones que correspondan.  
* **Rendimiento**: beneficio generado durante un periodo mensual específico, calculado como un porcentaje aplicado sobre el balance o capital según las reglas vigentes.

Estos conceptos no son intercambiables y deben tratarse de forma independiente en toda la plataforma.

### **10.2 Capital Inicial y Modificaciones**

El capital inicial es introducido manualmente por el Administrador en el momento de creación del usuario o cuando se produce un incremento de capital.

Las características del capital son:

* Se expresa siempre en USD.  
* No se modifica automáticamente.  
* Puede incrementarse manualmente por decisión administrativa.

Cualquier modificación del capital inicial:

* Requiere confirmación explícita.  
* Queda registrada en los logs del sistema.  
* No recalcula rendimientos pasados.

### **10.3 Balance del Cliente**

El balance representa el valor total actualizado de la inversión del cliente tras el último cierre mensual aprobado.

Características del balance:

* Se expresa siempre en USD.  
* Se actualiza únicamente durante el proceso de cierre mensual.  
* No varía en tiempo real.

El balance es el valor que se utiliza como referencia para:

* Visualización en el dashboard.  
* Cálculo de rendimientos futuros.  
* Determinación de importes retirables.

### **10.4 Rendimiento Mensual**

Los rendimientos se calculan con una periodicidad mensual y siguen un proceso controlado. La generación de rendimientos depende tanto del porcentaje mensual definido como del momento efectivo en que el capital comienza a ser elegible para generar rendimiento.

#### **10.4.1 Elegibilidad del Capital para Generación de Rendimiento**

Los clientes pueden invertir en cualquier momento. Sin embargo, la fecha de inicio de generación de rendimiento se determina según la fecha de ingreso y registro del capital:

* Capital ingresado **antes o el día 14 (inclusive)** del mes:  
  * Genera rendimiento a partir del **día 15 del mismo mes**.  
* Capital ingresado **después del día 14** del mes:  
  * Genera rendimiento a partir del **día 1 del mes siguiente**.

Cuando un capital comienza a generar rendimiento a partir del día 15 del mes, dicho capital recibe un porcentaje proporcional equivalente al **50% del rendimiento mensual** correspondiente a ese mes.

Existe además un **umbral mínimo de inversión**:

* Si el capital total invertido **no supera los 50.000 USD**, dicho capital **no genera rendimientos ni comisiones**, independientemente del porcentaje mensual definido.

#### **10.4.2 Porcentaje Mínimo Garantizado**

Cada cliente tiene definido un **porcentaje mínimo de rendimiento garantizado**, configurable desde Backoffice.

Este porcentaje **no es un campo libre** y debe seleccionarse obligatoriamente desde un menú predefinido con los siguientes valores:

* 2.5%  
* 3.25%  
* 4.25%  
* 5%

No se permite introducir valores fuera de este conjunto.

Durante el cierre mensual:

* Si el porcentaje mensual del periodo es **inferior** al porcentaje mínimo garantizado del cliente, se aplica este último.  
* El porcentaje mínimo garantizado prevalece sobre cualquier otro cálculo de rendimiento.

Los rendimientos se calculan con una periodicidad mensual y siguen un proceso controlado.

El flujo general es el siguiente:

1. El Administrador introduce el porcentaje de rendimiento correspondiente al mes.  
2. El sistema calcula el rendimiento individual de cada cliente.  
3. Se presenta un listado de validación antes de ejecutar los cambios.  
4. El Administrador aprueba o excluye clientes de forma explícita.

No se aplican rendimientos de forma automática ni programada.

### **10.5 Listado de Validación de Rendimientos**

Antes de ejecutar el cierre mensual, el sistema presenta un listado que incluye, para cada cliente:

* Balance anterior.  
* Porcentaje de rendimiento aplicado.  
* Importe del rendimiento calculado.  
* Balance resultante tras el cierre.

Cada registro dispone de un control de aprobación que permite:

* Confirmar la aplicación del rendimiento.  
* Excluir clientes específicos del cierre mensual.

Solo los registros aprobados se ejecutan y afectan al balance.

### **10.6 Ejecución del Cierre de Rendimientos**

Una vez aprobado el listado de validación:

* El sistema actualiza el balance de los clientes incluidos.  
* Se genera el registro histórico del rendimiento mensual.  
* Los cambios se consolidan de forma irreversible.

Los cierres no pueden revertirse desde la plataforma.

### **10.7 Impacto en Comisiones y Retiros**

Los rendimientos ejecutados impactan directamente en:

* El cálculo de comisiones de agentes y superagentes.  
* El balance disponible para retiros futuros.

Un rendimiento no ejecutado o excluido:

* No genera comisiones.  
* No incrementa el balance.

### **10.8 Precisión y Redondeo**

Los cálculos financieros internos pueden manejar mayor precisión, pero:

* Todos los valores mostrados al usuario se redondean a dos decimales.  
* Se utiliza un criterio de redondeo estándar (round half up).

Esta regla se aplica de forma consistente en toda la plataforma.

### **10.9 Auditoría y Trazabilidad**

Cada cierre mensual genera registros auditables que incluyen:

* Mes y año del cierre.  
* Porcentaje aplicado.  
* Usuario administrador responsable.  
* Fecha y hora de ejecución.

Estos registros permiten reconstruir el histórico financiero completo de cada cliente.

**11\. Agentes y Super Agentes**

La plataforma APPCHULA incorpora una estructura comercial basada en Agentes y Super Agentes. Esta estructura define cómo se generan, acumulan y liquidan las comisiones derivadas de los rendimientos de los clientes.

La gestión de estas relaciones y de las comisiones asociadas se realiza de forma manual y controlada desde el Backoffice. No existen flujos automáticos ni autoasignaciones por parte de los usuarios.

Las reglas descritas en esta sección aplican a todos los usuarios y sustituyen cualquier lógica anterior relacionada con comisiones.

**11.1 Principios Generales**

Las comisiones se rigen por los siguientes principios:

* Las comisiones se calculan únicamente sobre meses cerrados.  
* No existen comisiones sin cierres mensuales previamente aprobados.  
* El cliente asociado debe tener más de 1 mes de antigüedad efectiva para generar comisiones.  
* Los porcentajes de comisión son fijos y configurables desde Backoffice.  
* El sistema calcula importes, pero no ejecuta pagos de forma automática.  
* Todas las operaciones quedan registradas y son auditables.

**11.2 Super Agentes**

#### **11.2.1 Regla de Comisión**

El Super Agente percibe una comisión fija aplicada de forma homogénea a todos los clientes que tenga asociados, ya sea de forma directa o a través de agentes.

#### **11.2.2 Periodicidad de la Comisión**

La comisión del Super Agente se calcula y se liquida de forma mensual, al final de cada mes natural, una vez aprobado el cierre mensual correspondiente.

#### **11.2.3 Base de Cálculo**

La comisión del Super Agente se calcula sobre el balance del cliente correspondiente al cierre del mes anterior.

Solo se incluyen en el cálculo aquellos clientes que:

* Tengan más de 1 mes de antigüedad desde el inicio efectivo de generación de rendimientos.

#### **11.2.4 Acreditación de la Comisión**

La comisión del Super Agente:

* Se acredita como una operación financiera más dentro del sistema.  
* Se incorpora como incremento del balance del Super Agente.  
* Forma parte del balance total y genera rendimientos en ciclos posteriores.  
* Está sujeta a las mismas reglas de retiro que el resto del balance.

Cada acreditación queda registrada indicando:

* Mes de referencia.  
* Base de cálculo utilizada.  
* Porcentaje aplicado.  
* Usuario administrador responsable de la aprobación.

**11.3 Agentes**

#### **11.3.1 Regla de Comisión**

El Agente percibe una comisión fija aplicada sobre los clientes directos que tenga asociados.

La comisión del Agente:

* Sale del balance del Super Agente al que está vinculado.  
* No se genera si el cliente no cumple el requisito de antigüedad mínima de 1 mes.

#### **11.3.2 Generación de Comisiones**

La comisión del Agente se calcula mensualmente y únicamente sobre meses cerrados.

La base de cálculo es:

* El balance disponible del cliente después de cada fecha de retiro.

Esto implica que:

* El cálculo se realiza mes a mes.  
* No se calcula sobre capital pendiente de retiro.  
* No se incluyen meses abiertos o parciales.

#### **11.3.3 Acumulación de Comisiones**

Las comisiones generadas por el Agente:

* Se acumulan mensualmente.  
* Se mantienen como importe pendiente de cobro.  
* No se acreditan inmediatamente al balance del Agente.  
* No generan rendimientos mientras estén acumuladas.

#### **11.3.4 Cobro de Comisiones**

El cobro de comisiones de los Agentes se realiza exclusivamente en las fechas de retiro establecidas por la plataforma:

* 31 de marzo  
* 30 de junio  
* 30 de septiembre  
* 31 de diciembre

En la fecha de cobro:

* El importe acumulado se acredita al balance del Agente.  
* El mismo importe se descuenta del balance del Super Agente asociado.

Ambas operaciones se registran como movimientos financieros independientes y quedan vinculadas entre sí a nivel de auditoría.

**11.4 Trazabilidad y Auditoría de Comisiones**

El sistema mantiene un registro completo y auditable de:

* Comisiones generadas por cliente y mes.  
* Comisiones acumuladas por Agente.  
* Acreditaciones mensuales de Super Agentes.  
* Cobros trimestrales de Agentes.  
* Descuentos aplicados a Super Agentes.

Cada registro incluye:

* Periodo de referencia.  
* Base de cálculo.  
* Porcentaje aplicado.  
* Importe resultante.  
* Usuario administrador responsable.

Estos registros permiten reconstruir de forma histórica cualquier cálculo o liquidación de comisiones.

**12\. Backoffice, Logs y Auditoría**

El Backoffice constituye el **núcleo operativo y administrativo** de la plataforma APPCHULA. Todas las acciones críticas, configuraciones y validaciones se realizan desde este entorno, que está reservado exclusivamente a usuarios internos con permisos específicos.

El Backoffice no es un panel auxiliar, sino la pieza central que garantiza el correcto funcionamiento del sistema, el cumplimiento de las reglas de negocio y la trazabilidad de todas las operaciones.

### **12.1 Objetivos del Backoffice**

El Backoffice tiene como objetivos principales:

* Centralizar la gestión de usuarios y relaciones comerciales.  
* Controlar y validar todas las operaciones financieras.  
* Permitir la edición controlada de datos críticos.  
* Garantizar trazabilidad y auditoría completa del sistema.

### **12.2 Roles Internos**

Existen dos roles internos con acceso al Backoffice:

**Administrador**

* Acceso total al sistema.  
* Puede crear, editar y bloquear usuarios.  
* Puede modificar datos financieros.  
* Ejecuta cierres mensuales y liquidaciones.  
* Aprueba y ejecuta retiros.

**Operador**

* Acceso de solo lectura a la mayoría de secciones.  
* Puede visualizar información financiera.  
* Puede añadir notas internas a los perfiles de usuario.  
* No puede ejecutar acciones financieras ni modificar datos críticos.

### **12.3 Gestión de Usuarios**

Desde el Backoffice, el Administrador puede:

* Crear nuevos usuarios.  
* Asignar roles (Cliente, Agente, Super Agente).  
* Modificar información personal y corporativa.  
* Bloquear o desbloquear usuarios.  
* Asignar relaciones comerciales.

Todas estas acciones requieren confirmación explícita y quedan registradas en logs.

### **12.4 Edición de Datos Financieros**

Los datos financieros editables desde Backoffice incluyen:

* Capital inicial.  
* Incrementos de capital.  
* Porcentaje de rendimiento asegurado.  
* Ajustes manuales de balance (casos excepcionales).

La edición de estos datos:

* Requiere confirmación explícita.  
* Puede requerir 2FA según configuración.  
* Genera siempre un registro de auditoría.

### **12.5 Gestión de Retiros**

El Backoffice permite:

* Visualizar todas las solicitudes de retiro.  
* Aprobar o rechazar solicitudes.  
* Marcar retiros como ejecutados.

El sistema no permite ejecutar pagos, únicamente registrar el estado de la operación.

### **12.6 Notas Internas**

Cada usuario dispone de una sección de notas internas accesible desde Backoffice.

Características:

* Solo visibles para Administradores y Operadores.  
* Cada nota incluye autor, fecha y contenido.  
* No son visibles para el cliente.

Las notas sirven para documentar decisiones, incidencias o comunicaciones relevantes.

### **12.7 Logs y Auditoría**

El sistema mantiene logs inmutables de todas las acciones críticas, incluyendo:

* Creación y edición de usuarios.  
* Cambios en relaciones comerciales.  
* Modificaciones financieras.  
* Ejecución de cierres y liquidaciones.  
* Cambios de estado en retiros.

Cada registro incluye:

* Usuario que ejecuta la acción.  
* Fecha y hora.  
* Valores anteriores y posteriores (cuando aplica).

Estos logs permiten auditoría completa y reconstrucción histórica de cualquier evento.

**13\. Cierre Mensual**

El cierre mensual es el proceso operativo más importante de la plataforma APPCHULA. Este proceso consolida la información financiera del mes cerrado y actualiza los balances de los clientes según el rendimiento obtenido.

El cierre mensual no se ejecuta de forma automática. Requiere intervención del Administrador y está diseñado con una lógica de validación previa para minimizar errores y garantizar que todos los cálculos son revisables y auditables.

### **13.1 Objetivos del Cierre Mensual**

El cierre mensual tiene como objetivos:

* Aplicar el rendimiento mensual a los balances.  
* Consolidar el histórico mensual de rendimientos.  
* Preparar la base para el cálculo de comisiones.  
* Garantizar que el dashboard refleje información coherente y definitiva.

### **13.2 Secuencia General del Proceso**

El proceso se ejecuta en el siguiente orden:

1. Introducción del porcentaje de rendimiento del mes.  
2. Cálculo automático de balances resultantes.  
3. Presentación de listado de validación.  
4. Aprobación y ejecución del cierre.  
5. Generación de registros históricos y logs.

### **13.3 Introducción del Rendimiento Mensual**

El Administrador introduce manualmente el porcentaje de rendimiento correspondiente al mes que se acaba de cerrar.

Características:

* El porcentaje es único por cierre mensual.  
* Se aplica únicamente sobre meses cerrados.  
* No se permiten cierres parciales automáticos.

### **13.4 Listado de Validación de Balances**

Una vez introducido el porcentaje, el sistema genera un listado previo de validación.

Para cada cliente, el listado muestra:

* Cliente  
* Balance actual  
* Porcentaje mensual aplicado  
* Importe del rendimiento calculado  
* Nuevo balance resultante

Cada registro incluye un checkbox que permite al Administrador:

* Confirmar la ejecución del ajuste  
* Excluir clientes específicos del cierre

Los registros aparecen preseleccionados por defecto.

### **13.5 Ejecución del Cierre**

Al confirmar el cierre:

* Solo los clientes seleccionados son actualizados.  
* El balance se actualiza de forma irreversible.  
* Se registra el rendimiento mensual en el histórico.

No existen recalculados retroactivos.

### **13.6 Errores y Exclusiones**

Si un cliente es excluido del cierre:

* Su balance permanece sin cambios.  
* No genera rendimiento en ese mes.  
* No genera comisiones asociadas a ese mes.

Esto permite manejar incidencias operativas sin bloquear el cierre global.

### **13.7 Relación con Comisiones**

Una vez ejecutado el cierre:

* Los balances resultantes pasan a ser la base de cálculo de comisiones para el mes siguiente.  
* El sistema puede generar los listados de comisiones para revisión.

El cálculo y ejecución de comisiones se realiza en procesos separados, pero siempre basados en cierres mensuales aprobados.

### **13.8 Relación con Retiros**

En meses donde corresponda una ventana de retiro:

* El cierre mensual debe completarse antes de ejecutar retiros.  
* El balance final del cierre determina el importe disponible para retiro.

Esto garantiza coherencia entre rendimientos aplicados y capital retirado.

### **13.9 Auditoría y Logs**

El cierre mensual genera registros auditables que incluyen:

* Mes y año  
* Porcentaje aplicado  
* Listado de clientes afectados  
* Usuario administrador responsable  
* Timestamp de ejecución

Estos registros garantizan trazabilidad completa de cada cierre.

**14\. Backups y Recuperación de Datos**

La plataforma APPCHULA debe contar con mecanismos de backup y recuperación de datos que garanticen la **integridad, disponibilidad y continuidad operativa** del sistema ante fallos técnicos, errores humanos o incidentes de seguridad.

Este bloque define los requisitos mínimos de respaldo y recuperación, alineados con el perfil institucional del producto y las buenas prácticas del sector.

### **14.1 Alcance de los Backups**

Los backups deben cubrir, como mínimo, los siguientes componentes:

* Base de datos principal (usuarios, balances, rendimientos, comisiones, retiros).  
* Registros históricos y logs de auditoría.  
* Configuración crítica del sistema.

No se incluyen en el alcance:

* Datos generados fuera del sistema.  
* Archivos temporales o cachés no persistentes.

### **14.2 Frecuencia y Retención**

Requisitos de backup:

* Backups automáticos diarios.  
* Backups incrementales cuando aplique.  
* Retención mínima configurable (recomendado: 30–90 días).

Los backups deben almacenarse en ubicaciones separadas del entorno de producción.

### **14.3 Seguridad de los Backups**

* Los backups deben almacenarse cifrados.  
* El acceso a los backups está restringido a personal autorizado.  
* No deben ser accesibles desde el frontend ni por usuarios finales.

### **14.4 Recuperación de Datos (Restore)**

El sistema debe permitir procesos de restauración controlados:

* Restauración total del sistema.  
* Restauración puntual de datos específicos cuando sea posible.

Las operaciones de restauración:

* Solo pueden ser ejecutadas por Administradores.  
* Deben registrarse en logs de auditoría.  
* No deben ejecutarse sin validación previa.

### **14.5 Objetivos de Recuperación**

El sistema debe definirse con objetivos claros:

* **RPO (Recovery Point Objective)**: pérdida máxima de datos aceptable.  
* **RTO (Recovery Time Objective)**: tiempo máximo de recuperación del servicio.

Estos valores deben documentarse y revisarse periódicamente.

### **14.6 Pruebas y Validación**

Los procesos de backup y restauración deben ser probados de forma periódica para garantizar su efectividad.

Las pruebas:

* No deben afectar a entornos de producción.  
* Deben quedar documentadas.  
* Permiten validar tiempos y procedimientos.

### **14.7 Auditoría**

Todas las acciones relacionadas con backups y restauraciones deben generar registros auditables, incluyendo:

* Fecha y hora.  
* Usuario responsable.  
* Tipo de operación.

