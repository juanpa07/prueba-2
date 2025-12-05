graph TD
    A[Inicio: Landing Page] --> B{Acceder al Formulario};
    B --> C[Llenar Campos de Formulario];
    C --> D{Datos Obligatorios y Válidos?};
    D -- No --> E[Mostrar Errores de Validación]
    E --> C;
    D -- Sí --> F{Validación de Recaptcha};
    F -- Fallo --> E;
    F -- Éxito --> G[Enviar Datos al Backend (Drupal)];
    G --> H{Procesamiento Exitoso?}; // <--- POSIBLE PUNTO DE CONFLICTO
    H -- Sí --> I[Mostrar Mensaje de Confirmación];
    H -- No --> J[Mostrar Error de Servidor o Integración];
    I --> K[Fin];
    J --> K;
