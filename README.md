graph TD
    subgraph Planificador ["Rol: Planificador / Usuario"]
        A([Inicio: Definir Pedidos y Previsiones]) --> B[Ejecutar Asistente de MRP]
        G[Revisar Informe de Recomendaciones] --> H{¿Aprobar Recomendaciones?}
        H -- Sí --> I[Convertir Recomendaciones a Órdenes de Compra]
    end

    subgraph MRP ["Rol: Sistema / Motor de MRP"]
        B --> C{¿Socio de Negocio Activo / No Bloqueado?}
        C -- No --> C1[Excluir automáticamente de cálculos de demanda/abastecimiento]
        C -- Sí --> D[Consolidar Demanda Bruta:<br/>- Sales Orders<br/>- Forecasts]
        D --> E[Aplicar Reglas de Negocio de Proveedores:<br/>- Asignar Proveedor Preferente<br/>- Calcular Lead Time y Fechas de Liberación<br/>- Ajustar por Múltiplos y MOQ]
        E --> F[Agrupar Recomendaciones por Proveedor]
        F --> G
        I --> J[Generar Documentos Preliminares / Órdenes de Compra<br/>Heredando precios y acuerdos marco]
    end

    subgraph BPF ["Rol: Socios de Negocio (Proveedores / Clientes)"]
        J --> K([Fin: Envío de Órdenes de Compra al Proveedor])
    end

    classDef planificador stroke:#818cf8,fill:#eef2ff;
    classDef mrp stroke:#2dd4bf,fill:#f0fdfa;
    classDef bpf stroke:#a78bfa,fill:#f5f3ff;
    
    class Planificador planificador;
    class MRP mrp;
    class BPF bpf;
```[cite: 6]

### Pasos rápidos para agregarlo a tu repositorio de GitHub:

1. En tu repositorio, haz clic en **Add file** -> **Create new file**.
2. Nómbralo con la extensión `.md` (por ejemplo, `diagrama.md`) para que GitHub lo renderice automáticamente como una página bonita.
3. Pega el bloque anterior de Markdown que incluye las etiquetas de ```mermaid ... ```.
4. Haz clic en **Commit changes...** para guardar los cambios. GitHub detectará la sintaxis y mostrará el diagrama visualmente de inmediato.
