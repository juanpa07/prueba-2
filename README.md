📄 Documento de Especificación de Requisitos (DDR)Proyecto: Formulario de Captura para Landing Page en Drupal1. Introducción y ContextoEste documento especifica los requisitos para el desarrollo de una nueva landing page con un formulario de captura dentro del sitio web existente de Drupal.Objetivo: Recopilar información de empresas interesadas mediante un formulario claro y verificable1.Alcance: El formulario será desarrollado en Storybook y se integrará en Drupal mediante un módulo/bloque que gestionará el almacenamiento de datos, permitiendo la consulta por parte del administrador.2. Tipologías de RequisitosLos requisitos se clasifican en Funcionales (RF), No Funcionales (RNF) y Restricciones (RES)2.2.1. Requisitos Funcionales (RF)IDHistoria de UsuarioDescripción DetalladaPrioridad (MoSCoW) RF-001Como Usuario, quiero rellenar y enviar el formulario de registro para compartir mi información con la empresa.El formulario debe renderizarse correctamente, incluir todos los campos y validar la entrada de datos.Must 4RF-002Como Usuario, quiero ver un mensaje de confirmación al enviar el formulario para saber que mis datos se recibieron correctamente.Después de un envío exitoso, el frontend debe mostrar un mensaje claro de éxito.Must 5RF-003Como Administrador, quiero acceder y ver un listado de todos los envíos del formulario para consultar la información recopilada.La información debe ser consultable a través de una Vista de Drupal6.Must 7RF-004Como Sistema, quiero validar el campo de Recaptcha antes del envío para prevenir el spam.El envío debe ser bloqueado si el Recaptcha no se resuelve correctamente.Must 8RF-005Como Sistema de Backend, quiero almacenar los datos del formulario para su posterior consulta.El sistema debe guardar el registro completo en el tipo de contenido/Webform de Drupal elegido.Must 92.2. Requisitos No Funcionales (RNF)IDDescripciónCriterio de VerificaciónPrioridad (MoSCoW)RNF-001RendimientoEl formulario debe cargar en menos de 3 segundos para el 95% de las consultas.Should 10RNF-002SeguridadLa comunicación entre frontend y backend debe usar HTTPS, y la validación del Recaptcha debe realizarse en el servidor.Must 11RNF-003UsabilidadEl diseño del formulario (Storybook) debe ser responsivo y verse correctamente en todos los dispositivos.Should 122.3. Restricciones (RES)IDDescripciónTipoRES-001Tecnología de FrontendEl formulario debe ser desarrollado en Storybook13.RES-002Plataforma BackendEl almacenamiento, gestión y consulta de los datos debe realizarse exclusivamente dentro del entorno Drupal14.RES-003SeguridadSe requiere la implementación de Recaptcha 3.3. Especificación Detallada del Formulario3.1. Estructura de CamposNo.Nombre del CampoTipo de DatoObservaciones1.Razón socialStringCampo obligatorio.2.Sitio webURLCampo obligatorio.3.País de constituciónString/ListaCampo obligatorio.4.Domicilio de constituciónStringCampo obligatorio.5.Países en los que operaSelección MúltipleArray de Strings.6.Sector / VerticalObjeto/TablaSección de Datos Condicionales:6.1Listado de opciones que usa VCI en el PSUString/Lista6.2Recibió Financiamiento?Booleano (Sí/No)6.2.1MontoNuméricoVisible y obligatorio si 6.2 es 'Sí'.6.2.2InstrumentoStringVisible y obligatorio si 6.2 es 'Sí'.6.3Número de personas que trabajanNuméricoCampo obligatorio.3.2. Criterios de Aceptación (Given-When-Then)Los criterios de aceptación definen cómo se verificará el cumplimiento del requisito15151515.Escenario RF-001 (Envío Exitoso):DADO que el Usuario ha completado todos los campos obligatorios de manera válida, Y ha pasado la validación de Recaptcha,CUANDO pulsa el botón "Enviar",ENTONCES el sistema muestra el mensaje de confirmación. 16Escenario RF-004 (Validación Condicional):DADO que el campo "Recibió Financiamiento" es "Sí", Y el campo "Monto" está vacío,CUANDO el Usuario pulsa "Enviar",ENTONCES el envío se detiene, Y se muestra un mensaje de error para el campo "Monto".Escenario RF-005 (Almacenamiento):DADO que se ha realizado un envío exitoso,CUANDO el Administrador accede a la Vista de Drupal,ENTONCES el registro aparece con los subcampos anidados o agrupados correctamente.4. Modelos Visuales (Mermaid)Los diagramas UML son una forma estándar de dibujar cómo funciona un sistema17171717.4.1. Diagrama de Flujo (Proceso de Envío del Usuario)Fragmento de códigograph TD
    A[Inicio: Landing Page] --> B{Acceder al Formulario};
    B --> C[Llenar Campos de Formulario];
    C --> D{Datos Obligatorios y Válidos?};
    D -- No --> E[Mostrar Errores de Validación]
    E --> C;
    D -- Sí --> F{Validación de Recaptcha};
    F -- Fallo --> E;
    F -- Éxito --> G[Enviar Datos al Backend (Drupal)];
    G --> H{Procesamiento Exitoso?};
    H -- Sí --> I[Mostrar Mensaje de Confirmación];
    H -- No --> J[Mostrar Error de Servidor o Integración];
    I --> K[Fin];
    J --> K;
4.2. Diagrama de Secuencia (Flujo de Proceso del Sistema)Fragmento de códigosequenceDiagram
    participant U as Usuario
    participant F as Formulario Storybook (Frontend)
    participant M as Módulo Drupal (Backend)
    participant R as Servicio Recaptcha (Externo)
    participant D as Base de Datos Drupal

    U->>F: 1. Datos del Formulario + Token Recaptcha
    activate F
    F->>M: 2. Petición POST con Datos y Token
    deactivate F
    activate M
    M->>R: 3. Verificar Token Recaptcha
    activate R
    R-->>M: 4. Respuesta de Validación (OK/Error)
    deactivate R

    alt Recaptcha Éxito
        M->>D: 5. Almacenar Datos (Webform / Tipo Contenido)
        activate D
        D-->>M: 6. Confirmación de Almacenamiento
        deactivate D
        M-->>F: 7. Respuesta de Éxito HTTP 200
        activate F
        F-->>U: 8. Mostrar Mensaje de Confirmación
        deactivate F
    else Recaptcha Fallo o Error de Almacenamiento
        M-->>F: 7. Respuesta de Error HTTP 4xx/5xx
        activate F
        F-->>U: 8. Mostrar Mensaje de Error
        deactivate F
    end

    deactivate M
Sugerencia: Para ob
