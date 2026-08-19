graph LR
    %% Class Definitions matching Legend Colors
    classDef crm fill:#53b313,stroke:#333,stroke-width:2px,color:#fff;
    classDef service fill:#ffee00,stroke:#333,stroke-width:2px,color:#000;
    classDef sales fill:#ff8c00,stroke:#333,stroke-width:2px,color:#fff;
    classDef purchasing fill:#008acb,stroke:#333,stroke-width:2px,color:#fff;
    classDef production fill:#530066,stroke:#333,stroke-width:2px,color:#fff;
    classDef inventory fill:#737373,stroke:#333,stroke-width:2px,color:#fff;
    classDef finance fill:#ff0000,stroke:#333,stroke-width:2px,color:#fff;
    classDef reporting fill:#e066ff,stroke:#333,stroke-width:2px,color:#fff;

    %% CRM / Business Partner Master
    subgraph CRM_SRM ["CRM / SRM"]
        Activities["Activities"]:::crm
        Customer["Customer"]:::crm
        Lead["Lead"]:::crm
        Supplier["Supplier"]:::crm
        BPMaster["Business Partner Master"]:::crm
    end

    Activities --- Customer
    Customer --- Lead
    Lead --- Supplier
    Supplier --- BPMaster

    %% Sales Flow
    subgraph Sales_Flow ["Sales Process"]
        Opportunity["Opportunity"]:::sales
        Pricing["Pricing"]:::sales
        SalesQuotation["Sales Quotation"]:::sales
        SalesOrder["Sales Order"]:::sales
        DeliveryNote["Delivery Note"]:::sales
        ARInvoice["AR Invoice"]:::sales
        IncomingPayments["Incoming Payments"]:::sales
    end

    Customer ==> Opportunity
    Opportunity --> Pricing
    Pricing --> SalesQuotation
    SalesQuotation --> SalesOrder
    SalesOrder --> DeliveryNote
    DeliveryNote --> ARInvoice
    ARInvoice --> IncomingPayments

    %% Service Flow
    subgraph Service_Flow ["Service"]
        CustomerEquipmentCard["Customer Equipment Card"]:::service
        ServiceCall["Service Call"]:::service
        ServiceContract["Service Contract"]:::service
        ServiceBilling["Service Billing"]:::service
    end

    Customer -.-> CustomerEquipmentCard
    CustomerEquipmentCard --> ServiceCall
    ServiceCall --> ServiceContract
    ServiceContract --> ServiceBilling
    ServiceBilling -.-> ARInvoice

    %% Purchasing Flow
    subgraph Purchasing_Flow ["Purchasing Process"]
        PurchaseRequest["Purchase Request"]:::purchasing
        PurchaseQuotation["Purchase Quotation"]:::purchasing
        PurchaseOrder["Purchase Order"]:::purchasing
        GoodsReceiptPO["Goods Receipt PO"]:::purchasing
        APInvoice["AP Invoice"]:::purchasing
        OutgoingPayments["Outgoing Payments"]:::purchasing
    end

    Customer -.-> PurchaseRequest
    Lead -.-> PurchaseRequest
    Supplier -.-> PurchaseQuotation
    PurchaseRequest --> PurchaseQuotation
    PurchaseQuotation --> PurchaseOrder
    PurchaseOrder --> GoodsReceiptPO
    GoodsReceiptPO --> APInvoice
    APInvoice --> OutgoingPayments

    %% Inventory & Production Master Data
    subgraph Inventory_Master ["Inventory & Item Master"]
        ItemMaster["Item Master"]:::inventory
        WarehouseMgmt["Warehouse Management"]:::inventory
    end

    CustomerEquipmentCard --- ItemMaster
    ItemMaster --- WarehouseMgmt
    WarehouseMgmt --- SalesOrder

    subgraph Production_Flow ["Production & MRP"]
        BOM["Bill of Materials"]:::production
        MRP["Material Requirements Planning"]:::production
        Sourcing["Sourcing"]:::production
        DemandPlanning["Demand Planning"]:::inventory
        ProductionOrder["Production Order"]:::production
        IssueToProduction["Issue to Production"]:::production
        ReceiptFromProduction["Receipt from Production"]:::production
    end

    BOM --> MRP
    MRP --> Sourcing
    Sourcing --> PurchaseOrder
    SalesOrder --- ProductionOrder
    ProductionOrder --> IssueToProduction
    IssueToProduction --> ReceiptFromProduction

    %% Finance & Accounting
    subgraph Finance_Flow ["Finance"]
        ChartOfAccounts["Chart of Accounts"]:::finance
        GLAccounts["General Ledger Accounts"]:::finance
        GLDetermination["G/L Account Determination"]:::finance
        CostAccounting["Cost Accounting"]:::finance
        JournalEntries["Journal Entries"]:::finance
        AP_AR["AP / AR"]:::finance
        CashManagement["Cash Management"]:::finance
        Reconciliation["Reconciliation"]:::finance
        FinancialReporting["Financial Reporting"]:::finance
    end

    ChartOfAccounts --> GLAccounts
    GLAccounts --> GLDetermination
    GLDetermination --> CostAccounting
    CostAccounting --> JournalEntries
    DeliveryNote --- JournalEntries
    APInvoice --- AP_AR
    AP_AR --- CashManagement
    CashManagement --- Reconciliation
    Reconciliation --- FinancialReporting

    %% Reporting Module
    subgraph Reporting_Flow ["Reporting"]
        BackorderReporting["Backorder Reporting"]:::reporting
        InventoryAuditReport["Inventory Audit Report"]:::reporting
        AccountBalancesReport["Account Balances Report"]:::reporting
        ProductReporting["Product Reporting"]:::reporting
    end

    ProductionOrder -.- BackorderReporting
    JournalEntries --- InventoryAuditReport
    APInvoice -.- AccountBalancesReport
    FinancialReporting --- ProductReporting
