@startuml Diagram_ERD
skinparam backgroundColor #FEFEFE
skinparam class {
    BackgroundColor #E6F3FF
    BorderColor #0066CC
    ArrowColor #333333
}

title Diagram ERD - baza danych platformy (PostgreSQL)

entity "users" as users {
    * id : SERIAL <<PK>>
    --
    * email : VARCHAR(255) <<UNIQUE>>
    * password_hash : VARCHAR(255)
    * first_name : VARCHAR(100)
    * last_name : VARCHAR(100)
    phone : VARCHAR(20)
    role : ENUM('client', 'admin')
    is_active : BOOLEAN
    * created_at : TIMESTAMP
    updated_at : TIMESTAMP
}

entity "hosting_services" as hosting {
    * id : SERIAL <<PK>>
    --
    * user_id : INTEGER <<FK>>
    * domain : VARCHAR(255) <<UNIQUE>>
    * plan : VARCHAR(50)
    php_version : VARCHAR(10)
    disk_limit_mb : INTEGER
    db_type : VARCHAR(20)
    * status : ENUM('provisioning', 'active', 'suspended', 'terminated')
    k8s_namespace : VARCHAR(100)
    * created_at : TIMESTAMP
    expires_at : TIMESTAMP
}

entity "vps_services" as vps {
    * id : SERIAL <<PK>>
    --
    * user_id : INTEGER <<FK>>
    * plan : VARCHAR(50)
    * os_template : VARCHAR(100)
    cpu_cores : INTEGER
    ram_mb : INTEGER
    disk_gb : INTEGER
    ip_address : VARCHAR(45)
    proxmox_vm_id : INTEGER
    vnc_port : INTEGER
    * status : ENUM('provisioning', 'active', 'stopped', 'suspended', 'terminated')
    * created_at : TIMESTAMP
    expires_at : TIMESTAMP
}

entity "storage_services" as storage {
    * id : SERIAL <<PK>>
    --
    * user_id : INTEGER <<FK>>
    * plan : VARCHAR(50)
    disk_limit_mb : INTEGER
    used_mb : INTEGER
    minio_bucket : VARCHAR(100)
    * status : ENUM('active', 'suspended', 'terminated')
    * created_at : TIMESTAMP
    expires_at : TIMESTAMP
}

entity "storage_files" as files {
    * id : SERIAL <<PK>>
    --
    * storage_service_id : INTEGER <<FK>>
    * file_name : VARCHAR(500)
    * file_path : VARCHAR(1000)
    file_size_bytes : BIGINT
    mime_type : VARCHAR(100)
    is_deleted : BOOLEAN
    deleted_at : TIMESTAMP
    * created_at : TIMESTAMP
}

entity "file_shares" as shares {
    * id : SERIAL <<PK>>
    --
    * file_id : INTEGER <<FK>>
    * token : VARCHAR(255) <<UNIQUE>>
    * share_url : TEXT
    expires_at : TIMESTAMP
    * created_at : TIMESTAMP
}

entity "email_accounts" as email {
    * id : SERIAL <<PK>>
    --
    * hosting_service_id : INTEGER <<FK>>
    * address : VARCHAR(255) <<UNIQUE>>
    * password_hash : VARCHAR(255)
    quota_mb : INTEGER
    * created_at : TIMESTAMP
}

entity "orders" as orders {
    * id : SERIAL <<PK>>
    --
    * user_id : INTEGER <<FK>>
    * service_type : ENUM('hosting', 'vps', 'storage')
    service_id : INTEGER
    * plan : VARCHAR(50)
    * amount : DECIMAL(10,2)
    * currency : VARCHAR(3)
    * status : ENUM('pending', 'paid', 'cancelled', 'refunded')
    payment_reference : VARCHAR(255)
    * created_at : TIMESTAMP
}

entity "audit_logs" as logs {
    * id : SERIAL <<PK>>
    --
    * user_id : INTEGER <<FK>>
    * action : VARCHAR(100)
    * resource_type : VARCHAR(50)
    resource_id : INTEGER
    ip_address : VARCHAR(45)
    details : JSONB
    * created_at : TIMESTAMP
}

' Relacje
users ||--o{ hosting : "posiada"
users ||--o{ vps : "posiada"
users ||--o{ storage : "posiada"
users ||--o{ orders : "składa"
users ||--o{ logs : "generuje"
hosting ||--o{ email : "zawiera"
storage ||--o{ files : "przechowuje"
files ||--o{ shares : "udostępnia"

@enduml
